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
