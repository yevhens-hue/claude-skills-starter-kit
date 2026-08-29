# Claude Code & Multi-Agent Skills Starter Kit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Compatible-orange.svg)](https://anthropic.com)
[![LangGraph](https://img.shields.io/badge/LangGraph-Ready-blue.svg)](https://langchain.com)

An open-source starter kit of production-ready agent skills, test-driven workflows, and hybrid RAG search architectures for **Claude Code**, **Cursor**, **Windsurf**, and autonomous multi-agent harnesses.

---

## ⭐️ What's Inside the Free Starter Kit:

1. **`tdd-workflow`**: Enforces strict Red-Green-Refactor test loops before writing application logic.
2. **`api-and-interface-design`**: Schema-first contract stability with Zod and Pydantic validation.
3. **`doubt-driven-development`**: Adversarial self-review catching memory leaks and edge cases.
4. **`hybrid-rag-vector-search`**: Reciprocal Rank Fusion (RRF) combining PostgreSQL `tsvector` with `pgvector`.
5. **`observability-and-instrumentation`**: Zero-blindspot JSON logging, token cost tracking, and latency profiling.

---

## 📖 Deep-Dive Technical Guides (Dev.to)

- 📝 [How I Architected 84 Custom Skills for Claude Code](https://dev.to/yevhen_shaforostov_5a73a4/how-i-architected-84-custom-skills-for-claude-code-to-automate-my-daily-engineering-3fap)
- 📝 [5 Claude Code Skills That Saved Me 10 Hours a Week](https://dev.to/yevhen_shaforostov_5a73a4/5-claude-code-skills-that-saved-me-10-hours-a-week-with-real-code-architecture-c4f)
- 📝 [How to Build a Production-Grade RAG Agent with Hybrid Search in 30 Minutes](https://dev.to/yevhen_shaforostov_5a73a4/how-to-build-a-production-grade-rag-agent-with-hybrid-search-in-30-minutes-31of)

---

## 🚀 Complete Production Packs & Starter Kits (Gumroad)

Get access to our complete production engineering assets *(use coupon `LAUNCH20` for 20% off)*:

| Package | Description | Link |
|---|---|---|
| **LangGraph Multi-Agent Starter Kit ($29)** | Full TypeScript & Python codebase, Supervisor topology, Redis+pgvector memory, and RRF search. | [Get Starter Kit ($23.20)](https://shaforostov5.gumroad.com/l/ebttz/LAUNCH20) |
| **Claude Code Prompts Pack ($19)** | 50 battle-tested production prompt templates for multi-agents, security, and refactoring. | [Get Prompts Pack ($15.20)](https://shaforostov5.gumroad.com/l/vytivj/LAUNCH20) |
| **Claude AI Engineering Skills Pack ($49)** | Complete collection of 84 production skills covering full-stack AI engineering. | [Get 84 Skills Pack ($39.20)](https://shaforostov5.gumroad.com/l/njzfuo/LAUNCH20) |

---

## 🛠️ Quickstart

Clone the repository and copy the skills to your local Claude configuration:

```bash
git clone https://github.com/yevhens-hue/claude-skills-starter-kit.git
cp -r claude-skills-starter-kit/skills/* ~/.gemini/config/skills/
```

## 📄 License
MIT License. Created by [Yevhen Shaforostov](https://github.com/yevhens-hue).
