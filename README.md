# ChessSense HUD

<p align="center">
  <img src="assets/Logo.png" alt="ChessSense Logo" width="130" height="130" />
</p>

<p align="center">
  <strong>Hardware-accelerated live chess telemetry, AI Grandmaster coaching, and stealth companion for Chess.com & Lichess.</strong>
</p>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#installation--getting-started">Installation</a> •
  <a href="#keyboard-shortcuts">Shortcuts</a> •
  <a href="#configuration">Configuration</a> •
  <a href="#license">License</a>
</p>

---

## Overview

**ChessSense** is a high-performance desktop telemetry assistant and browser extension engineered for serious chess analysis, training, and real-time Grandmaster tactical assistance. It bridges live web games on **Chess.com** and **Lichess.org** with a native, GPU-accelerated WPF desktop dashboard running local **Stockfish** calculation and **llama.cpp** LLM tactical commentary.

Designed from the ground up with **anti-detection camouflage**, **stream-proofing**, and **ergonomic stealth**, ChessSense delivers deep game insights without cluttering your browser window or drawing suspicious move distributions.

---

## Key Features

### 1. Live Board Telemetry & 2D Replica
* **Zero-Lag FEN Synchronization**: High-frequency mutation observer extension captures board states directly from Chess.com and Lichess DOM.
* **Interactive 2D Board**: Real-time replica featuring high-contrast glyphs, threatened square radar glows, move highlights, and coordinate labeling.
* **Multi-PV Candidate Arrows**: Visualizes top 3 engine recommendations simultaneously, color-coded by tactical ranking:
  * Rank #1: Best Move (Emerald Green)
  * Rank #2: Strong Alternative (Cyan)
  * Rank #3: Dynamic Choice (Amber)
* **Opponent Ponder Predictor**: Anticipates the opponent's most likely response with ghosted indicators before they play.
* **Threat Radar & Audio Alerts**: Highlights hanging pieces, tactical skewers, and pins, with optional subtle audio beeps.

### 2. Anti-Detection Humanizer & Fair-Play Camouflage
* **Move Delay Advisor (Smart Tempo Meter)**: 
  * Calculates believable human move times based on game phase, position complexity, and remaining clock.
  * Displays a real-time countdown delay bar (e.g., *"Wait 3.2s - Safe Human Tempo"*) to prevent robotic move-time clustering.
  * Configurable profiles: *Bullet/Blitz (<150ms)*, *Realistic Blitz (0.8s - 2.5s)*, *Standard Rapid (2.0s - 5.5s)*, and *Classical Deep (4.0s - 9.0s)*.
* **Smart Softening Mode**:
  * Deliberately avoids inhuman micro-tactics when an equivalent, natural Master alternative exists within a configurable centipawn tolerance (e.g., 35 cp).
  * Maintains a believable **92-95% accuracy** profile instead of suspicious 99.8% computer perfection.

### 3. Pre-Game Opponent Scouting (Intel Engine)
* **Zero-Click Live Detection**: Automatically captures your opponent's username as soon as a match starts on Chess.com or Lichess.
* **Psychological Tilt Meter**: Analyzes recent match history and loss streaks to detect tilt (*Tilt Alert: Lost 3+ games recently, prone to impulsive sacrifices* or *On Heater*).
* **3-Point Grandmaster Directives**:
  * *Repertoire Exploit*: Steers games into lines where opponent struggles.
  * *Tactical Trap*: Explains whether to trade queens or create sharp tactical complications.
  * *Clock Pressure*: Identifies time-trouble vulnerabilities and move-burn tendencies.
* **Kryptonite Openings**: Highlights specific variations where the opponent's historical win rate collapses under 35%.
* **Repertoire Breakdown**: Aggregates top played openings as White and Black with win/draw/loss progress bars.
* **On-Demand Player Search**: Dedicated Scout Tab allowing you to look up any username on Chess.com or Lichess at any time.

### 4. Local AI Grandmaster Coach (llama.cpp)
* **Bilingual Tactical Guidance**: Delivers real-time strategic commentary in both **English** and **Arabic** (*توجيه تكتيكي*).
* **Local Offline Privacy**: Powered by local quantized models (e.g., Qwen 3.8B/9B GGUF) via `llama.cpp` with zero cloud dependencies and zero data leaving your machine.
* **Tactical Presets**:
  * *Tactical GM (تحليل وتكتيك)*: Deep tactical explanations and motif breakdowns.
  * *Threats & Defense (كشف التهديدات)*: Focuses on blunders, hanging pieces, and prophylactic safety.
  * *Move Only (نقلة مباشرة فقط)*: High-speed, concise move directives.

### 5. Advantage Momentum & Post-Game Analytics
* **Evaluation Momentum Sparkline**: Real-time advantage curve plotting centipawn swings throughout the match with glowing gold highlights.
* **Syzygy 7-Piece Endgame Tablebase**: Instant mathematical proof of win, draw, or loss in endgames.
* **Post-Game Game Review Panel**:
  * Accuracy percentage calculations for both White and Black.
  * Move classification tallies (Brilliant `!!`, Best `!`, Blunder `??`).
  * One-click **Copy PGN** with inline engine evaluations and direct **Open in Lichess** button.

### 6. Stealth, Stream-Proofing & Safety
* **Screen-Capture Bypass (WDA_EXCLUDEFROMCAPTURE)**: 
  * Window-level hardware flag hides the ChessSense HUD completely from Discord screenshares, OBS Studio, and video recording tools.
* **Solo Board Mode (`Alt+B` / `F2`)**: 
  * Instantly hides all engine lines, evaluations, and AI panels, compacting the window to only the live replica board.
