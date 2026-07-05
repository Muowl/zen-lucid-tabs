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

**As a Zen Mod (recommended — enables the preferences UI):**

```powershell
git clone https://github.com/Muowl/zen-lucid-tabs
cd zen-lucid-tabs
.\install.ps1        # close Zen first; restart it afterwards
```

The script sideloads the mod into your most recently used Zen profile: it copies the files into `chrome/zen-themes/<id>/`, registers the entry in `zen-themes.json`, and rebuilds the compiled `chrome/zen-themes.css` that Zen injects at startup. Use `-ProfilePath` to target a specific profile, `-Uninstall` to remove. Re-run it after editing `chrome.css` to apply changes (or toggle the mod off/on in the Zen Mods settings page).

> Sideloading is required because the Zen Mods **Import mods** button only installs mods published in the official theme store — it looks the mod up by id and silently fails for anything else.

**Manual (userChrome.css):**
1. `about:support` → Open Profile Folder → create `chrome/userChrome.css`
2. Paste the contents of `chrome.css`
3. Enable `toolkit.legacyUserProfileCustomizations.stylesheets` in `about:config` and restart

The preferences UI is only available when installed as a Zen Mod; with plain `userChrome.css`, edit the CSS variables at the top of the file instead.

## Compatibility

Zen's internal attributes change between versions. The selectors for Essentials (`tab[zen-essential]`) and collapsed sidebar (`:root[zen-sidebar-expanded]`) are marked with `TODO` comments in `chrome.css` — verify them in the Browser Toolbox if those states don't pick up the style.

## License

MIT
