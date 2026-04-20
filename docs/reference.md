# Reference Guide

This guide provides the API reference for the core classes of the **ExoModel AI** framework.

## ExoModel

The `ExoModel` class is the foundation of the framework, combining Pydantic's data validation with LLM-powered capabilities.

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

The `ExoAgent` manages LLM interactions, RAG (Retrieval-Augmented Generation) context, and tool orchestration.

::: exomodel.exoagent.ExoAgent
    options:
      members:
        - add_rag_sources
        - set_external_tools
        - run
        - get_web_markdown

---

## ExoModelList

A specialized container for managing collections of `ExoModel` entities.

::: exomodel.exomodel_list.ExoModelList
    options:
      members:
        - create_list
        - update_list
        - to_csv
        - to_ui
