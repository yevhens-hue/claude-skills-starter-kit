---
name: multi-agent-crew-patterns
description: Hierarchical & Sequential Multi-Agent Team Topologies. Covers role specialization (Researcher, Writer, Reviewer, Engineer), shared state passing, task dependency DAGs, error recovery, and consensus voting algorithms for autonomous task resolution.
---

# 👥 Multi-Agent Crew & Team Topologies

Multi-Agent Crews partition complex, multi-domain goals across specialized AI agents. Each agent possesses explicit role definitions, goal boundaries, dedicated toolsets, and structured inter-agent communication channels.

---

## 🏗️ Topology Patterns

```
      ┌────────────────────────────────────────────────────────┐
      │  1. SEQUENTIAL PIPELINE (Linear Dependency DAG)         │
      │  [Researcher Agent] ──► [Writer Agent] ──► [Reviewer]  │
      └────────────────────────────────────────────────────────┘

      ┌────────────────────────────────────────────────────────┐
      │  2. HIERARCHICAL ORCHESTRATION (Manager + Workers)     │
      │                   ┌──────────────┐                     │
      │                   │ Manager Agent│                     │
      │                   └──────┬───────┘                     │
      │          ┌───────────────┼───────────────┐             │
      │          ▼               ▼               ▼             │
      │  [Data Collector]  [Code Generator] [QA Auditor]       │
      └────────────────────────────────────────────────────────┘
```

---

## 1. Role Specification & Agent Identity

Every agent in a crew requires 4 distinct configuration parameters:

```python
from dataclasses import dataclass
from typing import List, Callable

@dataclass
class CrewAgentConfig:
    role: str            # e.g., "Senior Security Auditor"
    goal: str            # e.g., "Identify SQL injection and auth vulnerabilities in PRs"
    backstory: str       # Domain perspective, tone, and constraints
    tools: List[Callable]# Accessible function tools
    allow_delegation: bool = False
```

---

## 2. Sequential Task Execution Engine

Tasks depend on outputs from prior steps:

```python
class TaskDAG:
    def __init__(self, description: str, agent: CrewAgentConfig):
        self.description = description
        self.agent = agent
        self.output = None

class SequentialCrewRunner:
    def __init__(self, agents: List[CrewAgentConfig], tasks: List[TaskDAG]):
        self.agents = agents
        self.tasks = tasks

    def execute(self, initial_input: str) -> str:
        previous_output = initial_input
        for task in self.tasks:
            print(f"--> [AGENT: {task.agent.role}] Executing Task: {task.description}")
            prompt = f"Previous Step Output:\n{previous_output}\n\nTask: {task.description}"
            # Execute LLM call with agent instructions & tools
            task.output = self.invoke_agent(task.agent, prompt)
            previous_output = task.output
        return previous_output

    def invoke_agent(self, agent: CrewAgentConfig, prompt: str) -> str:
        # LLM ReAct execution stub
        return f"[{agent.role} Result]: Processed '{prompt[:60]}...'"
```

---

## 3. Consensus Voting & Verification Loop

For high-stakes decisions (financial transfers, deployment gating, legal compliance), employ an **N-Agent Majority Voting Consensus**:

```python
def consensus_gating_vote(voters: List[CrewAgentConfig], task_payload: str) -> dict:
    votes = []
    for voter in voters:
        decision = get_agent_vote(voter, task_payload) # Returns {"vote": "APPROVE" | "REJECT", "reason": "..."}
        votes.append(decision)
        
    approvals = sum(1 for v in votes if v["vote"] == "APPROVE")
    passed = approvals >= (len(voters) // 2 + 1)
    
    return {
        "consensus_passed": passed,
        "approval_count": approvals,
        "total_voters": len(voters),
        "details": votes
    }
```
