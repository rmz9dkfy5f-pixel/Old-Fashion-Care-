# Progress Note

This file is GitHub-specific tracking and must be kept unconditionally.

Use this as the current active progress note.

For historical notes, copy completed entries into `PROGRESS_NOTES.md`.

---

## Latest — Add Dedicated apple-touch-icon.png (180×180) (2026-07-28)

Closed a long-open, well-documented gap — R-006 in `docs/governance/PROJECT_RISK_REGISTER.md`, an
open `BACKLOG.md` item, and a repeat finding in both SEO hygiene audits under `seo/audits/`. All 6
pages previously fell back to `favicon.svg` for iOS home-screen bookmarks; no apple-touch-icon
existed anywhere in the repo.

**This batch:**
- Rasterized the repo's own `favicon.svg` lettermark (rounded charcoal square, centered coral "O")
  to a 180×180 PNG via a throwaway Playwright render (HTML wrapper, SVG inlined at fixed 180×180,
  background matched to the SVG's `#2A2825` fill for seamless corners) — a mechanical conversion of
  an already-approved mark, not new design work; script not committed, scratch-only.
- Verified output dimensions via `sips -g pixelWidth -g pixelHeight` (180×180 confirmed) and visual
  inspection before promoting into the repo at `images/apple-touch-icon.png` (not repo root, so it
  automatically inherits the existing `netlify.toml` `/images/*` 1-year immutable cache rule — zero
  `netlify.toml` changes).
- Added `<link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon.png">` to all
  6 pages' `<head>`, directly below the existing favicon `<link>` — textually identical on all 6.
  `hero-preview.html` (noindex, nofollow, dev-only) intentionally left unchanged.
- Recorded a short `docs/project/DECISION_LOG.md` entry clarifying that this doesn't contradict the
  2026-07-05 decision to remove the broken apple-touch-icon tag (that decision's rejected
  alternative was about cropping a photo, not rasterizing the site's own vector mark).

**Not done, and not in this batch:** no commit, tag, snapshot, or push — staged only, per this
repo's phase-gate convention. All other open items unchanged (Web3Forms configuration blocked on
client purchase finalization, form-analytics events, HSTS header, `care giver pics/` folder
decision, care-07–11 review, iOS Safari check).

**With this task complete, no new agent-actionable items remain open** — the same standing optional
follow-ups from `BACKLOG.md` remain available whenever the user wants to continue.

---

## Previous — Image Optimization: 4 Live `care-*.jpg` Photos Resized & Deployed (2026-07-23)

The standing user-confirmed next task from the prior two closeouts (2026-07-22 session-end and 
2026-07-23 recovery audit) was image optimization — compress the oversized photos shipped to every
homepage visitor. This batch completed that task for the 4 live/referenced photos; the 5
unreferenced dead files (`care-07`–`11`) remain a separate, already-tracked decision.

**This batch (image compression only, no HTML/CSS/copy changes):**
- Resized the 4 oversized, live `care-*.jpg` files (`care-03`/`04`/`05`/`06`) from native 
  3507–5760px down to ~800×533px (landscape) / 533×800px (portrait) via macOS `sips -Z 800 
  -s formatOptions 82`, matching this repo's established tool precedent.
- Verified all 4 resized files in a scratch directory first — checked dimensions via `sips -g`,
  confirmed EXIF orientation was nil (no unexpected rotation), confirmed file sizes dropped by
  93.9% (6.96 MB → 0.428 MB total).
- Promoted resized files into place via `cp` once all 4 passed inspection.
- Confirmed via `git diff --stat` that only the 4 target image files changed (binary), no other
  changes.
- Updated tracking docs: `BACKLOG.md` (image optimization now Completed), 
  `PROJECT_RISK_REGISTER.md` (R-007 marked Partial, 4 fixed / 5 remain), this file, and others below.

**Why this scope, not all 9:** The original R-007 stated "9 of 14" files, but exploration found this 
actually splits into: 4 files genuinely referenced in `index.html`'s photo grid (downloaded by every 
visitor, so real page-weight impact) and 5 files unreferenced anywhere in the HTML (dead assets, 
hygiene-only impact). User explicitly chose to scope this slice to the 4 live files only, leaving the 
5 dead files for a separate decision already tracked in `BACKLOG.md` ("Review whether care-07–11 
should replace any current grid photo").

**Not done, and not in this batch:** the 5 unreferenced `care-07`–`11` files remain untouched and 
uncompressed, pending that separate decision. All other open items unchanged from prior closeouts 
(Web3Forms configuration blocked on client purchase finalization, form-analytics events, HSTS header, 
apple-touch-icon, `care giver pics/` folder decision, iOS Safari check).

**With this task complete, no new agent-actionable items remain open** — the standing user-confirmed 
next choice is to either tackle one of the optional follow-ups from `BACKLOG.md` or close out this 
session. See `BACKLOG.md` "Build Later" for the full list of user-owned or optional items.

---

## Current Progress

Date:

```text
2026-07-28
```

Phase:

```text
Phase 2 — apple-touch-icon.png slice implemented and verified. Staged, not committed.
```

Current slice:

```text
Add dedicated 180×180 apple-touch-icon.png and reference it on all 6 pages — implemented, verified,
awaiting explicit user go-ahead to commit/push.
```

Completed:

- [x] Rasterized favicon.svg to images/apple-touch-icon.png (180×180) via a throwaway Playwright script
- [x] Added <link rel="apple-touch-icon"> to all 6 production pages
- [x] Verified dimensions, visual correctness, and tag coverage
- [x] Updated BACKLOG.md, PROJECT_RISK_REGISTER.md (R-006 → R-C09), DECISION_LOG.md, and the rest
      of this batch's required tracking docs

In progress:

- [ ] None

Blocked:

- [ ] Web3Forms configuration (`contact.html`) — blocked on the client finalizing the purchase, not
      agent-actionable until then
- [ ] `oldfashioncare.com` hosting mismatch — user-confirmed expected/known, not currently blocking
      anything; no action needed unless raised again

Next action:

> No agent-actionable next action remains open right now. Commit/push is staged and ready whenever
> the user requests it.

Checks run:

```bash
sips -g pixelWidth -g pixelHeight images/apple-touch-icon.png   # confirmed 180x180
grep -l 'rel="apple-touch-icon"' *.html                          # confirmed exactly 6 files
```

Commit status:

```text
Not yet committed — file staged, tracking docs updated; user will request commit as a separate step.
```

Approval status:

```text
User asked directly to fix the apple-touch-icon gap (approved during Plan Mode via ExitPlanMode);
implementation followed the approved plan exactly. No code was changed beyond what the plan
specified.
```
