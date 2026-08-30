---
name: langchain-agent-patterns
description: Production LangChain LCEL & Agent Architecture. Covers LangChain Expression Language (LCEL) piping, @tool docstrings & type hint schema generation, output parsers (StrOutputParser, JsonOutputParser), agent state management, and agent.invoke() ReAct execution loops.
---

# 🦜🔗 LangChain LCEL & Agent Architecture Patterns

LangChain provides composable primitives for building LLM pipelines via LangChain Expression Language (LCEL) and orchestrating tool-calling agents.

---

## 1. LangChain Expression Language (LCEL)

LCEL uses the pipe operator (`|`) to compose components into declarative, streamable chains.

$$\text{PromptTemplate} \xrightarrow{\quad\vert\quad} \text{ChatModel} \xrightarrow{\quad\vert\quad} \text{OutputParser}$$

```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_openai import ChatOpenAI

# 1. Define Prompt Template
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are an expert software architect. Provide concise code reviews."),
    ("user", "{code_snippet}")
])

# 2. Define Model
model = ChatOpenAI(model="gpt-4o-mini", temperature=0.0)

# 3. Compose Chain via LCEL Pipe
review_chain = prompt | model | StrOutputParser()

# 4. Invoke Chain
result = review_chain.invoke({"code_snippet": "def add(a, b): return a + b"})
print(result)
```

---

## 2. Defining Tools (`@tool`)

In LangChain, docstrings are **mandatory** because they are passed directly to the LLM as the tool description. Type hints are used to build the JSON input schema.

```python
from langchain_core.tools import tool
from pydantic import BaseModel, Field

# Option A: Functional @tool with Type Hints & Docstring
@tool
def calculate_tax(amount: float, tax_rate: float = 0.20) -> float:
    """
    Calculates tax amount for a given financial transaction value.
    
    Args:
        amount: The total monetary amount.
        tax_rate: Tax percentage in decimal format (default: 0.20 for 20%).
    """
    return amount * tax_rate

# Option B: Pydantic Schema for Complex Tools
class SearchInput(BaseModel):
    query: str = Field(description="Search keywords or domain query")
    max_results: int = Field(default=5, description="Number of results to return")

@tool("domain_search", args_schema=SearchInput)
def domain_search(query: str, max_results: int = 5) -> str:
    """Searches the internal knowledge database for matched records."""
    return f"Found {max_results} matches for '{query}'"
```

---

## 3. Agent Execution (`agent.invoke()`)

When calling `agent.invoke()`, LangChain automatically handles:
1. Converting tool definitions to LLM tool schemas.
2. Intercepting tool-call requests from the model.
3. Executing local tool functions with parsed parameters.
4. Feeding observations back into the model in a ReAct loop.

```python
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_core.prompts import ChatPromptTemplate

tools = [calculate_tax, domain_search]

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful financial assistant."),
    ("placeholder", "{chat_history}"),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}"),
])

# Create Agent & Agent Executor
agent = create_tool_calling_agent(model, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

# Execute ReAct Agent Loop
response = agent_executor.invoke({"input": "What is the 20% tax on $1,500?"})
print(response["output"])
```
