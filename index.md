---
title: Slopocalypse — Build Progress
---

# Build progress

*Last updated: 2026-10-23 04:38 (Hanoi)*

Public status page for the Slopocalypse build. The work itself happens in private repositories. This page shows only milestones, progress and test status.

## Milestones

| Milestone | Status | Notes |
|---|---|---|
| Architecture (sessions 1–8) | ✅ Done | 24 decision records, full requirements traceability, design reviews |
| M0 Bootstrap | ✅ Done | Contracts repo, platform skeleton, CI and guards, host preparation |
| M1 Walking skeleton + tech spikes | 🚧 In progress | Gateway, policy, intake, catalog, orchestrator ✅; Blender spike S2 ✅; renderer spike S1 ✅ (golden parity pending) |
| **M2 Vertical slice** | ✅ Acceptance test passing | Rig → animation → sprites ✅; Godot + SlopEngine packages with real import tests ✅; publication + delivery ✅; license-free end-to-end test ✅ |
| **M3 Breadth, web targets, migration, Unity** | 🚧 In progress | Phaser ✅, web glTF ✅, legacy-ssg (3D) ✅, vehicles with wheels ✅, tilesets (iso + square, Tiled export) ✅, exact textures ✅, pixel-art look ✅, prop sprites with animation templates ✅, colour tilesets ✅; Off Road migrated: its own load check passes on platform assets ✅; Monty 17/17, **its own full load check passes** ✅; Airborne Ranger 65/65 sprites, **its own load check passes** ✅; Sidefighter 28/29, **the game's own load check passes** ✅; meshopt ✅; Unity needs a licence |
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

- **04:38.** **Off Road is complete: all 49 of its models** rebuild, pass their checks and load in the game. That includes the stadium crowds (7 baked poses each), the animated drivers and the painted trucks.

- **04:13.** Off Road's drivers and fans now come with their animations (celebrations, cheering and idles), and its load check still passes. Cars in Off Road and 4dstunts get their paint colours (8 per car in 4dstunts), done the simpler way: the whole car is tinted, not just the paint. 4dstunts: all 47 cars and props whose packs are on this machine now build and pass their checks.

- **03:53.** **All three 2D games now pass their own load checks on platform assets:** Monty (every scene: rooms, keys, ladders, bonus, deaths, game over), Sidefighter (full matches) and Airborne Ranger. Monty needed one fix to its test script, made only in a scratch copy: the script copied the whole game scene out of the browser on each check. Airborne Ranger's 4 tilesets also build now, with the declared terrain pairs and pack textures.

- **03:17.** **Airborne Ranger passes its own load check on platform sprites** (it still uses the old maps). Destroyed states and effect animations (bursts, smoke, flames) were added. **One command now migrates a game**: `scripts/migrate-game.ts`. It takes in the packs, builds everything with import tests, stops for your review (or `--approve-all`), then publishes and delivers in the layout the game already loads. It has been tested on Monty (2D) and Off Road (3D).

- **02:58.** **Sidefighter passes its own load check on platform assets.** Sweeps knock down, jump kicks land, and dashes go both ways. Rounds and the match play out.

- **02:43.** **Sidefighter fights on platform assets.** In the game's own check, jabs land, blocks work, combos chain, fireballs fly and clash, and full rounds and the match play out. The new sprites carry the hit timing, the striking limb and per-frame hand, foot and head positions. The fighters' hair now sits on their heads, and Airborne Ranger props get destroyed states. Three moves still failed the check (sweep, jump kick, forward dash): they were cut from longer takes and need trimming. That is fixed and being re-checked now.

- **01:33.** **Monty runs on the new platform's assets.** A copy of the game was loaded with sprites delivered in the old layout it expects. Joe idles and walks (using your Mixamo moves), the rooms draw with the new tileset, and the props sit at the right size. Sidefighter rebuilt 28 of its 29 sprites, with every fighter's full moveset. Airborne Ranger's sprites are complete (65/65) after one more size fix. One limit: on this machine, the game's own full test is too slow under software graphics (about a minute per room change, also with the old assets), so it isn't run to the end yet.

