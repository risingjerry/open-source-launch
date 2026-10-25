# Owner defaults (template)

Copy this file to `owner-defaults.md` in the same folder and fill it in. That
file holds your personal launch defaults and is not meant to be published.
Where it conflicts with generic advice in `SKILL.md`, it wins.

## Identity and accounts

- GitHub account that owns public repos: `<your-login>`. If the machine's
  default `gh` account differs, run publishing commands with
  `GH_TOKEN=$(gh auth token -u <your-login>) gh ...` instead of switching the
  global account.
- Commit identity: `<your-login> <ID+your-login@users.noreply.github.com>` as
  repo-local git config. Use the ID-prefixed noreply address so commits stay
  attributed after a username change. Never a real email.
- License and copyright holder: `MIT, <your-login>`.
- Reference repo for your quality bar: `<url>`.

## Repo naming

Search-first: use the words people (and their agents) type, e.g.
`<product>-agent-cli` / `<product>-cli`, never internal jargon. Decide the name
before the first public push; GitHub redirects after a rename, but the first
impression and early links use the original. Keep the installed command name
unchanged.

## Machine-specific things to strip

List what only exists on your machine: secret managers, local client or host
names, workspace paths, real chat/user/group IDs, private notes folders.
Public docs replace them with standard mechanisms (env vars,
`~/.config/<tool>`, platform keychain, a prompt on first run).

## Demo GIF technique

The demo must show the product's flagship capability, not a fallback path.
Use free-licensed footage/data, never personal media.

If `vhs` fails with your ffmpeg, render terminal-styled HTML frames (fake data,
typing effect via successive frames), screenshot them with headless Chrome at
2x, and assemble with ffmpeg `palettegen`/`paletteuse`. Keep it under 3 MB,
store it under `docs/`, and inspect every frame for private data.

## Social preview style

House style: an OpenAI-blog-cover look.

1. Generate a text-free background with any image model:
   "Abstract expressive oil painting filling the whole frame, wide 16:9: thick
   visible brushstrokes, canvas texture, soft painterly blending, diagonal
   sweeping strokes, editorial OpenAI-blog-cover aesthetic. ABSOLUTELY NO text,
   letters, logos, symbols, faces or objects. Keep the center calmer and
   mid-tone so white text stays readable. Palette: <dominant brand color
   family> with accents of coral, pink, turquoise and golden yellow."
   Take the dominant color from the product's brand so your repos read as one
   series.
2. Icon: the product's glyph in white, left of the wordmark. Prefer Simple
   Icons (CC0, `cdn.jsdelivr.net/npm/simple-icons@<ver>/icons/<name>.svg`);
   add a trademark note in the README.
3. `social_preview.py --background bg.png --icon icon.svg --name "Product|Agent CLI"
   --out docs/social-preview.jpg`. Two-line wordmark, no tagline by default.
4. Square-safe: chat apps crop previews to a centered square, so the lockup
   must sit inside the middle ~540px. Check with
   `ffmpeg -i out.jpg -vf crop=640:640 square.jpg`.
5. Ship the kit in the repo: `docs/social-preview/{render.py,background.jpg,
   icon.*,README.md}` with the rebuild command and the background prompt.

## Promotion

- Social posts: draft, never post directly. Sincere and specific, at most one
  emoji, demo attached, link on the last line, only relevant hashtags.
- Awesome lists: check each list's current rules and record the earliest
  eligible date. Example — hesreallyhim/awesome-claude-code: repo at least 14
  days old with commits after that, or 100+ stars; human-only web issue form,
  one project at a time, objective one-line description without emoji.
- After launch: a few genuine follow-up commits over the next two weeks keep
  the repo eligible and visibly alive.
