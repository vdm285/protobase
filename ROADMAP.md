# ProtoBase roadmap (single source of truth) — DRAFT

Updated: 2026-09-29. Owner: Victor. Any AI working on this repo reads `AGENTS.md`, then this file,
and updates both when something changes. Newest state at the top of each section.

> **Everything below is a draft proposed by Claude (senior) from reading the code**
> (`docs/READOUT-2026-09-25.md`). Nothing here is decided until Victor confirms it in an interview.
> Defaults apply only if he says "go with the defaults".

Legend: ✅ done · 🔨 in progress · ⏳ waiting on Victor · 🔜 next · 💤 later · 🚩 flag (known limit)

---

## Where we are, in one line
**A working first draft is live (vdm285.github.io/protobase). Next: interview Victor, then a small
safety patch for the live page, then a rebuild in ListoLista's style.**

---

## The horizon (checkpoints, not calendar dates)
Each checkpoint starts only when Victor is comfortable with the previous one.

| # | Checkpoint | Who uses it | Goal | Status |
|---|---|---|---|---|
| 0 | Safety patch | anyone opening the live page | Pinned + verified scripts, no crash on bad storage, no empty/garbled records. Same look. | ⏳ draft; needs Victor's OK to go live |
| 1 | **Pocket notebook for one** | Victor, on his phone and Mac | Instant, Spanish, offline, installable; create/rename/delete databases; edit/delete records with undo; import/export; tests | 🔜 after the interview |
| 2 | Data bench for future apps | Victor (+ maybe his close circle) | Opt-in: field types, CSV / spreadsheet export, "copy for an AI prompt", maybe an encrypted shared database via the ListoLista kit | 💤 |
| 3 | Public, open source | anyone | Licence, English, README, contribution rules | 💤 |

---

## Checkpoint 0: safety patch (small, keeps today's look)
- 🔜 Pin React 18.3.1, ReactDOM 18.3.1, Babel 7.29.9 with `integrity` hashes (in the readout, A1).
- 🔜 Self-host the Tailwind 3.4.17 Play file (its CDN has no CORS header, so hashes can't be checked).
- 🔜 try/catch around every storage read and write; keep a bad value aside instead of crashing.
- 🔜 Don't save a record whose values are all empty; reserve `id`; warn on blank/duplicate names.
- ⏳ Victor's OK to merge into `main` (that publishes it).

## Checkpoint 1: pocket notebook for one (draft plan)
1. ⏳ Interview (questions at the end of the readout) → confirm the decisions below.
2. 🔜 Spec + protected tests for `src/db.js` (records, databases, import merge, key rules), `jsc`.
3. 🔜 `db.js` implementation (local-junior chore) + senior review.
4. 🔜 Rebuild the page in plain JS (ListoLista's `tools/build.py` pattern), Spanish UI, under 60 KB,
   no third-party code at run time; open straight into the last database's new-record form.
5. 🔜 New / rename / delete database; edit / delete record; Deshacer.
6. 🔜 Import (merge or replace) and "Exportar todo"; ask the browser for persistent storage.
7. 🔜 Offline start + home-screen install (reuse ListoLista's `sw.js`, manifests, icons).
8. 🔜 Old data (today's `protobase_*` keys) migrates automatically.
9. ⏳ Phone test on Victor's Android + iPhone (checklist below), then go live.

## Decisions waiting on Victor (priority order)
| # | Decision | Why it matters | Suggested default (needs Victor) |
|---|---|---|---|
| D1 | **What is ProtoBase for?** Standalone notebook (exports feed AI prompts / new apps) or live data hub other apps read | Shapes everything; a hub needs a shared address, which weakens ListoLista's security | Standalone notebook + export/import files |
| D2 | **Stack:** plain JS like ListoLista, or keep React-style components (Preact + htm, ~5 KB, no build) | Speed, shared code, what you want to learn | Plain JS, same build and tests as ListoLista |
| D3 | Language | Spanish-first rule vs. a developer audience | Spanish UI first; English later |
| D4 | Fix the live page now (checkpoint 0) | Unpinned scripts run on the address ListoLista uses | Yes, as soon as you OK it |
| D5 | **Web address:** stay on `vdm285.github.io/protobase` or give each app its own address | Shared storage and risk with your other apps; changing later resets installs and local data | Stay for now; decide together with ListoLista's checkpoint 2 |
| D6 | Sharing a database with someone | Needs the encrypted relay (64 KB cap today) | No sharing in checkpoint 1; export/import only |
| D7 | Field types (rating, date, number, yes/no, photo) | Cleaner exports; photos would outgrow browser storage | Text only in checkpoint 1; types opt-in at checkpoint 2; no photos |
| D8 | Demo data for new visitors | First impression; today's demo can't be deleted | One empty database "Mi primera base" + an optional example |
| D9 | Licence | Needed before inviting contributions | Unlicense (same as compa-precio and ai-council-workbench) |
| D10 | Name | Brand for the portfolio | Keep "ProtoBase" |

## 🚩 Known limits (flags)
- Data lives only in the browser that typed it; clearing site data on `vdm285.github.io` wipes this
  app and Victor's three other apps there. Export is the only backup (import coming in checkpoint 1).
- iPhone Safari may delete browser storage after seven days of browsing without opening the app;
  an installed home-screen app is safer. Export regularly.
- Not truly offline yet: a page already open keeps working; opening it with no signal likely fails.
- Two open tabs can overwrite each other's new records (fix in checkpoint 1).

## Phone test checklist (checkpoint 1, draft)
- [ ] Open the link on Android and iPhone: the last database's form is ready in the first second.
- [ ] Create a database, add 3 records, edit one, delete one, Deshacer.
- [ ] Airplane mode, close the app completely, reopen from the home-screen icon: it opens, data is there.
- [ ] Exportar todo → file saved; delete a database; Importar the file → it comes back.
- [ ] Anything confusing, slow or annoying → note it here.

## After checkpoint 1 (backlog, not ordered)
- 💤 Field types; search and sort; CSV / "export to Google Sheet".
- 💤 "Copy for an AI prompt" (database + short schema description, ready to paste).
- 💤 Encrypted shared database via ListoLista's `sync.js` + `merge.js` + relay (opt-in).
- 💤 Use ProtoBase to collect data for other apps (e.g. store aisle orders for ListoLista).
- 💤 Accessibility pass (labels, screen-reader names for icon buttons).

## Log (newest first)
- 2026-09-29: working rules from Victor's HQ in `AGENTS.md`; personal details removed from the docs;
  decision column renamed "Suggested default (needs Victor)".
- 2026-09-25: readout of the first draft (`docs/READOUT-2026-09-25.md`), draft `AGENTS.md` and this
  roadmap (merged into `main` and pushed 2026-09-29; readout branch deleted).
- 2026-04-26: first working draft published (single-file React app, `localStorage`).
