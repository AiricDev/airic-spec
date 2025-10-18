## **Airic Spec v0.2**

**Document-Centric Agent Runtime Model**

### **1. Overview**

Airic defines a **document-driven runtime architecture** for human–AI collaboration.
It treats *documents as executable context* — the canonical source of truth for agent behavior, workflows, and quality criteria.
Rather than aiming for one-shot automation, Airic establishes a **composable, auditable collaboration protocol** between humans and AI.

---

### **2. Core Principles**

1. **Everything is a Document**
   Every behavior, workflow, schema, or template exists as a Markdown document with lightweight metadata.
   Documents are both persistent memory and executable runtime.

2. **Declarative Runtime Binding**
   Each work document declares:

   ```yaml
   agent: <AgentName>
   doctype: <DoctypeName>
   ```

   These references link to other documents that define how the agent thinks and how the document should be structured.

3. **Self-Describing System**
   The very documents that define agents and doctypes are themselves governed by other doctypes.
   Airic is recursive — its own architecture is described within the same model (“documents all the way down”).

4. **Live Collaboration**
   Documents are *active*: agents read, plan, execute, and write back directly into the same document, creating a continuous feedback loop.

5. **Inheritance and Local Overrides**
   Documents form a tree. Parent docs set shared rules; child docs inherit context and override locally.
   This promotes consistency without rigidity.

---

### **3. Document Types**

#### **3.1. Recursive Definition**

Airic does not hard-code document types.
Instead, it defines *patterns of metadata and structure* that themselves live as documents.

Example chain:

```text
Doctype → defines → Agent Definition
Agent Definition → defines → Agent Behavior
Doctype Definition → is defined by → Doctype: "Doctype Definition"
```

This recursive property means the system can evolve, extend, or rewrite its own meta-layer entirely through documents — no external code changes required.
Agents can help users author new agent or doctype definitions within the same environment, effectively *co-creating the system that defines them*.

#### **3.2. Foundational Doctypes (Reference Set)**

While users can define arbitrary doctypes, a minimal bootstrapping set is provided:

| Doctype                 | Purpose                                               | Example Metadata                                                       |
| ----------------------- | ----------------------------------------------------- | ---------------------------------------------------------------------- |
| **Agent Definition**    | Defines persona, tone, tools, and collaboration rules | `system_prompt`, `tools_allowed`, `protocol`, `output_schema`          |
| **Doctype Definition**  | Defines structure and quality criteria                | `sections`, `mandatory_fields`, `acceptance_criteria`, `review_rubric` |
| **Workflow Definition** | Encodes executable, stepwise logic                    | `steps`, `inputs`, `outputs`, `expected_results`                       |
| **Work Document**       | User-facing doc binding agent + doctype               | `agent:`, `doctype:` metadata                                          |
| **Meta-Doctype**        | Describes the schema of doctypes themselves           | Used to generate or validate new doctypes                              |

These are reference implementations only — they can be extended, versioned, or replaced.

---

### **4. Runtime Behavior**

1. **Agent Instantiation**
   The runtime loads the referenced `agent:` document, constructing a composite system prompt and tool context.

2. **Context Stack Assembly**
   Parent documents are traversed to build an inherited context stack; shared rules and variables propagate downward.

3. **Structural Enforcement**
   The `doctype:` definition enforces required sections and validation rubrics, providing explicit collaboration scaffolding.

4. **In-Page Execution**
   Workflow steps are executed within the live document. Agents write results and commentary back in-page.

5. **Context Indexing**
   All live documents and discussions form a searchable, graph-like knowledge base. Agents query this index for precise context retrieval.

---

### **5. Example**

```markdown
---
agent: ResearchPartner
doctype: ResearchBrief
---

# Topic
LLM-driven UI generation

# Objective
Survey approaches for declarative interface synthesis.

# Findings
...
```

```markdown
# Agent: ResearchPartner
system_prompt: >
  You are a methodical research partner who decomposes problems
  and structures findings clearly.
tools_allowed: [web_search, note_append]
tone: analytical
output_schema: markdown_sections
```

```markdown
# Doctype: ResearchBrief
sections:
  - Topic
  - Objective
  - Findings
acceptance_criteria:
  - Each section is present and non-empty
  - Sources are cited
```

---

### **6. Extensibility**

* **Composable definitions** — any document can extend or override another via inheritance.
* **Pluggable runtimes** — compatible with MCP, Notion API, or local Markdown stores.
* **Cross-agent orchestration** — workflows may spawn sub-agents via linked documents.
* **Self-evolution** — new doctypes and agents can be generated by the system itself.

---

### **7. Use Cases**

* Collaborative knowledge work (research, planning, review)
* AI-native project management and documentation systems
* Enterprise compliance workflows with auditability
* Developer frameworks for structured, context-aware agent execution

---

### **8. Compatibility**

Airic’s model aligns conceptually with:

* **Anthropic Skills** — runtime persona switching
* **Notion Agents** — document-defined context
* **OpenAI MCP** — structured model context protocol

Airic generalizes these into a single, declarative, recursively self-describing framework.

---

### **9. License**

Draft specification — © Leric 2025.
Released under the **CC BY-SA 4.0** license for open research and implementation.
