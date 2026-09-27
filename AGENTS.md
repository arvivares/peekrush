# PeekRush — Development Guidelines

- **Self-Hosted & Independent**: This project is fully independent, running with native TanStack Start, Nitro, Tailwind CSS, Colyseus game server, and PostgreSQL.
- **Git Workflow**: Follow standard git hygiene. Ensure the branch remains buildable and all automated checks pass before pushing.
- **Architecture**:
  - Web client (TanStack Start + Nitro + React + Tailwind CSS) in `/` and `src/`.
  - Game server (Node.js + Colyseus + Sharp) in `apps/game-server/`.
  - Shared contracts and schemas in `packages/contracts/`.
