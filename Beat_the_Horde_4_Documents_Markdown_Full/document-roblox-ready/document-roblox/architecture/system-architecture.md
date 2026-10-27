# Beat the Horde — System Architecture

**Approved repository layout:** 3 independent Git repositories. **Runtime deployment:** Roblox experience plus Next.js Web and NestJS API. **Backend style:** modular monolith, microservices-ready.

```mermaid
flowchart TB
  Specs[Docs Repo: GDD GSD Balance TDD Contracts]
  subgraph Admin[Admin Repo]
    Web[Next.js Web] -->|HTTPS| Api[NestJS API]
    Api --> Prisma[Prisma ORM] --> DB[(PostgreSQL)]
  end
  subgraph Game[Roblox Repo]
    Client[Roblox Client] <-->|RemoteEvents| Server[Luau Server]
    Server --> Lobby[Lobby] 
    Server --> Run[Run and Combat]
  end
  Lobby --> Store[(Roblox DataStore)]
  Server -->|HTTPS published config| Api
  Specs -.-> Admin
  Specs -.-> Game
```

Admin config in PostgreSQL; persistent player profile in Roblox DataStore; temporary run state in server memory. Roblox clients never connect to PostgreSQL.
