# BepInEx Profiler

**Per-plugin frame-time profiler and throttle manager for HoneySelect 2**  
by [Kumiho](https://github.com/KumihoArts) — v0.3.0 beta

Identifies exactly which BepInEx plugins are consuming your frame time, with live per-plugin throttle controls and a full companion app for detailed analysis.

---

## Screenshots

*Coming soon*

---

## Features

- **Live profiling** — tracks every plugin's Update, LateUpdate, FixedUpdate, Render (camera), Harmony patch overhead, and Coroutine cost per frame
- **Companion app** — WPF desktop app with four panels: Live list, Graph, Throttle controls, and Stats
- **Per-plugin throttle** — reduce how often a plugin runs (÷2 / ÷4 / ÷8 / ÷16) or disable it entirely, without restarting the game
- **Capability badges** — `[T]` throttleable, `[D]` disable-only, `[M]` monitor-only, so you know at a glance what can be reduced
- **Throttle presets** — save and load named throttle configurations
- **Dark and light theme** — full theme support in the companion app
- **Export to CSV** — snapshot current profiling data to disk
- **Settings panel** — adjust font size, rolling average window, and display filters

---

## Requirements

- HoneySelect 2 with BepInEx 5.x
- Windows 10 or later
- .NET 4.8 (included in Windows 10 1903+, otherwise install from Microsoft)

---

## Installation

1. Download **BepInExProfiler-v0.3.0-beta.zip** from the [Releases](../../releases/latest) page
2. Extract the zip into your HS2 folder — the structure should look like:
   ```
   HoneySelect2/
     BepInEx/
       plugins/
         Kumiho/
           BepInExProfiler.dll
           ProfilerApp.exe
   ```
3. Launch the game — the companion app opens automatically

---

## Usage

The companion app can be set to launch automatically when the game starts. If it doesn't the first time, run `ProfilerApp.exe` from `BepInEx/plugins/Kumiho/` manually.
Then set up automatic launching in the F1 menu.

### In-game hotkeys

| Hotkey | Action |
|--------|--------|
| **F7** | Toggle in-game overlay on/off |
| **F8** | Move overlay (drag mode) |
| **F9** | Export snapshot to CSV |

### Companion app panels

| Panel | Description |
|-------|-------------|
| **⚡ Live** | Full plugin list sorted by cost. Left-click to pin to Graph, right-click for throttle menu. |
| **📈 Graph** | Scrolling time-series for up to 6 pinned plugins. |
| **⚙ Throttle** | Per-plugin throttle buttons. Save/load presets. |
| **📊 Stats** | Frame budget bar, category breakdown, top offenders. |

### Capability badges

Every plugin in the Throttle and Live panels shows a badge:

| Badge | Meaning |
|-------|---------|
| `[T]` | **Throttleable** — frame-skip (÷2/÷4/÷8/÷16) works |
| `[D]` | **Disable only** — Harmony/coroutine based; throttle has no effect, only ✕ stops it |
| `[M]` | **Monitor only** — no hooks found; cost is observed but not reducible |

### Settings

Click the **⚙** button in the top bar to open Settings:
- **Font size** — adjust text size in the Live panel (8–20)
- **Rolling window** — frames used to average each plugin's cost (10–300)
- **Hide inactive** — hide plugins with zero measured cost
- **Hide unnamed** — hide unresolved Harmony entries

---

## Files created by the profiler

| Path | Description |
|------|-------------|
| `Documents\BepInExProfiler\window.cfg` | Window layout, theme, and settings |
| `Documents\BepInExProfiler\presets.json` | Saved throttle presets |
| `Documents\BepInExProfiler\profiler_*.csv` | Exported snapshots |
| `BepInEx\plugins\Kumiho\BepInExProfiler_log.txt` | Plugin log — include this when reporting bugs |

---

## Known limitations

- Plugins that do all their work inside coroutines or Harmony patches show `[D]` and can only be stopped, not throttled
- Camera/graphics mod components appear in the profiler only after Studio fully loads (~3 seconds after entering Studio)
- The companion app is Windows-only (WPF / .NET 4.8)

---

## Reporting bugs

Open an [Issue](../../issues) and attach `BepInExProfiler_log.txt` from `BepInEx/plugins/Kumiho/`.

---

## License

© 2026 Kumiho. See [LICENSE](LICENSE) for details.  
**Personal use only. Do not redistribute.**
