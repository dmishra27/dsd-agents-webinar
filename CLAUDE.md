# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Educational webinar series by Data Science Dojo and SambaNova teaching production-ready AI agent development from scratch using LangGraph, LangChain, and the SambaNova Cloud API. Content is delivered as Jupyter notebooks organized by session.

## Development Commands

```bash
# Install dependencies
uv sync

# Configure API keys
cp .env.example .env   # then fill in keys

# Launch JupyterLab
uv run jupyter lab notebooks/

# Run all tests
pytest tests/

# Run a single test file
pytest tests/test_session_2.py -v

# Execute all notebooks and generate report
python run_notebooks.py
```

## Required API Keys (`.env`)

- `SAMBANOVA_API_KEY` — required for all LLM calls (model: `MiniMax-M2.5`)
- `TAVILY_API_KEY` — required for web search tools
- `LANGFUSE_*` / `LANGSMITH_*` — optional observability tracing

## Architecture

### Notebook Sessions
Each session lives in `notebooks/session_N/` and builds on the previous:
- **Session 1** — ReAct agent conceptual introduction
- **Session 2** — LangGraph agents with tools, state management, ReAct loop
- **Session 3** — Memory and context engineering
- **Sessions 4–6** — Placeholders (content forthcoming)

### Key Patterns

**State Management:** Uses `TypedDict` with LangGraph's annotated reducers. State fields use `Annotated[list, reducer_fn]` for merge behavior (see `file_reducer` pattern in session 2 tests).

**Tool Binding:** `model.bind_tools(tools)` — responses are checked via `response.tool_calls` attribute. Tools return `Command` objects to update LangGraph state directly, not just return values.

**ReAct Loop:** Implemented explicitly with a max-iterations guard, tool invocation dispatch, and message accumulation — not using LangGraph's built-in `create_react_agent`.

**Notebook Utilities:** `notebooks/utils.py` provides Rich-based display helpers (`display_messages`, panel formatting) for formatted output inside Jupyter.

### Tests

- `tests/test_session_2.py` — unit tests for calculator tool, file reducer, state types, ReAct loop logic
- `tests/test_observability.py` — validates Langfuse/LangSmith configuration presence in notebooks
