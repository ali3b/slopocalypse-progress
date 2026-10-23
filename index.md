---
title: Slopocalypse — Build Progress
---

# Build progress

*Last updated: 2026-10-23 18:30 (Hanoi)*

Public status page for the Slopocalypse build. The work itself happens in private repositories. This page shows only milestones, progress and test status.

## Milestones

| Milestone | Status | Notes |
|---|---|---|
| Architecture (sessions 1–8) | ✅ Done | 24 decision records, full requirements traceability, design reviews |
| M0 Bootstrap | ✅ Done | Contracts repo, platform skeleton, CI and guards, host preparation |
| M1 Walking skeleton + tech spikes | 🚧 In progress | Gateway, policy, intake, catalog, orchestrator ✅; Blender spike S2 ✅; renderer spike S1 ✅ (golden parity pending) |
| **M2 Vertical slice** | ✅ Acceptance test passing | Rig → animation → sprites ✅; Godot + SlopEngine packages with real import tests ✅; publication + delivery ✅; license-free end-to-end test ✅ |
| **M3 Breadth, web targets, migration, Unity** | 🚧 Mostly done | **All five existing games pass their own load checks on platform-built assets** (Off Road 49/49, Monty 17/17, Sidefighter 29 sprites, Airborne Ranger: every sprite except the prisoner (not on this machine), 4dstunts 47 pack models). Supporting work: one-command migration (`migrate-game`); Phaser, web glTF and both legacy formats; tilesets; animated props; effects; destroyed states; fighting-game hit data; character clips and crowd poses; paint variants; texture budgets; levels of detail. **Airborne Ranger maps** now build on the new stack (see the log). **Open:** Airborne Ranger's prone and crawl moves, 4dstunts procedural track pieces, and Unity (needs a licence) |
| M4 Unreal and public channels | ⏳ | |
| M5 SlopEngine MVP | ⏳ | |
| M6 SlopStudio MVP | 🚧 Started | Level tool in `slop-studio`: builds the games' maps from their existing level scripts (ADR-0025) |

## Retiring the old generator: parity checklist

Goal: everything the four games got from the old generator (ssg) is built by the platform, so it is never needed again.

- [x] Airborne Ranger maps (12 missions) built by the new level tool
- [x] Tilesets identical to the old ones (variants, junction tiles, textures)
- [x] Prop collision outlines, facings, turret sweep, walk and run speeds
- [x] Procedural animation templates. Character: throw, shoulder-fire, kneel, prone, crawl. Vehicle: drive, fire, propeller. Effects: smoke, fire, impact and others (explosion: one check failing)
- [x] Moveset and declaration rules, held items, states, view options: the old renderer ported, each sprite rendered from its recorded settings
- [ ] Airborne Ranger: last sprite differences (tank muzzle, held-item points, two props' framing)
- [ ] Items held only in some clips (grenade, rocket launcher, bomb, camera, knife)
- [ ] Destroyed states that swap to a separate wreck model; destroy-transition timing
- [ ] Sprite options: uniform, split, shadow, effects layer, mirror, still, portrait, low three-quarter view, scale, clip length
- [ ] Clip trimming, labels and timing suffixes (Sidefighter)
- [x] 4dstunts procedural geometry: 28 track pieces, 16 terrains, 5 skydomes, 1 composite
- [x] 4dstunts: posed drivers, decal-free paint
- [x] Off Road: lift kit, fit, glass, alpha, animated characters, crowd poses, paint
- [ ] Final sweep: every game's import report fully mapped and its own checks passing, then retire ssg

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

- **18:30.** **Airborne Ranger runs entirely on the platform through the ported old renderer.** That covers sprites, tilesets and maps, with nothing from the old generator's outputs. All 76 packages build and the game's load and route checks pass, with mission times identical to the old build. Compared with the old exports, most sprites match exactly: frame sizes, pivots and the median socket position. Differences are being run down for the tank's muzzle point, held-item points on a few frames and two props' framing. Some come from old exports made before the generator's latest code; the explosion keeps its current orange look, by the owner's decision.

  **4dstunts is complete:** all 119 models now match the old build, including the 6 posed drivers and the decal-free car. Two more fixes on the way: the War pack's background mountains had been built at 1/100th size (fixed with the old generator's per-pack scale rule), and a platform self-test that was failing passes again. Sidefighter and Off Road are being re-checked on the latest code.

