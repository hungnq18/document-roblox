# Run State and Clock

```mermaid
stateDiagram-v2
  [*] --> Lobby
  Lobby --> Init: Validated start
  Init --> Phase1
  Phase1 --> PhaseBoss1: SurvivalElapsed = 360s
  PhaseBoss1 --> Portal1: Boss defeated
  Portal1 --> Phase2: Continue
  Phase2 --> PhaseBoss2: SurvivalElapsed = 720s
  PhaseBoss2 --> Portal2: Boss defeated
  Portal2 --> MapBoss: Continue
  MapBoss --> Results
  Portal1 --> Results: Exit if permitted
  Portal2 --> Results: Exit if permitted
  Phase1 --> Results: Death
  Phase2 --> Results: Death
  Results --> Lobby: Idempotent reward commit
```

Mini-boss triggers at 120, 270, 480 and 630 survival seconds. Survival clock pauses for boss and portal encounters. Remaining-enemy portal gate is TBD; exit rewards follow approved tables, not assumptions.
