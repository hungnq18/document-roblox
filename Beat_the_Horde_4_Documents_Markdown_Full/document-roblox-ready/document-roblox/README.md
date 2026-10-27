# Beat the Horde — Documentation Repository

**Git strategy:** 3 repos, with **Next.js + NestJS** together in the Admin repo. This repository contains specifications, not game or web runtime.

## Core documents

- [DOC-01 GDD](docs/DOC-01_GDD.md) — Game design and approved rules
- [DOC-02 GSD](docs/DOC-02_GSD.md) — System behavior and dependencies
- [DOC-03 Balance Calculator](docs/DOC-03_Balance_Calculator.md) — 23 worksheets exported as STATIC Markdown; Excel formulas do not recalculate here
- [DOC-04 TDD](docs/DOC-04_TDD.md) — technical design, Lobby appendix and multi-repo FS-01 architecture

## Architecture

- [System](architecture/system-architecture.md)
- [Repository boundaries](architecture/repository-architecture.md)
- [Lobby](architecture/lobby-architecture.md)
- [Admin](architecture/admin-architecture.md)
- [Run State](architecture/run-state-machine.md)

## API contracts (draft/specification)

- [OpenAPI](contracts/openapi.yaml)
- [Published config JSON Schema](contracts/config.schema.json)

**Decision precedence:** Approved latest GDD/GSD/BL-V2 rules supersede historical contradictory text. PROPOSED and TBD are not approved. Contract files are partial planning specifications, not evidence of a functioning deployed service.

**Asset policy:** 13 original PNG images are preserved under `assets/images/`; links inside documents are relative.

## Repositories

- `hungnq18/document-roblox` — docs, diagram sources, specs
- `beat-the-horde-roblox` — Luau + Rojo and Roblox DataStore
- `beat-the-horde-admin` — Next.js Web and NestJS API + Prisma / PostgreSQL
