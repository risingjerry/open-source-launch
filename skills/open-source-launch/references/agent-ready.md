# Agent-ready repositories

Target user: someone who pastes the repo URL into Claude Code, Codex, or
Cursor and says "install this and use it". The agent has never seen the
project, has no access to the maintainer's machine, and will follow whatever
the repo tells it. Make that path short, correct, and safe.

## What to ship

| File | Who reads it | Purpose |
| --- | --- | --- |
| `AGENTS.md` | Codex, Cursor, Gemini CLI, most agents working *inside* the repo | install, test, call, guardrails |
| `CLAUDE.md` | Claude Code | one line: `@AGENTS.md` (single source of truth) |
| `skills/<name>/SKILL.md` | any agent *using* the tool from anywhere | how to drive the tool for real tasks |
| `--help`, JSON output, exit codes | every agent at runtime | self-discovery without docs |
| README "Use with AI agents" section | the human deciding to adopt | skill install + example prompts |

`AGENTS.md` is for working on the repo; the skill is for using the product.
Don't make one a copy of the other — link between them.

## Writing the bundled skill

```markdown
---
name: <tool-name>
description: <What it does + when to use it, with the words a user would
  actually say, incl. other languages if the audience uses them. 1–3 lines.>
---
```

The description decides whether the agent ever loads the skill; write it as
triggers ("Use when the user wants to … / mentions …"), not as marketing.

Body, in order of importance:

1. **First contact** — how to check it's installed and authenticated
   (`tool --version`, `tool auth status`), and the exact install command if not.
2. **Mental model** — 3–5 lines: what the tool talks to, what output looks
   like (envelope/schema), what's safe by default.
3. **Safety rules** — which commands are read-only, which write, the
   preview → `--execute` / `--yes` gates, what never to do automatically,
   how to handle rate limits and unknown outcomes (verify before retrying).
4. **Recipes** — 5–10 real tasks as copyable command sequences with the
   expected key fields of the output.
5. **Errors** — common error codes → what the agent should do next.
6. **Pointers** — link USAGE.md / docs for the full reference instead of
   pasting everything.

Keep it under ~200 lines. Never include the maintainer's private setup:
secret managers, personal account names, real IDs, local paths, internal
client names. Use placeholders (`@your_group`, `~/Videos/input.mp4`).

## Install routes to document

Verify the current convention of the skills CLI before documenting it.

```bash
# any agent supported by the skills CLI
npx skills add OWNER/REPO

# Claude Code, manual
cp -R skills/<name> ~/.claude/skills/

# Codex and other ~/.agents-compatible agents, manual
cp -R skills/<name> ~/.agents/skills/
```

If the project has an installer, an opt-in `--with-skill` flag that copies the
skill (backing up an existing one) is a nice touch. Never install skills
silently.

## Runtime surface

Agents parse output; humans skim it. Prefer:

- one JSON object per invocation on stdout with a schema/version field,
  `ok`, and a stable error `code`; human logs on stderr;
- non-zero exit codes on failure;
- dry-run/preview as the default for writes, sends, deletes, purchases;
- idempotency keys or request IDs for anything that can't be safely repeated;
- `--help` on every subcommand that lists flags and one example.

If the tool is long-lived or tool-call shaped, an MCP server can be a second
entry point — but only add one when you'll maintain it.

## Test it like a stranger's agent

In a clean temp directory with a temporary `HOME`, give a fresh agent session
only the repo URL and a realistic request. Watch whether it installs the
tool, finds the skill, and completes the task without guessing. Every place it
hesitated is a doc fix.
