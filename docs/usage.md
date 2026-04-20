# Core Usage

## 🏗 Defining ExoModels

Inherit from `ExoModel` to give your data structures "AI powers." Use Pydantic's `Field` to provide descriptions that the LLM will use to understand the context of each field.

```python
from exomodel import ExoModel
from pydantic import Field

class Project(ExoModel):
    title: str = Field(description="The name of the project")
    budget: float = Field(description="Estimated cost in USD")
```

## 🧠 AI Interactions

### Initialization by Prompt
You can instantiate a model directly from a natural language description.

```python
project = Project(prompt="A web application for a futuristic pet store using React and Python.")
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
project.update_field("budget", "Increase the budget by 20% due to scope expansion.")
```

## 📋 Managing Collections

Use `ExoModelList` to handle multiple entities, enabling bulk generation and UI-ready exports.

```python
from exomodel_list import ExoModelList

projects = ExoModelList(item_class=Project)
projects.create_list("Generate 5 innovative AI startup ideas.")

# Export to CSV
projects.to_csv("startups.csv")

# UI Representation
print(projects.to_ui())
```
