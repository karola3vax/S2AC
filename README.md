# Source2AntiCheat (S2AC)

Modular anti-cheat plugin for Counter-Strike 2 built on CounterStrikeSharp.

## Features
- Active anti-aim detection (desync + spin + jitter patterns from RunCommand).
- Silent aim detection (weapon-aware bullet deviation analysis).
- Aimbot snap detection (shot-window + target-lock contextual flick analysis).
- Aimbot tracking detection (lock consistency, low-delta tracking, optional LOS checks).
- RapidFire and DoubleTap detection using weapon cycle-time heuristics.
- Bunnyhop detection (speed clamp + perfect-jump streak analysis).
- Anti-wallhack networking filter using RayTrace capability + CheckTransmit.
- MySQL persistence (`detections`, `bans`).
- Embedded REST backend for web panel consumption.
- CSV telemetry logging for ML training (`data/aimbot_tracking_samples.csv`).

## Requirements
- CounterStrikeSharp
- RayTraceImpl plugin/capability (`raytrace:craytraceinterface`)
- .NET 8 runtime

## Build
```powershell
dotnet build Source2AntiCheat.csproj
```

## Config Path
`addons/counterstrikesharp/configs/plugins/Source2AntiCheat/Source2AntiCheat.json`

## Config (example)
```json
{
  "ConfigVersion": 5,
  "SilentAim": {
    "Enabled": true,
    "MaxDeviation": 15.0,
    "SuspicionThreshold": 3,
    "ShotDataExpiryMs": 250,
    "ViolationDecay": 1,
    "BanPlayer": false
  },
  "AimbotSnap": {
    "Enabled": true,
    "AngleThreshold": 30.0,
    "MinPitchDelta": 4.0,
    "SuspicionThreshold": 5,
    "RequireRecentShot": true,
    "ShotWindowTicks": 4,
    "TargetDotThreshold": 0.9965,
    "MaxTargetDistance": 6000.0,
    "BanPlayer": false
  },
  "AimbotTracking": {
    "Enabled": true,
    "TrackTime": 3.0,
    "SuspicionThreshold": 5,
    "TrackingConeDot": 0.998,
    "TargetLockTicks": 6,
    "MaxCrosshairDelta": 1.0,
    "MinPlayerSpeed": 50.0,
    "MinTargetSpeed": 50.0,
    "RequireVisibility": true,
    "DecayRate": 1.0,
    "BanPlayer": false
  },
  "RapidFire": {
    "Enabled": true,
    "BanPlayer": true,
    "Threshold": 3,
    "TickTolerance": 1,
    "MinExpectedCycleTicks": 2,
    "CooldownSeconds": 2.0
  },
  "DoubleTap": {
    "Enabled": true,
    "MaxTicks": 1,
    "SuspicionThreshold": 3,
    "CooldownSeconds": 2.0,
    "BanPlayer": true
  },
  "Bunnyhop": {
    "Enabled": true,
    "SpeedLimit": 320.0,
    "Threshold": 64,
    "DecreasePlayerSpeed": true,
    "PerfectJumpThreshold": 16,
    "MinJumpSpeed": 240.0,
    "DetectionCooldownSeconds": 3.0
  },
  "ActiveAntiAim": {
    "Enabled": true,
    "AngleThreshold": 58.0,
    "SpinDeltaThreshold": 95.0,
    "JitterDeltaThreshold": 70.0,
    "SuspicionThreshold": 5,
    "CooldownSeconds": 2.0,
    "BanPlayer": true
  },
  "Database": {
    "Enabled": false,
    "ConnectionString": "Server=127.0.0.1;Port=3306;Database=s2ac;User ID=s2ac;Password=change_me;Pooling=true;",
    "AutoCreateTables": true,
    "CommandTimeoutSeconds": 8
  },
  "WebPanel": {
    "Enabled": false,
    "Prefix": "http://127.0.0.1:8085/",
    "ApiKey": "",
    "DefaultPageSize": 100,
    "MaxPageSize": 500
  },
  "MlTrackingCsv": {
    "Enabled": true,
    "CsvFileName": "aimbot_tracking_samples.csv",
    "SampleEveryTicks": 1,
    "SampleOnlyWhenTargeted": false
  },
  "Enforcement": {
    "DetectionCooldownSeconds": 1.0,
    "BanDebounceSeconds": 15.0
  }
}
```

## REST API
- `GET /api/health`
- `GET /api/detections?limit=100`
- `GET /api/player/{steamId}/detections?limit=100`
- `GET /api/suspicious-players?hours=24&minDetections=3&limit=100`
- `GET /api/bans?limit=100`

If `WebPanel.ApiKey` is set, send it as `X-Api-Key`.
