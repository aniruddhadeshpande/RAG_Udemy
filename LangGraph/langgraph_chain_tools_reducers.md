# 1️⃣ Chains in LangGraph

## What is a Chain (in LangGraph terms)?

A **chain** is a **linear graph** where nodes execute **one after another** with **no branching**.

From the *Chains PDF diagram*:

```
START → Node 1 → Node 2 → Node n → END
```



👉 This is the **simplest possible graph**.

---

## Key Characteristics of Chains

### 1. Linear flow

* Only **normal edges**
* No conditions
* No decisions

### 2. Nodes

* Each node is a **Python function**
* Each node:

  * takes `state`
  * updates `state`
  * passes it forward

### 3. State (Chat Messages)

From the notes:

* **Human input → state**
* **AI output → state**

This is why chains are often used for:

* chat pipelines
* preprocessing → LLM → postprocessing

---

## Chains with Chat Models

From the PDF:

* Chat models are used **inside graph nodes**
* Tools can be **bound** to chat models
* LLM acts as a **processing unit**, not a decision-maker

Example mental model:

```
Human msg → LLM → formatted response → END
```

---

## When should you use Chains?

✔ Predictable flow
✔ No branching
✔ No tool-based decisions

> **Chains = deterministic execution**

---

## One-line definition (important)

> **A chain in LangGraph is a linear, stateful sequence of nodes with no conditional routing.**

---

## Quick check (answer in 1 word)

**Question:**
Does a chain require conditional edges?

---

# 2️⃣ Router in LangGraph

Now comes the **big jump**.

---

## Why Router is needed

From the router subtitle:

> Earlier, the LLM could *suggest* a tool call, but it could not **execute** it or **route execution**. 

Router fixes this.

---

## What is a Router?

> **A router allows the LLM to decide whether to:**
>
> * respond directly
> * OR call a tool
> * OR route to another node

---

## Router Workflow (from diagram)

```
START
  ↓
LLM (with bound tools)
  ↓
Conditional Edge (Router)
 ├─ Direct Response → END
 └─ Tool Call → Tool Node → END
```

Key insight:
👉 **LLM becomes the decision-maker**

---

## What changes with Router?

### Before Router

* LLM only **returns tool call metadata**
* Execution stops

### After Router

* Tool is:

  * executed
  * output captured
  * returned to graph

This is where:

> **Node + Tools + Routing = Agent**

---

## Why Router = Agent

From the subtitle:

> This is a simple example of an agent where the LLM directs control flow. 

So:

* LLM = brain
* Tools = capabilities
* Router = control logic

✔ No human intervention
✔ Automatic decision-making

---

## Chain vs Router (Very Important)

| Aspect         | Chain  | Router      |
| -------------- | ------ | ----------- |
| Flow           | Linear | Conditional |
| Decisions      | ❌      | ✅           |
| Tool execution | ❌      | ✅           |
| Agent-like     | ❌      | ✅           |
| Complexity     | Low    | Medium–High |

---

## One-line summary

> **Chains execute, Routers decide.**

---

## Final Study Check (very important)

**Question:**
Why does adding a router convert a simple graph into an **agent**?

Answer in **one sentence**.
After that, next topic can be: **“Multi-tool router & agent loop”** (natural next step).

Great — this is an **important jump** in LangGraph 👍
I’ll teach **Tools Integration + Reducers** directly from your **PDF + subtitles**, in **study mode** (concept → diagram → why → quick check).

Sources used:

* Tools diagram PDF 
* Tools + Reducers subtitles (part 1 & 2)

---

# 1️⃣ Tools Integration in LangGraph

## Why Tools are needed

LLMs:

* do **not** have real-time data
* cannot access external systems

From subtitle example:

> “What is the current temperature of New York?”
> LLM alone **cannot answer** this. 

So we give the LLM **tools**.

---

## What is a Tool?

A **tool** is:

* a Python function
* with a **schema (JSON-like)**
* that interacts with:

  * APIs
  * Databases
  * External services

Examples:

* Weather API
* Calculator (`add(a, b)`)
* Vector database query

---

## Core Idea: Tool Binding

From the diagram:

```
Human → LLM (Brain)
           ↓
        Bound Tools
           ↓
     External Systems
```



### Binding means:

> Teaching the LLM **what tools exist** and **when to use them**

Once bound:

* LLM decides:

  * answer directly
  * OR call a tool

👉 LLM = **brain**

---

## Tool Call Flow (Simple Example)

Question:

> “What is 2 + 2?”

Execution:

```
START
 ↓
Chatbot (LLM + tools)
 ↓
Tool Condition
 ├─ Tool call → Tool Node → END
 └─ Direct answer → END
```

This matches exactly the diagram & explanation. 

---

## Tool Node (Very Important)

LangGraph provides:

* a **prebuilt ToolNode**

Why needed:

* LLM only **requests** tool calls
* ToolNode **executes** them

Without ToolNode:

* tool call metadata exists
* ❌ no actual execution

---

## Tool Condition

`tools_condition`:

* checks LLM output
* routes execution:

| LLM Output  | Route       |
| ----------- | ----------- |
| Tool call   | → Tool node |
| Normal text | → END       |

This converts a chatbot into an **agent**.

---

## Key Tool Insight (Exam-worthy)

> **LLM decides, ToolNode executes.**

---

# 2️⃣ Reducers in LangGraph

Now the **state problem**.

---

## The Problem Reducers Solve

By default:

* state updates **override** previous values

Example:

```
messages = [HumanMessage]
↓
messages = [AIMessage]   ❌ old message lost
```

But in chatbots we want:

```
[Human → AI → Tool → AI]
```

---

## What is a Reducer?

A **reducer** defines:

> *How state updates are merged*

From subtitle:

> “Reducers allow us to specify how state updates are performed.” 

---

## `add_messages` Reducer (Most Important)

Purpose:

* **append messages**
* instead of overriding

Used with:

```text
Annotated[List[AnyMessage], add_messages]
```

Effect:

```
Old messages + New message → Combined state
```

---

## Why Reducers are Critical

Without reducers:

* conversation history is lost
* tools + agents break

With reducers:

* memory grows
* agents reason better

👉 **Reducers = memory mechanics**

---

## Tools + Reducers Together (Big Picture)

| Component  | Role          |
| ---------- | ------------- |
| LLM        | Brain         |
| Tool       | Capability    |
| ToolNode   | Execution     |
| Reducer    | Memory        |
| StateGraph | Orchestration |

This is **full agent architecture**.

---

## One-line Summary (Lock this 🔒)

> **Tools extend what an LLM can do; reducers preserve what the agent remembers.**

---

## Final Study Check (answer in 1 sentence)

**Question:**
Why does LangGraph require reducers when building tool-based chat agents?

(After this, next natural topic → **Multi-tool agents + looping graphs**.)
