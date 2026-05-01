# Reference Guide

This guide provides the API reference for the core classes of the **ExoModel AI** framework.

## ExoModel

`ExoModel` is the base class for all AI-powered data models. Inherit from it to define your schema — each field becomes a target for LLM-driven population and updates. Key behaviors:

- **`__init__(prompt=...)`** — instantiates the object and populates its fields from a natural language description.
- **`update_object` / `update_field`** — update the whole object or a single field using natural language.
- **`get_rag_sources`** — override this classmethod to attach documents (PDFs, URLs, markdown files) that the object will consult at runtime.
- **`run_analysis`** — triggers a self-assessment of the object's current state against the attached RAG sources.
- **`to_ui` / `to_csv`** — export the object as a formatted string or CSV row.

Any method on an `ExoModel` subclass can be decorated with `@llm_function` to expose it as an agentic tool. `ExoAgent` will discover and call it automatically based on natural language instructions.

::: exomodel.exomodel.ExoModel
    options:
      members:
        - update_object
        - update_field
        - run_analysis
        - run_object_prompt
        - run_filling_instructions
        - to_csv
        - to_ui
        - get_rag_sources
        - add_rag_source

---

## ExoAgent

`ExoAgent` is the underlying LLM engine used by `ExoModel`. It manages the conversation context, loads RAG sources into a vector store, and routes tool calls. You can use it directly when you need lower-level control over LLM interactions — for example, to build custom agent loops, attach external tools, or fetch and parse web content before passing it to a model.

::: exomodel.exoagent.ExoAgent
    options:
      members:
        - add_rag_sources
        - set_external_tools
        - run
        - get_web_markdown

---

## ExoModelList

`ExoModelList` is a typed collection for managing multiple `ExoModel` instances. Use it when you need to generate, update, or export a batch of entities in a single operation — for example, turning a raw transcript into a list of structured records, or exporting pipeline results to CSV for downstream processing.

::: exomodel.exomodel_list.ExoModelList
    options:
      members:
        - create_list
        - update_list
        - to_csv
        - to_ui
