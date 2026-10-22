---
title: Slopocalypse — Build Progress
---

# Build progress

*Last updated: 2026-10-22 22:16 (Hanoi)*

Public status page for the Slopocalypse build. The work itself happens in private repositories. This page shows only milestones, progress and test status.

## Milestones

| Milestone | Status | Notes |
|---|---|---|
| Architecture (sessions 1–8) | ✅ Done | 24 decision records, full requirements traceability, design reviews |
| M0 Bootstrap | ✅ Done | Contracts repo, platform skeleton, CI and guards, host preparation |
| M1 Walking skeleton + tech spikes | 🚧 In progress | Gateway, policy, intake, catalog, orchestrator ✅; Blender spike S2 ✅; renderer spike S1 ✅ (golden parity pending) |
| **M2 Vertical slice** | 🚧 In progress | Rig → animation → sprites ✅ with human review; Godot + SlopEngine packaging next |
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
- [x] Host: Blender 5.2.3 LTS worker image (sandboxed: no network, read-only)
- [x] Host: three.js render worker image (sandboxed headless Chromium)

## M1 checklist

- [x] Application service + HTTP / CLI / MCP facades (auth, roles, idempotency, async operations)
- [x] Source intake with Synty adapter: real pack of 646 files classified in 1.7 s
- [x] Orchestrator: caching, retries, cancellation, crash recovery (7/7 tests)
- [x] **End to end:** pack → recipe → sandboxed Blender → approved asset with provenance, in ~5 s
- [x] Spike S2: real Synty character normalised deterministically (byte-identical reruns)
- [x] Spike S1: real Synty soldier retargeted onto a walk cycle and rendered as an 8-direction sprite set, 80 frames in 1.4 s, deterministic

## Build log

- **22:16.** Vertical-slice flow runs through the API: 2 packs → soldier + walk/idle retargeted → 8-direction sprite set; reviewer approval cascades. 14 s end to end.

- **22:03.** First real sprites: a Synty soldier walking in 8 iso directions, rendered by the new sandboxed pipeline. Retargeting maps 22/22 bones.

- **21:56.** M1 walking skeleton works end to end. Blender worker passes spike S2: deterministic output, correct metric scale.

- **22:20.** Contracts v0.1.0 tagged. Platform core modules done; PostgreSQL store passes concurrency contract tests.

- **2026-10-22 21:50.** Architecture complete. M0 started. Progress page created.
