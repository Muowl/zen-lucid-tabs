# Lucid Tabs 🌙

**Unloaded tabs that stay visible — dimmed, but never invisible.**

A Zen Browser mod that makes unloaded (sleeping) tabs easy to spot without letting them vanish into dark themes. Instead of relying on heavy transparency, Lucid Tabs dims gently and draws a subtle ring in your workspace's accent color.

## Why

Most "unloaded tab" styles use `grayscale + low opacity`. On dark themes this makes sleeping tabs blend into the background until they're effectively invisible. Lucid Tabs keeps them *asleep but lucid*:

- **Softer dimming** (`opacity: 0.65`) so labels stay readable
- **Accent ring + glow** derived from `--zen-primary-color` via `color-mix()` — follows whatever theme color you use
- **Icon-aware**: in Essentials and collapsed sidebar, the favicon is the tab's whole identity, so tabs dim less, keep partial color, and get a stronger ring
- **Hover wake-up**: tabs regain presence on hover, before you click

## Preferences

Configurable from the Zen Mods settings page:

| Preference | Options |
|---|---|
| Indicator style | Accent ring + glow (default) · Dashed outline · None (dim only) |
| Keep favicon colors | Disables grayscale, dims with opacity only |

## Install

**Manual (userChrome.css):**
1. `about:support` → Open Profile Folder → create `chrome/userChrome.css`
2. Paste the contents of `chrome.css`
3. Enable `toolkit.legacyUserProfileCustomizations.stylesheets` in `about:config` and restart

Preference-based options require installing as a Zen Mod (import/store); with plain `userChrome.css`, edit the CSS variables at the top of the file instead.

## Compatibility

Zen's internal attributes change between versions. The selectors for Essentials (`tab[zen-essential]`) and collapsed sidebar (`:root[zen-sidebar-expanded]`) are marked with `TODO` comments in `chrome.css` — verify them in the Browser Toolbox if those states don't pick up the style.

## License

MIT
