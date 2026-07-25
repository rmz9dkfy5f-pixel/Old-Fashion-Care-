# Slice Reviews

Use this file after each completed vertical slice.

---

## 2026-07-24 — Mobile hero reworked to image-right overlay (branch `design/editorial-sage-hero-split-depth`)

**Status:** Implemented, verified, pushed (`aa02b33`), deployed live. Supersedes an intermediate
same-day pass (`cb73699`) after live review.

**Slice summary:**
Mobile had no equivalent to desktop's left-anchored `mask-image` arch (photo dissolving into the
cream page) — it fell back to a plain rounded photo card stacked below the text. Two passes:

1. `cb73699` (superseded): rotated the desktop mask 90° for a full-bleed, photo-below-text stacked
   layout with a top-anchored arch.
2. `aa02b33` (final): reworked into an overlay directly mirroring desktop — photo as a
   right-anchored full-height band, the desktop's left-anchored mask restored and widened, text
   overlaid on the cream left, gradient beginning just right of the headline. User picked the
   "big headline, photo overlaps" tradeoff over "smaller headline, clean split" via
   `AskUserQuestion` with ASCII previews.

**What was preserved:** desktop (`>820px`) hero rules untouched both passes (verified
pixel-unchanged at 1440px); no markup change in either pass (absolute positioning + z-index reused
the existing content-then-media element order).

**What changed:** `css/editorial-sage.css`'s `@media (max-width: 820px)` hero block only, plus one
`:root` token (`--es-ink-rgb`, added by the first pass, retained by the second).

**Verification:** local Playwright screenshots at 360/390/430/768/1440px each pass; live
re-verification via `curl` + a production Playwright screenshot after each deploy, matching local
renders exactly.

**Known tradeoff, flagged not hidden:** on a narrow phone (≤430px) only the care recipient's face
is visible in the photo crop — the band is physically too narrow for both figures at a readable
size. Both figures are visible from tablet width (≥768px) up. Accepted as emotionally apt framing
for the narrow case, not a defect.

---

## 2026-07-15 — Hero variant: split + depth (branch `design/editorial-sage-hero-split-depth`)

**Status:** Implemented + committed to a local branch (not pushed). Baseline branch + `main` untouched.

**Slice summary:**
The lowest-disruption of two new hero variants exploring a more alive homepage hero. Keeps the
existing Editorial Sage two-column hero and all copy — **CSS-only**, no markup change — and adds
depth so the photo stops reading as a flat pasted rectangle:
1. `.es-hero__media::before` — the crisp cream-ellipse arc became a **feathered** radial fill
   (`var(--es-cream)` → transparent), so the photo dissolves organically into the cream text column.
2. `.es-hero__media::after` (new) — a low-opacity ink **vignette** (top/bottom bands + an outer
   radial) that gives the photo dimension while keeping the faces bright.
3. `.es-hero__media` — `isolation: isolate` so the photo / vignette / cream-curve layers stack
   self-contained. Mobile keeps the stacked rounded photo card, now with the same subtle vignette.

**Sibling variant:** `design/editorial-sage-hero-cream-immersive` (full-bleed photo + cream gradient
scrim, copy over the photo). Baseline `design/editorial-sage-elder-friendly` left unchanged.

**What was preserved:** all hero copy + markup, the two-column layout, every other section, and
`css/style.css`. Only the `.es-hero*` CSS block changed.

**Verification:** Playwright (Brave engine) at 1440/1024/768/390 — **0 horizontal overflow, 0
console errors**. Text stays in its own column (no text-over-photo), so no new contrast risk. No
build/test/lint scripts exist in this static repo.

**Out of scope / next:** the user picks a variant; pushing branches to origin and any merge to
`main` are separate explicit decisions.

---

## 2026-07-14 — SEO/perf hygiene pass + SEOKit install (branch `design/editorial-sage-elder-friendly`)

**Status:** Complete; committed + pushed to the branch. `main` untouched.

**Slice summary:**
Turned the user's "can any of the kits add value?" question into a concrete, verified slice:
1. Ran `seo-hygiene-check` and installed **SEOKit** (`/seo` commands + skills + read-only agents +
   `seo/` context); produced `seo/audits/hygiene-2026-07-14.md`.
2. Ran `performance-budget-pass` and applied its two lowest-risk fixes: font-loading via
   preconnect + `<link>` (removed the render-blocking `@import`) across all 6 pages, and
   `loading="lazy"` on the `index.html` founder photo.
3. Evaluated **MKTKit**, judged it a poor fit for a static business site, rolled it back, and kept
   its one useful artifact (`docs/marketing/cta-proposal-2026-07-14.md`).

**What was preserved:** all page copy, `<head>` metadata, JSON-LD, and the contact form markup —
only the font-loading mechanism and one `loading` attribute changed on the live pages.

**Verification:** grep/source inspection across all 6 pages (font links ×3/page, `@import` removed,
lazy attr present). No build/test/lint scripts in this static repo; browser smoke-test not re-run.

**Out of scope / deferred:** Formspree ID (user deferred); og:image dims, sitemap lastmod,
`LocalBusiness` JSON-LD on 3 pages, apple-touch-icon; resizing `care-03..11.jpg`.

---

## 2026-07-13 — Editorial Sage redesign (branch `design/editorial-sage-elder-friendly`)

**Status:** Implemented on an isolated branch — not committed, not merged, `main` untouched.

**Slice summary:**
Executed the vault's `EDITORIAL_SAGE_REDESIGN_EXECUTION_PLAN.md` in its own controlled slices,
pausing after the homepage for user review (approved) before doing the secondary pages:

1. Design system — new `css/editorial-sage.css` (cream/sage/ink/sand tokens, Lora + Source Sans 3,
   light cream sticky header + mobile menu + footer, accessibility baseline), plus
   `docs/design/EDITORIAL_SAGE_DESIGN_SPEC.md` and the reference image.
2. Homepage — organic curved hero split, sage trust band, 5 line-icon service cards, sand process
   strip, Meet Regina split, dignified references statement, rounded contact panel.
3. Secondary pages — `about`, `services`, `how-it-works`, `questions`, `contact` all rebuilt onto
   the shared shell + design language; interior components added (page hero, prose, service detail,
   pull quote, values grid, detailed steps, FAQ, contact form).

**What was preserved:** every page's `<head>` metadata (title/description/canonical/OG/Twitter/
JSON-LD/Plausible/favicon), all verified copy and business facts, the FAQ accordion, and the
Formspree contact form (action + hidden fields + `#form-success`). `css/style.css` left untouched.

**Verification:** Playwright across all 6 pages at 1440/768/390 (0 horizontal overflow, 0 console
errors); mobile menu open/Escape/aria; FAQ click + keyboard; contact-form success with Formspree
action intact; 124-ref local link sweep clean; one `<h1>` per page; SEO metadata intact.

**Deviations from the reference (intentional):** testimonials shown as a dignified no-cards
statement (reference quotes are AI-generated — user decision); Regina uses the real photo, not the
concept's generated portrait.

**Follow-up:** awaiting user review + commit decision; a merge to `main` (changing the live site) is
a separate explicit decision; Formspree endpoint remains the pre-existing placeholder.

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
