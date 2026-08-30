---
name: geo-aeo-agent-optimization
description: Generative Engine Optimization (GEO) and Answer Engine Optimization (AEO) rules, structured entity grounding (JSON-LD), inverted pyramid content formatting, and SOPs for training AI agents to maximize visibility across Google AI Overviews, Perplexity, and ChatGPT Search.
---

# 🌐 GEO & AEO AI Agent Optimization Skill

This skill defines deterministic rules, schemas, and SOPs for optimizing web content, technical documentation, and AI agent output for Generative Engine Optimization (**GEO**) and Answer Engine Optimization (**AEO**).

---

## 🎯 1. Core Principles of GEO/AEO vs Traditional SEO

| Dimension | Traditional SEO | AEO (Answer Engine Opt.) | GEO (Generative Engine Opt.) |
| :--- | :--- | :--- | :--- |
| **Primary Target** | Blue links on Google SERP | Featured Snippets, PAA (People Also Ask) | Citations in Perplexity, ChatGPT Search, Gemini, Google AI Overviews |
| **Parsing Unit** | Keywords in Title & H1 | 40-55 word direct answer block below H2 | Knowledge graphs, semantic entity triples, verified numbers |
| **Validation Gate** | Backlinks & CTR | Rich Results & schema matching | Information Gain score & consensus authority |

---

## 📐 2. Deterministic Content Formatting Rules (For AI Agents)

1. **Inverted Pyramid Answer Block (First 60 Words):**
   * Immediately below any major heading (`<h2>`, `<h3>`), place a 40–55 word direct, declarative answer.
   * Prohibit pleasantries ("In this article...", "It is important to remember").
   * *Formula:* `[Entity/Topic] is [Category/Definition] that [Primary Function/Mechanism] by [Concrete Differentiator].`

2. **Structured Table Extraction (Markup Rule):**
   * Always format multi-item comparisons (features, pricing, benchmarks) into native HTML `<table>` blocks with strict `<th>` and `<td>` tags.
   * LLM crawlers prioritize tabular data over narrative prose for comparative queries.

3. **Information Gain & Primary Statistics:**
   * Every article must contain at least 2 proprietary or specific numerical findings (e.g., *"Based on an evaluation of 500+ endpoints..."*).
   * LLMs favor citing primary sources with novel metrics rather than duplicate paraphrases.

4. **Entity Bold Formatting in Lists:**
   * In bullet points, always start with a bold entity label (e.g., `**1. Protocol Latency:** ...`).

---

## 🏷️ 3. JSON-LD Entity Graph Schema Template

Every optimized page must inject unambiguous Schema.org entity definitions linking to external knowledge graphs (Wikidata / Crunchbase):

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://example.com/#organization",
      "name": "BrandName",
      "url": "https://example.com",
      "sameAs": [
        "https://www.wikidata.org/wiki/Q...",
        "https://www.crunchbase.com/organization/..."
      ]
    },
    {
      "@type": "TechArticle",
      "@id": "https://example.com/post-slug/#article",
      "isPartOf": { "@id": "https://example.com/#website" },
      "headline": "Exact Target Topic Headline",
      "about": [
        {
          "@type": "Thing",
          "name": "Primary Entity",
          "sameAs": "https://en.wikipedia.org/wiki/..."
        }
      ],
      "author": {
        "@type": "Person",
        "name": "Yevhen Shaforostov",
        "jobTitle": "AI Product Manager & Full-Stack AI Engineer",
        "sameAs": "https://github.com/yevhens-hue"
      }
    }
  ]
}
</script>
```

---

## ✅ 4. AI Agent Quality & Validation Gate (Pass/Fail Checklist)

Before publishing or accepting an agent output, run this 5-point verification:
1. [ ] **Inverted Answer Block:** Does the H2 have an immediate 40–55 word direct definition?
2. [ ] **Entity Ambiguity:** Are all brand and technical terms linked to unambiguous schema definitions?
3. [ ] **Information Gain:** Does the text contain at least one verifiable quantitative statistic or benchmark?
4. [ ] **Tabular Comparison:** Is comparative data presented in a clean `<table>` instead of a paragraph?
5. [ ] **Zero Fluff:** Are all filler introductions removed?
