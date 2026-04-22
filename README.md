# BepInEx Profiler

Are you a stats-nerd? That's cool, me too.

Per-plugin frame-time profiler for HoneySelect 2 and other BepInEx games, with a companion app for throttling plugins at runtime.
Made by Sly aka Kumiho -- **v0.4.0 beta**

In active development, so expect updating this thing a lot.

> **Beta.** Works well in testing but there are probably edge cases I haven't hit yet. If something breaks, open an issue and attach your `BepInExProfiler_log.txt`.

---

## What's new in v0.4.0

The companion app has been fully rewritten from scratch in Avalonia (.NET 8). Same idea, much better execution.

- **Dashboard + Live-only views** -- dashboard shows the plugin list alongside a detail panel; live-only goes full-width with cost bars for a quick at-a-glance read
- **Cost bars** -- each row in live-only mode has a colour-coded bar: green for cheap, orange for mid, red for expensive. Log-scaled so even light plugins register visibly
- **Live Sort** -- toggle it on and the list continuously re-sorts so the heaviest plugin is always at the top. Toggle it off to lock the order
- **Sort indicators** -- the active sort column shows a ▼ or ▲ arrow so it's always clear what you're sorted by and in which direction
- **Split tab** -- graph on top, throttle on bottom in the same panel. Throttle a plugin and watch its line react in the graph immediately
- **Pin colours** -- the coloured strip on the left edge of each pinned row now matches that plugin's line on the graph
- **7 accent colours** -- Purple, Cyan, Green, Red, Blue, Pink, Gold, each with proper dark and light variants
- **Auto-throttle** -- set a target FPS and the app will automatically throttle all `[T]` plugins to ÷2 when you drop below it, releasing them when you recover
- Requires .NET 8 now instead of .NET 4.8 (see Requirements)

---

## Screenshots

![All panels](screenshots/overview.png)

*All four panels open at once: Live, Throttle, and Stats*

![Live and Graph](screenshots/graph.png)

*Live list with four plugins pinned to the graph*

![Throttle active](screenshots/throttle.png)

*Throttle panel with frame-skip active on several plugins*

![Stats panel](screenshots/stats.png)

*Stats panel: frame budget, category breakdown, top offenders*

![Live panel](screenshots/live.png)

*Live panel sorted by cost with real plugin data, light mode*

![In-game overlay](screenshots/ingame-overlay.png)

*The in-game overlay (Ctrl+P) works without the companion app*

---

## What it does

Shows you exactly which of your installed plugins are eating frame time. Broken down by Update, LateUpdate, FixedUpdate, Render, Coroutines, and Harmony patch overhead. More importantly, it lets you throttle or disable individual plugins live without touching anything on disk or restarting the game.

The tool is split into two parts:

**The plugin (`BepInExProfiler.dll`)** runs inside the game. On its own you get a basic IMGUI overlay (Ctrl+P to toggle) that lists plugin costs, plus hotkeys for moving it and exporting to CSV. All the configuration lives in the BepInEx F1 menu under `[BepInEx Profiler]`.

**The companion app (`ProfilerApp.exe`)** is where the actual usability is. It connects to the plugin over a local pipe and gives you:
- A live sortable table with per-category cost columns and colour-coded cost bars
- Dashboard and live-only view modes
- A graph panel you can pin specific plugins to (up to 6)
- A throttle panel with per-plugin ÷2/÷4/÷8/÷16 frame-skip controls, full disable, and saveable presets
- A stats panel with frame budget breakdown and top offenders
- Auto-throttle based on a target FPS

If you're just curious which plugin is killing your FPS, the in-game overlay is fine. If you actually want to do something about it, you need the companion app.

---

## About the .exe

I know. An `.exe` from a random modder is a red flag. Here's what it actually is: an Avalonia desktop app that reads data from the game over a named pipe and draws a window. That's it.

