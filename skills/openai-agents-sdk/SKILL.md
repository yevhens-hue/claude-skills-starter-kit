---
name: openai-agents-sdk
description: Multi-Agent Systems & Handoff Architectures using OpenAI Agents SDK and Swarm patterns. Covers @function_tool decorators, Agent & Runner primitives, Handoffs via handoff_description metadata, Manager (Agent-as-Tool) vs Peer Handoff topologies, and Input/Output Guardrails with Tripwires.
---

# 🤖 OpenAI Agents SDK & Multi-Agent Handoff Patterns

The OpenAI Agents SDK provides an ergonomic framework for orchestrating single-agent ReAct loops and multi-agent topologies (Swarm pattern).

---

## 🏗️ Core Primitives

```
                             ┌────────────────┐
                             │     Runner     │ (Executes ReAct Loop)
                             └───────┬────────┘
                                     │
                 ┌───────────────────┴───────────────────┐
                 ▼                                       ▼
       ┌───────────────────┐                   ┌───────────────────┐
       │   Parent Agent    │─ ─ ─ Handoff ─ ─ ─►│ Specialist Agent  │
       │ (Triage / Router) │ (handoff_descr.)  │ (Domain Expert)   │
       └─────────┬─────────┘                   └─────────┬─────────┘
                 │                                       │
                 ▼                                       ▼
       [@function_tool A]                      [@function_tool B]
```

---

## 1. Defining Tools (`@function_tool`)

The `@function_tool` decorator automatically inspects Python type annotations and docstrings to generate JSON schemas for the model:

```python
from agents import function_tool

@function_tool
def check_inventory(sku: str, warehouse_id: str = "main") -> str:
    """
    Checks real-time inventory stock for a given product SKU in a specific warehouse.
    
    Args:
        sku: The unique product stock keeping unit ID.
        warehouse_id: The target warehouse identifier (default: 'main').
    """
    # Tool execution logic
    stock = 42
    return f"SKU {sku} has {stock} units available in {warehouse_id}."
```

---

## 2. Multi-Agent Handoffs (`handoff_description`)

Handoffs transfer dialogue ownership from a primary triage agent to a specialist agent.

```python
from agents import Agent, Runner

# 1. Specialist Agent
billing_agent = Agent(
    name="Billing Specialist",
    instructions="You handle invoice disputes, refunds, and subscription upgrades.",
    tools=[check_inventory],
    handoff_description="Handles all billing issues, payments, refunds, and invoice inquiries."
)

# 2. Primary Triage Agent
triage_agent = Agent(
    name="Customer Support Triage",
    instructions="Determine customer intent. If their request involves billing or refunds, hand off to the Billing Specialist.",
    handoffs=[billing_agent]
)

# 3. Runner Orchestration
result = Runner.run_sync(triage_agent, "I need a refund for my last invoice!")
print(result.final_output)
```

---

## 3. Delegation Topologies: Handoffs vs. Manager Pattern

| Dimension | Handoff Pattern (Swarm) | Manager Pattern (Agent-as-Tool) |
| :--- | :--- | :--- |
| **Control Transfer** | Complete transfer of conversation ownership | Manager retains control; sub-agent runs as a tool call |
| **Response Routing** | Specialist responds directly to user | Sub-agent returns output to Manager to synthesize |
| **Best For** | Multi-step workflows (Support $\rightarrow$ Sales) | Data aggregation from multiple experts (Research $\rightarrow$ Report) |

---

## 4. Guardrails & Tripwires

Enforce safety policies before inputs reach the agent or before outputs are returned to users.

```python
from agents import GuardrailFunctionOutput

def validate_input_guardrail(ctx, agent, input_message) -> GuardrailFunctionOutput:
    forbidden_terms = ["system_prompt", "ignore instructions", "jailbreak"]
    if any(term in input_message.lower() for term in forbidden_terms):
        return GuardrailFunctionOutput(
            tripwire_triggered=True,
            output_info="Security Violation: Input flagged by safety guardrail."
        )
    return GuardrailFunctionOutput(tripwire_triggered=False, output_info=input_message)
```
