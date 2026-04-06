---
name: sensei-roadmap-sync
description: Update a roadmap's progress based on evidence in memory files. Advances the current theme marker only if memory shows the user has meaningfully progressed on it. Use after a session completes, or when the user runs /sensei-roadmap-sync.
user-invocable: true
argument-hint: "[roadmap file] (optional, defaults to the only active roadmap or asks if multiple)"
---

# sensei-roadmap-sync

Sync roadmap progress with what the memory files actually show.

## Steps

### 1. Find the active roadmap
- List all files in `roadmap/` matching `*.md`
- If a file was passed as argument, use it
- If exactly one file has `Status: active`, use it
- If multiple active roadmaps exist, list them and ask the user which one

### 2. Read the current theme
- Find the line marked `**← current**` in the roadmap — this is the theme under evaluation
- Note the tech from the roadmap header

### 3. Check for evidence of mastery in memory
- Read `memory/<tech>.md`
- Look for evidence that the current theme is now reasonably covered:
  - Is it referenced in **Strong Points**? (good sign)
  - Is it still in **Areas of improvements** or **Important understanding gaps** with no progress? (not ready)
  - Have recent sessions in `_sessions.json` covered this theme?
- Apply a **conservative standard**: only advance if there is positive evidence of progress. When in doubt, do not advance.

### 4. Decision
**If the theme is mastered:**
- Mark it done: `- [ ] **← current** <theme>` → `- [x] <theme>`
- Mark the next incomplete item as current: `- [ ] <theme>` → `- [ ] **← current** <theme>`
- If crossing into a new phase, note that in the report
- If no more incomplete items remain, set `Status: complete` in the header

**If the theme is not yet mastered:**
- Do not modify the roadmap file
- Tell the user what evidence is missing and suggest running `/sensei-next roadmap/<file>.md` to keep practicing this theme

### 5. Report
- Always explain the decision with one sentence referencing the specific memory evidence used