It doesn't make any network connections, doesn't write anywhere outside `Documents\BepInExProfiler\`, doesn't touch the registry, doesn't inject into anything. You can verify this yourself with [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon), [VirusTotal](https://www.virustotal.com/gui/home/upload), or [Wireshark](https://www.wireshark.org/), all of which are free tools.

The source isn't public because this is a personal project I'm not ready to open-source. That's a legitimate reason to be skeptical, and I'm not going to tell you not to be. If you'd rather not run it, the in-game overlay (Ctrl+P) works fine on its own. But you just won't have the throttle controls or the detailed panels.

---

## Compatibility

Built and tested on HoneySelect 2, but the plugin itself doesn't use anything game-specific -- it's pure BepInEx 5.x and Unity APIs. Koikatsu, AI Shoujo, and other games running BepInEx 5 on Mono should work fine. Camera component detection may behave slightly differently depending on the game's setup, but nothing should break.

If you try it on another game and run into issues, open an issue and mention which game.

---

## Requirements

- BepInEx 5.x (tested on HS2, should work on KK, KKS, AIS, and similar)
- Windows 10 or later
- **.NET 8 Desktop Runtime** for the companion app -- grab it [from Microsoft](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) if you don't have it (get the ".NET Desktop Runtime 8" x64 installer)

---

## Installation

1. Grab the zip from [Releases](../../releases/latest)
2. Extract into your HS2 folder -- you want it to end up like:
   ```
   HoneySelect2/BepInEx/plugins/Kumiho/
     BepInExProfiler.dll
     ProfilerApp.exe
   ```
3. Launch the game. The companion app should open on its own.

If it doesn't open automatically, check the BepInEx F1 menu -- there's an `Auto-Launch Companion App` toggle under `[BepInEx Profiler]`. You can also just run `ProfilerApp.exe` directly from the folder.

---

## Controls

| | |
|---|---|
| **Ctrl+P** | Toggle in-game overlay |
| **Ctrl+M** | Move/resize overlay |
| **Ctrl+E** | Export snapshot to CSV |
| **F1** | BepInEx config menu (hotkeys, font size, rolling window, etc.) |

---

## Companion app

Switch between **Dashboard** (plugin list + detail panel) and **Live only** (full-width list with cost bars) using the toggle button in the toolbar.

**Plugin list**
- Left-click a row to pin it to the graph (up to 6). The coloured strip on the left edge matches that plugin's graph line.
- Right-click for a quick throttle menu.
- Sortable by any column -- the active column shows ▼/▲. Click again to flip direction.
- **Live Sort** (● button) -- when on, the list re-sorts every frame so the heaviest plugin is always first. Click to toggle.

**Detail tabs** (dashboard mode)

- **Graph** -- scrolling time-series for pinned plugins
- **Stats** -- frame budget bar, breakdown by category, top offenders
- **Throttle** -- per-plugin throttle controls and presets
- **Split** -- graph on top, throttle on bottom. Throttle a plugin and see its line react immediately

**Cost bars** (live-only mode)

Each row has a bar in the Cost column. Colour goes green → orange → red based on how expensive that plugin is relative to the heaviest one. Log-scaled so light plugins still show a visible bar rather than nothing.

**Auto-throttle**

Enable in Settings (⚙) and set a target FPS. When FPS drops below it, all `[T]` plugins are automatically throttled to ÷2. They're released as soon as FPS recovers. Your manual throttle settings are never touched.

Each plugin shows a capability badge:

- `[T]` -- throttleable. Frame-skip works.
- `[D]` -- Harmony/coroutine based. Throttle buttons do nothing, but you can still disable it with ✕.
- `[M]` -- monitor only. No hooks found, cost is tracked but can't be reduced.

---

## Files

| | |
|---|---|
| `Documents\BepInExProfiler\window.cfg` | Layout, theme, settings |
| `Documents\BepInExProfiler\presets.json` | Throttle presets |
| `Documents\BepInExProfiler\profiler_*.csv` | CSV exports |
| `BepInEx\plugins\Kumiho\BepInExProfiler_log.txt` | Log file -- attach this to bug reports |

---

## Known issues

- Plugins that only use Harmony patches or coroutines (`[D]`) can't be throttled, only stopped
- Camera/graphics components (like HS2Graphics) don't appear until a few seconds after entering Studio
- If a plugin misbehaves after being disabled, hit Reset all in the Throttle panel
- Companion app is Windows only

---

## Troubleshooting

**Companion app doesn't open on auto-launch?**
→ You're probably missing the .NET 8 Desktop Runtime. Download the "Windows x64 Desktop Runtime" installer from [Microsoft](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) and run it, then try again.

**Companion app not connecting after it opens?**
→ Wait a few seconds after the game finishes loading — the plugin needs to finish patching before it starts streaming data.
→ Check `BepInExProfiler_log.txt` for errors.

**Auto-launch not triggering at all?**
→ Open the BepInEx config (F1 in-game), find `[BepInEx Profiler]`, and make sure `Auto-Launch Companion App` is enabled.
→ You can also just run `ProfilerApp.exe` manually from `BepInEx\plugins\Kumiho\`.

---

## Disclaimer

This tool modifies how plugins execute at runtime. While it doesn't touch any game files or save data, disabling or heavily throttling a plugin mid-session can cause unexpected behaviour -- crashes, missing functionality, broken scenes. If something goes wrong, use Reset all in the Throttle panel, or just restart the game.

Install and use at your own risk. I'm not responsible for broken saves, corrupted scenes, or any other damage to your game or setup.

---

## Bugs / feedback

Open an [issue](../../issues). Include the log file and what you were doing when it broke.
Or for small stuff, DM me on discord: sly97.

---

*Personal use only -- see [LICENSE](LICENSE). Don't redistribute.*
