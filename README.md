# Code Sensei

> *"I cannot teach anybody anything. I can only make them think."* — Socrates

**Code Sensei** is an AI-powered learning tool that acts as your personal coding sensei. It doesn't hand you answers — it pushes you to think, struggle, and grow. Each session is a short, targeted exercise adapted to your current skill level, followed by a debrief and memory update so the next session builds on the last.

![alt text](image.png)

---

## Philosophy

Most AI tools write code *for* you. Code Sensei does the opposite.

The AI assesses your level, creates a focused exercise (typically 5–10 minutes), watches you work, and only gives hints when you're genuinely stuck. Your progress is tracked across sessions so exercises always hit the right difficulty.

---

## How it works

1. **Tell the sensei what you want to practice** — a language, a concept, a pattern
2. **The sensei assesses your level** from the memory files, or asks for a quick self-evaluation
3. **A short exercise is generated** in a dedicated folder under `exercises/`
4. **You solve it** — the sensei gives progressive hints if needed, never the full answer upfront
5. **Debrief** — feedback on what you did well and what to improve, timed against your start
6. **Memory is updated** — your knowledge profile evolves session after session

---

## Structure

```
code-sensei/
├── CLAUDE.md       # AI agent instructions (load this into your agent/IDE)
├── memory/             # Your personal knowledge profile (gitignored)
│   ├── _user.md        # Your background
│   ├── python.md
│   ├── typescript.md
│   └── ...
└── exercises/          # Generated exercises (gitignored)
    └── topic_name/
        └── exercise.ts / exercise.py / ...
```

`memory/` and `exercises/` are gitignored — your progress stays local.

---

## Getting started

### 1. Clone the repo

```bash
git clone https://github.com/vhaguet-jint/code-sensei.git
cd code-sensei
```

### 2. Load the agent instructions

Point your AI agent or IDE at `CLAUDE.md`. The filename uses `_off` intentionally to prevent auto-loading — you control when the sensei is active.

**With Claude Code:**
```bash
claude --append-system-prompt "$(cat CLAUDE.md)"
```
Or add it as a skill/hook in your Claude Code config.

### 3. Disable autocomplete

**This is non-negotiable.** Autocomplete defeats the purpose entirely. Disable it in your IDE before starting.

### 4. Start a session

Just tell the agent what you want to work on:
> *"I want to practice async Python"*
> *"Give me a TypeScript generics exercise"*

---

## Supported technologies

Any technology your AI model knows — memory files are created on demand. Common ones:

- Python
- TypeScript / JavaScript
- React
- SQL
- Terraform
- Go
- ...

---

## Notes

- Tested with Claude models (recommended for memory consistency across sessions)
- Each user maintains their own local `memory/` — this repo is the framework, not the data
- Exercises are meant to be deleted once completed
