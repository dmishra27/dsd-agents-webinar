# SESSIONS.md

Progress tracker for the Deep Agents from Scratch webinar series.

---

## Covered

### Session 1 — Build Your First Agent (`notebooks/session_1/0_create_agent.ipynb`)

**Theme:** What separates a chain from an agent.

The session runs a single benchmark task ("research AI agents, create a plan, write a summary report") twice — first through a plain `llm.stream()` call, then through a fully assembled ReAct agent — so the difference is directly observable.

**Key concepts introduced:**

- **ReAct loop** — Reason + Act. The agent's core execution pattern: call model → check for tool calls → execute them → feed results back → repeat. Every framework wraps this same while loop.
- **Agent vs Chain** — A chain is one LLM call from training data. An agent plans, acts, observes, and iterates.
- **Custom State (`DeepAgentState`)** — Extends `AgentState` with `todos: list[Todo]` (task tracking) and `files: dict` (virtual filesystem), giving the agent a persistent workspace across steps.
- **Annotated reducer pattern** — `Annotated[dict, file_reducer]` merges state updates instead of replacing them. Without this, a tool writing a second file silently erases the first.
- **Command pattern** — Tools that need to update state fields directly (not just return a string) return a `Command(update={...})`. Used by `write_todos` and `write_file` to atomically update state and append a `ToolMessage`.
- **System prompt as workflow** — The plan → search → write → iterate sequence comes entirely from the system prompt, not hardcoded logic. First hint at harness engineering.
- **Tools as capabilities** — `web_search` (find information), `write_todos` (track progress), `write_file` (persist output). Each expands what the agent can do without changing the loop.

**Learning progression:** Deliberate failure baseline → state schema → tools (simple to complex) → assembly → same task re-run → state inspection → conceptual consolidation → series map.

**Required setup:** `SAMBANOVA_API_KEY` + `TAVILY_API_KEY` in `.env`. Langfuse and LangSmith tracing are optional.

---

## Not Yet Covered

### Session 2 — Agent Architecture Deep Dive (`notebooks/session_2/1_build_first_agent.ipynb`)

Introduces the vocabulary of runtimes, frameworks, and harnesses. Builds the raw ReAct loop from scratch (no framework), then shows how LangGraph wraps it. Runs the same benchmark task under three harness configurations to demonstrate that harness engineering moves the needle more than model swapping.

Key topics: explicit ReAct loop implementation, `create_react_agent`, the three harness knobs (system prompt / tools & state / middleware), `DeepAgentState` with `file_reducer`, `Command`-returning tools, middleware preview.

### Session 3 — Context Engineering (`notebooks/session_3/1_context_engineering.ipynb`)

Introduces the 5 context types every agent manages. Focuses on compression (offloading large tool outputs to files) and isolation (subagent delegation). Introduces the `deepagents` library as a production abstraction over the patterns built in Session 2.

Key topics: `search_and_offload` pattern, `create_deep_agent()`, `AgentMiddleware` with `before_model`/`after_model` hooks, `SummarizationToolMiddleware`, automatic 20K-token offloading, subagent delegation via `subagents=[...]`.

### Session 4 — Skills, Tools & MCP (date: May 13)

Connecting agents to the outside world via Model Context Protocol (MCP). Building custom tools with structured I/O. Connecting agents to APIs, databases, and external services.

### Session 5 — Sub-agents and Delegation (forthcoming)

Context isolation via sub-agent spawning, MCP integration, advanced tool patterns.

### Session 6 — Middleware and Evaluation (forthcoming)

Loop detection, self-verification, context compression middleware. Measuring agent performance.

---

## Study Session — 2026-04-15

**What we did:**

- Initialised `CLAUDE.md` with commands, required API keys, and architecture notes for future Claude Code sessions.
- Walked through the overall architecture of the agentic system — the three-layer model (runtime / framework / harness), how the ReAct loop works at each layer, the `DeepAgentState` schema, the Command pattern, context engineering patterns, and the supervisor/subagent delegation model across Sessions 1–3.
- Identified the environment variables required to run Session 1: `SAMBANOVA_API_KEY` and `TAVILY_API_KEY` are required; Langfuse and LangSmith keys are optional.
- Summarised what Session 1 teaches end-to-end: the deliberate chain baseline, state schema, tool definitions, agent assembly, and the chain vs agent comparison.
- Identified the key concepts introduced in Session 1: ReAct loop, agent vs chain, custom state, annotated reducers, Command pattern, system prompt as workflow, and tools as capabilities.
- Traced the cell-by-cell learning progression in Session 1: how each cell builds on the previous, and why the ordering (baseline → state → tools → assembly → re-run → inspect → consolidate) is deliberate.
- Created this `SESSIONS.md` file.

**Next steps:**

- Run Session 1 notebook end-to-end to observe the chain vs agent contrast hands-on.
- Work through Session 2 (`notebooks/session_2/1_build_first_agent.ipynb`) — focus on the three harness configurations experiment and the explicit ReAct loop implementation.
- Work through Session 3 (`notebooks/session_3/1_context_engineering.ipynb`) — focus on the `search_and_offload` pattern, `create_deep_agent()`, and the supervisor/subagent delegation model.
- Session 4 is scheduled for May 13 (Skills, Tools & MCP).
