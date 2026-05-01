# Core Usage

## 🏗 Defining ExoModels

Inherit from `ExoModel` to give your data structures "AI powers." 

```python
from exomodel import ExoModel

class Project(ExoModel):
    title: str = ""
    budget: float = 0.0
```

## 🧠 AI Interactions

### Initialization by Prompt
You can instantiate a model directly from a natural language description.

```python
project = Project(prompt="The project is about a new AI-powered platform for a pet shop and will cost 50K")
```

### Self-Analysis with RAG
ExoModels can analyze themselves against specific business rules provided via RAG.

```python
class BusinessDocument(ExoModel):
    content: str
    
    @classmethod
    def get_rag_sources(cls):
        return ["docs/compliance_rules.pdf"]

doc = BusinessDocument(content="Draft content...")
analysis = doc.run_analysis()
```

### Field Updates
Update specific fields using natural language.

```python
project.update_field("budget", "Increase the budget to 70K due to scope expansion.")
```

## ⚙️ Agentic Tools with `@llm_function`

Any method on an `ExoModel` or `ExoModelList` class can be exposed as an agentic tool by applying the `@llm_function` decorator. Once decorated, `ExoAgent` discovers the method automatically and can invoke it as part of its reasoning loop — no manual tool registration needed.

```python
from exomodel import ExoModel, llm_function

class Proposal(ExoModel):
    client: str = ""
    budget: float = 0.0
    approved: bool = False

    @llm_function
    def approve(self) -> str:
        """Approves the proposal if the budget meets the minimum threshold."""
        if self.budget >= 10000:
            self.approved = True
            return "Proposal approved."
        return "Budget too low — proposal rejected."
```

When an `ExoAgent` receives a natural language instruction like *"approve this proposal"*, it identifies `approve` as a callable tool and invokes it directly on the object. The method's docstring is used by the agent to understand when and how to call it — keep it clear and descriptive.

This pattern works the same way on `ExoModelList` methods, making it possible to build agents that operate over entire collections with a single instruction.

---

## 📋 Managing Collections

Use `ExoModelList` to handle multiple entities, enabling bulk generation and UI-ready exports.

```python
from exomodel import ExoModel, ExoModelList

projects = ExoModelList(item_class=Project)
projects.create_list("Generate 5 innovative AI startup ideas.")

# Export to CSV
projects.to_csv("startups.csv")

# UI Representation
print(projects.to_ui())
```
