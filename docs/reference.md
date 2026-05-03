# Reference Guide

This guide provides the API reference for the core classes of the **ExoModel AI** framework.

## ExoModel

`ExoModel` is the base class for all AI-powered data models. Inherit from it to define your schema — each field becomes a target for LLM-driven population and updates. Key behaviors:

- **`create(prompt)`** — class method that instantiates and fully populates a new object from a natural-language prompt. This is the canonical way to create instances. *(Passing `prompt=` to `__init__` still works but emits a `DeprecationWarning` and will be removed in the next major version.)*
- **`update_object` / `update_field`** — update the whole object or a single field using natural language.
- **`master_prompt(prompt)`** — single entry point for chat-style interactions; routes the request to the appropriate `@llm_function` tool via an orchestrator agent.
- **`run_analysis` / `run_filling_instructions`** — self-assessment and field-guidance, both grounded in RAG sources when configured.
- **`run_object_prompt`** — free-form reasoning over the instance data without modifying any fields.
- **`run_llm`** — the single choke-point for all LLM traffic; supports structured output, agent modes, and per-call temperature/token overrides.
- **`get_rag_sources`** — override this classmethod to attach documents (PDFs, URLs, markdown files) that the object will consult at runtime.
- **`llm_tools`** — property that returns all `@llm_function` methods on the instance as LangChain `StructuredTool` objects (cached after the first access).
- **`to_ui(format)`** — export the object as a formatted string. `format` can be `"html"` (default, Telegram-style), `"markdown"` (Discord/CLI), or `"plain"` (no markup).
- **`to_csv`** — export the object as a CSV row.

Any method on an `ExoModel` subclass can be decorated with `@llm_function` to expose it as an agentic tool. `ExoAgent` will discover and call it automatically based on natural language instructions. Four built-in tools (`call_update_object`, `call_run_object_prompt`, `call_run_object_analysis`, `call_run_filling_instructions`) are pre-registered on every instance and used by `master_prompt`.

Both decorators are importable from the top-level package:

```python
from exomodel import ExoModel, ExoAgent, ExoModelList, llm_function, llm_action
```

::: exomodel.exomodel.ExoModel
    options:
      members:
        - create
        - update_object
        - update_field
        - master_prompt
        - run_object_prompt
        - run_analysis
        - run_filling_instructions
        - run_llm
        - get_rag_sources
        - add_rag_source
        - llm_tools
        - to_csv
        - to_ui

---

## ExoAgent

`ExoAgent` is the underlying LLM engine used by `ExoModel`. It manages the conversation context, loads RAG sources into a vector store, and routes tool calls. You can use it directly when you need lower-level control over LLM interactions — for example, to build custom agent loops, attach external tools, or fetch and parse web content before passing it to a model.

The provider is selected from the `MY_LLM_MODEL` environment variable using a `"provider:model"` string (e.g. `google_genai:gemini-2.5-flash-lite`). The provider package must be installed separately — see `pip install exomodel[google|anthropic|openai|cohere]`.

Four agent personas are supported via the `mode` parameter on `run()`:

| Mode | Behaviour |
| :--- | :--- |
| `generalist` | Answers from the LLM's trained knowledge only. |
| `specialist` | Answers exclusively from the RAG knowledge base; refuses to guess. |
| `hybrid` | Prefers RAG context but supplements with general knowledge when needed, marking it `[General knowledge]`. |
| `orchestrator` | Routes the request to the best matching tool; used internally by `master_prompt`. |

`specialist` and `hybrid` modes silently downgrade to `generalist` when no RAG sources are configured.

- **`__init__(temperature, max_tokens)`** — initialises the agent with instance-level defaults for sampling temperature and response length.
- **`add_rag_sources`** — enqueues sources for lazy indexing; embedding happens on the next `run()` call.
- **`set_external_tools`** — registers LangChain `StructuredTool` objects (typically from `ExoModel.llm_tools`) and invalidates the cached agent.
- **`all_tools`** — property that returns the combined list of RAG tools and external tools.
- **`run`** — sends a prompt to the agent; supports per-call `temperature` and `max_tokens` overrides, structured output via `response_schema`, and all four modes.
- **`get_usage` / `reset_usage`** — track and reset accumulated token usage across all calls.
- **`get_web_markdown`** — fetches a URL and returns its body as clean Markdown (capped at 16 000 characters), useful for pre-processing web content before passing it to a model.

::: exomodel.exoagent.ExoAgent
    options:
      members:
        - add_rag_sources
        - set_external_tools
        - all_tools
        - run
        - get_usage
        - reset_usage
        - get_web_markdown

---

## ExoModelList

`ExoModelList` is a typed collection for generating, updating, and exporting batches of `ExoModel` instances. Use it when you need to operate on multiple entities in a single LLM call — for example, turning a raw transcript into a list of structured records, or exporting pipeline results to CSV for downstream processing.

It holds an `ExoAgent` via composition rather than inheriting `ExoModel`, so only list-relevant operations are exposed. RAG sources are inherited automatically from the item class via `get_rag_sources()`.

Two instantiation patterns are supported:

```python
# Subclass (recommended — Pydantic-native, model_validate works):
class ShoppingList(ExoModelList[ShoppingItem]):
    pass
sl = ShoppingList()

# Direct instantiation (quick / inline use):
sl = ExoModelList(item_class=ShoppingItem)
```

- **`__init__(item_class, prompt)`** — initialises the list for a concrete `ExoModel` subclass. Passing `prompt` immediately calls `create_list(prompt)` after init.
- **`create_list`** — generates and replaces the entire item list from a natural-language prompt. Returns `self` for method chaining.
- **`update_list`** — updates the list via a natural-language instruction; serialises the current state to CSV for context before regenerating. Note: the LLM regenerates the entire list on each call, which can be expensive for large lists.
- **`add_rag_source`** — appends a RAG source (URL, PDF, or text file) to this instance, merging with any sources inherited from the item class.
- **`run_llm`** — sends a prompt to the underlying `ExoAgent`, lazily creating it on first call.
- **`to_csv`** — serialises all items to a single CSV string with one header row; returns an empty string when the list is empty.
- **`to_ui`** — returns an HTML-formatted string listing all items, suitable for Telegram or CLI display.

::: exomodel.exomodel_list.ExoModelList
    options:
      members:
        - create_list
        - update_list
        - add_rag_source
        - run_llm
        - to_csv
        - to_ui
