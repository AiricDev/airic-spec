## **Airic Spec v0.2**

**Memory-Centric Agent Runtime Model**

### **1. Overview**

Airic v0.2 defines a **memory-based agent runtime architecture**.

In this model, an Agent's identity is defined not by its instructions, but by its **exclusive memory context** (Context Sharding). Its capabilities (Skills), conversely, are not hardcoded but are **dynamically loaded from Live Documents**.

Collaboration achieves scalability by adhering to a strict **Message Passing** protocol between these distinct memory contexts, rather than relying on a shared global context window.

---

### **2. Core Operational Principles**

1.  **Memory as Identity (Context Sharding)**
    An Agent is defined by the specific slice of data it "remembers" functionality (e.g., "The Payment Service Agent" vs "The Personal Journal Agent").
    *   *Principle*: Context is the scarce resource. Physical isolation of context ensures high attention density and reduces hallucination.

2.  **Document as Skill (Dynamic Loading)**
    Skills are explicitly defined in **Live Documents** (SOPs, Prompt Templates, Tool sets). Any Agent (Memory Entity) can load any Skill Document to perform a specific type of work.
    *   *Formula*: `Runtime Agent = Specific Memory (Identity) + Loaded Skill Document (Capability)`

3.  **Collaboration via Message Passing**
    Agents do not share a global context. When Agent A needs information from Agent B, it must explicitly request it via a message.
    *   *Analogy*: Like microservices communicating via API, or humans communicating via Slack. This enforces clear system boundaries.

4.  **Everything is a Document**
    The system remains self-describing. Memory definitions, Skill definitions, and Task instances are all structured Markdown documents.

---

### **3. The Runtime Model**

#### **3.1. Anatomy of a Runtime Entity**

A running Agent instance is instantiated by a Work Document declaring two orthogonal bindings:

```yaml
---
agent: <MemorySpec>   # WHO: Defines the Context/Memory Scope
skill: <SkillSpec>    # HOW: Defines the Capability/Methodology
---
```

#### **3.2. Document Types (The "Everything is a Document" Hierarchy)**

The architecture distinguishes between three primary document types:

| Document Type | Role | Content Definition |
| :--- | :--- | :--- |
| **Memory Spec** | **Identity** | Defines data source bindings (e.g., folders, repos), read/write permissions, and persistable long-term memory. |
| **Skill Spec** | **Capability** | Defines System Prompts, available Tools, Function Calls, Standard Operating Procedures (SOPs), and Checklists. |
| **Task Instance** | **Workspace** | The actual instance of work. It binds a Memory Spec and a Skill Spec to execute a specific task. |

#### **3.3. Collaboration Protocol**

1.  **Intra-Agent Loop (The "Focus" Mode)**
    *   The Agent loads its **Memory Spec** (e.g., the current codebase) and the **Skill Spec** (e.g., "Refactoring Guide").
    *   It reads the **Task Instance**.
    *   It executes the task steps, writing results and artifacts directly back into the Task Instance.

2.  **Inter-Agent Communication (The "Collaboration" Mode)**
    *   **Trigger**: The Agent realizes it lacks necessary context (e.g., "I see the API call, but I don't know the database schema").
    *   **Action**: The Agent uses a `send_message` tool.
    *   **Routing**: The message is routed to the Agent defined by the target's Memory Spec (e.g., "Database Agent").
    *   **Response**: The target Agent processes the request within *its own* context and returns a concise answer.
    *   **Integration**: The response is pasted explicitly into the requesting Agent's context.

---

### **4. Example Scenarios**

#### **Scenario A: The Specialized Developer**

**Document**: `tasks/feature-implementation.md`
```markdown
---
agent: agents/backend-service-mem.md   # Identity: Has access to Backend Repo & Logs
skill: skills/tdd-implementation.md    # Skill: Knows Test-Driven Development flow
---

# Objective
Implement the new specific User API.

# Agent Execution
1. [Skill] Writing failing test case...
2. [Memory] Reading `src/models/User.ts`...
3. [Action] Implementation complete.
```

#### **Scenario B: Cross-Context Collaboration**

**Document**: `tasks/system-integration.md`
```markdown
---
agent: agents/frontend-mem.md
skill: skills/api-integration.md
---

# Agent Execution
...
3. [Blocker] I need the expected return format for `GET /users/me`.
4. [Tool: send_message(recipient="agents/backend-mem.md", content="What is the JSON schema for GET /users/me?")]
5. [Incoming Message from Backend Agent]:
   > The schema is `{ "id": "uuid", "email": "string" }`.
6. [Action] updating TypeScript interface...
```

---

### **5. Extension & Compatibility**

*   **MCP Compatibility**: Airic v0.2 treats **MCP Servers as Memory Sources** and **MCP Tools as Skill Components**, allowing seamless integration with the Model Context Protocol.
*   **Self-Evolution**: Since Skills are documents, an Agent can be tasked to improve a Skill Document, effectively "learning" or "optimizing" its own future behavior.

---

### **6. License**

Draft specification — © Leric 2026.
Released under the **CC BY-SA 4.0** license.
