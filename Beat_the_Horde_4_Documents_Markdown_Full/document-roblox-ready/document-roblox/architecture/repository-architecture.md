# Repository Architecture

| Repository | Files owned | CI / release boundary |
|---|---|---|
| `document-roblox` | Docs, source diagrams, contract specs | Links/markdown/schema lint |
| `beat-the-horde-roblox` | `default.project.json`, `src/client`, `src/server`, `src/shared`, tests | Luau checks / Rojo / Studio |
| `beat-the-horde-admin` | `apps/web` Next.js, `apps/api` NestJS, Prisma, infra | Web deploy, API deploy, Prisma migrations |

Git repositories are independent from deploy units. No submodule required. Keep versioned API compatibility tests in both code repositories. `docs/` contains gameplay specs; actual code does not belong in this repository.
