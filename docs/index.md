# 🌌 ExoModel AI
### Your Python objects, autonomous.
### Describe what you want — the object thinks, acts, and responds.

[![GitHub Link](https://img.shields.io/badge/GitHub-Link-blue)](https://github.com/exomodel-ai/exomodel)
[![PyPI version](https://img.shields.io/pypi/v/exomodel?color=blue)](https://pypi.org/project/exomodel/)
[![License](https://img.shields.io/github/license/exomodel-ai/exomodel)](https://github.com/exomodel-ai/exomodel/blob/main/LICENSE)

---

Your domain objects already know how to do things. Making them understand a natural language instruction shouldn't require adapters, intent routers, and prompt templates written by hand.

ExoModel bridges natural language intent and object behavior — so your objects can understand instructions, call their own methods, and respond with guaranteed structure. No glue code. No rewrites.

[Get Started](getting-started.md){ .md-button .md-button--primary } [See it in 5 minutes](usage.md){ .md-button }

```bash
pip install "exomodel[google]"
```

---

## The glue code that slows everything down

Every time you connect an LLM to a domain object, you end up writing the same things again: adapters between your objects and the model, an `if intent == X` router you'll regret in a month, prompt templates buried in business logic, and provider-specific boilerplate that locks you in before you've shipped anything useful.

| Traditional approach | With ExoModel |
| :--- | :--- |
| Write adapters and parsers before every feature | Objects understand instructions directly — no translation layer |
| Map intent to methods by hand (`if intent == X`) | Objects discover and call their own methods based on intent |
| Glue code buries your domain logic | Domain code stays domain code |
| Tied to one LLM provider | Swap providers without touching business logic |

---

## Why not just use LangChain directly?

LangChain is great — ExoModel is actually built on top of it. But there's a gap between "I have an LLM" and "I have a working business application", and that's exactly what ExoModel fills.

**LangChain thinks in pipelines. Your business thinks in objects.**
When you model a `Proposal`, a `LeadContact`, or an `AuditReport`, you're thinking in entities — not chains. LangChain makes you wrap your objects around the AI. ExoModel puts the AI inside the objects, where it belongs.

**You still have to write all the glue code.**
With raw LangChain, you manage prompt templates, output parsers, schema validation, RAG retrieval, and tool registration — separately, by hand, for every entity. ExoModel handles all of that by convention. Define the schema, attach the documents, and the object knows what to do.

**The output is still just text.**
LangChain returns strings. Your application needs structured, validated, typed objects that can act — objects you can save to a database, send to an API, render in a UI, or use to trigger further methods. ExoModel's output is always a live, schema-validated object — no parsing, ready to do work.

---

## Three steps. No boilerplate.

### Step 1 — Install

Pick the provider you want. Swap any time — one line:

```bash
pip install "exomodel[google]"      # Gemini (default)
pip install "exomodel[anthropic]"   # Claude
pip install "exomodel[openai]"      # OpenAI / Azure OpenAI
pip install "exomodel[cohere]"      # Cohere
pip install "exomodel[all]"         # all providers
```

Set your keys once in `.env` at the root of your project:

```env
MY_LLM_MODEL=google_genai:gemini-2.5-flash-lite
MY_EMB_MODEL=google_genai:gemini-embedding-001   # optional — auto-detected from provider
GOOGLE_API_KEY=your-key-here
```

**You're configured. No pipeline setup, no boilerplate.**

### Step 2 — Inherit

```python
from exomodel import ExoModel

class Proposal(ExoModel):
    client: str = ""
    budget: float = 0.0
    flagged_for_legal: bool = False

    def flag_for_review(self):
        self.flagged_for_legal = True

    @classmethod
    def get_rag_sources(cls):
        return ["proposal_rules.md"]
```

**Your object now knows your business rules. No wiring required.**

### Step 3 — Instruct

```python
# The object updates fields and calls the right method — from one instruction
p = Proposal.create("Draft a 50k proposal for Tesla")
p.update("apply the 10% safety margin and flag it for legal review")
# → budget updated, flag_for_review() called — no if/else, no manual routing

print(p.to_ui())

# The object checks itself against proposal_rules.md
print(p.run_analysis())
```

**You get a validated, grounded object — not a string you have to parse.**

The `@llm_function` decorator makes any method callable by the agent. The object decides which one to call based on context — no routing code:

```python
from exomodel import ExoModel, llm_function

class LeadContact(ExoModel):
    name: str = ""
    status: str = "new"
    score: int = 0

    @llm_function
    def qualify(self):
        """Qualify this lead based on company size and budget signals."""
        self.status = "qualified"

    @llm_function
    def disqualify(self, reason: str):
        """Mark this lead as disqualified and record why."""
        self.status = f"disqualified: {reason}"

lead = LeadContact.create("Sarah from Acme Corp, mentioned $200k budget")
lead.master_prompt("Evaluate this lead and take the right action")
# → qualify() or disqualify() called autonomously based on context
```

---

## What you gain

<div class="grid cards" markdown>
-   ### 🧠 Natural Language Updates
    Zero parsers. Zero try/except on LLM output. Give your objects instructions in plain English — they update their own fields with guaranteed structure, no manual mapping.
-   ### 📚 Native RAG Grounding
    Your objects consult their own documents at runtime. Attach PDFs, URLs, or text files to the class — the object reasons within your actual business context, not a generic prompt.
-   ### 🤖 ExoAgent Orchestration
    No routing logic to write. ExoAgent reads intent and decides which method or tool to invoke — you get optimized accuracy and cost without an `if/else` chain.
-   ### 🔌 Schema-Validated Output
    Feed raw human input, get back structured output — ready for your APIs, databases, or UI. No parsing step, no shape mismatch, no surprises.
-   ### ⚙️ Agentic Tools with `@llm_function`
    Expose any method as an agent tool with one decorator. ExoAgent discovers and calls it autonomously at runtime — no registration, no wiring.
-   ### 📊 Collection Operations
    Operate on entire collections in one LLM call. Generate, update, and export lists of objects without writing a loop.
</div>

---

## Use Cases

=== "Consultative Apps"
    Build AI advisors that guide users through complex processes — insurance claims, financial planning, legal intake — by populating structured objects in real-time during the consultation.

=== "Agentic Middleware"
    Bridge the gap between human language and rigid backends. Use ExoModel to ensure every LLM output fits your existing API's exact specification before it hits the wire.

=== "Sales & CRM"
    Deploy agents that draft professional proposals, calculate pricing against business rules, and update lead status autonomously — without a human in the loop for routine work.

=== "Auditing & Compliance"
    Create objects that read their own source contracts to populate audit fields and flag inconsistencies. No manual oversight required for the initial pass.

=== "Intelligent Dashboards"
    Transform raw logs or transcripts into lists of structured objects (`ExoModelList`), ready for data visualization — in the time it takes to write a prompt.

---

## What shipping with ExoModel looks like

Your domain code stays domain code. You swap providers in one line. You ship AI features without writing routing logic or prompt templates — the object handles that. Your teammates can read, extend, and debug your models without needing to understand what the LLM is doing under the hood. When requirements change, you update a method — not a pipeline.

---

## Framework Architecture

| Class | Role |
| :--- | :--- |
| **`ExoModel`** | The intelligent object foundation — schema-driven, AI-powered, RAG-aware. Subclass this to define your entities. |
| **`ExoAgent`** | The reasoning engine that routes tool calls, manages LLM context, and processes RAG sources. Used internally by `ExoModel`; also available for direct use. |
| **`ExoModelList[T]`** | Typed collection for bulk generation, updating, and export of `ExoModel` instances in a single LLM call. |
| **`@llm_function`** | Decorator that turns any method into an agentic tool, discoverable and callable by `ExoAgent` at runtime via `master_prompt`. |

---

## Logging

ExoModel uses Python's standard `logging` module. No output is shown by default.

```python
import logging

# Show warnings and errors only (recommended for production):
logging.getLogger("exomodel").setLevel(logging.WARNING)

# Show all internal traces (useful for debugging):
logging.basicConfig(level=logging.DEBUG)
logging.getLogger("exomodel").setLevel(logging.DEBUG)
```

---

## 🤝 Contributing

We welcome contributions\! ExoModel is built by developers for developers.

1.  **Fork** the Project.
2.  Create your **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3.  **Commit** your Changes (`git commit -m 'Add some AmazingFeature'`).
4.  **Push** to the Branch (`git push origin feature/AmazingFeature`).
5.  Open a **Pull Request**.

---

## 📄 License

Distributed under the Apache License 2.0. See `LICENSE` for more information.

---

## 📬 Contact

Have questions or feedback? Reach out at [contact@exomodel.ai](mailto:contact@exomodel.ai).
