# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Lucid Tabs is a Zen Browser mod (CSS-only, no build step): it styles unloaded/sleeping tabs so they stay visible on dark themes — dimmed with an accent-colored ring instead of grayscale + heavy transparency. Four files matter: `chrome.css` (all styling), `preferences.json` (settings UI shown in the Zen Mods page), `theme.json` (mod metadata), `install.ps1` (sideload installer).

## Commands

```powershell
.\install.ps1                      # sideload into most recently used Zen profile (close Zen first)
.\install.ps1 -ProfilePath <path>  # target a specific profile
.\install.ps1 -Uninstall           # remove from profile
```

There are no tests or linters. To verify a change: run `install.ps1`, restart Zen, middle-click a pinned/Essential tab to unload it (Zen's default `reset-unload-switch` behavior), and check the styling. A second middle-click on an already-unloaded tab **closes** it (`zen.pinned-tab-manager.wheel-close-if-pending` defaults to true) — don't misread that as a bug.

## How Zen loads mods (why install.ps1 does three things)

Zen does NOT read each mod's `chrome.css` directly. At startup it injects one compiled file, `<profile>/chrome/zen-themes.css`, and only regenerates it when a mod is installed/removed/toggled **through the settings UI**. A working install therefore needs all three:

1. Mod files copied to `<profile>/chrome/zen-themes/<mod-id>/`
2. Entry registered in `<profile>/zen-themes.json` (keyed by mod id)
3. Compiled `<profile>/chrome/zen-themes.css` rebuilt (concatenation of every enabled mod's chrome.css)

`install.ps1` does all three, mirroring `ZenMods.mjs#writeStylesheet` in zen-browser/desktop. After editing `chrome.css`, either re-run the script or toggle the mod off/on in the Zen Mods settings page (which triggers Zen's own rebuild). Editing files in the profile without a rebuild silently does nothing.

The mod's stable UUID is `52a805b5-2450-4f21-8664-d69bb96be507` — it's hardcoded in `install.ps1` and identifies the mod in profiles; don't regenerate it.

The Zen Mods **Import mods** button cannot install this mod: it extracts only the `id` from the JSON and fetches the mod from the official theme store (`zen-browser.github.io/theme-store/themes/<id>/theme.json`), silently 404ing for unpublished mods. Publishing via PR to `zen-browser/theme-store` is the only way to make Import/store install work.

## How preferences reach the CSS

Entries in `preferences.json` become real Firefox prefs, and Zen exposes them to CSS in two different ways depending on type — `chrome.css` relies on both:

- **checkbox** (`mod.lucid-tabs.keep-color`) → queried with `@media (-moz-bool-pref: "mod.lucid-tabs.keep-color")`
- **dropdown** (`mod.lucid-tabs.style`) → the selected value lands as an attribute on `:root`, with dots turned into hyphens: `:root[mod-lucid-tabs-style="dashed"]`

Defaults are applied by Zen only during a mod rebuild, so a new preference won't take effect until the mod is toggled or reinstalled.

## CSS architecture and Zen coupling

`chrome.css` keys everything off two Zen/Firefox attributes: `[pending="true"]` (set by Firefox when a tab is unloaded — Zen's middle-click unload path, `explicitUnloadTabs`, leaves it set) and `tab[zen-essential="true"]` (Essential tabs). Tunables are CSS variables on `:root` at the top of the file; the accent color derives from `--zen-primary-color` via `color-mix()`.

These attributes are internal to Zen and change between versions. The Essentials and collapsed-sidebar selectors are marked with `TODO` comments in `chrome.css` — when a state doesn't pick up styling, verify the attributes in the Browser Toolbox against the current Zen source (`zen-browser/desktop`, `src/zen/tabs/zen-tabs/vertical-tabs.css` and `src/zen/tabs/ZenPinnedTabManager.mjs`) before assuming the CSS is wrong.

Keep `main` on GitHub always working: installed profiles register raw URLs pointing at `main` (`raw.githubusercontent.com/Muowl/zen-lucid-tabs/main/...`), and Zen re-downloads from there if a profile's mod folder goes missing.
