---
name: open-source-launch
description: Turn a local or private project into a public GitHub repository that strangers and their AI agents can install, use, and find — and that earns stars. Use when the user wants to open-source something, make a repo public, "变成 public repo / 开源 / 发到 GitHub 让更多人用 / 建影响力 / 拉 star", make a tool work on other people's machines, let other agents (Claude Code, Codex, Cursor) pick it up, or improve GitHub SEO/GEO/AEO and launch promotion. Also use for a read-only launch-readiness audit or a refresh of an already-public repo.
---

# Open Source Launch

The goal is impact: a stranger finds the repo, their agent installs it, they
get a real result in minutes, and they star it because it worked. Every step
below serves one of three outcomes:

1. **It runs anywhere** — nothing depends on the maintainer's machine.
2. **Any agent can pick it up** — an AI that has never seen it knows how to
   install it, call it, and stay safe.
3. **People can find it** — search engines, GitHub, and AI answer engines
   surface it for the questions real users ask.

Judge the project against these outcomes, not against a checklist. A small CLI
needs a fraction of what a desktop app needs; skip what does not serve the
three outcomes and say so.

Personal defaults (accounts, commit identity, demo technique, house style,
promotion channels) live in `references/owner-defaults.md`, which stays
private. Read it first when it exists; otherwise copy
[`references/owner-defaults.example.md`](references/owner-defaults.example.md)
to create one.

## Scope and authorization

- "看看 / audit" → read-only report, change nothing.
- "开源 / 变成 public repo / make it public" → prepare everything, push to a
  **private** remote, get CI green, then flip visibility after the privacy gate
  passes. The request itself authorizes the flip unless the user asked to
  review first; report what was flipped.
- Posting on social media, submitting to awesome lists, publishing packages,
  and rewriting Git history each need their own go-ahead. Draft them; don't
  send them.
- Work on a branch or in place on a not-yet-public repo; never clobber
  unrelated dirty changes.

## 1. Privacy sweep — the only hard blocker

A public repo is forever: forks and caches outlive deletion. Before the first
public push:

```bash
python3 <skill-directory>/scripts/audit_open_source.py /path/to/repo --json
python3 <skill-directory>/scripts/audit_open_source.py /path/to/repo --run-gitleaks --strict
```

`<skill-directory>` is the folder containing this file. The script reports
locations, never secret values. Then look beyond what a regex can see:

- `git log --all -p` for personal paths, real names, emails, phone numbers,
  account/chat/user IDs, hostnames, tokens, session files;
- screenshots and GIFs, pixel by pixel; examples and fixtures (replace real
  data with obviously fake data);
- agent instruction files (`AGENTS.md`, `CLAUDE.md`, bundled skills) — they
  often encode the maintainer's private setup (secret managers, client names,
  local paths). Generalize them;
- proprietary binaries, fonts, extracted third-party assets, copied code
  without a compatible license.

If history is dirty, stop and ask: the choices are a history rewrite or a fresh
single-commit public repo. Never silently rewrite.

## 2. Portability — prove it off this machine

Hunt hidden dependencies: absolute paths, `$HOME` state, globally installed
tools, undeclared runtimes or native libraries, private package indexes,
secret managers only the maintainer has. Replace them with documented,
standard mechanisms (env vars, `~/.config/<tool>`, a `--flag`).

Then prove it the way a stranger would: fresh `git clone` into a temp dir,
temporary `HOME`, run the documented install, run the first command, see the
documented result. If a platform is genuinely required (macOS-only app,
paid desktop software), say it in the first screen of the README instead of
hiding it. Details: [`references/portability-contract.md`](references/portability-contract.md).

## 3. Agent-ready — the part most repos miss

More and more users will never read the README themselves; they'll say "install
this and use it" to their agent. Ship the agent's manual inside the repo:

