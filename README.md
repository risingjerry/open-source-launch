<div align="center">

# Open Source Launch

**An Agent Skill that turns a local or private project into a public GitHub repo strangers can install, their AI agents can use, and search engines can find.**

Privacy sweep, clean-clone install proof, an agent kit (`AGENTS.md` + bundled Skill), a README that converts, SEO/GEO metadata, a painterly social preview, and launch drafts — for Claude Code, Codex, and any `SKILL.md`-compatible agent.

[![Tests](https://github.com/risingjerry/open-source-launch/actions/workflows/test.yml/badge.svg)](https://github.com/risingjerry/open-source-launch/actions/workflows/test.yml)
[![Release](https://img.shields.io/github/v/release/risingjerry/open-source-launch?display_name=tag)](https://github.com/risingjerry/open-source-launch/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-18a558.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/risingjerry/open-source-launch?style=social)](https://github.com/risingjerry/open-source-launch/stargazers)

English · [简体中文](README.zh-CN.md)

![Open Source Launch](docs/social-preview.jpg)

</div>

## Why

Making a repo public is not the same as shipping an open-source project. Most
"public" repos only run on the author's machine, leak a path or a token in
history, and are invisible to the AI agents that more and more people now ask
to "install this for me".

Open Source Launch gives your agent one job with three outcomes:

1. **It runs anywhere** — no hidden dependency on your machine, proven by a
   fresh clone in a temporary `HOME`.
2. **Any agent can pick it up** — the repo ships its own agent manual
   (`AGENTS.md`, `CLAUDE.md`, a bundled `skills/<name>/SKILL.md`, JSON output,
   safe defaults).
3. **People can find it** — keyword-rich description and topics, FAQ phrased as
   the questions people type or ask an AI, and a social preview that looks good
   when the link is shared.

It judges the project against those outcomes, not a fixed checklist: a small
CLI gets a fraction of what a desktop app needs.

## Install

```bash
npx skills add risingjerry/open-source-launch
```

Or copy [`skills/open-source-launch/`](skills/open-source-launch) into
`~/.claude/skills/` (Claude Code) or `~/.agents/skills/` (Codex and others).
Start a new session so the agent discovers it.

## Use it

Just ask your agent:

```text
Audit this repo for an open-source launch. Don't change anything.
```

```text
Make this project a public repo: privacy check, make it run on any machine,
add the agent kit, write the README, then flip it public.
```

```text
Refresh our already-public repo to the launch standard and draft the X post.
```

What happens, in order:

| Step | What the agent does | Hard blocker? |
| --- | --- | --- |
| 1. Privacy sweep | Runs the bundled auditor (+ optional Gitleaks), reviews history, media and agent files | **Yes** |
| 2. Portability | Hunts absolute paths and machine-only state, proves install from a clean clone | **Yes** (it must work) |
| 3. Agent-ready | Adds `AGENTS.md`, `CLAUDE.md`, a bundled Skill, JSON / exit codes, an agent section in the README | No |
| 4. README | One-screen conversion page: outcome, demo, install, first result, FAQ, star ask | No |
| 5. Findable | Description, ~14 topics, GEO-style FAQ, painterly social preview | No |
| 6. Ship gate | Shows you an evidence table, then flips visibility and reads back the live repo | — |
| 7. Launch kit | Drafts posts and an awesome-list plan with each list's real rules | Drafts only |

Posting, list submissions, package publishing and history rewrites each need
your separate go-ahead — the Skill drafts them, it never sends them.

## What's inside

```text
skills/open-source-launch/
├── SKILL.md                         # the workflow
├── agents/openai.yaml               # Codex display metadata
├── scripts/
│   ├── audit_open_source.py         # privacy / package / agent-kit auditor
│   ├── social_preview.py            # 1280×640 painterly social preview renderer
│   └── install_repository_pulse.py  # optional repo metrics chart
├── references/
│   ├── agent-ready.md               # how to write AGENTS.md and a bundled Skill
│   ├── portability-contract.md
│   ├── discovery-contract.md        # SEO / GEO / social preview
│   ├── adoption-contract.md         # launch channels
│   ├── repository-pulse.md
│   └── owner-defaults.example.md    # copy to owner-defaults.md for your house style
└── tests/
```

**Your own defaults.** Copy `references/owner-defaults.example.md` to
`owner-defaults.md` and fill in your GitHub account, commit identity, house
style and promotion rules. The Skill reads it first. Keep it out of any public
copy.

### The auditor

Python standard library only. It reports locations, never secret values:

```bash
python3 skills/open-source-launch/scripts/audit_open_source.py . --json
python3 skills/open-source-launch/scripts/audit_open_source.py . --run-gitleaks --strict
```

It checks package basics, credential-shaped content, tracked env files,
machine-specific references, risky artifacts, external symlinks, runtime
declarations, Git state, and whether `AGENTS.md` / `CLAUDE.md` exist. A clean
scan is the first gate, not "launch-ready".

### The social preview renderer

```bash
python3 skills/open-source-launch/scripts/social_preview.py \
  --background bg.png --icon icon.svg --name "Product|Agent CLI" \
  --out docs/social-preview.jpg
```

Generate a text-free oil-painting background with any image model (prompt in
`owner-defaults.example.md`); the script lays a white icon and wordmark inside
the central square so chat apps that crop to a square still show it. Needs
Google Chrome or Chromium.

## Requirements

- An agent that loads `SKILL.md` Skills (Claude Code, Codex, Cursor and others).
- Python 3.10+ for the helper scripts; `git`.
- Optional: `gh` (GitHub reads/writes), Gitleaks (history scan), Chrome
  (social preview), Node.js (`npx skills add`).
- No telemetry.

## FAQ

### How do I make a GitHub repo public safely?

Run the privacy sweep first: the bundled auditor plus a look through
`git log -p`, screenshots and agent instruction files. If anything private is
in history, the Skill stops and asks whether to rewrite history or publish a
fresh single-commit repo. It never rewrites silently.

### How do I make my repo usable by Claude Code or Codex?

Ship an `AGENTS.md` (install, test, call, never-do), a one-line `CLAUDE.md`
with `@AGENTS.md`, and a bundled `skills/<name>/SKILL.md`. The Skill writes
these for you; [`agent-ready.md`](skills/open-source-launch/references/agent-ready.md)
explains what goes in them.

### How do I get my GitHub project found by search engines and AI answers?

Use the words people search for in the repo name and description, add focused
topics, phrase FAQ headings as real questions, and set a social preview. See
[`discovery-contract.md`](skills/open-source-launch/references/discovery-contract.md).

### Does it publish anything without asking?

Flipping visibility is covered by "make it public" unless you ask to review
first. Social posts, awesome-list submissions, package releases and history
rewrites are drafted only.

### Does it work for apps that only run on macOS?

Yes. It doesn't pretend otherwise: platform requirements go on the first
screen of the README.

## Uninstall

```bash
npx skills remove open-source-launch
```

Or delete the `open-source-launch` folder from your skills directory.

## Development

```bash
python3 -m unittest discover -s skills/open-source-launch/tests -v
python3 -m compileall -q skills/open-source-launch
python3 skills/open-source-launch/scripts/audit_open_source.py . --strict
```

See [CONTRIBUTING.md](CONTRIBUTING.md). Report vulnerabilities through
[private vulnerability reporting](https://github.com/risingjerry/open-source-launch/security/advisories/new),
not a public issue. MIT licensed.

## Repository activity

[![Repository Pulse for risingjerry/open-source-launch](https://raw.githubusercontent.com/risingjerry/open-source-launch/metrics/repository-metrics.svg)](https://github.com/risingjerry/open-source-launch)

---

If it helped you ship, a ⭐ helps others find it.

[![Star History Chart](https://api.star-history.com/svg?repos=risingjerry/open-source-launch&type=Date)](https://star-history.com/#risingjerry/open-source-launch&Date)
