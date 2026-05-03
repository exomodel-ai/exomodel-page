# Getting Started

## 📦 Installation

ExoModel is LLM-agnostic. Install only the provider package you need:

```bash
pip install exomodel[google]      # Gemini (default)
pip install exomodel[anthropic]   # Claude
pip install exomodel[openai]      # OpenAI / Azure OpenAI
pip install exomodel[cohere]      # Cohere
pip install exomodel[all]         # all providers
```

## ⚙️ Configuration

Then set your model and API key in a `.env` file at the root of your project:

```env
MY_LLM_MODEL=google_genai:gemini-2.5-flash-lite
MY_EMB_MODEL=google_genai:gemini-embedding-001   # optional — auto-detected from provider
GOOGLE_API_KEY=your-key-here
```

## 🚀 First Steps

To verify your installation, try creating a simple model:

```python
from exomodel import ExoModel

class Agent(ExoModel):
    name: str = ""
    specialty: str = ""

# Create and populate from a natural-language prompt
agent = Agent.create("Create a specialized AI agent for coding assistance.")
print(agent.to_ui())
```

If the model populates with a name and specialty, your environment is set up correctly.

## 🔇 Logging

ExoModel uses Python's standard `logging` module. By default all output is silent. To enable verbose output during development:

```python
import logging
logging.getLogger("exomodel").setLevel(logging.DEBUG)
```

For production, leave it at the default (`WARNING`) or configure it through your app's logging setup.

---

**Next:** Learn how to attach documents, run self-analysis, and manage collections in [Core Usage](usage.md).