- **17:45.** **Off Road is now faithful to the old build, and the comparison showed the earlier "pass" was not.** Compared against the old exports, the earlier platform build had:
  - no animated characters: every driver, fan and flagman clip was a 0.03 s two-key take;
  - crowd poses that were the bind pose at 100× scale;
  - opaque truck windows, see-through grandstand seats, the wrong texture on the lift kit and an olive podium.

  The load check passed anyway, because it doesn't look at these. All of it is fixed by porting the old generator's own code (clip merging, pose baking, glass, cut-out alpha, texture matching, attachments, size fitting, paint recolouring). Now 44 of 49 models match the old exports within 1 cm; the other 5 differ only in fields the game doesn't read. The load check passes on an all-platform copy (test, race, title, podium, shop).

  **Also merged: the old sprite renderer, ported as a second render mode.** On sprites it was tested on, it reproduces the old files exactly and the pixels to within ±1 colour level. Airborne Ranger is migrating through it now, with all 75 sprite entries fully mapped.

- **17:15.** **4dstunts re-checked on the merged code: 119 of 119 models built by the platform, nothing from the old generator.** Every check the game has passes: garage, race and props, all 11 tracks, smoothness, and piece fit, whose output is identical to the old build's. Left for 4dstunts are the 6 posed drivers (they come from a combined character file) and one car's decal-free paint. The sprite-format additions the renderer port needed are released (contracts 0.12: clip names, extra sheets for split characters, additive blending, named body boxes).

- **16:40.** **4dstunts' procedural models now build on the platform: all 50 of them.** That's the 28 track pieces, 16 terrains, 5 skydomes and the gas-station composite. The old generator's geometry code was ported as is.
  - **Track pieces:** identical to the old ones in every vertex count, collider, bound and connection socket. Piece-fit gives exactly the old gaps.
  - **Terrains:** ground, water and colliders are identical, and the scattered props match in count.
  - **Skydomes:** two are identical; the rest differ by about 0.1 % of their triangles, because their ring props are platform-built meshes.
  - **Game checks:** on a 4dstunts copy built entirely by the platform (119/119 models), every check passes: garage, race, props, all 11 tracks, piece fit and smoothness.
  - **Packs:** two more were copied in from Downloads (Town, Swamp Marshland).
  - **Still open for 4dstunts:** posed drivers (they come from a combined character file) and one car's decal-free paint.

  The work is merged into the platform.

- **16:20.** **New approach for the last sprite gaps: render each sprite exactly as the old generator last did.** The old generator's database records the exact settings of every sprite it rendered: moves, held items, states, framing and options. Two pieces are being built:
  - the platform now reads those settings once, along with the models and animation clips each sprite used (done for Airborne Ranger and Sidefighter)
  - a third agent is porting the old renderer itself into the platform, as a second render mode

  When both land, held items, carry poses, states with wreck models, stills, split sheets and the remaining view options come out as before, without rebuilding each feature by hand.

- **15:35.** **The old generator's animation library now runs inside the platform.** Airborne Ranger builds all 75 sprites with it, and the game's load and route checks pass. The Ranger's grenade throw, rocket launcher, bomb and medkit placing, camera shot, prone, prone-shoot and crawl now come from the same procedural moves as before. Their timing (release and shot frames) and the crawl speed match the old build exactly. The searchlight sweep, radar spin, tank drive and fire, smoke and fire effects use the old motions too. Still open: the explosion effect fails one platform check, and some clip names (the MG gunner's crouch moves) need the old generator's moveset rules. Those rules are being ported next.

- **14:50.** Work now runs in three parallel streams: the animation templates (me), 4dstunts' procedural track, terrain and skydome pieces (a second agent) and Off Road's last gaps (a third agent). The old generator's procedural animation library, about 60 templates (character moves, vehicle motions, effects, prop motions), is ported and built into the platform's renderer. Airborne Ranger is now rebuilding with it: the Ranger's crawl, prone, throw and kneel moves, the vehicles' drive and fire, and real explosion, smoke and fire effects instead of stand-ins.

- **14:25.** **Decision: finish full parity with the old generator, then move on for good.** There are about 10 groups of remaining gaps across the four games (checklist above). Most come from one old library, the procedural animation templates, which will be ported whole into the platform's renderer next. The decision record (ADR-0025) now covers that, plus the old sprite options and geometry generators.

