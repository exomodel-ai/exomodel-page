# 🌌 ExoModel AI
### The Object-Oriented Framework for Agentic AI

[![PyPI version](https://img.shields.io/pypi/v/exomodel?color=blue)](https://pypi.org/project/exomodel/)
[![License](https://img.shields.io/github/license/exomodel-ai/exomodel)](https://github.com/exomodel-ai/exomodel/blob/main/LICENSE)


**ExoModel** is more than an LLM wrapper—it's a paradigm shift in AI-native development. By extending LangChain and Pydantic, it transforms static data structures into "living" entities that can think, reason, and update themselves using autonomous RAG.



---

## ⚡ Why ExoModel?

While traditional agent frameworks focus on chat, ExoModel focuses on the **Business Entity**. It brings type safety and structure to the chaotic world of LLMs.

| Traditional AI Apps | With ExoModel |
| :--- | :--- |
| Passive data models | Models with "Contextual Consciousness" |
| Fragile manual prompting | Type-safe "Object-to-Prompt" interface |
| Disconnected RAG pipelines | Knowledge injected directly into the class |
| Complex JSON parsing | Guaranteed schema-validated outputs |

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
</div>

---

## 🚀 Quick Start

Get it running in your environment:

```bash
pip install exomodel
```

Define your first **Agentic Entity**:

```python hl_lines="4 11 14"
from exomodel import ExoModel
from pydantic import Field

class Proposal(ExoModel):
    client: str = Field(description="Company legal name")
    value: int = Field(description="Project budget")
    
    @classmethod
    def get_rag_sources(cls):
        # The object now 'knows' your specific business rules
        return ["docs/pricing_guidelines.pdf"]

# Initialize an object from unstructured text
p = Proposal(prompt="Draft a 50k proposal for Tesla")

# The object analyzes itself based on the RAG rules
print(p.run_analysis()) 
```

-----

## 🛠 Framework Architecture

ExoModel is built to be modular, scalable, and provider-agnostic.

  - **ExoModel:** The intelligent data foundation (Pydantic + AI).
  - **ExoAgent:** The reasoning orchestrator (Generalist, Specialist, or Hybrid).
  - **ExoModelList:** Bulk management and agentic collection processing (CSV/UI exports).

-----

## 🎯 Use Cases

  * ### 🤝 Consultative Apps
    Build AI advisors that guide users through complex processes (like insurance claims or financial planning) by populating structured models in real-time during the consultation.
  * ### 🔌 Seamless API Integration
    Bridge the gap between human language and rigid backends. Use ExoModel as an **Agentic Middleware** to ensure every LLM output fits your API's exact specifications.
  * ### 📊 Sales & CRM Automation
    Deploy agents that draft professional proposals, calculate pricing based on business rules, and update lead status autonomously.
  * ### 🕵️♂️ Smart Auditing & Compliance
    Create objects that "read" their own source contracts to populate audit fields and flag inconsistencies without manual oversight.
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
