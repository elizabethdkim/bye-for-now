# bye-for-now

A [Claude Code](https://claude.com/claude-code) skill that incrementally saves your session as a structured Markdown log to your notes vault (Obsidian or any folder).

Run it anytime — at the end of a session or partway through — and it captures everything new since the last save: your prompts, key responses, decisions, code changes, and open items. It's safe to run repeatedly; each save only appends what's new, so you never lose context if a session dies unexpectedly.

## What it captures

Each log is one Markdown file with frontmatter and these sections:

- **Summary** — a running 2–3 sentence overview
- **Conversation Log** — numbered prompts with response summaries
- **Key Decisions** — what was decided and why
- **Code Changes** — a table of files touched and the reason
- **Context for Next Session** — what you'd need to pick this up later
- **Open Items** — unfinished tasks and questions
- **Update Log** — a timestamp each time you save

Files are named `YYYY-MM-DD-HHmm-<short-topic>.md` and saved to `<YOUR_VAULT_PATH>/Sessions/`.

## Install

1. Copy the `skills/bye-for-now/` folder into your Claude Code skills directory:

   ```bash
   mkdir -p ~/.claude/skills
   cp -R skills/bye-for-now ~/.claude/skills/
   ```

2. Open `~/.claude/skills/bye-for-now/SKILL.md` and replace every `<YOUR_VAULT_PATH>`
   with the absolute path to the folder where you want logs saved. For example:

   ```
   <YOUR_VAULT_PATH>/Sessions/   →   /Users/you/Documents/MyVault/Sessions/
   ```

3. Start a new Claude Code session. The skill is now available.

## Usage

Invoke it any of these ways:

- Type `/bye-for-now`
- Say "save this conversation" / "log this session" / "bye for now"

Run it periodically during long sessions — it's incremental, so re-running just adds the new exchanges.

## License

MIT — see [LICENSE](LICENSE).
