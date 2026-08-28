# ⚡ Claude Skills Starter Kit (Free Open Source)

> Drop-in domain skill files that turn **Claude Code** and **Antigravity** into a Senior AI Engineer, Architect, and Product Manager.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Supported-7C3AED.svg)](https://claude.ai)
[![Skills](https://img.shields.io/badge/Free_Skills-5-emerald.svg)](./skills)
[![Full Pack](https://img.shields.io/badge/Full_Pack-84_Skills-orange.svg)](https://shaforostov5.gumroad.com/l/njzfuo)

---

## 🔥 Looking for the Full Production Suite?

This repo contains **5 starter skills**. 

If you want the **complete battle-tested pack of 84 production-ready skills** (AI Agents, RAG, pgvector, LangGraph, Multi-Agent Crews, OCR Vision, B2B Growth, and PRD tools) used daily in production at a B2B SaaS:

👉 **[Get the Full 84-Skill Pack on Gumroad ($49)](https://shaforostov5.gumroad.com/l/njzfuo)**

---

## 📦 What's in this Starter Kit (5 Skills)

| Skill | Category | Description |
|---|---|---|
| **`agent-introspection-debugging`** | 🤖 AI Agents | Structured self-debugging workflow & synthetic failure injection for AI systems |
| **`tdd-workflow`** | ⚙️ Engineering | Test-driven development with 80%+ test coverage enforcement |
| **`api-design`** | 📐 Architecture | REST & Interface design standards, error codes, and idempotent contracts |
| **`security-review`** | 🔒 Security | Pre-flight security checklists, OWASP guardrails, and input sanitization |
| **`prd-critic`** | 📋 Product | Automated PRD review against user outcomes, edge cases, and testability |

---

## 🚀 Quickstart Installation (30 Seconds)

### Step 1: Clone the Repo
```bash
git clone https://github.com/yevhens-hue/claude-skills-starter-kit.git
```

### Step 2: Copy to your local Claude / Antigravity config
```bash
# For Claude Code / CLI:
mkdir -p ~/.claude/skills
cp -r claude-skills-starter-kit/skills/* ~/.claude/skills/

# Or for Gemini / Antigravity IDE:
mkdir -p ~/.gemini/config/skills
cp -r claude-skills-starter-kit/skills/* ~/.gemini/config/skills/
```

### Step 3: Run Claude
That's it! When you ask Claude to write tests, design an API, or debug an agent, it will automatically load the corresponding `SKILL.md` into its context window.

---

## 🧠 How it Works

Instead of polluting system prompts with thousands of tokens of static instructions, each skill is defined in a structured `SKILL.md` with YAML metadata.

```markdown
---
name: agent-introspection-debugging
description: Structured self-debugging workflow for AI agent failures
---

# 🕵️ Agent Introspection Protocol
...
```

The AI assistant automatically indexes and loads relevant skills on-demand based on your conversation context.

---

## 🌟 Star this repo!
If you find this useful, please give it a ⭐️ on GitHub!

---

## 📄 License
MIT © [Yevhen Shaforostov](https://github.com/yevhens-hue)