- **14:05.** **Sidefighter re-checked on the updated pipeline and still passes:** 31/31 packages, and its own load check runs full matches with no failures. Monty is no longer part of the migration, since its sprites come from another source. Its tileset was checked first and is pixel-identical to the old one. Small fix: tileset files now use the old generator's naming exactly. The games being migrated are Off Road, 4dstunts, Sidefighter and Airborne Ranger.

- **13:05.** **Walk and run speeds fixed.** The platform now measures how fast a character really moves in an animation, the same way the old generator did: from the planted feet. Every Airborne Ranger character's speed matches the old build to within 0.01 m/s (the Ranger walks at 1.22 m/s and runs at 2.81). On the all-platform build (sprites, tilesets and maps), the game's load check and route check both pass, and mission times match the old build exactly. Only the crawl speed is still missing, because the crawl animation isn't found yet. Next: re-check Monty and Sidefighter on the updated pipeline.

- **12:35.** **Airborne Ranger's 12 missions now build on the new stack**, outside the old generator. A new level tool (`slop-studio`, a private repo) replays each mission's existing level script against the platform-built sprites and tilesets.
  - **Engine check:** fed the old assets, it rebuilds all 12 missions byte-identical to the old exports.
  - **Built against the platform's own assets:** terrain, collision, zones and every object's position, sprite, facing and state match the old maps. Only collision outlines differ slightly.
  - **Game checks:** the game's load check passes, and its route check finds every route with no minefields on them. Route lengths match the old figures to within a metre or two.
  - **Tilesets:** the platform now builds them the way the old generator did (random variants, three- and four-terrain junction tiles, textures, tints). All four of Airborne Ranger's tilesets match the old ones tile for tile.
  - **Sprites:** the searchlight's sweep, ssg's one-facing rule for animated props, and the old collision outlines (rock, fern) are fixed.
  - **Decision recorded:** ADR-0025 (accepted). The map and tileset code was ported from the old generator so that results come out identical.
  - **Defect found, being fixed next:** walk and run speeds are wrong for animation clips that run in place. The Ranger's run speed comes out as 0.001 m/s instead of 2.81. The game moves characters at these speeds, and its load check doesn't test movement, so the earlier "passes" result missed it.

- **09:23.** **Every model and sprite the games need from your packs now builds.** Characters that Synty ships inside combined files (the Sidefighter boss, the Airborne Ranger prisoner, the 4dstunts drivers) are picked out by name. Five more packs were added. Sidefighter has 31/31, Airborne Ranger 75/75 and 4dstunts 69/69, and all three games' own checks pass. Still open: Airborne Ranger's maps and 4dstunts' procedural track pieces.

- **06:44.** Airborne Ranger's hostage now sits, using a seated clip from your Mixamo library, and its animation list matches the old export exactly. Final re-check: 74 of 74 packages pass and the game's own load check passes. End of the overnight run.

- **06:20.** Airborne Ranger's officer and hostage now build: their moveset maps onto Synty's basic idle, walk and run animations, as the old generator did. 74 of 74 packages pass their checks, and the game's own load check passes. The only one left is the prisoner, whose model isn't in the packs on this machine.

- **06:04.** Sidefighter's last broken effect (a spinning ring) is fixed: side-view games now draw props face-on, as Monty already did. Sidefighter has every sprite except the boss, whose pack isn't on this machine, and its load check still passes.

- **05:46.** **Overnight wrap-up: every game was re-checked on the final code, and all pass their own load checks** (Off Road, 4dstunts, Monty, Sidefighter, Airborne Ranger). Monty's skeleton has its hit and death animations back. One command (`migrate-game`) now rebuilds any of the five games from your packs. Still open:
  - Airborne Ranger's maps
  - 4dstunts' procedural track pieces
  - assets in packs that aren't on this machine (the Sidefighter boss, some 4dstunts scenery and drivers)
  - Unity, which needs a licence

- **05:15.** **All five games now pass their own load checks on platform-built assets.** 4dstunts races with platform cars. Paint colours now recolour just the car's paint (liveries, white bases) rather than the whole car. Texture budgets work: Off Road's models went from 46 MB to 5.7 MB, and the 4dstunts cars match the old file sizes (about 600 KB). Scenery has levels of detail.

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
