# game-dev-2d

A [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) and Codex plugin marketplace for 2D game development: design the game on paper with an audited Game Design Document, then generate, process, and pack game-ready art assets straight from your editor.

The repository carries two catalogs:

- `.claude-plugin/marketplace.json` for Claude Code, sourcing each plugin from its repo's `main`
- `.agents/plugins/marketplace.json` for Codex, pinned to a release tag and commit SHA

A plugin appears in the Codex catalog only once its repository ships a valid `.codex-plugin/plugin.json`.

## Installation

### Claude Code

```bash
# One-time: add the marketplace
claude plugin marketplace add MisterVitoPro/game-dev-2d

# Then install whichever plugins you want (see per-plugin sections below)
claude plugin install gdd@game-dev-2d
claude plugin install pixel-sprite-generator@game-dev-2d
```

### Codex

```bash
# One-time: add the marketplace
codex plugin marketplace add MisterVitoPro/game-dev-2d

# Inspect available entries, then install a Codex-enabled plugin
codex plugin list
codex plugin add gdd@game-dev-2d
```

Start a new Codex session after installing so bundled skills are loaded.

---

## Plugins

### gdd  ![version](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FMisterVitoPro%2Fgdd%2Fmain%2F.claude-plugin%2Fplugin.json&query=%24.version&label=version&color=blue)

**Game Doc Forge: turn a game idea into a Game Design Document a team can build from.**

**Claude Code and Codex ready.** Ships both manifests and Codex-valid skills; the pipeline is host-neutral. The Codex catalog pins release `v2.0.0`.

A pillar-first concept interview pins the pitch, design pillars (each with a named cut), fantasy, audience, and exclusions, then forks into a comprehensive video-game interview (fourteen modules plus genre probes) or tabletop interview (sixteen modules covering board, card, dice, party, miniatures, and TTRPG designs) at quick, standard, or comprehensive depth. Every question lands in a ledger as decided, assumed, open, skipped, or superseded; the GDD writer may not state anything the ledger does not support, and an auditor checks. Optional historical, market, and technical web research runs in parallel and targets open ledger items first. The draft is written from a family-specific template with decision log, assumptions, open questions, and glossary appendices, then supplementary documents (stats tables, item lists, lore bible, rules references, sell sheets, playtest plans) generate in parallel and an INDEX.md ties the session together. Every session is resumable from `state.json`.

```bash
# Claude Code
claude plugin install gdd@game-dev-2d
/gdd:create my-game               # run the full pipeline for a new project
/gdd:resume my-game               # continue, answer open questions, extend, or revise
/gdd:status                       # progress report, or list all sessions

# Codex
codex plugin add gdd@game-dev-2d
$gdd:create my-game
$gdd:resume my-game
$gdd:status
```

→ [Plugin docs](https://github.com/MisterVitoPro/gdd#readme)

---

### pixel-sprite-generator  ![version](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FMisterVitoPro%2Fpixel-sprite-generator%2Fmain%2F.claude-plugin%2Fplugin.json&query=%24.version&label=version&color=blue)

**Author a sprite in YAML, get a game-ready pixel-art PNG.**

**Claude Code only.** The plugin does not yet ship a `.codex-plugin/plugin.json`, so it is absent from the Codex catalog.

Drives pluggable, config-driven image backends (OpenAI, Automatic1111, SwarmUI) through a single renderer, then post-processes the result into clean RGBA pixel art: background removal, downscaling, palette quantization, outlining, and recoloring. When no backend is reachable it falls back to a deterministic JSON-grid renderer, so the pipeline never hard-stops. Outputs land as small PNGs in `assets/sprites/`, with optional spritesheet packing (`--pack`) producing TexturePacker/Aseprite-compatible atlases. Everything is driven by a single `pixel-sprite.config.yaml`.

```bash
claude plugin install pixel-sprite-generator@game-dev-2d
/pixel-sprite-generator:init      # scaffold config, dirs, and example sprites
```

→ [Plugin docs](https://github.com/MisterVitoPro/pixel-sprite-generator#readme)

---

## Why a marketplace?

Each plugin is independently versioned and installable. Add the marketplace once; mix and match. The Claude Code catalog references each plugin's repo via a `github` source tracking `main`, so a release is live as soon as it merges; the Codex catalog pins a release tag and commit SHA, so each Codex release is an explicit bump here.

## Troubleshooting

If `/plugin` shows errors after installing:

```bash
claude --debug                      # shows plugin load errors
/plugin                             # opens the plugin manager UI
```

For Codex, inspect the configured marketplace and plugin state:

```bash
codex plugin marketplace list
codex plugin list
```

If a catalog entry is missing, confirm that its pinned source contains `.codex-plugin/plugin.json`. Start a new session after installing or updating a plugin.

`gdd` writes every session under `.gdd/sessions/<project>/` in your working directory; add `.gdd/` to `.gitignore` if you do not want sessions committed. Web research stages need network access and can be skipped at the checkpoint.

`pixel-sprite-generator` runs a Python renderer (`scripts/render_sprites.py`); Python 3 with `Pillow` and `PyYAML` must be installed for sprites to actually render. Cloud/local image backends (OpenAI, Automatic1111, SwarmUI) are optional — without one, the deterministic JSON-grid fallback is used.

## Contributing

1. Build your plugin in its own repo with a `.claude-plugin/plugin.json` (set `name`, `version`, `description`). Add a `.codex-plugin/plugin.json` on the same version if you want it installable from Codex.
2. Register it in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) with a `github` source pointing at that repo.
3. For Codex, tag the release as plain `v<version>` in the plugin repository and register the same URL, tag `ref`, and commit `sha` in [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) with `policy` and `category` metadata. CI checks that the pin matches the tag.
4. Bump `version` in the plugin's manifests on each release — the README version badges read it live from `main`, so they update automatically; bump the Codex pin in the same change.

## License

MIT. See [LICENSE](LICENSE).
