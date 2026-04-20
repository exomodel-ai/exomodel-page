# Getting Started

## 📦 Installation

ExoModel is currently in **Beta**. You can install it using pip:

```bash
pip install exomodel
```

## ⚙️ Configuration

ExoModel requires connection to an LLM provider and an embedding model. Ensure you have the following environment variables configured:

### LLM API Keys
- `GOOGLE_API_KEY`: If using Google Gemini models.
- `OPENAI_API_KEY`: If using OpenAI models.

### Model Selection
- `MY_LLM_MODEL`: The specific model to use (e.g., `gemini-2.5-flash` or `gpt-4o`).
- `MY_EMBEDDING_MODEL`: The embedding model for RAG (e.g., `gemini-embedding-001` or `text-embedding-3-small`).

### Example `.env` file
```text
GOOGLE_API_KEY=your_api_key_here
MY_LLM_MODEL=gemini-2.5-flash
MY_EMBEDDING_MODEL=gemini-embedding-001
```

## 🚀 First Steps

To verify your installation, try creating a simple model:

```python
from exomodel import ExoModel
from pydantic import Field

class Agent(ExoModel):
    name: str
    specialty: str

# Initialize with a prompt
agent = Agent(prompt="Create a specialized AI agent for coding assistance.")
print(agent.name)
print(agent.specialty)
```
