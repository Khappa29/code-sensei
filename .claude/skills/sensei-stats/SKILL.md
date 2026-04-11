---
name: sensei-stats
description: Update the Code Sensei stats profile. Generates a throwaway script to compute metrics from _sessions.json and memory files, runs it, then rewrites memory/_stats.md. Use when the user runs /sensei-stats or asks to update their stats.
user-invocable: true
---

# sensei-stats

Refresh the Code Sensei `_stats.md` profile.

## Steps

1. **Locate the project root** — find the `memory/_sessions.json` file by looking in the current working directory and its parents. The project root is the directory that contains both `memory/` and `CLAUDE.md`.

2. **Write a throwaway script** to `memory/_compute_stats` that:
   - Reads `memory/_sessions.json`
   - Reads all `memory/*.md` files (excluding `_stats.md`, `_user.md`) and extracts the `Overall rating` line from each
   - Computes per-tech: session count, avg duration, total hints, hints/session, last session date, completion rate
   - Computes global: total sessions, total practice time, current streak (consecutive days with at least one session)
   - Reads all `roadmaps/*.md` files (if the folder exists): for each, extract the title (first `#` heading), the tech (from the `Tech:` field in the header line), the status (`active` or `complete`), and count `[x]` (done) vs `[ ]` (remaining) items
   - Prints a JSON result to stdout

   Use **Node.js** by default (guaranteed available since Claude Code runs on Node) — write the script to `memory/_compute_stats.mjs` using only Node built-ins (`fs`, `path`). Fall back to Python (`memory/_compute_stats.py`) only if Node is not available.

3. **Run the script** with the Bash tool:
   ```bash
   node memory/_compute_stats.mjs
   # or if using Python fallback:
   python memory/_compute_stats.py
   ```

4. **Delete the script** after reading the output:
   ```bash
   rm memory/_compute_stats.mjs
   # or: rm memory/_compute_stats.py
   ```

5. **Rewrite `memory/_stats.md`** using the computed data. Use this layout:

```markdown
# Code Sensei — Stats
_Last updated: YYYY-MM-DD_

## Overview
- **Total sessions:** N
- **Total practice time:** Xh Ym
- **Current streak:** N days

## Skills

| Tech | Rating | Sessions | Avg time | Hints/session | Last session |
|------|--------|----------|----------|---------------|--------------|
| Python | B | 4 | 8.5 min | 1.75 | 2026-04-02 |
| TypeScript | C | 3 | 7.3 min | 1.33 | 2026-03-18 |

## Roadmaps

| Roadmap | Tech | Progress | Status |
|---------|------|----------|--------|
| Python for ML Interviews | python | 3 / 8 | active |

## Session history

| Date | Tech | Topic | Duration | Hints | Done |
|------|------|-------|----------|-------|------|
| 2026-04-02 | python | async | 9 min | 2 | ✓ |
...
```

- Omit the Roadmaps section entirely if the `roadmaps/` folder does not exist or is empty

- Sort the skills table by number of sessions descending
- Sort the session history table by date descending, show all sessions
- For completion: ✓ if completed, ✗ if not
- For streak: count the number of consecutive days (ending today or yesterday) that have at least one session. A gap of more than 1 day resets the streak.

6. Confirm to the user that `_stats.md` has been updated and print the overview section.
