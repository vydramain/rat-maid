# Newgrounds Release Plan

## Goal

**Ratmaid: 1-hour narrative arcade-cleaning game for Newgrounds.**

Not a "big game". Not a "cleaning simulator". Not a "system on years scale".
A short, dense, weird web release where the player follows MAID-7 from routine cleaning to a corporate incident in about an hour.

Working title shortlist (not final):

1. Ratmaid: Cleanup Protocol
2. MAID-7: Cleanup Protocol *(strongest standalone)*
3. Bottom Up Maid
4. Ratmaid: No Evidence
5. MAID-7: Leave No Trace

## Technical target

- **Engine:** Godot 4.6
- **Export:** HTML5, single-threaded
- **Renderer:** Compatibility (WebGL 2.0)

Reasons:

- Godot 4 web export only works via Compatibility renderer; Forward+ and Mobile are not supported on web.
- Threaded web export needs cross-origin isolation headers that Newgrounds does not reliably provide; single-threaded is the recommended default for web-hosted games.
- Newgrounds has favored HTML5 over Flash since 2018.

The README currently lists Forward+. This must change before content work begins.

## Campaign structure

Five scenes, ~58 minutes first playthrough:

| Scene                    | Time   | Notes                                                                    |
| ------------------------ | -----: | ------------------------------------------------------------------------ |
| 0. Menu / Attract        |      — | Already mostly built (menu, settings, pause, localization, gamepad).     |
| 1. Tutorial              |  8 min | Movement, mop, vacuum, bags, incinerator, trash-block hitch, charge return. |
| 2. Cafe Breach           | 12 min | First fight. Mop only, no firearms.                                      |
| 3. Evidence Storage      | 15 min | Police station / evidence storage. Firearms appear. Casings as evidence. |
| 4. Corporate Warehouse   | 20 min | Final level. Technicians, guards, other androids, server destruction.    |
| 5. Ending                |  3 min | MAID-7 sees her own file, pauses half a second, erases it, walks out.    |

## Minimal mechanic set

| Evidence       | Source             | Cleanup                              |
| -------------- | ------------------ | ------------------------------------ |
| Body           | enemy death        | drag to trash block / incinerator    |
| Blood          | enemy death        | mop mode                             |
| Casings        | gunfire            | walk over / interact nearby          |
| Cameras/server | story objective    | interaction                          |

Counters for bodies, blood, and casings already live in `level_manager.gd`. Extend that state machine — do not rewrite it.

## Out of scope (deferred to post-release)

- Deep per-corpse bag system
- Vacuum cleaner as a separate inventory item
- Full bloody bootprint trails
- Hitman-style perfect cleanup ritual
- Detective character / hunt arc
- Large corporate-conspiracy story arc
- Procedural levels
- Stealth system
- Complex melee combat
- Custom per-location music (if time-constrained)

If a proposal pulls any of these back in, treat it as a deliberate scope reopen, not a default.

## Roadmap

### Milestone 1 — Web sanity

Goal: project opens in a browser.

- Switch renderer to Compatibility.
- Add HTML5 export preset.
- Disable thread support.
- Confirm main menu loads in browser.
- Confirm audio, fullscreen, input, gamepad.
- Add `docs/web-export-checklist.md`.

Blocks everything else.

### Milestone 2 — Campaign skeleton

Goal: whole game playable in ~10 minutes as greybox.

- `CampaignState.gd` autoload.
- `change_to_next_level()`.
- Scenes: `tutorial.tscn`, `cafe.tscn`, `police_storage.tscn`, `corporate_warehouse.tscn`, `ending.tscn`.
- Placeholder rectangles for art.
- Each level has a win condition.

### Milestone 3 — Tutorial + hints

Goal: the player understands what to do.

First concrete code change: implement `_check_cleanup_hint()` in `level_manager.gd` (currently two `pass` statements).

Hints:

- "Убери тело"
- "Отмой кровь"
- "Собери гильзы"
- "Иди к выходу"
- "Вернись к зарядке"

### Milestone 4 — Story pass

Goal: the game stops being a level list and starts being a story.

- Short dialogues.
- MAID-7 system messages.
- A glitch.
- A mission change.
- The final file.

No multi-minute cutscenes. Newgrounds rewards a fast tempo.

### Milestone 5 — Content pass

Goal: 45–60 minutes of first playthrough — match the table above.

### Milestone 6 — Newgrounds polish

- Short splash.
- Cover art.
- Title screen.
- Restart.
- Mute / music volume.
- Content rating.
- Credits.
- Web performance.
- Tested in Firefox and Chromium.

## First commit after this plan

```
Add web export preset and cleanup objective hints
```