- **00:35.** **Third game: Airborne Ranger.** 64 of its 65 sprites rebuilt at true world scale on the iso grid, and all Phaser checks pass. **Monty is now complete (17/17).** Joe was built from your Mixamo martial-arts clips, which the platform can now take in as an animation library. Bugs fixed along the way:
  - Some Synty files came in 100 times too small.
  - The tank and the camo tents had the wrong texture; the netting now has see-through gaps.
  - Off Road models are meshopt-compressed again, as the game expects.

  Sidefighter is running now.

- **23:45.** **Second real game: Monty.** Its real 2D asset file was rebuilt from your local packs: 16 of 17 assets (the tomb tileset, the skeleton walking side-on, 14 animated props), and every Phaser import check passes. Comparing with the old generator's output caught and fixed three bugs: grey textures on Dark Fantasy props, flat props seen edge-on, and tiny props shrinking when they bob. Only Joe is left. His martial-arts moves came from Mixamo uploads, which aren't in any Synty pack.

- **23:35.** Monty path (2D Phaser games). The platform now reads the 2D asset file (`ssg.assets.json`) as well as the 3D one. It covers colour tilesets, props animated with bob, spin and similar templates, and characters whose clips are found by name. Anything it can't do yet is reported per asset. Six end-to-end suites pass.

- **23:24.** The Off Road truck now matches the old generator exactly on the details the game uses: lift-kit wheels, the same wheel node names, radius 0.5499 m, length 4.5 m. Simplify, fit and attach are supported. A batch recipe now isolates per-asset failures.

- **23:14.** Comparing Off Road with its original screenshot caught a serious texture bug: GLBs shipped without images. It is fixed and covered by a regression test. Rocks, cliffs and stands now look like the original game.

- **22:58.** **First real game on the new platform.** Off Road's asset declaration was imported, 48 of 49 models rebuilt and delivered into a copy of the game, and the game's own load check passes: the truck sits on 4 wheels, ground error 1 mm. Two intake bugs were found and fixed along the way.

- **23:05.** Tilesets: seamless corner-transition tiles (iso diamond and square), exported to Tiled and checked in Phaser. Pixel-art sprite look added. Real Synty textures now bind exactly by name. 4 end-to-end suites green.

- **22:48.** M3 targets for the existing games: Phaser 3.80.1 (headless import test checks pivot and frame sizes), web glTF and legacy formats (three.js 0.186.1 import test). A real Synty rally car comes out with all 4 wheels classified, at axle height.

- **22:31.** Vertical-slice acceptance test (VS-1) passes in full: vendor pack update with rollback, incompatible-rig and licence refusals, second-resolution cache reuse. 21 s, no licensed content. Moving to M3.

- **22:25.** Publication and delivery are done: local edits in a game project are never overwritten, and unprotected public-web publishing is refused. The whole vertical slice now has a committed license-free end-to-end test, using a synthetic humanoid generated by Blender, that passes in 13 s.

- **22:16.** The vertical slice reaches the engines. Godot 4.7.2 imports the soldier headless (41-bone skeleton, 16 sprite animations), and the SlopEngine loader verifies all 8 directions. Two target integration repos were created.

- **22:16.** Vertical-slice flow runs through the API: 2 packs → soldier + walk/idle retargeted → 8-direction sprite set; reviewer approval cascades. 14 s end to end.

- **22:03.** First real sprites: a Synty soldier walking in 8 iso directions, rendered by the new sandboxed pipeline. Retargeting maps 22/22 bones.

- **21:56.** M1 walking skeleton works end to end. Blender worker passes spike S2: deterministic output, correct metric scale.

- **22:20.** Contracts v0.1.0 tagged. Platform core modules done; PostgreSQL store passes concurrency contract tests.

- **2026-10-22 21:50.** Architecture complete. M0 started. Progress page created.
