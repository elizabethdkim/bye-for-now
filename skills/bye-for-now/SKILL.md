---
name: bye-for-now
description: Incrementally save session logs to your notes vault. Use when the user invokes /bye-for-now, says "save this conversation", "log this session", "bye for now", or at the end of a conversation when asked to save. Safe to run multiple times per session — only captures what's new.
---

# bye-for-now skill

> **Setup:** Replace every `<YOUR_VAULT_PATH>` below with the absolute path to the
> folder where you want session logs saved (e.g. your Obsidian vault or any notes
> directory). Logs are written to `<YOUR_VAULT_PATH>/Sessions/`.

## Trigger

User invokes `/bye-for-now`, says "bye for now", or asks to save/log the conversation.

## What This Skill Does

Writes an **incremental** session log to your notes vault at `<YOUR_VAULT_PATH>/`. Safe to run multiple times during a session — each invocation only adds new content since the last save. This protects against losing context if the instance dies unexpectedly.

## Instructions

When invoked, do the following:

### Step 1: Check for an existing session log

Look in `<YOUR_VAULT_PATH>/Sessions/` for a file matching today's date and this session's topic (e.g., `2026-03-22-*`). Read the most recent one.

- **If a matching file exists from this session**: Read it to determine what has already been captured. Note the last prompt number or heading that was logged. You will only append new content.
- **If no matching file exists**: This is the first save — create a new file and capture everything from the start.

### Step 2: Identify new content

Review the conversation and identify everything that happened **after** the last saved prompt/exchange. Extract:
- New user prompts (verbatim — the user's exact wording, word for word, not paraphrased)
- Your responses (verbatim — the full response text exactly as written, not a summary)
- New decisions made and their reasoning
- New code changes: files modified, what changed, and why
- Errors encountered and how they were resolved
- Open questions or unfinished work
- Any new context that would help resume this work

### Step 3: Write or update the file

**If creating a new file:**
- Generate the filename using format `YYYY-MM-DD-HHmm-<short-topic>.md`
- Write the full template (see below)
- Save to `<YOUR_VAULT_PATH>/Sessions/` (create directory if needed)

**If updating an existing file:**
- Append new conversation log entries after the last existing entry, continuing the prompt numbering
- Update the **Summary** section to reflect all work done so far (not just the new portion)
- Update the **Key Decisions** section — add any new decisions
- Update the **Code Changes** table — add any new changes
- **Replace** the **Context for Next Session** section entirely with current state
- **Replace** the **Open Items** section entirely with current state
- Update the `time` field in frontmatter to the current time (reflects last save)
- Add an `## Update Log` entry at the bottom noting when this incremental save happened

### Step 4: Confirm

After writing, confirm:
- The file path
- Whether this was a new file or an incremental update
- How many new exchanges were captured
- A one-line summary of what's new since last save

## File Template

```markdown
---
date: {{YYYY-MM-DD}}
time: {{HH:mm}}
last_saved: {{HH:mm}}
save_count: {{number of times saved this session}}
tags:
  - claude-session
  - {{topic-tag}}
project: {{project directory name}}
---

# Session: {{Short Description}}

## Summary
{{2-3 sentence overview of what was accomplished SO FAR — update this each save}}

## Conversation Log

### Prompt 1
> {{User's prompt — verbatim, word for word}}

**Response:** {{Your full response — verbatim, exactly as written, not a summary}}

### Prompt 2
> {{Next prompt — verbatim}}

**Response:** {{Full response — verbatim}}

{{...repeat for all prompts...}}

## Key Decisions
- {{Decision 1}}: {{reasoning}}

## Code Changes
| File | Change | Reason |
|------|--------|--------|
| {{path}} | {{what changed}} | {{why}} |

## Context for Next Session
{{What someone (or Claude) would need to know to seamlessly pick up where this session left off. Include: current state, next steps, blockers, relevant file paths, and any preferences or patterns established.}}

## Open Items
- [ ] {{Unfinished task or question}}

## Update Log
- {{HH:mm}} — Initial save (prompts 1–N)
- {{HH:mm}} — Incremental save (prompts N+1–M): {{one-line summary of what's new}}
```

## Notes

- Record both prompts and responses **verbatim** — exact wording, copied in full, never summarized or paraphrased
- Err on the side of capturing too much context rather than too little
- For code changes, include file paths and line numbers when relevant
- Tag with relevant topic tags for vault search
- If the conversation is very long, group related exchanges under logical headings rather than numbering every single prompt
- The user is encouraged to run this periodically (not just at session end) to protect against instance death — make incremental saves fast and lightweight
- When updating, do NOT re-summarize already-captured prompts — just append new ones
