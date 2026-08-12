# CLAUDE.md — Operation Driftless

Read this before touching anything. It exists so nobody has to re-explain the
project at the start of every session.

## What this project is

A single self-contained file, `index.html`, at the root of the repo. It is an
interactive repair playbook titled **Operation Driftless**, used to rebuild six
Nintendo Switch Joy-Con controllers — replacing the drifting potentiometer
sticks with magnetic TMR stick modules, plus new batteries, SL/SR ribbons,
shells and grips.

Everything is in that one file: HTML, CSS in a `<style>` block, and vanilla JS
in a `<script>` block at the bottom. No build step, no framework, no bundler,
no npm. **Keep it that way.** The only external requests are the Google Fonts
stylesheet and two lazy-loaded YouTube embeds.

## Who uses it, and how

Two people: **Dad** (lead engineer) and **Henry, age nine** (fabrication and
procedure). They read it off an **iPad, propped up on the workbench, mid-repair,
with greasy hands and tools in them.**

That single fact drives most design decisions:

- **Tap targets are big.** Whole step rows are tappable, not tiny checkboxes.
- **No fiddly interactions.** No drag, no long-press, no hover-only affordance,
  no multi-step dialogs. One tap does one thing.
- **Nothing that loses their place.** No full-page reloads or view swaps that
  scroll them back to the top. They may be halfway down Phase 1 with a
  controller open in front of them.
- **Nothing destructive without a guard.** A stray palm on the screen must
  never wipe progress.
- **Prose is plain and explains the "why."** A nine-year-old is the audience.
  Read the existing `.why` and `.warn` asides for the voice — direct, concrete,
  a little funny, never condescending. Match it.

## Structure of the page

- **Sticky progress rail** — overall percent and an `x/y` step count.
- **Header** — title, crew line, four stat tiles.
- **BRIEF** — what drift is and why magnets fix it.
- **RULES** — six ground rules, plus the role-tag legend.
- **LOADOUT** — a Minecraft-style inventory grid of parts and tools, quantities
  in the bottom-right corner of each slot.
- **FILM** — two YouTube teardown videos.
- **BUILD** — the three phases (below).
- **TRACK** — six unit tiles, built by JS, currently a plain done/not-done toggle.
- **Footer**.

### The three-phase repair process

The whole plan rests on one rule stated in the page: **open each controller
exactly once.** Do not propose changes that undercut this.

1. **Phase 1 — Strip it to the skeleton** (`.phase.p1`, gold). Steps 1.1–1.7.
   Everything old comes out. Ends with a bare circuit board and an empty shell.
2. **Phase 2 — Build up the board on the bench** (`.phase.p2`, cyan). Steps
   2.1–2.3. New parts go onto the loose board flat on the table. Nothing goes
   near a shell. Ends on a spoken-aloud checkpoint.
3. **Phase 3 — Drop it into its new home** (`.phase.p3`, grass green). Steps
   3.1–3.7. New shell is populated with buttons, the assembly drops in, it
   closes for good, then calibrate and drift-test.

The intended working order is **assembly line, not one-at-a-time**: all six
units through Phase 1, then all six through Phase 2, then all six through
Phase 3.

### Role tags

Every step carries exactly one role tag, rendered as `<span class="tag …">`:

| Tag        | Class        | Color              | Meaning              |
| ---------- | ------------ | ------------------ | -------------------- |
| `DAD`      | `.tag.dad`   | gold `--gold`      | Dad does it          |
| `HENRY`    | `.tag.henry` | cyan `--cyan`      | Henry does it        |
| `TOGETHER` | `.tag.both`  | green `--grass`    | Both hands on deck   |

The split is deliberate and safety-driven — ribbon cables and prying are always
DAD, per ground rule 02. **Never reassign a step's role tag without being
asked.** Any new step needs a tag.

## Visual design system

The aesthetic is a **Zelda: Breath of the Wild Sheikah Slate** (dark slate
panels, glowing gold and cyan runic accents, pixel-font labels) crossed with a
**Minecraft inventory grid** (chunky beveled slots with a light bottom-right
edge and dark top-left edge, quantity numerals in the corner).

### Fonts — loaded from Google Fonts, already in `<head>`

| Font              | Used for                                                        |
| ----------------- | --------------------------------------------------------------- |
| **Bungee**        | Big display headings: `h1`, `h2`, card/phase `h3`, stat numbers, unit names |
| **Silkscreen**    | Pixel-style small caps labels: tags, step numbers, eyebrows, section numbers, `.qty`, footer. Almost always uppercase with wide `letter-spacing` |
| **Space Grotesk** | All body copy and step text                                     |

