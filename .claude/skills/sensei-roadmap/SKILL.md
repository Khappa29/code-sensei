---
name: sensei-roadmap
description: Create a personalized learning roadmap through an interactive conversation. Reads memory files to understand current level, interviews the user about their goals and context, then writes a roadmap file. Use when the user runs /sensei-roadmap or asks to create a learning plan.
user-invocable: true
argument-hint: "[tech] (optional)"
---

# sensei-roadmap

Create a personalized learning roadmap through a short conversation.

## Steps

### 1. Read context
- Read `memory/_user.md` if it exists
- Read `memory/<tech>.md` if a tech was specified and the file exists
- Read the last 20 entries of `memory/_sessions.json` if it exists

### 2. Interview the user
Ask the following questions **one at a time**, waiting for each answer before continuing. Do not dump all questions at once. Adapt or skip questions if the answer is already clear from memory files or a previous answer.

- What technology do you want a roadmap for? (skip if already provided)
- What is your goal? (examples: job interviews, personal project, exam, fun, career switch)
- What is your timeline? (e.g. 1 month, 3 months, no deadline)
- Any specific areas you want to focus on or avoid?

Keep the tone friendly and brief. 2–3 questions max if context is already rich.

### 3. Generate the roadmap
Build a roadmap that is:
- **Realistic** for the user's current level (from memory) and timeline
- **Goal-oriented** — themes directly serve the stated goal
- **Phased** — group themes into 2–4 phases of increasing depth
- **Estimated** — each phase has a rough time estimate (in hours), each theme has none (keep it light)
- **Not too detailed** — themes, not individual exercises. 4–7 items per phase max.

Mark the first incomplete theme with `**← current**`.

Use this exact file format:

```markdown
# Roadmap: <descriptive title>
_Created: YYYY-MM-DD | Tech: <tech> | Status: active_

## Goal
<1–2 sentences on what the user wants to achieve>

## Context
<1–2 sentences on who the user is and why this roadmap was created — inferred from memory + interview>

## Roadmap

### Phase 1 — <name> (est. Xh)
- [ ] **← current** <theme>
- [ ] <theme>
- [ ] <theme>

### Phase 2 — <name> (est. Xh)
- [ ] <theme>
- [ ] <theme>
```

### 4. Save the file
- Create the `roadmap/` folder if it does not exist
- Save as `roadmap/<tech>-<goal-slug>.md` where `<goal-slug>` is a short kebab-case label derived from the goal (e.g. `python-ml-interviews`, `typescript-job-ready`)
- Tell the user the file path and suggest they start with `/sensei-next` or just tell the sensei the first theme