* **Emergency Panic Mode (`Alt+H` / `Ctrl+Shift+X`)**:
  * Immediate killswitch: hides all arrows, evaluations, and floating banners across both the desktop HUD and the browser in 0 ms.
* **Voice Move Announcer (TTS)**:
  * Discreetly speaks top moves aloud using Windows Text-to-Speech for eyes-free gameplay.

### 7. Obsidian Gold Design System
* **Curated Royal Palette**: Custom dark luxury aesthetic inspired by the royal gold chess emblem:
  * Pure Obsidian Charcoal (`#100F0E`), Antique Bronze borders (`#3A3024`), and Royal Gold accents (`#CAB087`).
* **Harmonic Board Tiles**: Warm antique parchment light squares (`#D7CBB8`) and rich mahogany bronze dark squares (`#483C30`).
* **Theme Switching**: Includes *Midnight Dark* and *Clean Light* modes with customizable accent swatches.

---

## Architecture

```text
┌──────────────────────────────────────┐        ┌────────────────────────────────────────┐
│     Google Chrome / Brave / Edge     │        │           ChessSense Desktop           │
│  ┌────────────────────────────────┐  │        │               (WPF .NET)               │
│  │     Chess.com / Lichess.org    │  │        │  ┌──────────────────────────────────┐  │
│  │   [Live Board DOM Observer]    │  │        │  │    Live 2D Board & Telemetry     │  │
│  └───────────────┬────────────────┘  │        │  └──────────────────┬───────────────┘  │
│                  │                   │        │                     │                  │
│  ┌───────────────▼────────────────┐  │  WS    │  ┌──────────────────▼───────────────┐  │
│  │   MV3 Extension Background     ├──────────────►    Local WebSocket Server        │  │
│  │    (Multi-Arrow & Radar SVG)   ◄──────────────┤          (Port 5050)             │  │
│  └────────────────────────────────┘  │ (JSON) │  └──────────┬───────────────┬───────┘  │
└──────────────────────────────────────┘        │             │               │          │
                                                │  ┌──────────▼────────┐ ┌────▼───────┐  │
                                                │  │  Stockfish 17+    │ │ llama.cpp  │  │
                                                │  │  (UCI Engine)     │ │ (Local LLM)│  │
                                                │  └───────────────────┘ └────────────┘  │
                                                └────────────────────────────────────────┘
```

1. **Extension Client**: A lightweight Chrome Manifest V3 extension attaches mutation observers to chessboard DOM elements, serializing active moves and FEN positions.
2. **WebSocket Bridge**: Bidirectional real-time JSON protocol communicating over `ws://127.0.0.1:5050/ws/`.
3. **Engine Core**: Native Stockfish process managed via asynchronous standard I/O streaming UCI protocol evaluations.
4. **AI Coach Core**: Integrated `llama.cpp` streaming engine providing real-time natural language tactical insights.

---

## Installation & Getting Started

### Prerequisites
* Windows 10 or Windows 11 (64-bit)
* [.NET 10.0 SDK](https://dotnet.microsoft.com/download) or higher
* Google Chrome, Brave, Edge, or any Chromium-based browser

### 1. Build & Run the Desktop Application
```bash
# Clone the repository
git clone https://github.com/ZerroDevs/ChessCar.git
cd ChessCar

# Build the WPF project
dotnet build ChessSense.csproj -c Release

# Run ChessSense
dotnet run --project ChessSense.csproj
```

### 2. Install the Browser Extension
1. Open your browser and navigate to `chrome://extensions/`.
2. Enable **Developer mode** (toggle in top-right corner).
3. Click **Load unpacked**.
4. Select the `extension/` folder inside the repository.
5. The **ChessSense** icon will appear in your browser toolbar with a status indicator showing `Connected` when the desktop app is active.

---

## Keyboard Shortcuts

| Shortcut | Function | Description |
| :--- | :--- | :--- |
| `Alt + B` / `F2` | **Toggle Solo Board Mode** | Hides analytics & compacts window to only the replica board. |
| `Alt + H` | **Emergency Panic Mode** | Instantly purges and hides all visual arrows and banners. |
| `Ctrl + Shift + X` | **Panic Mode (Secondary)** | Alternative high-speed panic killswitch. |
| `Ctrl + Shift + S` | **Toggle Screen-Capture Bypass** | Toggles window visibility to screen recorders / OBS. |
| `Alt + R` | **Recheck Board Sync** | Forces a complete rescan and FEN re-evaluation. |

---

## Configuration

Settings are persistently stored in `chess_sense_config.json` and can be adjusted directly from the in-app **Settings & Customization** tab:

```json
{
  "Theme": "obsidian",
  "AccentColor": "#CAB087",
  "EnableMoveDelayAdvisor": true,
  "DelayProfile": "Standard Rapid (2.0s - 5.5s)",
  "EnableSmartSoftening": true,
  "SofteningToleranceCp": 35.0,
  "EnableMultiArrows": true,
  "EnableThreatRadar": true,
  "EnableScreenCaptureBypass": true,
  "SelectedSpeedPreset": "Bullet / Blitz (<150ms)",
  "SelectedEloLevel": "Max / GM (3500+)"
}
```

---

## Educational & Fair-Play Disclaimer

ChessSense is designed as a **training companion, tactical analysis tool, and educational Grandmaster study aid**. 

Using automated engine assistance during live rated games against non-consenting players violates the Fair Play Policies of Chess.com, Lichess.org, and FIDE. The developers assume no liability for misuse, account bans, or fair-play sanctions resulting from using this software outside of casual practice, unrated study games, or bot matches.

---

## License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

Developed with precision by **ZeroNuxs** ([ZerroDevs](https://github.com/ZerroDevs)).
