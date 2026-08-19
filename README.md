# agentic_ai_hands_on


## 🛠️ Tools — uv

**uv** is used for Python project, dependency, and environment management.

```bash
# Project
uv init
uv sync

# Dependencies
uv add <package>
uv add --dev <package>
uv remove <package>
uv lock

# Run
uv run python main.py
uv run <command>

# Python versions
uv python install 3.12
uv python list

# Development
uv run pytest
uv run ruff check .
uv run ruff format .

# Build & publish
uv build
uv publish
```
