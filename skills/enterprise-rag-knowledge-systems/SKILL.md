---
name: enterprise-rag-knowledge-systems
description: Real-time COGS metering (calculating exact $ cost per voice minute, SMS unit, LLM token, and per-tenant margin analytics) for multi-tenant AI SaaS platforms.
---

# 📊 COGS Metering & Usage Billing Skill

This skill defines the telemetry and real-time cost metering architecture for multi-tenant AI SaaS platforms.

---

## 🛠️ COGS Calculation Blueprint

For every voice call, SMS conversation, or AI workflow, compute exact operational cost (COGS):

$$\text{COGS} = (\text{Voice Mins} \times C_{\text{voice}}) + (\text{SMS Units} \times C_{\text{sms}}) + (\text{Tokens}_{\text{in}} \times C_{\text{in}}) + (\text{Tokens}_{\text{out}} \times C_{\text{out}})$$

Where default cost rates are:
- $C_{\text{voice}} = \$0.07 / \text{min}$ (Retell AI / Telephony)
- $C_{\text{sms}} = \$0.0079 / \text{msg}$ (Twilio / Telnyx)
- $C_{\text{in}} = \$0.00000025 / \text{token}$ (Claude 3.5 Haiku Input)
- $C_{\text{out}} = \$0.00000125 / \text{token}$ (Claude 3.5 Haiku Output)

---

## 💻 Sample Metering Record (JSON)

```json
{
  "tenant_id": "rooftop_881",
  "call_id": "call_abc992",
  "duration_seconds": 184,
  "usage": {
    "voice_minutes": 3.06,
    "voice_cost_usd": 0.214,
    "llm_tokens_total": 3450,
    "llm_cost_usd": 0.0042,
    "telephony_cost_usd": 0.045
  },
  "total_cogs_usd": 0.2632
}
```
