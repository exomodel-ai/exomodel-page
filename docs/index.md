# 🌌 ExoModel AI
### The Object-Oriented Framework for Agentic AI

[![GitHub Link](https://img.shields.io/badge/GitHub-Link-blue)](https://github.com/exomodel-ai/exomodel)
[![PyPI version](https://img.shields.io/pypi/v/exomodel?color=blue)](https://pypi.org/project/exomodel/)
[![License](https://img.shields.io/github/license/exomodel-ai/exomodel)](https://github.com/exomodel-ai/exomodel/blob/main/LICENSE)


**ExoModel** brings AI capabilities directly into your data models. Instead of building prompt pipelines around your objects, your objects become the agents — they populate themselves from natural language, consult their own documents via RAG, and validate their state against business rules. Built on top of LangChain and Pydantic.

---

## ⚡ Why ExoModel?

While traditional agent frameworks focus on chat, ExoModel focuses on the **Business Entity**. It brings type safety and structure to the chaotic world of LLMs.

| Traditional AI Apps | With ExoModel |
| :--- | :--- |
| Passive data models | Models that populate and update themselves from natural language |
| Fragile manual prompting | Type-safe field mapping — the schema is the prompt |
| Disconnected RAG pipelines | Documents attached directly to the class, consulted at runtime |
| Complex JSON parsing | Guaranteed schema-validated outputs via Pydantic |

---

## 🔥 Core Power-Ups

<div class="grid cards" markdown>
-   ### 🧠 Smart CRUD
    Create and update class instances using natural language. ExoModel understands intent and maps it to specific schema fields.
-   ### 📚 Native RAG Grounding
    Attach PDFs, URLs, or Docs directly to your class. The object uses this "brain" to validate its own state against business rules.
-   ### 🤖 ExoAgent Orchestration
    A centralized brain that manages tool routing and persona switching to optimize LLM accuracy and cost.
-   ### 🔌 API-First Design
    Transform messy human input into the strict JSON schemas required by your existing legacy APIs and services.
-   ### ⚙️ Agentic Tools with `@llm_function`
    Decorate any method on an `ExoModel` or `ExoModelList` class with `@llm_function` to expose it as an agentic tool. The agent discovers and calls it autonomously — no manual tool registration required.
</div>

---

## 🚀 Quick Start

### 1. Setup your Environment
Install the package. 

```bash
pip install exomodel
```

Configure your API keys in a `.env` file at the root of your project.

```text
# .env
GOOGLE_API_KEY=your_key_here
MY_LLM_MODEL=gemini-2.5-flash
MY_EMBEDDING_MODEL=gemini-embedding-001
```

### 2\. Create a Knowledge Base

Create a file named `proposal_rules.md`. This "grounds" your AI objects in real-world logic.

```markdown
# Proposal Rules
- We only accept projects above $10,000.
- Every proposal must include a 10% safety margin in the pricing.
- We do not work with companies in the tobacco industry.
```

### 3\. Define and Run your Entity

Inherit from `ExoModel` to give your data structures autonomous reasoning powers.

```python hl_lines="3 8 11"
from exomodel import ExoModel

class Proposal(ExoModel):
    client: str = ""
    budget: float = 0.0
    
    @classmethod
    def get_rag_sources(cls):
        # The object now 'knows' your specific business rules
        return ["proposal_rules.md"]

# Initialize and populate the object from raw text
p = Proposal(prompt="Draft a 50k proposal for Tesla")

# print the object
print(p.to_ui())

# The object analyzes itself based on the 'proposal_rules.md'
print(p.run_analysis()) 

```

-----

## 🛠 Framework Architecture

ExoModel is built to be modular, scalable, and provider-agnostic.

  - **ExoModel:** The intelligent data foundation — schema-driven, AI-powered, RAG-aware.
  - **ExoAgent:** The reasoning orchestrator that routes tool calls and manages LLM context.
  - **ExoModelList:** Bulk management and agentic collection processing (CSV/UI exports).
  - **`@llm_function`:** A decorator that turns any method into an agentic tool, discoverable and callable by `ExoAgent` at runtime.

-----

## 🎯 Use Cases

These are examples of how ExoModel maps to real-world problems across different domains.

  * ### 🤝 Consultative Apps
    Build AI advisors that guide users through complex processes (like insurance claims or financial planning) by populating structured models in real-time during the consultation.
  * ### 🔌 Seamless API Integration
    Bridge the gap between human language and rigid backends. Use ExoModel as an **Agentic Middleware** to ensure every LLM output fits your API's exact specifications.
  * ### 📊 Sales & CRM Automation
    Deploy agents that draft professional proposals, calculate pricing based on business rules, and update lead status autonomously.
  * ### 🕵️♂️ Smart Auditing & Compliance
    Create objects that read their own source contracts to populate audit fields and flag inconsistencies without manual oversight.
  * ### 📈 Intelligent Dashboarding
    Instantly transform raw logs or transcripts into lists of structured objects (`ExoModelList`), ready for data visualization.

-----

## 🤝 Contributing

We welcome contributions\! ExoModel is built by developers for developers.

1.  **Fork** the Project.
2.  Create your **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3.  **Commit** your Changes (`git commit -m 'Add some AmazingFeature'`).
4.  **Push** to the Branch (`git push origin feature/AmazingFeature`).
5.  Open a **Pull Request**.

-----

## 📄 License

Distributed under the Apache License 2.0. See `LICENSE` for more information.



