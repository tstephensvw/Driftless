# ROADMAP — Operation Driftless

Planned features for the playbook (`index.html`). Every one of these must match
the existing Sheikah slate aesthetic and stay inside the single self-contained
file — see `CLAUDE.md`.

**Status values:** `TODO` · `IN PROGRESS` · `DONE`

When an item is finished, its status is updated to `DONE` in the same commit as
the code that finished it.

---

## 1. Persistent progress with a guarded reset

**Status: TODO**

Save every checked step and unit to the browser's `localStorage` so progress
survives closing the tab, locking the iPad, or the browser reloading the page
mid-repair. This replaces the current `window.storage` calls, which do nothing
in a real browser — right now nothing is actually being saved.

Add a reset button that clears all progress, guarded so it cannot be triggered
by accident with a stray palm: a confirm step ("tap again to wipe"), not a
single tap. Guard against corrupt or half-written saved data so a bad value can
never leave the page stuck or blank.

## 2. Per-unit state machine with an active-unit picker

**Status: TODO**

Track all six controllers independently instead of sharing one global set of
checkboxes. Each unit moves through: **Not Started → Phase 1 Stripped → Phase 2
Staged → Phase 3 Complete → Tested**, with its own step checkmarks and its own
progress rail.

Add an always-reachable active-unit picker so tapping a step checks it off for
the controller currently on the bench. The unit tiles in the TRACK section show
each unit's current state, colored by phase (gold / cyan / green), and become
the picker. State advances automatically as a phase's steps are completed, and
the design must still support the assembly-line order — all six through Phase 1
before anyone starts Phase 2.

## 3. Camera capture per step

**Status: TODO**

A camera button on each step that opens the iPad's camera, so they can shoot
"this is how it looked before I pulled it" reference photos — the screw layout,
which way a ribbon faced, where a spring sat.

Photos are stored locally on the device (IndexedDB, since these are too big for
`localStorage`), scoped to the active unit so Unit 03's photos never show up
under Unit 05. Each step shows small thumbnails of its shots; tap one to view it
full-size, and allow deleting a bad shot. Nothing is ever uploaded anywhere.

## 4. Read-aloud steps and hands-free voice mode

**Status: TODO**

A speaker button on each step that reads it out loud using the browser's
built-in speech, so a step can be heard while both people have their hands full
and eyes on the controller. This directly supports ground rule 04 — the step
gets said out loud before anyone moves.

On top of that, an optional hands-free voice mode, off by default and clearly
toggled, that listens for a small fixed set of commands: **"next step,"** **"repeat
that,"** and **"check it off."** Must degrade gracefully — on any browser without
speech recognition the buttons simply do not appear, and read-aloud keeps
working on its own.

## 5. Per-unit and per-phase timing with a comparison chart

**Status: TODO**

Time how long each unit takes, broken down by phase, starting when the first
step of a phase is checked and stopping at the last. Pause sensibly when the
page is left in the background so a lunch break doesn't count as build time.

Show a bar chart comparing all six units side by side — drawn by hand in the
page's own style, not a charting library — plus a headline "you got **N% faster**"
figure comparing the first completed unit against the most recent. This is the
payoff for doing all six: watching the assembly line get quicker.

## 6. Installable offline PWA

**Status: TODO**

Make the playbook installable to the iPad home screen and fully usable with no
internet — the garage or workbench may have no usable Wi-Fi. Needs a web app
manifest, a service worker that caches the page and its fonts, and a Sheikah-
styled app icon (glowing gold-and-cyan eye motif on dark slate) at all the sizes
iOS and Android ask for.

The two YouTube embeds cannot be cached; when offline they should show a tidy
in-style placeholder rather than a broken frame. The service worker must update
cleanly so an old cached copy never sticks around after a deploy.

## 7. Branching troubleshooting decision tree

**Status: TODO**

A guided "something's wrong" section that asks one question at a time and
branches to the next based on the answer, ending in a specific fix — rather than
a wall of text to scan while frustrated. Written in the same plain, explain-the-
why voice as the rest of the page, and reachable without losing your place in
the build steps.

Covers these six failure paths:

1. **Drift after the swap** — still drifting with a brand-new stick installed.
2. **Dead buttons** — one or more face buttons, D-pad or triggers not registering.
3. **Pairing failure** — the controller will not connect to the console.
4. **Charging failure** — will not charge, or will not hold a charge.
5. **Dead SL/SR** — the side rail buttons do nothing.
6. **Shell will not close flush** — a gap or bulge when the halves go together.

Most branches should point back at the seating of a ribbon connector, which the
page already flags as the number one mistake.
