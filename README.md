# game-dev-2d

A [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) plugin marketplace for 2D game development: tools that generate, process, and pack game-ready art assets straight from your editor.

```bash
# One-time: add the marketplace
claude plugin marketplace add MisterVitoPro/game-dev-2d

# Then install whichever plugins you want (see per-plugin sections below)
claude plugin install pixel-sprite-generator@game-dev-2d
```

---

## Plugins

### pixel-sprite-generator  ![version](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FMisterVitoPro%2Fpixel-sprite-generator%2Fmain%2F.claude-plugin%2Fplugin.json&query=%24.version&label=version&color=blue)

**Author a sprite in YAML, get a game-ready pixel-art PNG.**

Drives pluggable, config-driven image backends (OpenAI, Automatic1111, SwarmUI) through a single renderer, then post-processes the result into clean RGBA pixel art: background removal, downscaling, palette quantization, outlining, and recoloring. When no backend is reachable it falls back to a deterministic JSON-grid renderer, so the pipeline never hard-stops. Outputs land as small PNGs in `assets/sprites/`, with optional spritesheet packing (`--pack`) producing TexturePacker/Aseprite-compatible atlases. Everything is driven by a single `pixel-sprite.config.yaml`.

```bash
claude plugin install pixel-sprite-generator@game-dev-2d
/pixel-sprite-generator:init      # scaffold config, dirs, and example sprites
```

→ [Plugin docs](https://github.com/MisterVitoPro/pixel-sprite-generator#readme)

---

## Why a marketplace?

Each plugin is independently versioned and installable. Add the marketplace once; mix and match. Plugins are referenced from their own repos via `github` sources, so each one ships and versions on its own cadence.

## Troubleshooting

If `/plugin` shows errors after installing:

```bash
claude --debug                      # shows plugin load errors
/plugin                             # opens the plugin manager UI
```

`pixel-sprite-generator` runs a Python renderer (`scripts/render_sprites.py`); Python 3 with `Pillow` and `PyYAML` must be installed for sprites to actually render. Cloud/local image backends (OpenAI, Automatic1111, SwarmUI) are optional — without one, the deterministic JSON-grid fallback is used.

## Contributing

1. Build your plugin in its own repo with a `.claude-plugin/plugin.json` (set `name`, `version`, `description`).
2. Register it in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) with a `github` source pointing at that repo.
3. Bump `version` in the plugin's `plugin.json` on each release — the README version badges read it live, so they update automatically.

## License

MIT. See [LICENSE](LICENSE).
