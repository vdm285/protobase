# AGENTS.md — ProtoBase (read this first)

Vendor-neutral briefing for any AI agent working on this repo (Claude, ChatGPT/Codex, Gemini,
Grok, or a local model). `CLAUDE.md` just imports it. Whoever changes direction or architecture
updates this file and `ROADMAP.md`.

> **Draft, 2026-09-25.** Written by Claude (senior) from the code and README only, before
> interviewing Victor. The mission, principles as applied here and every default in `ROADMAP.md`
> are to be confirmed with him. Full analysis: `docs/READOUT-2026-09-25.md`.

## Owner and working rules (from Victor's HQ, 2026-09-29)
Owner: Victor (github.com/vdm285). Stage: learning, portfolio and open source; not commercial.
- **Replies:** checklist first (what's done, what needs Victor), then short details, in plain language.
- **Language:** English with AIs; products for Victor's close circle start in Spanish.
- **Who decides:** technical calls (tools, code, tests, free installs, pushes, `main` included) are the
  agent's; tell Victor after. Design, direction and business: discuss first, Victor decides. A suggested
  default may apply after 7 days of silence for technical choices only. Always ask first: force pushes,
  deleting his data, anything posted or sent in his name, logins and passwords.
- **Pushback:** on logic or design flaws and untested claims (not caution or licence caveats).
- **Depth:** build quick, ideas deep ("quick" or "deep" from Victor overrides). When our own test
  contradicts a trusted source, show both sides briefly and test both.
- **Code and words:** lean code with a short "why" note (not golfed); short, plain text for users;
  reusable modules. Naming: say if an industry-standard name exists, else propose a few
  analogy-based names.
- **Test it ourselves:** when a claim is thin or contested, run a small test: pass mark first, plus a
  control that can fail.
- **Brief card:** before any unattended agent run, Victor approves a one-screen card (goal, will / won't
  do, what he will see, budget and stop rule).
- **Checkpoints:** show what works, a live preview and "how to try it on your phone"; numbered steps
  whenever his hands are needed.
- **Locked prototype skeleton:** a `prototype` branch, locked on GitHub, holds only the data the app
  keeps and the rules it enforces.
- **Look:** plain by default (bare wireframe first); polish is an opt-in layer where the visuals are
  the product.
- **To-dos:** one dated list per project, in its `ROADMAP.md`.

Personal context: in Claude's per-project memory, outside git.

## Mission (draft, from the README and the first commit)
**A pocket notebook for structured data.** Capture records on the spot (a restaurant visit, an app
idea) as property/value pairs, keep them in small named databases in the browser, and export them
as JSON for AI prompts or for building future apps. Repo description: "An app that holds databases
for future apps. Good for prototyping/tinkering." The benchmark, as for all Victor's projects, is
**Windows Notepad**: opens instantly, you see what you type, nothing else in the way.

Open question for the interview: standalone notebook whose exports feed other work (current code,
recommended) vs. a live data hub other apps read.

## Design principles (Victor's house rules, apply to every project)
1. **Zero-click start:** open straight into the last database, ready to type a record.
2. **Few clear choices:** secondary actions (export, import, rename, delete) in one labelled menu.
3. **Pay for what you use:** the bare core loads first; optional features load only when used.
4. **No accounts, no login walls** (at most "Sign in with Google", and only if ever essential).
5. **Optionality:** dead-simple default; power features (field types, sharing, CSV) are opt-in.
6. **Zero running cost** for Victor.
7. **Spanish first** for Mexican users (English later), unless the interview decides otherwise.

## Current state (2026-09-25)
- `main` (live at https://vdm285.github.io/protobase/, GitHub Pages): one commit (081e24e,
  2026-04-26). A single `index.html` React 18 app compiled in the browser by Babel, styled by the
  Tailwind Play CDN, all loaded unpinned from unpkg / cdn.tailwindcss.com. Data in `localStorage`
  (`protobase_projects`, `protobase_data`). No build, tests, licence, service worker or manifest.
- Works: add records with free-form properties, cards and JSON views, copy, export current database.
- Main gaps: only two hard-coded databases; no edit/delete; no import; not really offline; ~826 KB
  compressed of third-party scripts per visit; English only.
- **Security flag:** unpinned CDN scripts run on `vdm285.github.io`, the same origin (and storage)
  as ListoLista, compa-precio and ai-council-workbench. Checkpoint 0 in `ROADMAP.md` fixes this first.
- This briefing, the `ROADMAP.md` draft and the readout were merged into `main` (pushed 2026-09-29);
  the readout branch was deleted.

## Relation to ListoLista (~/projects/listolista)
Sibling app with the same principles and the same web address. ListoLista's checkpoint-1 build has
the parts ProtoBase lacks: `tools/build.py` (src/ → one `index.html`), `src/sw.js` + manifests +
icons (offline, install), a safe storage helper, `jsc` tests, and (opt-in, later) end-to-end
encrypted sync (`src/sync.js`, `src/merge.js`, `relay/`). Plan: **copy those pieces**, keep two
separate apps. Do not read or write ListoLista's storage keys from ProtoBase, and never the reverse.

## How to work here
- **Until the rebuild:** `index.html` is edited directly (it is the whole app). Keep every CDN script
  pinned to an exact version with an `integrity` hash, or self-hosted.
- **After the rebuild (checkpoint 1):** edit `src/`, never `index.html`; run `python3 tools/build.py`.
- One branch per piece of work; `main` is the live site: merge only tested work.
- Tests before features: pure logic (records, import/merge, key rules) lives in plain JS files tested
  headless with macOS `jsc` (built in; no Node needed). UI is checked in a browser and on Victor's
  phones. Tests are protected: don't weaken them to make code pass.
- Never show record text with `innerHTML`/`dangerouslySetInnerHTML`; text only.
- Storage keys stay prefixed `protobase_`; every read/write in try/catch; no data loss on a bad value.
- Local juniors (via `~/local-ai/scripts/delegate.sh`, see `~/local-ai/docs/delegation.md`) get
  bounded chores with a pass/fail command (e.g. "make `sh tests/run.sh` pass for `src/db.js`").
- Research lives in `docs/research/` (dated, fact-checked, sources with dates). Quotes under 15 words.
- Keep `ROADMAP.md` as the single source of truth for status and decisions; update it after each step.
