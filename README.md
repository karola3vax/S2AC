<div align="center">

# 🛡️ CS2 Anti-Wallhack

**Advanced Server-Side Player Data Withholding (Fog of War) for Counter-Strike 2**

[![CounterStrikeSharp](https://img.shields.io/badge/CounterStrikeSharp-API-gray?style=flat-square)](https://github.com/roflmuffin/CounterStrikeSharp)
[![RayTraceAPI](https://img.shields.io/badge/RayTraceAPI-Required-gray?style=flat-square)](https://github.com/daffyyyy/Ray-Trace)
[![License](https://img.shields.io/badge/License-MIT-gray?style=flat-square)](#)

</div>

---

## 🕶️ Overview

**CS2 Anti-Wallhack** is a state-of-the-art CounterStrikeSharp plugin designed to combat ESP and Wallhack cheats at the server level. Unlike traditional anti-cheats that rely on client-side behavioral analysis, this plugin utilizes **RayTracing** to physically block networked player data from reaching the client until the enemy is actually within the player's line of sight or about to peek.

Inspired by Valorant's Fog of War architecture, it guarantees a competitive, pop-in-free environment with high performance.

## ✨ Core Features

* **📐 10-Point AABB Bound Tracing:** Uses the exact 3D limits (`Mins`/`Maxs`) of a player's model to calculate visibility from 10 dynamic points (8 corners, center, and head), ensuring pixel-perfect accuracy.
* **🏃 Velocity-based Prediction (No Pop-in):** Expands the enemy's bounding box into the future based on their movement speed and direction. This optimistically sends data *just before* they peek a corner, eliminating the "pop-in" effect.
* **🔭 Aggressive FOV Culling:** An optional, highly optimized culling method that drops ray-trace calculations for enemies standing behind the player (outside the 180° front plane).
* **👻 Spectator & Ragdoll Support:** Perfect handling of dead viewers and spectator modes, ensuring observers always see the full picture without stale cached data blocking their view.
* **⚙️ Zero-Hallucination API:** Built strictly against the latest CS2 Schemas and CounterStrikeSharp native structures for a 0-warning, robust compilation.

---

## 📦 Installation

This plugin requires two main dependencies to function correctly.

1. Install [CounterStrikeSharp](https://github.com/roflmuffin/CounterStrikeSharp) (v276 or higher).
2. Install the [Ray-Trace API](https://github.com/daffyyyy/Ray-Trace) plugin by `daffyyyy`.
3. Download the latest release of `CS2-AntiWallHack`.
4. Extract the `CS2-AntiWallHack` folder into your `game/csgo/addons/counterstrikesharp/plugins` directory.
5. Restart the server or run `css_plugins load CS2-AntiWallHack`.

*(Note: The plugin is designed to safely wait for the RayTrace capability to load, preventing `KeyNotFoundException` startup crashes.)*

---

## 🛠️ Configuration

The plugin automatically generates a `CS2-AntiWallHack.json` configuration file in the plugin's directory. All settings can be tuned on the fly without restarting the server.

```json
{
  "EnableAntiWallhack": true,
  "RayTracePoints": 10,
  "PredictorDistance": 150.0,
  "UpdateFrequency": 2,
  "IncludeTeammates": false,
  "IncludeBots": true,
  "BotsDoLOS": true,
  "UseFovCulling": false,
  "ShowDebugInfo": false
}
```

### ⚙️ Parameter Details

| Setting | Default | Description |
| :--- | :---: | :--- |
| `UpdateFrequency` | `2` | Number of server ticks to skip between RayTrace calculations. Higher = more CPU saving, less accuracy. |
| `RayTracePoints` | `10` | Number of points to ray-trace on the target's bounding box. Max is 10. |
| `PredictorDistance` | `150.0` | How far into the future (in units) the bounding box is expanded based on the target's velocity. |
| `IncludeTeammates` | `false` | If `true`, the plugin will also try to hide teammates from each other behind walls. |
| `IncludeBots` | `true` | If `false`, bots will always have their data transmitted to everyone (not hidden by the anti-wallhack). |
| `BotsDoLOS` | `true` | If `true`, bots will act like real players and calculate their own line of sight. Great for CPU stress-testing. |
| `UseFovCulling` | `false` | Aggressively hides players behind you without doing RayTrace math to save CPU. May cause pop-in on fast 180° flicks. |

---

<div align="center">
  <sub>Built with precision for the Counter-Strike 2 Competitive Scene.</sub>
</div>
