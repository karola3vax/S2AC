---
title: S2AC
version: "1.1.6"
game: Counter-Strike 2
type: Server-side anti-cheat
runtime: "CounterStrikeSharp + Metamod"
author: karola3vax
---

# S2AC

> **Premium server-side anti-cheat for Counter-Strike 2 community servers.**

S2AC is made for communities that want decisive cheat detection, clean staff visibility, and ban-ready defaults without noisy operator output. It watches real server events, command data, player behavior, cvars, names, chat, and protected server-side signals, then turns that evidence into reports staff can trust during a live match.

![CS2](https://img.shields.io/badge/CS2-Community%20Servers-orange)
![Defaults](https://img.shields.io/badge/Defaults-Ban--Ready-red)
![Evidence](https://img.shields.io/badge/Evidence-Actionable-blue)
![Version](https://img.shields.io/badge/S2AC-1.1.6-black)

---

## Why S2AC 🛡️

| Need | What S2AC Delivers |
| --- | --- |
| ⚡ **Fast enforcement** | Strong command-data and impossible-input detections can punish quickly. |
| 📋 **Clean staff view** | Chat, logs, webhooks, and status output use consistent detector names. |
| 🎯 **Strong-player safety** | Skill-result checks require repeated evidence before punishment. |
| ✅ **Operational confidence** | Reports show who triggered it, what happened, where it happened, and why S2AC acted. |

> [!IMPORTANT]
> S2AC is server-side. It does not scan player PCs. It detects behavior the server can observe: shots, kills, movement, view angles, command data, chat, names, cvars, reputation lists, and protected client-event behavior when available.

## Coverage 🎮

| Area | Protection |
| --- | --- |
| 🎯 **Aim assistance** | Aimbot, aimlock, silent aim, spinbot, anti-aim, triggerbot. |
| ⚡ **Weapon abuse** | Doubletap and weapon fire timing that should not be possible. |
| 🏃 **Movement automation** | Bunnyhop automation, automated strafing, invalid movement input, perfect counter-strafe timing. |
| ⏱️ **Subtick abuse** | Subtick timing manipulation and subtick input spam. |
| 💥 **Exceptional kills** | Repeated flashed kills, smoke kills, wallbang kills, airborne kills, and noscope kills. |
| 📈 **Result patterns** | Unusual accuracy and headshot rates over meaningful combat samples. |
| 🔒 **Server protection** | Chat spam, radio spam, blocked names, no-spread hardening, local reputation lists. |
| 🧩 **Client-event watch** | Protected client-event behavior when the server gives S2AC the required access. |

## Detector Showcase 🔎

| Public Name | What It Catches |
| --- | --- |
| **Aimbot** | Unnatural aim snaps or corrections toward enemies. |
| **Aimlock** | Repeated lock-on behavior after sharp aim correction. |
| **Silent Aim** | Bullet impact does not match where the player appeared to aim. |
| **Spinbot** | Extreme spinning or impossible view movement. |
| **Anti-Aim** | Impossible or abusive view angles. |
| **Triggerbot** | Instant target shots, with hit confirmation by default. |
| **Doubletap** | Weapon fire faster than the weapon should allow. |
| **Bunnyhop Automation** | Perfect jump timing or impossible sustained speed. |
| **Automated Strafing** | Repeated automated movement-turn patterns in command data. |
| **Invalid Movement Input** | Movement commands normal clients should not send. |
| **Subtick Timing Manipulation** | Fine-timing input repeatedly flattened to the start of the tick. |
| **Subtick Input Spam** | Too many fine-timing inputs in a very short time. |
| **Perfect Counter-Strafe Automation** | Counter-strafe timing that is too perfect too often. |
| **Exceptional Kill** | Repeated kills in unusual situations reported by the server. |
| **Unusual Accuracy** | Accuracy and headshot results that stay too high after enough samples. |
| **Suspicious Client Events** | Protected client-event behavior linked to suspicious clients. |

## Evidence In Practice 📌

S2AC reports are designed for fast review.

```text
S2AC detected Doubletap.
Player: Sergei
Action: Banned
Map: de_mirage
Evidence: This player fired faster than the weapon should allow.
```

Each report can include:

- Player name and SteamID64
- Detection name
- Action taken
- Map
- Source
- Evidence summary
- Timestamp
- Steam profile link in Discord
- Optional staff role mention for bans

> [!TIP]
> Discord and file logging use bounded background queues. Slow webhooks or disk writes do not block active detector work.

## Default Posture ⚙️

S2AC ships with a production-minded profile.

| Setting | Behavior |
| --- | --- |
| `Enabled` | Turns a detector on or off. |
| `BanPlayer` | Chooses ban-ready enforcement or report-only behavior. |
| `SendWebhook` | Controls Discord alerts for supported detections. |
| Config cleanup | Fixes impossible values such as zero limits, negative values, or inverted ranges. |

Valid custom values are respected. If a value is lower than S2AC's ban-ready recommendation, S2AC keeps it and shows a warning in status output.

<details>
<summary><strong>Why some detections are faster</strong></summary>

Impossible input and command-data abuse are strong signals, so they can act quickly.

Result-based checks such as exceptional kills, high accuracy, and high headshot rates can overlap with skilled play, so they require repeated evidence.

</details>

## Operator Flow 🚀

- [x] Install the release package.
- [x] Add Discord webhook settings if desired.
- [x] Run `css_s2ac_status`.
- [x] Keep generated defaults unless you have a clear reason to tune.
- [x] Review config warnings after game updates or config changes.

| Command | Purpose |
| --- | --- |
| `css_s2ac_status` | Shows version, command-data health, file health, detector status, counters, cvar protection, and config warnings. |
| `css_s2ac_evidence [player-or-steamid]` | Shows recent evidence for one player from memory and today's detection log. |

Console can run both commands. In-game use requires admin access unless the command is made public in config.

## Install Layout 📦

Copy the release package into the CS2 server `game/csgo` folder.

```text
game/csgo/addons/counterstrikesharp/plugins/S2AC/
game/csgo/addons/counterstrikesharp/gamedata/s2ac.gamedata.json
game/csgo/addons/metamod/S2ACNative.vdf
game/csgo/addons/metamod/gamedata/s2ac_native.gamedata.json
```

S2AC expects:

- Counter-Strike 2 dedicated server
- CounterStrikeSharp
- Metamod:Source
- .NET 8 runtime available to CounterStrikeSharp

> [!NOTE]
> Keep the plugin files, gamedata file, and command data helper from the same release. Mixing release files can make command-data features unavailable.

## Files Owners Actually Use 🗂️

| File or Folder | Purpose |
| --- | --- |
| `addons/counterstrikesharp/configs/plugins/S2AC/S2AC.json` | Main settings generated by CounterStrikeSharp. |
| `addons/counterstrikesharp/plugins/S2AC/lang/en.json` | Chat and Discord wording. |
| `addons/counterstrikesharp/plugins/S2AC/data/antidll_events.txt` | Protected client-event watch list. |
| `addons/counterstrikesharp/plugins/S2AC/detections/` | Daily detection history. |

## Honest Boundaries 🤝

| S2AC Does | S2AC Does Not |
| --- | --- |
| Detect server-visible cheating behavior. | Scan player computers. |
| Explain reports in clean operator language. | Promise every private cheat is instantly detectable. |
| Use command data when available. | Hide when command-data access is unavailable. |
| Support report-only or ban behavior per detector. | Replace good server moderation judgment. |

## Recommended Live Setup ✅

> **For competitive-style public servers, start with the generated defaults.**

- Keep `BanPlayer=true` for detectors you trust to enforce.
- Keep Discord webhooks enabled for staff visibility.
- Check `css_s2ac_status` after install, after CS2 updates, and after config changes.
- Treat config warnings as a sign that a value is below the ban-ready recommendation.
- Keep plugin files, gamedata, and command data helper from the same release.

---

## The Standard ⭐

> **Clear detection. Clear evidence. Clear action.**

No mystery output. No vague staff alerts. No hidden status. Just readable server-side enforcement for CS2 communities.
