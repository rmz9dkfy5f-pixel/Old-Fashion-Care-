# Slice Reviews

Use this file after each completed vertical slice.

---

## 2026-07-28 — Add Dedicated apple-touch-icon.png (180×180)

**Status:** Implemented — verified, staged for commit/push (user will request commit separately)

**Slice summary:**
Closed a long-open, well-documented gap: no dedicated apple-touch-icon existed anywhere in the repo
(all 6 pages fell back to `favicon.svg`), tracked as R-006 in `docs/governance/PROJECT_RISK_REGISTER.md`,
an open `BACKLOG.md` item, and a repeat finding in two SEO hygiene audits
(`seo/audits/hygiene-2026-07-14.md`, `seo/audits/hygiene-2026-07-19.md`). Rasterized the repo's own
`favicon.svg` lettermark to a 180×180 PNG via a throwaway Playwright render — a mechanical
conversion, not new design work — and added the `<link rel="apple-touch-icon">` tag to all 6 pages.

**Scope decision:** Rasterizing the site's own already-approved vector mark does not conflict with
the 2026-07-05 decision to remove a broken apple-touch-icon tag rather than create a placeholder —
that decision's rejected alternative concerned cropping a photo, not rendering the SVG mark (see the
2026-07-28 `docs/project/DECISION_LOG.md` entry). `hero-preview.html` (noindex, nofollow, dev-only)
intentionally left unchanged — out of scope, not one of the 6 production pages.

**Files changed:** New `images/apple-touch-icon.png` (binary, 180×180). Modified: `index.html`,
`about.html`, `services.html`, `how-it-works.html`, `questions.html`, `contact.html` — one identical
line added to each `<head>`, directly below the existing `<link rel="icon">` tag. No CSS/JS changes.
`netlify.toml` untouched (placement under `images/` inherits its existing cache rule automatically;
CSP `img-src 'self' data:;` already permits a same-origin image).

**Verification:** `sips -g pixelWidth -g pixelHeight` confirmed the promoted PNG is exactly 180×180.
Visual inspection confirmed a solid rounded charcoal square with a centered coral "O", no
transparency/background artifacts. Grep sweep confirmed exactly 6 files contain
`rel="apple-touch-icon"`, textually identical, and `hero-preview.html` contains zero. No automated
test suite exists for this static site; verification was manual/scripted.

**Risks accepted:** None new. This closes R-006 with no other site behavior affected.

**Tracking files updated:** BACKLOG.md (moved to Completed), PROJECT_RISK_REGISTER.md (R-006 closed
as R-C09), docs/project/DECISION_LOG.md (new clarifying entry), PROGRESS_NOTE.md, PROGRESS_NOTES.md,
docs/project/COMMIT_NOTES.md, docs/project/CHANGELOG.md, docs/project/STATUS.md, this file.

**Next task decision:** No new urgent items. Remaining open items are user-owned or optional
follow-ups tracked in `BACKLOG.md`/`PROJECT_RISK_REGISTER.md` (Web3Forms configuration, form-
analytics events, HSTS header, `care giver pics/` folder decision, care-07–11 review, iOS Safari
check).

---

## 2026-07-23 — Image Optimization: Compress the 4 Live `care-*.jpg` Photos

**Status:** Implemented — verified, staged for commit/push (user will request commit separately)

