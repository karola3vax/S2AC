# Source2AntiCheat (S2AC)

Source2AntiCheat is a comprehensive, open-source Anti-Cheat solution for Counter-Strike 2 servers, built on [CounterStrikeSharp](https://github.com/roflmuffin/CounterStrikeSharp).

> [!WARNING]  
> This plugin is designed to run on both Windows and Linux servers.
> Ensure you have `CounterStrikeSharp` (v200+) installed with `DynamicFunctions` support.

## 🔥 Key Features

### 1. Active Anti-Aim Detection (New)

* **Mechanism**: Compares player's input (`UserCmd`) against networked eye angles.
* **Detection**: Flags "Desync" and "Fake Angles" (>58°) used by Rage/Hvh cheats.
* **Infrastructure**: Uses `DynamicHooks` to intercept `RunCommand`.

### 2. Silent Aim Detection

* **Mechanism**: Analyzes bullet impact vectors vs view angles.
* **Detection**: Catches bullets that deviate significantly from crosshair (Rifles >12°, Snipers >2°).
* **Source**: Ported from `AC-Master`.

### 3. Aimbot Snap (Ragebot)

* **Mechanism**: Tracks eye angle changes per tick.
* **Detection**: Flags instant, inhuman snaps (>30° in 1 tick) that lock onto enemy bones.

### 4. Aimbot Tracking (Legitbot)

* **Mechanism**: Monitors crosshair "stickiness".
* **Detection**: Identifier "Pixel Perfect" tracking on moving targets for extended periods (>3s).

### 5. Double Tap Prevention

* **Mechanism**: Tick-base analysis.
* **Prevention**: Detects and blocks firing two shots within a single tick window.

### 6. Anti-Wallhack (Fog of War)

* **Engine**: Ray-Traced Occlusion Culling.
* **Function**: Prevents ESP/Wallhacks by not networking enemy player data until they are potentially visible.

### 7. Legacy Modules (Refactored)

* **Anti-Bunnyhop**: Detects perfect jump patterns and clamps velocity.
* **Anti-RapidFire**: Limits weapon cycle speeds (OS-Agnostic).

## 📥 Installation

1. Download the latest release.
2. Extract contents to `game/csgo/addons/counterstrikesharp/plugins/Source2AntiCheat`.
3. Restart your server.

## ⚙️ Configuration

Config file is located at: `addons/counterstrikesharp/configs/plugins/Source2AntiCheat/Source2AntiCheat.json`

```json
{
  "ActiveAntiAim": { "Enabled": true },
  "SilentAim": { "Enabled": true },
  "AimbotSnap": { "Enabled": true },
  "DoubleTap": { "Enabled": true },
  "General": {
    "Webhook": "YOUR_WEBHOOK_URL" 
  }
}
```

## Credits

* **AC-Master**: SilentAim & DoubleTap logic.
* **CS2-Essentials**: UserCmd & TeleportFix infrastructure.
* **Fatality.win / PureLiquid**: Exploit analysis and behavioral patterns.
