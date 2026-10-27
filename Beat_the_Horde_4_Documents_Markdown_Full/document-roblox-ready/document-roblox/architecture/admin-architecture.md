# Admin Architecture — Next.js + NestJS

**Selected stack:** Next.js TypeScript frontend; NestJS TypeScript modular monolith API; Prisma/PostgreSQL.

```mermaid
sequenceDiagram
  actor Admin
  participant UI as Next.js
  participant API as NestJS
  participant DB as PostgreSQL
  participant RB as Roblox Server
  Admin->>UI: Edit balance draft
  UI->>API: Authenticated save draft
  API->>API: RBAC and schema validation
  API->>DB: Write draft
  Admin->>UI: Publish
  UI->>API: Publish draft
  API->>DB: Commit immutable release and audit
  RB->>API: HTTPS GET published config
  API-->>RB: Versioned JSON
  RB->>RB: Validate and freeze next Run config
```

**Proposed endpoints** are specified in `../contracts/openapi.yaml` (not verified deployed). Public read routes must not expose admin writes.
