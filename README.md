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
- Optional SimpleAdmin ban handoff (API + command fallback).

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
  "ConfigVersion": 7,
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
    "MinDotGain": 0.02,
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
    "RequireRecentShot": true,
    "ShotWindowTicks": 8,
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
  "DatabaseConfig": {
    "Enabled": false,
    "DatabaseType": "MySQL",
    "DatabaseHost": "localhost",
    "DatabasePort": 3306,
    "DatabaseUser": "root",
    "DatabasePassword": "",
    "DatabaseName": "s2ac",
    "DatabaseSSlMode": "preferred",
    "ConnectionString": "",
    "PreferSimpleAdminConnection": false,
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
    "BanDebounceSeconds": 15.0,
    "DefaultBanDurationMinutes": 0
  },
  "SimpleAdminIntegration": {
    "Enabled": false,
    "UseForBans": true,
    "UseCommandFallback": true,
    "CommandName": "css_addban",
    "ReasonPrefix": "[S2AC]"
  }
}
```

`DatabaseConfig` and legacy `Database` keys are both supported.
If `ConnectionString` is empty, S2AC builds it from `DatabaseHost/Port/User/Password/Name/SSlMode`.

## REST API
- `GET /` -> visual dashboard
- `GET /api/health`
- `GET /api/detections?limit=100`
- `GET /api/player/{steamId}/detections?limit=100`
- `GET /api/modules/stats?hours=24`
- `GET /api/suspicious-players?hours=24&minDetections=3&limit=100`
- `GET /api/bans?limit=100`

If `WebPanel.ApiKey` is set, send it as `X-Api-Key`.
The visual dashboard at `/` includes an API key field and stores the key in browser local storage.
