# Agent guide

This repo ships one Agent Skill: `skills/open-source-launch/`. The workflow
lives in `skills/open-source-launch/SKILL.md`; read it before changing
behavior.

## Install the Skill

```bash
npx skills add risingjerry/open-source-launch
```

Or copy `skills/open-source-launch/` to `~/.claude/skills/` or `~/.agents/skills/`.

## Test

```bash
python3 -m unittest discover -s skills/open-source-launch/tests -v
python3 -m compileall -q skills/open-source-launch
python3 skills/open-source-launch/scripts/audit_open_source.py . --strict
```

All three run in CI on Python 3.10, 3.12 and 3.14. Helper scripts use the
Python standard library only; keep it that way.

## Rules

- Keep the Skill self-contained under `skills/open-source-launch/`; repo docs
  and community files stay outside it.
- Never commit `references/owner-defaults.md` (personal defaults). Only the
  `.example.md` template is public.
- The auditor reports locations, never matched secret values.
- Remote writes (visibility flips, posts, releases, history rewrites) follow
  the authorization rules in `SKILL.md`.
- Update `README.md` and `README.zh-CN.md` together, and add a `CHANGELOG.md`
  entry for user-visible changes.
