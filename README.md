[README.md](https://github.com/user-attachments/files/26575137/README.md)
# Ananke — Ollama Coding Agent

> *Ananke — goddess of inevitability, necessity, and fate. She will get it done.*

A local **coding agent** that runs in your terminal, is using [Ollama](https://ollama.com). Give it a task in plain English — it reads your project, thinks, writes files, runs commands, and loops until the job is done. All on your own machine. No cloud, no API costs, no data leaving your system.

---

## Demo

```
$ ananke

╔══════════════════════════════════════════╗
║        Ollama Coding Agent  v1.0         ║
╚══════════════════════════════════════════╝
Project: C:\Users\you\Desktop\myproject

Available models:

   1.  deepseek-coder-v2:16b-lite-instruct-q6_K
   2.  glm-4.7-flash:latest
   3.  qwen2.5-coder:14b
   4.  llama3.2:1b

Pick a model (number): 2
✓ Using model: glm-4.7-flash:latest

ℹ Reading project files...
✓ Project loaded (25,858 chars of context)

════════════════════════════════════════════════════════════════════════════════
Type your task. Type exit or quit to leave.
Type reload to re-read project files.
════════════════════════════════════════════════════════════════════════════════

You: write a file server with upload, download and delete using flask

Agent: I'll create a complete Flask file server backend with a frontend UI...
```

---

## Features

- **Model picker** — lists all your installed Ollama models at startup, you choose
- **Full project context** — reads your entire codebase before acting so it understands what already exists
- **Three action types:**
  - `write_file` — creates or overwrites files
  - `run_command` — executes shell commands and feeds output back to the model
  - `read_file` — reads a specific file into context
- **Approval system** — every action requires confirmation before execution:
  - `y` — approve this action
  - `a` — always approve for the rest of the session
  - `n` — skip this action
  - `q` — quit
- **Agentic loop** — after each action results feed back to the model, which continues until the task is complete (up to 20 iterations)
- **Coloured terminal UI** — clean, readable output with ANSI colours
- **`reload` command** — re-read project files mid-session after manual edits
- **Windows installer** — `install.bat` adds `ananke` to your PATH so you can launch it from any folder

---

## Requirements

- Python 3.9+
- [Ollama](https://ollama.com) installed and running
- At least one model pulled

---

## Installation

### Windows (recommended)

1. Download or clone this repo
2. Right-click `install.bat` → **Run as administrator**
3. Open a **new** terminal
4. Navigate to any project folder
5. Type `ananke`

The installer copies the files to `%LOCALAPPDATA%\Ananke\` and permanently adds it to your user PATH.

### Manual (any OS)

```bash
pip install -r requirements.txt
cd your-project
python /path/to/agent.py
```

### Uninstall

Run `uninstall.bat` to remove Ananke and clean up PATH.

---

## Recommended Models

Tested and ranked for coding tasks:

| Model | Quality | Speed | VRAM |
|-------|---------|-------|------|
| `glm-4.7-flash` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ~20GB |
| `deepseek-coder-v2:16b-lite` | ⭐⭐⭐⭐ | ⭐⭐⭐ | ~12GB |
| `qwen2.5-coder:14b` | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ~10GB |
| `qwen2.5-coder:7b` | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ~6GB |

Pull a model:
```bash
ollama pull glm4:latest
ollama pull qwen2.5-coder:14b
```

---

## How It Works

```
You type a task
      ↓
Agent reads your entire project (tree + file contents)
      ↓
Sends everything to Ollama as context
      ↓
Model responds with explanation + action blocks
      ↓
Ananke shows each proposed action and asks for approval
      ↓
On approval → executes action → feeds result back to model
      ↓
Model decides if more actions are needed
      ↓
Loops until task is complete (max 20 iterations)
```

The model communicates actions in structured blocks:

```
```action
ACTION: write_file
PATH: src/server.py
CONTENT:
from flask import Flask
...
```
```

```
```action
ACTION: run_command
COMMAND: pip install flask
```
```

---

## Session Commands

| Command | Description |
|---------|-------------|
| `reload` | Re-read all project files into context |
| `exit` / `quit` | Exit Ananke |

---

## Configuration

Edit the top of `agent.py` to change defaults:

```python
OLLAMA_BASE   = "http://localhost:11434"  # Ollama API URL
MAX_FILE_SIZE = 32_000                    # Max chars per file read into context
IGNORE_DIRS   = {".git", "node_modules", ...}  # Directories to skip
IGNORE_EXTS   = {".pyc", ".exe", ...}          # File extensions to skip
```

---

## Tips

- **Be specific** — *"add JWT auth to the login route in app.py"* works better than *"add auth"*
- **Use `reload`** after manually editing files so Ananke has fresh context
- **Use `a` (always approve)** once you trust the model for a session to speed things up
- **Smaller models for small tasks** — 7b models are fast and great for simple edits
- **Bigger models for architecture** — 14b+ think about the whole project better
- **GLM-4.7** is the recommended model — it thinks like a developer, not just a prompt answerer

---

## License

MIT — do whatever you want with it.

---

*Built with [Ollama](https://ollama.com) · Runs 100% locally · No cloud · No subscriptions*
