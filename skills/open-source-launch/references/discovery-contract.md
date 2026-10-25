# Discovery: SEO, GEO, AEO

Three audiences read the same repo:

- **Search engines (SEO)** index the GitHub page — title, description, README
  headings and text.
- **GitHub search and Explore** rank by name, description, topics, README,
  stars, and recent activity.
- **AI answer engines (GEO/AEO)** — ChatGPT, Claude, Perplexity, Google AI
  Overviews — retrieve pages that answer a question directly and cite them.

You win all three with the same move: state plainly what the project is, for
whom, and what problem it solves, in the words users actually use.

## Start with a query map

Before writing, list 10–20 real queries in every language your users speak:

| Intent | Example queries |
| --- | --- |
| Category | "telegram cli", "headless video editor cli" |
| Task | "create telegram forum topics from command line" |
| Agent use | "let claude code control telegram", "AI agent edit video" |
| Comparison | "telethon vs tdlib cli", "剪映 自动化 工具" |
| Trust | "is it safe", "will my account get banned" |
| Failure | exact error messages users hit |

Then make sure each query has a place in the repo that answers it.

## Repository metadata

- **Name**: what people would type — the slug is weighted heavily by GitHub
  search, Google, and answer engines. Include the product and the category
  word (`telegram-agent-cli` beats `telegram-native`). Avoid clever or internal
  names nobody searches.
- **Description** (~150 chars visible): category + job + platform + the
  agent angle. Example: "Telegram CLI for AI agents (Claude Code, Codex):
  manage groups, channels, topics, members and bots via MTProto — JSON output."
- **Topics**: up to 20 allowed, use ~12–15 high-signal ones: category
  (`cli`, `telegram`), ecosystem (`claude-code`, `codex`, `ai-agents`,
  `agent-skills`, `mcp` if true), language (`python`), platform (`macos`),
  task words (`video-editing`, `automation`).
- **Homepage**: docs site or the README anchor if no site.
- **Social preview**: 1280×640, product name + one-line outcome + visual
  proof; it's what shows on X, Slack, Discord, WeChat link cards.

## README signals

- H1 = product name; the first sentence defines it in plain words with the
  category term ("X is a command-line tool that…"). Answer engines quote this.
- Headings that match queries ("Install", "Use with Claude Code",
  "How it works", "Limitations").
- **FAQ phrased as real questions** — each heading is a query, each answer
  starts with a direct one-sentence answer, then detail. This is the single
  highest-leverage GEO move.
- Concrete examples with real output; numbers with conditions ("38 commands",
  "30s 1080p renders in ~3.5s on M-series").
- Alt text on images; relative links that work on GitHub.
- A short "Alternatives / how it compares" section when users will ask — honest,
  sourced, no trash-talk.
- Bilingual README doubles reach (English canonical, `README.zh-CN.md`).

## Off-GitHub surfaces (where it pays)

- Package registry listing (PyPI/npm/Homebrew) with the same description.
- A blog post / 公众号 article / X thread explaining the problem and solution —
  long-form pages are what answer engines cite most.
- Directory and awesome-list entries (see adoption-contract.md).
- `llms.txt` only if there's a real docs site.

## Don'ts

Keyword stuffing, fake stars, fake testimonials, invented benchmarks,
misleading comparisons, mass-posting identical copy. They cost more trust than
they earn traffic.

## Verify

After publication: search the product name and 2–3 task queries on Google and
GitHub after a few days; ask two or three answer engines a task query and
note whether the repo appears. Record what was observed, with dates — don't
claim ranking from metadata alone.