**Slice summary:**
Compressed the 4 oversized, live-referenced photos that ship to every homepage visitor. R-007
(performance risk, confirmed/quantified in 2026-07-19 production-readiness audit) has been
**partially closed**: the 4 live files (the real page-weight impact, 6.7 MB → 0.428 MB) are now
resized and deployed; the 5 unreferenced dead files (care-07–11, a separate hygiene issue) were
deliberately left untouched pending a user decision already tracked in BACKLOG.md ("Review whether
care-07–11 should replace any current grid photo"). This slice strictly compressed; no HTML/CSS
changes, no markup edits, no new dependencies or tooling (used macOS `sips`, the repo's established
precedent for prior image work).

**Scope decision:** User confirmed this batch should target only the 4 live/referenced files
(`care-03`/`04`/`05`/`06` in the homepage `.photos-grid`), not the 5 dead files or the hero/founder
photos (already correctly sized). User confirmed this batch should not add `width`/`height`
attributes (a separate, real CLS-risk finding, but out of scope for the tracked R-007 backlog item).

**Files changed:** Binary only — exactly 4 image files (`images/care-03.jpg`, `images/care-04.jpg`,
`images/care-05.jpg`, `images/care-06.jpg`). Resized via `sips -Z 800 -s format jpeg -s
formatOptions 82` (max-edge scale-to-fit, JPEG quality 82), preserving aspect ratios. No HTML, CSS,
JavaScript, or other markup modified. `git diff --stat` confirmed scope: exactly 4 image binary
changes, nothing else.

**Verification:** Dimension/orientation integrity checks via `sips -g pixelWidth -g pixelHeight -g
orientation` on both original and resized files (confirmed portrait 533×800, landscape 800×533, all
EXIF orientations nil). File size summary: care-03 1.2 MB → 102 KB, care-04 989 KB → 93 KB, care-05
2.1 MB → 106 KB, care-06 2.4 MB → 117 KB (combined: 6.96 MB → 0.428 MB, 93.9% reduction). Resized
to scratch location first (`/tmp/ofc-image-opt/`), inspected all 4 files, promoted into place once
all passed — never a one-shot in-place operation. No automated test suite exists for this static
site; Playwright visual verification was not available in this environment, but inspection covered
dimension/orientation integrity and the resized files remain valid JPEG images with expected aspect
ratios.

**Risks accepted:** Quality artifacts at JPEG q82 (appropriate for small decorative grid images, not
a large hero). Scope bounded via explicit user decisions and `git diff --stat` guardrail. Original
hi-res files remain recoverable from the tracked-in-git `care giver pics/` folder (confirmed
byte-identical copies at original filenames).

**Tracking files updated:** BACKLOG.md (image optimization moved to Completed, separate decision on
care-07–11 untouched), PROJECT_RISK_REGISTER.md (R-007 marked Partial, 4 fixed / 5 remain),
PROGRESS_NOTE.md, PROGRESS_NOTES.md, COMMIT_NOTES.md, CHANGELOG.md, SLICE_REVIEWS.md (this file).

**Next task decision:** No new urgent items. With this task complete, the standing next-action options
are the optional follow-ups tracked in BACKLOG.md (Web3Forms configuration, form-analytics events,
HSTS header, apple-touch-icon, `care giver pics/` folder decision, iOS Safari check) or session
closeout. User will decide which (if any) to pick up next.

---

## 2026-07-18 — Close the Push-Workflow Tracking-File Gap (repo + AntBrainOS vault)

**Status:** Implemented — committed and pushed this slice

**Slice summary:**
A follow-up audit found `PROGRESS_NOTE.md` stale after the founder-photo push (missing it entirely).
Root-caused to the push prompt itself telling the agent `PROGRESS_NOTE.md`/`PROGRESS_NOTES.md` were
alternatives, letting a required file silently fall out of a push. This slice:
1. Corrected `PROGRESS_NOTE.md` content to actual current state.
2. Fixed `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`: moved
   `PROGRESS_NOTE.md` into the mandatory every-push list, removed the ambiguous guidance, added a
   `git diff --stat` cross-check before commit (Section 6).
3. Added Section 2a: the linked AntBrainOS vault project folder must be updated on every push, not
   only at session end.
4. Confirmed and fixed the identical bug in the AntBrainOS vault's own canonical copy of this
   prompt (no auto-sync exists between a repo's local copy and the vault template).
5. Added an independent Step 18a to the vault's `CLAUDE_CODE_SESSION_END.md`: cross-check a repo's
   required tracking files against `git diff --stat` before recording a closeout as clean/current.
6. Recorded the decision in this repo's `docs/project/DECISION_LOG.md` and the vault's own
   `08_DECISIONS/DECISION_LOG.md` (#36).

**Files changed:**
- This repo: `PROGRESS_NOTE.md`, `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`,
  `docs/project/{STATUS,CHANGELOG,DECISION_LOG,COMMIT_NOTES}.md`, `PROGRESS_NOTES.md`,
  `LESSONS_LEARNED.md`, `SLICE_REVIEWS.md` (this entry) — no site code
- AntBrainOS vault: `09_PROMPTS/Claude_Code_Prompts/04_Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`,
  `05_SOPS/Claude_Code/CLAUDE_CODE_SESSION_END.md`, `08_DECISIONS/DECISION_LOG.md`,
  `00_START_HERE/AGENT_HANDOFF.md`, `03_PROJECTS/Active/Old_Fashion_Care/*` (this project's own
  vault folder, per the new Section 2a)

**Validation:**
- No build/lint/test scripts exist for this static site (documented, not a gap).
- Manual review confirmed the push prompt's Sections 2/2a/4/6 read consistently with no remaining
  "prefer the other file" language.
- `git diff --stat` cross-checked against this repo's own required-tracking-file list before commit.

---

## 2026-07-07 — Complete the V3.4 migration (doc consolidation)

**Status:** Implemented — not yet committed

**Slice summary:**
The V3.4 baseline + 18-skill suite (installed 2026-07-05, merged to `main` 2026-07-06) only added
scaffolding — it never reconciled with this project's real v3.3 root docs, and the installed V3.4
skills hardcode `docs/governance/`/`docs/project/` paths, so they'd only ever see empty generic
templates without this work. This slice:
1. Moved 8 root docs with real content into `docs/project/` (STATUS, CHANGELOG, COMMIT_NOTES,
   CONTEXT, DECISION_LOG, PROJECT_BRIEF, RELEASE_NOTES, ROADMAP), overwriting empty stubs; authored
   real content for the new `docs/project/ARCHITECTURE.md`.
2. Merged 6 governance file pairs into `docs/governance/` (root had real content, V3.4 side was
   generic): CHANGE_CONTROL, ROLLBACK_PLAN, REPO_HEALTH_CHECK, DONE_CRITERIA (all straightforward
   merges), PROJECT_RISK_REGISTER (authored real project-specific risks — neither side had any),
   PHASE_GATES (two genuinely different gate taxonomies, reconciled into one). Deleted all 6 root
   duplicates.
3. Rewrote `AGENTS.md`/`CLAUDE.md`: kept v3.3-specific content (7-sub-agent workflow, phase-gate
   language, tracking-file list updated for moved paths), added V3.4's genuinely new pieces
   (Output Standard report format, no-auto-commit line, skill references).
4. Resolved `ai/agents/AGENT_REVIEW_GATES.md` (kept the real 7-agent table, added the candidate's
   new Review Gate D) and added a roster-authority note to `ai/agents/SUBAGENT_ROLES.md`.
5. Deleted all 3 `.v34_migration_review/*.v34-candidate` files and the now-empty directory.
6. Updated `README.md`, `START_HERE.md`, and 4 live files that still referenced old root paths.

**Files changed:**
- 17 files deleted (root docs consolidated), 25 files modified — see `docs/project/COMMIT_NOTES.md`
  for the full list; no HTML/CSS/JS site files touched

**Validation:**
- `git status --short` matched the planned scope exactly (verified count and file list)
- `python3 project-starter-kit-v3.4/scripts/v34_validate.py --target .` → PASS
- Local `href`/`src` resolution sweep across all 6 pages → clean (no regression)
- `grep` swept for dangling references to the 6 deleted root files; fixed 4 live files still
  pointing at old paths; remaining hits confirmed to be frozen `.starter-kit/archive/` history or
  generic `Prompts/*.md` templates, correctly left untouched

**Open items:**
- Commit/push pending user confirmation
- Hi-res hero photo swap and dedicated apple-touch-icon.png remain open (pre-existing, unrelated)

---

## 2026-07-05 — SEO audit across all 6 pages

**Status:** Implemented — not yet committed

**Slice summary:**
SEO audit of `index.html`, `about.html`, `services.html`, `how-it-works.html`, `questions.html`,
`contact.html` per the standing next-action from prior handoffs:
1. Metadata inventory (titles, descriptions, canonicals, OG tags, Twitter Card tags, sitemap.xml,
   robots.txt) — all already correct across all 6 pages; no changes made there.
2. Found `og:image`/`twitter:image` referenced a nonexistent `images/og-default.png` on all 6
   pages. Repointed to the existing `images/hero-ai.jpg`; corrected declared dimensions to the
   file's actual 1100×934 (was falsely declared 1200×630).
3. Found `apple-touch-icon` link tag referenced a nonexistent `images/apple-touch-icon.png` on all
   6 pages. Removed the tag; `favicon.svg` is the fallback icon.

**Files changed:**
- `index.html`, `about.html`, `services.html`, `how-it-works.html`, `questions.html`,
  `contact.html` — `og:image`, `og:image:width/height`, `twitter:image` values; apple-touch-icon
  link removed
- `STATUS.md`, `COMMIT_NOTES.md`, `PROGRESS_NOTE.md`, `PROGRESS_NOTES.md`, `CHANGELOG.md` —
  tracking (unconditional)

**Validation:**
- `grep -rn "og-default\|apple-touch-icon" *.html` → no matches after fix
- Scripted extraction of every local `href`/`src` across the 6 pages, checked against the
  filesystem → all resolve to existing files

**Open items:**
- Commit/tag/snapshot/push pending user confirmation
- Hi-res original hero photo still needed for `images/hero-ai.jpg`
- Consider a dedicated 180×180 `apple-touch-icon.png` instead of the `favicon.svg` fallback
- Next major slice: migration branch → main merge review

---

## 2026-06-30 — Hero smoother gradient + headline fix + sub barely-covers-hand

**Status:** Complete — commit b195aba — tag v1.5.0__hero-smoother-gradient-headline-fix__commit-b195aba

**Slice summary:**
Three fixes to the homepage hero following the b-period-anchor gradient iteration:
1. Orange headline regression fixed: "We make that possible." back to 1 line (`max-width: 565→620px`).
2. Gradient smoothed: 10-stop uniform ~0.02/% curve; visible fade anchored at 'h' in "where" (22%); seam at 46% covered at 0.26 opacity.
3. Sub-paragraph narrowed: `max-width: 648→580px`, `font-size: 0.97→0.94rem` — 4 lines hold, right edges barely reach caregiver's wrist/hand, shoulder stays visible.

**Files changed:**
- `css/style.css` — `.hero__headline`, `.hero::after`, `.hero__sub`
- `COMMIT_NOTES.md`, `PROGRESS_NOTE.md` — tracking (unconditional)

**Validation:**
- Headless Chromium 1554×900 desktop: 1-line orange headline, 4-line sub, smooth gradient, hand barely covered
- Headless Chromium 390×820 mobile: clean, no regression

**Open items:**
- User visual review of live hero pending
- Hi-res original photo still needed for `images/hero-ai.jpg`
- Next major slice: SEO audit or new hero adjustment per user direction

---

## 2026-06-29 — Hero copy↔people micro-alignment

**Status:** Complete — commit a2e7c9a

**Slice summary:**
Fine-grained visual tuning of the homepage hero. Approved via plan-mode gate. Four adjustments:
1. Sub-paragraph → 4 lines (was 5): font-size reduced, max-width widened.
2. Copy column nudged up: orange "possible." period bottom now aligns with older woman's earlobe bottom (~8px).
3. Gradient pulled left: more photo revealed, no seam, text readable.
4. Wider sub: lower lines subtly overlap caregiver's fingertips; hand/shoulder/faces preserved.

**Decision captured:** Gradient direction ambiguity resolved — user chose "pull dark toward left edge / reveal more photo."

**Files changed:**
- `css/style.css` — hero rules only (`.hero > .container`, `.hero__sub`, `.hero::after`)
- `COMMIT_NOTES.md` — iteration record appended
- `PROGRESS_NOTE.md` — current state updated

**Validation:**
- Headless Chromium at 1554×900 (desktop) and 390×820 (mobile)
- sips zoom crops: period↔earlobe ~8px, hand overlap subtle, no seam, mobile clean

**Open items:**
- User visual review of live hero pending
- Next major slice: SEO audit across all 6 HTML pages

---

## 2026-06-15 — Photo Social Proof + Starter Kit v3.3 Migration

**Status:** Complete

**Slice summary:**
Two slices delivered together:
1. Starter Kit v3.3 migration — added project control structure without touching existing site files
2. Photo social proof — caregiver photo grid on homepage + Regina founder photo on about page

**Files changed:**
- `about.html` — founder photo block replaced (initials → real img)
- `index.html` — photo grid section inserted between Testimonials and How It Works
- `css/style.css` — .photos-grid responsive layout, img.founder-photo rules added
- `images/` — new folder with 12 photos (clean URL-safe names)
- `.claude/agents/` — 7 sub-agents installed
- `ai/`, `docs/`, `plans/` — new directories with templates
- All root project control .md files — new additions (23 files)
- `.starter-kit/` — migration workspace

**Validation:**
- Static site — no build/lint/test scripts
- Manual browser check: all pages load, photos display, responsive layout confirmed
- No existing site files were modified (CHANGELOG conflict quarantined)

**Success criteria met:**
- [x] Photo grid appears on homepage with 6 caregiver photos
- [x] Regina's photo visible on about page (rectangular, not circular)
- [x] No existing HTML/CSS/JS files broken
- [x] Rollback path documented (git checkout main)

**Notes:**
- Photos care-07 through care-11 available in images/ but not yet used — reserve for future grid swap
- care giver pics/ (original folder) retained; images/ has clean copies
- Migration branch not yet merged to main — pending SEO audit