### Color tokens — declared in `:root`, use these, never raw hex

| Token          | Value     | Role                                            |
| -------------- | --------- | ----------------------------------------------- |
| `--slate`      | `#12181F` | Page background                                 |
| `--slate-2`    | `#1A232D` | Step rows, rule rows                            |
| `--panel`      | `#202B37` | Cards, callouts, video panels                   |
| `--slot`       | `#28333F` | Inventory slots, stat tiles, unit tiles          |
| `--edge-dark`  | `#0B1015` | Top/left bevel — the "carved in" edge            |
| `--edge-light` | `#3D4C5B` | Bottom/right bevel — the "raised" edge           |
| `--gold`       | `#E8A33D` | Primary accent, DAD, Phase 1                     |
| `--cyan`       | `#4FD6C8` | Secondary accent, HENRY, Phase 2, focus rings    |
| `--redstone`   | `#C9483F` | Warnings only (`.warn`)                          |
| `--grass`      | `#77B255` | Success/done, TOGETHER, Phase 3                  |
| `--ink`        | `#E9EEF2` | Body text                                        |
| `--muted`      | `#8FA0AE` | Secondary text                                   |
| `--rule`       | `rgba(143,160,174,.22)` | Hairline borders                   |

### Non-negotiable style rules for anything new

- **Match the Sheikah slate aesthetic. Always.** New UI has to look like it
  shipped with the original page — same tokens, same fonts, same bevels, same
  pixel-label voice. No default browser widgets, no Bootstrap-looking buttons,
  no rounded pill chrome, no drop shadows that aren't already in use, no new
  colors outside the token list.
- **Bevels, not border-radius.** The whole page has square corners. Chunky
  panels use `border-top`/`border-left: var(--edge-dark)` and
  `border-bottom`/`border-right: var(--edge-light)` (3–4px).
- Section header labels (`.sect-num`) are short uppercase words like `BRIEF`,
  `RULES`, `LOADOUT`, `FILM`, `BUILD`, `TRACK`. Follow that pattern.
- **Respect what's already there.** `@media (prefers-reduced-motion:reduce)`
  kills all animation; `:focus-visible` draws a cyan outline; the `@media
  (max-width:720px)` block collapses two-column grids to one. Keep all three
  working, and keep every new control keyboard-reachable with
  `tabIndex`/`role`/`aria-checked`, the way `.step` and `.unit` already do.
- **Vanilla JS only**, wrapped in the existing IIFE, no dependencies.

## Known gotcha: progress does not currently persist

The `save()`/restore code in `index.html` writes to `window.storage`, which is a
sandbox-host API that **does not exist in a normal browser**. On GitHub Pages it
silently does nothing, so closing the tab loses all progress. Fixing this is
ROADMAP item 1 (`localStorage`). Bear it in mind before assuming persistence
works.

## How to talk to this user

The repo owner is **not a developer** and is new to Claude Code.

- **Explain every change in plain English**, as you go — what you are about to
  do, what you actually did, and what it means for them on the iPad. No
  unexplained jargon: say "saves progress in the browser so it survives closing
  the tab," not "wires up a localStorage-backed reducer."
- Say plainly when something needs a manual tap in the GitHub web UI, and give
  the **exact taps in order**.
- Report honestly. If something did not work, say so.

## Deploying

The live site is published by GitHub Actions from
`.github/workflows/deploy.yml`.

- **Trigger:** every push to `main` (or a manual run from the Actions tab).
- **What it does:** checks out the repo and uploads it to GitHub Pages as-is.
  There is no build step — `index.html` at the repo root becomes the homepage.
- **Live URL:** https://tstephensvw.github.io/Driftless/
- **One-time manual setup** (already done, but for reference): in the repo's
  **Settings → Pages**, *Source* must be set to **GitHub Actions**.
- Work goes on a branch, but **nothing deploys until it lands on `main`.**
- Watch a deploy in the repo's **Actions** tab; it takes about a minute.

## Working agreements

- **Keep `index.html` self-contained.** One file, no build step, no
  dependencies. If a feature seems to need a bundler, find another way.
- **When you finish a ROADMAP item, flip its status to `DONE` in `ROADMAP.md`
  in the same commit as the code.** Never leave the roadmap stale — a
  half-updated roadmap is worse than none. If an item is only partly done, say
  so explicitly in its status rather than marking it `DONE`.
- Commit messages are plain English describing the user-visible change.
- Test on a narrow (iPad/phone) viewport before calling anything finished.