- `AGENTS.md` at the root — how to install, run tests, call the tool, and
  what never to do. Read by Codex, Cursor, and most agents. Add a one-line
  `CLAUDE.md` containing `@AGENTS.md` so Claude Code loads the same file.
- `skills/<name>/SKILL.md` — a portable Agent Skill so the tool is used
  correctly *outside* the repo too, installable with `npx skills add
  OWNER/REPO` or by copying to `~/.claude/skills/` / `~/.agents/skills/`.
- A machine-friendly surface: stable JSON output (or a documented schema),
  meaningful exit codes, complete `--help`, preview/dry-run by default for
  anything destructive or outward-facing.
- A README section "Use with Claude Code / Codex / other agents" with the
  skill install command and two or three example prompts.

How to write these well: [`references/agent-ready.md`](references/agent-ready.md).

## 4. README — the conversion page

If a README-writing Skill (e.g. `write-github-readme`) is installed, use it
for this pass. Either way, the README must make a
stranger go from "what is this?" to "it worked" in one screen: one-line
outcome, a real demo GIF or output, one install command, first result, then
capabilities, limits, FAQ, and a star ask at the end. English `README.md` as
canonical with synchronized `README.zh-CN.md` when the audience includes
Chinese users.

## 5. Be findable — SEO, GEO, AEO

Same facts, three consumers: Google, GitHub search, and AI answer engines.
Write keyword-rich description and focused topics, phrase FAQ headings as the
exact questions people type or ask an AI, name the category and the
problem in plain words, and publish a social preview image — the card shown
when the link is shared on X, WeChat, Slack, or Discord. House style is the
painterly "OpenAI blog cover": a full-bleed abstract oil painting with a
white icon + two-line wordmark lockup kept inside the central square (chat
apps crop previews to a square) — no terminal frames, chips, or screenshots.
Commit the generator kit under `docs/social-preview/` so it's reproducible. Generate a text-free background (prompt in
[`references/owner-defaults.example.md`](references/owner-defaults.example.md)), then lay the
text on with `python3 <skill-directory>/scripts/social_preview.py --help`
(1280×640 JPEG, under GitHub's 1 MB limit). GitHub has no API for it: upload
in repo Settings → Social preview (browser automation works; the upload
saves immediately), then look at the Settings preview to confirm. Playbook:
[`references/discovery-contract.md`](references/discovery-contract.md).

## 6. Ship gate

Before flipping to public, confirm and show the user a short table:

| Gate | Evidence |
| --- | --- |
| Privacy | audit clean, history reviewed, media reviewed |
| Runs anywhere | clean-clone install + first command output |
| Agent-ready | AGENTS.md, bundled skill, JSON/exit codes, README agent section |
| Quality | tests pass locally and in CI on the pushed commit |
| Trust | LICENSE, stated limits, how credentials/data are handled |
| Findable | description, topics, social preview, FAQ |

Only privacy and "it actually works" are true blockers. Everything else can
ship as a known gap — name it.

After the flip, read back the live state: visibility, description, topics,
rendered README (images load, links resolve), CI green on the public commit.

## 7. Launch and keep it alive

Publication is availability, not adoption. Prepare (don't post) the launch
kit: a short post per channel with one proof item and the link, plus a list of
relevant awesome lists and directories with their exact submission rules and
earliest eligible date. Many lists require repo age or star thresholds and
human-only submission — record the date and remind the user rather than
gaming the rule.

Keep shipping small real improvements after launch: recent commits, answered
issues, and tagged releases are themselves discovery and trust signals.
Optional metrics chart: [`references/repository-pulse.md`](references/repository-pulse.md).
Channel playbook: [`references/adoption-contract.md`](references/adoption-contract.md).

## Report back

Plainly: repo URL and visibility, what was cleaned, what was verified (with
the actual command and result), what the agent kit contains, CI status, the
launch drafts and their file paths, and the remaining gaps. Never call it
"launch-ready" from a static scan alone.
