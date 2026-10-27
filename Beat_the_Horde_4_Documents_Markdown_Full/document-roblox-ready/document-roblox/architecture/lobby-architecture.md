# Lobby Architecture

The Lobby is an authoritative server domain, not an Admin web service.

```mermaid
flowchart TB
  UI[Lobby UI on Roblox Client] -->|Validated RemoteEvents| Gate[Lobby Gateway on Roblox Server]
  Gate --> Loadout[Primary and Equipment Loadout]
  Gate --> Upgrade[Permanent Upgrades]
  Gate --> Tome[Tome Unlock and Slots]
  Gate --> Shop[Shop Gacha Daily]
  Gate --> Entry[Run Start Validator]
  Loadout --> Profile[ProfileService]
  Upgrade --> Profile
  Tome --> Profile
  Shop --> Profile
  Profile --> DS[(Roblox DataStore)]
  Entry --> Snapshot[Immutable Loadout and Config Snapshot]
  Snapshot --> Run[Run State Machine]
```

**Approved rules:** 2 Tome Slots default; slots 3/4 unlock using Coin or early Robux; Primary selected in Lobby; Secondary baseline upgraded in Lobby but Secondary must still be obtained during each Run via in-Run card. Temporary Gold/XP/Mastery reset when Run ends. Exact pricing and several shops/portal rules remain TBD. See DOC-01 and DOC-04 Lobby appendix.
