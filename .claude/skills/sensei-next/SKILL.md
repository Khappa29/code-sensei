---
name: sensei-next
description: Recommend what the user should practice next. Reads memory files and session history to suggest the most valuable next exercise. Optionally takes a roadmap file path to constrain the recommendation to the current roadmap theme. Use when the user runs /sensei-next or asks what to practice next.
user-invocable: true
argument-hint: "[roadmap file] (optional)"
---

# sensei-next

Recommend the next best thing to practice.

## Roadmap mode (when a roadmap file is passed as argument)

1. Read the roadmap file and find the line marked `**← current**` — this is the theme to practice.
2. Read `memory/<tech>.md` to understand current proficiency.
3. Output a short recommendation targeting that specific theme, and end with a prompt the user can paste directly to start the exercise. Do not suggest a different tech or topic — the roadmap drives the choice.

## Free mode (no argument)

### Steps

Follow this steps in this order :

1. First, read `memory/_user.md` to get a sense of who is the user, then read `memory/_stats.md` (if existing) to have a stats summary of the user.

2. Second, read the last 30 entries of `memory/_sessions.json` to get recency and frequency per tech. If the file has more than 30 entries, only consider the most recent 30.

3. Third, read the relevant memory files in `memory/` to know more about the user's proficiency in the fields.

4. Once you have these informations, reason about the best recommendation using these signals:
   - **Important gaps :** What important elements does the user not master that constitutes a big gap in his skills ?
   - **Progress potential :** Based on the user's profile, what would be the next step that could take the user to another level ?
   - **User preferences :** Does the user show a particular preference in the past exercices ?

4. Output a short, direct recommendation — 3 to 5 lines max:
   - Which tech and topic to practice
   - One sentence explaining why (reference a specific weak spot or last session date)
   - End with a prompt the user can paste directly: e.g. _"I want to practice TypeScript mapped types"_
   If you're uncertain and see no strong pattern ask the user if he'd like to learn something in particular.