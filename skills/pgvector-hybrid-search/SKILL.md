---
name: pgvector-hybrid-search
description: Production PostgreSQL & pgvector Hybrid Vector Search RAG Architecture. Implements native vector embeddings with HNSW/IVFFlat indexes, full-text TSVECTOR search, in-database Reciprocal Rank Fusion (RRF), Supabase/Prisma ORM integration, and connection pooling.
---

# 🐘 PostgreSQL + pgvector Hybrid Search & RAG Architecture

`pgvector` turns PostgreSQL into a high-performance vector database. Combining `pgvector` dense similarity search with PostgreSQL native Full-Text Search (`tsvector`) provides production-grade Hybrid RAG without requiring an external vector DB.

---

## 🏗️ Schema Setup (`pgvector` + `tsvector`)

```sql
-- 1. Enable pgvector extension
CREATE EXTENSION IF NOT EXISTS vector;

-- 2. Create Knowledge Base Table
CREATE TABLE document_chunks (
    id          BIGSERIAL PRIMARY KEY,
    document_id UUID NOT NULL,
    content     TEXT NOT NULL,
    metadata    JSONB DEFAULT '{}'::jsonb,
    
    -- Dense Vector Embedding (1536 dimensions for OpenAI / 1024 for Cohere)
    embedding   vector(1536),
    
    -- Sparse Full-Text Search Vector
    fts_tokens  tsvector GENERATED ALWAYS AS (to_tsvector('english', content)) STORED,
    
    created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- 3. Create HNSW Vector Index for fast approximate nearest neighbor (ANN) search
CREATE INDEX idx_chunks_embedding_hnsw 
ON document_chunks 
USING hnsw (embedding vector_cosine_ops)
WITH (m = 16, ef_construction = 64);

-- 4. Create GIN Index for Fast Full-Text Search
CREATE INDEX idx_chunks_fts 
ON document_chunks 
USING gin (fts_tokens);
```

---

## 2. In-Database Hybrid Search Query (RRF in SQL)

Perform dense vector search and full-text keyword search in a single SQL query, combining their ranks via **Reciprocal Rank Fusion (RRF)** ($k = 60$):

```sql
WITH vector_search AS (
    SELECT id, content, metadata,
           ROW_NUMBER() OVER (ORDER BY embedding <=> :query_embedding) AS rank
    FROM document_chunks
    ORDER BY embedding <=> :query_embedding
    LIMIT 50
),
fts_search AS (
    SELECT id, content, metadata,
           ROW_NUMBER() OVER (ORDER BY ts_rank_cd(fts_tokens, websearch_to_tsquery('english', :query_text)) DESC) AS rank
    FROM document_chunks
    WHERE fts_tokens @@ websearch_to_tsquery('english', :query_text)
    ORDER BY rank
    LIMIT 50
)
SELECT 
    COALESCE(v.id, f.id) AS id,
    COALESCE(v.content, f.content) AS content,
    COALESCE(v.metadata, f.metadata) AS metadata,
    (COALESCE(1.0 / (60 + v.rank), 0.0) + COALESCE(1.0 / (60 + f.rank), 0.0)) AS rrf_score
FROM vector_search v
FULL OUTER JOIN fts_search f ON v.id = f.id
ORDER BY rrf_score DESC
LIMIT 5;
```

---

## 3. Python Integration (AsyncPG / SQLAlchemy)

```python
import asyncpg
import numpy as np

async def query_hybrid_rag(pg_pool, query_text: str, query_embedding: list[float], limit: int = 5):
    embedding_array = f"[{','.join(map(str, query_embedding))}]"
    
    sql = """
    WITH vector_search AS (
        SELECT id, content, metadata,
               ROW_NUMBER() OVER (ORDER BY embedding <=> $1::vector) AS rank
        FROM document_chunks
        ORDER BY embedding <=> $1::vector
        LIMIT 50
    ),
    fts_search AS (
        SELECT id, content, metadata,
               ROW_NUMBER() OVER (ORDER BY ts_rank_cd(fts_tokens, websearch_to_tsquery('english', $2)) DESC) AS rank
        FROM document_chunks
        WHERE fts_tokens @@ websearch_to_tsquery('english', $2)
        ORDER BY rank
        LIMIT 50
    )
    SELECT 
        COALESCE(v.id, f.id) AS id,
        COALESCE(v.content, f.content) AS content,
        (COALESCE(1.0 / (60 + v.rank), 0.0) + COALESCE(1.0 / (60 + f.rank), 0.0)) AS rrf_score
    FROM vector_search v
    FULL OUTER JOIN fts_search f ON v.id = f.id
    ORDER BY rrf_score DESC
    LIMIT $3;
    """
    
    async with pg_pool.acquire() as conn:
        rows = await conn.fetch(sql, embedding_array, query_text, limit)
        return [dict(r) for r in rows]
```

---

## 4. Index Tuning Guidelines

* **HNSW vs IVFFlat:** Use `HNSW` for highest query QPS and accuracy without requiring table warming. Use `IVFFlat` only if memory is strictly constrained.
* **Distance Metrics:**
  * `<=>` Cosine Distance (Normalized embeddings, recommended for text).
  * `<->` L2 Euclidean Distance.
  * `<#>` Negative Inner Product (Dot product).
