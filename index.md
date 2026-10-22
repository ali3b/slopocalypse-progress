---
title: Slopocalypse — Build Progress
---

# Build progress

*Last updated: 2026-10-22 22:20 (Hanoi)*

Public status page for the Slopocalypse build. The work itself happens in private repositories. This page shows only milestones, progress and test status.

## Milestones

| Milestone | Status | Notes |
|---|---|---|
| Architecture (sessions 1–8) | ✅ Done | 24 decision records, full requirements traceability, design reviews |
| **M0 Bootstrap** | 🚧 In progress | Contracts repo, platform skeleton, CI and guards, host preparation |
| M1 Walking skeleton + tech spikes | ⏳ Next | Gateway, policy, intake, catalog, orchestrator; renderer and Blender spikes |
| M2 Vertical slice | ⏳ | First character through rig, animation and sprites to Godot and SlopEngine |
| M3 Breadth, web targets, migration, Unity | ⏳ | |
| M4 Unreal and public channels | ⏳ | |
| M5 SlopEngine MVP | ⏳ | |
| M6 SlopStudio MVP | ⏳ | |

## M0 checklist

- [x] Node.js 24 LTS on the build host
- [x] Architecture repository pushed (private)
- [x] Contracts repository v0.1.0: 15 schemas, 43 operations, 56 fixtures, compatibility gate, MCP/OpenAPI/CLI generators (7/7 tests)
- [x] Platform repository: module skeleton, dependency-rule check, asset guard, CI
- [x] Kernel, PostgreSQL + in-memory store, content-addressed artifact store, catalog, provenance, licence policy (10/10 tests)
- [x] Host: PostgreSQL 17 container
- [ ] Host: worker container images (Blender, renderer)

## Build log

- **22:20.** Contracts v0.1.0 tagged. Platform core modules done; PostgreSQL store passes concurrency contract tests.

- **2026-10-22 21:50.** Architecture complete. M0 started. Progress page created.
