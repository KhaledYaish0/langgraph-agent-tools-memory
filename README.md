
# LangGraph Agent with Tools & Memory

A LangGraph-powered agent that uses **tool calling** (Google Serper search + Pushover notifications), a **Gradio** chat UI, and **stateful memory** via in‑memory and **SQLite** checkpointing.

> This repository packages the notebook from **Week 4, Day 3** into a clean, runnable project. The original notebook is in `notebooks/langgraph_agent_lab2.ipynb`.

## Features
- **LangGraph** state machine with typed state (`TypedDict`) and `add_messages` reducer
- **Tools**: `search` (Google Serper via `langchain_community`) and `send_push_notification` (Pushover)
- **Gradio** chat interface (`gr.ChatInterface`)
- **Memory / Checkpointing**:
  - In-memory via `MemorySaver`
  - Persistent via `SqliteSaver` (`memory.db`)
- **LangSmith-ready** (optional; enable in `.env` if you use it)

## Architecture (Notebook Overview)
- Builds a `StateGraph[State]` with nodes: `chatbot`, `tools`
- Binds tools to `ChatOpenAI(model="gpt-4o-mini")`
- Conditional edge using `tools_condition` (routes tool calls to `ToolNode`)
- Returns back to `chatbot` after tools for the next decision
- Demonstrates **checkpointing** and **history** APIs; then switches to `SqliteSaver`

## Project Structure
```
langgraph-agent-tools-memory/
├─ notebooks/
│  └─ langgraph_agent_lab2.ipynb
├─ .env.example
├─ .gitignore
├─ LICENSE
├─ README.md
└─ requirements.txt
```

## Quickstart

### 1) Create and activate a virtual environment
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
```

### 2) Install dependencies
```bash
pip install -r requirements.txt
```

### 3) Configure environment variables
Copy `.env.example` to `.env` and set your keys:
- `OPENAI_API_KEY` (required)
- `SERPER_API_KEY` (required for search tool)
- `PUSHOVER_TOKEN` and `PUSHOVER_USER` (optional, only if you want push notifications)
- (Optional) LangSmith vars: `LANGSMITH_API_KEY`, `LANGCHAIN_TRACING_V2`,`LANGCHAIN_PROJECT`

```bash
cp .env.example .env
# then edit .env
```

### 4) Run the notebook
Use Jupyter or VS Code to open:
```
notebooks/langgraph_agent_lab2.ipynb
```
Run all cells. Gradio will launch a local UI with the tool-enabled agent.

> **Note**: The graph visualization in the notebook (`draw_mermaid_png`) may require local graph imaging support. If it errors, skip only that cell; the agent still runs.

## GitHub Upload (CLI)
From inside the project folder:

```bash
git init
git add .
git commit -m "Init: LangGraph agent with tools & memory"
git branch -M main
git remote add origin https://github.com/<YOUR_USERNAME>/langgraph-agent-tools-memory.git
git push -u origin main
```

## Environment Variables
```
# Required
OPENAI_API_KEY=sk-...
SERPER_API_KEY=...

# Optional (enable if you use push notifications)
PUSHOVER_TOKEN=...
PUSHOVER_USER=...

# Optional (LangSmith)
LANGSMITH_API_KEY=...
LANGCHAIN_TRACING_V2=true
LANGCHAIN_PROJECT=langgraph-agent-tools-memory
```

## Notes
- `memory.db` is created at runtime when using `SqliteSaver`.
- Keep your `.env` **out of version control** (already ignored).
- The `GoogleSerperAPIWrapper` expects `SERPER_API_KEY` in the environment.
- If you don't use Pushover, you can keep the tool defined but don’t call it (or remove the env vars).

## License
MIT — see `LICENSE`.
