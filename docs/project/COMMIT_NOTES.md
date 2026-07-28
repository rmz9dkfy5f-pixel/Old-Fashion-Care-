# Commit Notes

This file is GitHub-specific tracking and must be kept unconditionally.

Use it to prepare commits before they are made.

---

## Summary

Add a dedicated 180×180 `apple-touch-icon.png` (rasterized from `favicon.svg`) and reference it on all 6 pages (branch `main`)

## Description (pending commit)

- **What changed:** new `images/apple-touch-icon.png` (180×180, binary), rasterized from the
  existing `favicon.svg` lettermark via a throwaway Playwright render (HTML wrapper, SVG inlined at
  fixed 180×180, background matched to the SVG's own `#2A2825` fill for seamless corners; script not
  committed, scratch-only). Added `<link rel="apple-touch-icon" sizes="180x180"
  href="/images/apple-touch-icon.png">` to `index.html`, `about.html`, `services.html`,
  `how-it-works.html`, `questions.html`, `contact.html` — one identical line per page, directly below
  the existing `<link rel="icon">` favicon tag. No other files modified; `netlify.toml` untouched.
- **Why:** closes `docs/governance/PROJECT_RISK_REGISTER.md` R-006 and the matching `BACKLOG.md`
  item — a long-open, repeatedly-flagged gap (both SEO hygiene audits under `seo/audits/` named it).
  All 6 pages previously fell back to `favicon.svg` for iOS home-screen bookmarks.
- **What was verified:** `sips -g pixelWidth -g pixelHeight` confirmed the promoted PNG is exactly
  180×180. Visual inspection confirmed a solid rounded charcoal square with centered coral "O", no
  transparency/background artifacts. Grep sweep confirmed exactly 6 files contain the new tag,
  textually identical, and `hero-preview.html` (dev-only, noindex) contains zero. Confirmed
  `netlify.toml`'s CSP (`img-src 'self' data:;`) already permits a same-origin image — no config
  change needed. No automated test suite exists for this static site; verification was
  manual/scripted.
- **Remaining risk/follow-up:** none new. All other open items unchanged (Web3Forms configuration
  blocked on client purchase, form-analytics events, HSTS header, `care giver pics/` folder decision,
  care-07–11 review, iOS Safari check).

---

## Previous Commit

Compress the 4 live, referenced `care-*.jpg` photos via `sips` — 93.9% size reduction (6.96 MB → 0.428 MB) with no markup changes (branch `main`)

## Description (pending commit)

- **What changed:** exactly 4 image files, binary only: `images/care-03.jpg`, `images/care-04.jpg`,
  `images/care-05.jpg`, `images/care-06.jpg`. Resized from native 3507–5760px to ~800×533px 
  (landscape) / 533×800px (portrait) via `sips -Z 800 -s format jpeg -s formatOptions 82`, 
  preserving aspect ratios and JPEG quality q82. No HTML/CSS changes, no markup edits, same 
  filenames (transparent to all references in `index.html`). No other files modified.
- **Why:** R-007 (performance risk, confirmed/quantified in 2026-07-19 production-readiness audit). 
  These 4 are the only `care-*.jpg` files actually referenced on the live site and downloaded by 
  every visitor — the real page-weight impact. The 5 unreferenced dead files (`care-07`–`11`) were 
  explicitly left out per user scope decision, pending a separate `BACKLOG.md` decision on whether 
  to use them as photo-grid replacements.
- **What was verified:** (1) Pre-resize: confirmed `git status --short` clean, recorded original 
  sizes/dimensions. (2) Resize-to-scratch: ran `sips` on all 4, stored in `/tmp/ofc-image-opt/`. 
  (3) Scratch inspection: confirmed dimensions (533×800 for portrait files, 800×533 for landscape), 
  confirmed EXIF orientation was nil (no unexpected rotation), confirmed file sizes dropped 93.9% 
  overall (6.96 MB → 0.428 MB, and individually: care-03 1.2 MB → 102 KB, care-04 989 KB → 93 KB, 
  care-05 2.1 MB → 106 KB, care-06 2.4 MB → 117 KB). (4) Promotion: `cp` into place once all 4 
  passed inspection. (5) Scope confirmation: `git diff --stat` showed exactly the 4 image files, 
  binary diffs, no other changes. No automated test suite exists for this static site; visual 
  verification would normally be done via Playwright (not available in this environment) — 
  inspection was limited to dimension/orientation checks.
- **Remaining risk/follow-up:** the 5 unreferenced files remain uncompressed pending a separate user 
  decision (tracked separately in `BACKLOG.md`). All other open items unchanged (Web3Forms 
  configuration blocked on client purchase, form-analytics events, HSTS header, apple-touch-icon, 
  `care giver pics/` folder decision, iOS Safari check).

---

## Previous Commit

Record the contact-form vendor decision (Web3Forms, not Formspree) in tracking docs only (branch `main`)

## Description

- **What changed:** no code changed. Recorded a client decision — the contact form's eventual
  delivery vendor is Web3Forms, not Formspree — across tracking docs: new entry in
  `docs/project/DECISION_LOG.md` (includes implementation notes for the future swap), reworded
  `docs/governance/PROJECT_RISK_REGISTER.md` R-008/R-009, `BACKLOG.md`'s matching item, and
  `docs/governance/RELEASE_GATE.md`'s Functionality checklist + Release Decision notes. This file,
  `PROGRESS_NOTE.md`, and `STATUS.md` also updated to record the batch.
- **Why:** user directly instructed the vendor decision and asked for it to be documented
  ("document that, and we can pretty much close out").
- **What was verified:** `grep -rln "Formspree"` re-run after edits to confirm every tracking-doc
  reference either now names Web3Forms or is an untouched historical record (commit notes/changelog
  entries describing already-shipped past work, and the still-placeholder `contact.html`/`js/main.js`
  themselves, which are correctly out of scope for a docs-only decision).
- **Remaining risk/follow-up:** the actual Web3Forms swap (`contact.html`'s form `action` + a real
  `access_key` field) is not implemented and is not agent-actionable — blocked on the client
  finalizing the purchase. All other open items unchanged from the prior batch (image optimization,
  HSTS header, apple-touch-icon, analytics events, `care giver pics/` folder decision, iOS Safari
  check).

---

## Summary

Backfill 3 stale commit-hash placeholders and refresh the AntBrainOS vault baton entry for this project (branch `main`)

## Description

- **What changed:** (1) Backfilled 3 "hash once committed" placeholders with the real short
  hashes now that each commit exists: `docs/project/STATUS.md`'s v2.9.0 entry (`10ee3d0`),
  `docs/project/STATUS.md`'s previously-missed v2.6.0 founder-photo entry (`dd88bf4`), and
  `PROGRESS_NOTE.md`'s "Commit status" section (`10ee3d0`) — committed as `f75de99`. (2) Prepended
  a new dated entry to the AntBrainOS vault's `00_START_HERE/AGENT_HANDOFF.md`, superseding
  (without rewriting) a 3-day-stale entry that still named an already-completed audit re-run as
  the open next task. (3) Refreshed a matching stale pointer in this project's own vault
  `HANDOFF_TO_CLAUDE.md`. (4) Updated this repo's own tracking docs (this file, `STATUS.md`,
  `PROGRESS_NOTES.md`, `PROGRESS_NOTE.md`, `CHANGELOG.md`) to record the batch.
- **Why:** a `REPO_SESSION_START_RECOVERY_AUDIT.md` run resolved to `PASS WITH CONDITIONS`,
  naming both the unfilled hash placeholders and the stale vault baton entry as explicit
  conditions to close; the user then asked to fix them.
- **What was verified:** `git diff --stat` before the trailing commit confirmed only the 2
  intended files changed; `git show --stat` on `f75de99` confirmed it was docs-only; a fresh
  AntBrainOS vault snapshot was taken and verified (987/987 files, 0 file-list diff) before any
  vault edit, per that vault's own snapshot SOP; re-read of the new `AGENT_HANDOFF.md` entry
  confirmed the superseded entry was left untouched.
- **Remaining risk/follow-up:** none new from this batch. The optional/user-owned items from the
  prior push (real Formspree ID, form-analytics events, image optimization, HSTS header,
  apple-touch-icon, the `care giver pics/` folder decision, iOS Safari check) remain open,
  tracked in `BACKLOG.md` and `docs/governance/PROJECT_RISK_REGISTER.md` (R-007–R-013).

---

## Summary

Fix contact form, contrast, touch target, and content issues surfaced by an in-progress production-readiness audit (branch `main`)

## Description

- **What changed:**
  1. `js/main.js` — the contact form's submit handler unconditionally called `preventDefault()`
     and showed the success state on every submit, regardless of whether anything was actually
     sent (it never called Formspree at all). Rewrote to a real `fetch()` POST that only shows
     success on an actual `2xx` response, with a matching honest error state.
  2. `contact.html` — added the `#form-error` element (reuses the existing `.form-success` visual
     pattern) that the new JS error path shows.
  3. `css/style.css` — migrated `.nav__cta`, `.section--coral`, and `.phone-strip` from `--coral`
     (~3.87:1 on white, fails WCAG AA) to the already-established `--coral-fill` (~4.66:1) token;
     enlarged the mobile nav hamburger's touch target from 38×31px to 44×44px; added an
     open/closed "X" icon transform driven by the existing `aria-expanded` attribute.
  4. `js/main.js` — added an Escape-key handler that closes the mobile nav and returns focus to
     the hamburger button.
  5. `index.html` — replaced the homepage testimonials section's literal, unfilled
     bracket-placeholder text (3 entries reading `"[Testimonial to be added — contact a current
     client for a quote before launch.]"`) with 3 honest, first-party trust statements; updated
     the section's heading, overline, and `aria-label` to match.
- **Why:** all 5 surfaced from an in-progress production-readiness audit (9 of 13 planned
  skill-runs from the installed-but-never-executed 18/20-skill suite; see
  `docs/project/STATUS.md` for the audit's own status). Each was small, scoped, and explicitly
  approved by the user before implementation — fixes 1-2 and 4 via an inline "let's fix all three"
  after the Accessibility Pass, fix 3's touch-target/icon piece via an "approved" after the
  Responsive pass, fix 5 via a short Plan Mode cycle after the Content-Conversion-Readiness pass.
- **Verified:** no build/lint/test scripts exist (static site). Verified via local Playwright:
  contrast via `getComputedStyle().backgroundColor` resolving to the `--coral-fill` RGB value
  (`rgb(194, 81, 43)`) on all three selectors (catching and fixing a real cascade collision on
  `.phone-strip` along the way — its own later-declared `background: var(--coral)` was silently
  overriding the `.section--coral` fix); the contact form via mocked 200/500 `fetch` responses
  (success/error states both render correctly, button re-enables on error) plus one run against
  the real placeholder Formspree URL (confirms it now fails honestly instead of faking success);
  Escape-key and focus-return behavior via `document.activeElement`; the touch target via
  `getBoundingClientRect()` (44×44px exactly) and the icon's CSS transform values; the
  testimonials section via `grep` (zero remaining placeholder strings) and screenshots at
  390px/1440px (after scrolling the section into view to trigger its `.reveal` animation before
  capturing — an initial screenshot attempt captured the pre-animation `opacity:0` state, caught
  and corrected). Zero console errors and zero horizontal overflow confirmed across every check,
  at every viewport tested (360-1920px).
- **Remaining risk/follow-up:** the Formspree ID in `contact.html` is still the placeholder
  `REPLACE_WITH_FORMSPREE_ID` — the form now fails/succeeds honestly but won't actually deliver a
  message until a real ID is configured (needs the user's own Formspree account, not
  agent-actionable). WebKit/Firefox/iOS Safari were not tested — only a Chromium binary is
  available in this environment. The production-readiness audit itself is **not complete**: 4 of
  13 planned skill-runs remain, and neither the two consolidated report files nor the
  governance/tracking-doc reconciliation has been written yet — this commit covers only the code
  fixes found along the way. `oldfashioncare.com`'s hosting mismatch was re-confirmed unchanged;
  the user has stated this is expected/known right now, not a blocker.

---

## Summary

Fix a real staleness gap in the repo's own push workflow (branch `main`)

## Description

- **What changed:** `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` and
  `PROGRESS_NOTE.md` only — no site code. The push prompt previously told the agent to treat
  `PROGRESS_NOTE.md` (singular) and `PROGRESS_NOTES.md` (plural) as alternatives ("prefer
  `PROGRESS_NOTES.md`"), so the 2026-07-18 founder-photo push updated the plural historical log but
  silently skipped the singular current-state file. Moved `PROGRESS_NOTE.md` into the mandatory
  "every push" section, removed the ambiguous guidance, and added a `git diff --stat` cross-check
  (Section 6) so a required tracking file can't silently fall out of a push again. Added Section 2a:
  the linked AntBrainOS vault project folder (`03_PROJECTS/Active/Old_Fashion_Care/`) must now be
  updated on every push, not only at session end.
- **Why:** discovered via `REPO_SESSION_START_RECOVERY_AUDIT.md` that `PROGRESS_NOTE.md` was stale;
  root-caused to the push prompt's own ambiguous instruction rather than agent oversight alone. The
  user then asked for "no gaps... system to system, or session to session," which extended the fix
  to the vault's canonical copy of this same prompt (confirmed to carry the byte-identical bug — no
  auto-sync exists between a repo's local prompt copy and the vault template) and to
  `CLAUDE_CODE_SESSION_END.md` (new independent Step 18a: cross-check a repo's required tracking
  files against `git diff --stat` before recording a closeout as clean, rather than trusting the
  push prompt alone).
- **Verified:** no build/lint/test scripts exist for this static site. Verification is a manual
  review: push prompt sections read consistently with no remaining "prefer the other file"
  language; `PROGRESS_NOTE.md` content matches actual current repo state; this commit's own `git
  diff --stat` includes every file this repo's required-tracking list names.
- **Scope:** docs/process only — `Prompts/`, `PROGRESS_NOTE.md`, and the tracking docs listed in
  this push's `docs/project/STATUS.md` entry. No `.html`/`css`/`js` touched; live site unaffected.

---

## Summary

Fix distorted founder photo on `about.html` (branch `main`)

## Description

- **What changed:** `css/style.css` only. Added `aspect-ratio: auto;` to `img.founder-photo`.
  A leftover `.founder-photo` placeholder-box rule (predating the real photo) was still
  forcing `aspect-ratio: 3/4` (desktop) / `16/9` (mobile); with no `object-fit` override, the
  browser's default `fill` behavior stretched the real 354×514 portrait to fit those
  mismatched boxes — most visibly on mobile, where a portrait photo was squashed into a
  landscape 16:9 box.
- **Why:** user reported the photo looked compressed loading `about.html` on their phone at
  `old-fashion-care.craftandconscious.com`.
- **Verified:** Playwright screenshots at 390/768/1440px, before (via `git stash`) and after —
  bounding-box ratio went from 1.78 (mobile)/0.75 (desktop) to 0.689 at every width, matching
  the image's real intrinsic ratio exactly. 0 console errors throughout. Re-verified against
  the live URL after deploy (see below).
- **Deployment finding (pre-existing, not caused by this change):** `oldfashioncare.com` — the
  domain this repo's canonical URLs/sitemap/`REPOSITORY_HANDOFF_CONFIG.md` name as Netlify
  production — currently serves an unrelated third-party "Jottful" site, not this repo. The
  user-facing URL that actually runs this code, `old-fashion-care.craftandconscious.com`, is a
  separate VPS mirror manually synced via `git archive` + rsync; it was frozen at commit
  `fd7543c` (2026-07-12) until this push. Flagged in `docs/project/STATUS.md` for a hosting/DNS
  decision — out of scope for this fix.
- **Scope:** `css/style.css` + tracking docs only; no markup change, no other branch touched.

---

## Summary

Install AntBrainOS kit tooling — SEOKit, EngKit, TradeKit, handoff-repository (`main`, dev-tooling
only, live site unchanged)

## Description

- **What changed:** dev-tooling only. Added **SEOKit** (`.claude/commands/seo/` ×19,
  `.claude/skills/seo-*.md` ×4, `.claude/agents/{seo-auditor,serp-researcher}.md`, `seo/` context
  reused from the Editorial Sage branch), **EngKit** (`.claude/skills/eng/`, `/eng` + 26
  subcommands), **TradeKit** (`.claude/tradekit/`, 7 `/tk` cards + adapters), and the cross-tool
  **handoff-repository** skill (`.claude/skills/` + `.agents/skills/` + a filled
  `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`).
- **Why:** user asked to install as many fit-appropriate kits as possible across all branches, and
  explicitly authorized including `main`. EcomKit/VideoKit skipped (no surface); MKTKit skipped
  (previously rolled back).
- **Validation:** **live-site guard** — `git diff <pre-install main>..<post-install main> --
  '*.html' 'css/**' 'js/**' 'images/**' netlify.toml` is **empty**, proving the deployed Netlify
  site (appearance, behavior, CSP headers, redirects) is byte-for-byte unchanged. `netlify.toml`
  publishes the repo root with no served route to `.claude/`, so the added tooling is never served.
  Site-file guard grep clean; EngKit = 35 files, SEOKit = 19 commands, TradeKit = 7 cards, handoff
  skill+agent+config present.
- **Risks:** none to the live site. This is not a design change or a merge of any redesign branch —
  those remain separate, un-made decisions.

---

## Summary

Rework the homepage hero photo composition: enforce a 25%-navy/75%-image ratio, swap in the
client's hi-res original photo, and show the full frame (lamp, heads, curtain) on desktop with no
seam

## Description

- **What changed:** `css/style.css` — widened `--hero-photo-w` 57%→75% (band left edge 43%→25%)
  and compressed the `.hero::after` overlay so solid navy ends by ~18-25% instead of ~43-47%;
  added `@media(min-width:1024px){ .hero{ min-height: clamp(660px, 50vw, 900px) } }` so full-width
  `cover` shows the entire photo frame on wide screens instead of cropping the top. `images/hero-ai.jpg`
  replaced with a 1400x933 q85 resample of the client's clean hi-res original (source PNG kept
  locally, untracked per existing `.gitignore` `images/ChatGPT*` rule).
- **Why it changed:** prior hero passes never actually moved the navy/photo boundary far enough
  left despite repeated attempts (user-reported); the old hero photo was a soft, low-res crop and
  the client since supplied a clean hi-res original; the user separately asked to zoom out enough
  to show the full lamp, both full heads, and more of the left curtain without cropping.
- **What was verified:** Playwright headless rendering + canvas pixel-sampling at
  1024/1440/1790/2560px desktop, 768px tablet, 390px mobile (no PIL/ImageMagick available). Navy
  confirmed to end at ~25% at every desktop width (flat, not growing with viewport). Tablet and
  mobile renders confirmed byte-identical before/after (`cmp`).
- **Remaining risk or follow-up:** an interim `background-size: contain` zoom-out approach was
  tried and reverted mid-session — it made the navy column grow with viewport width (26% at
  1440px to ~45% at 1790px+) and left a mid-screen photo edge that hardened into a visible line on
  wide monitors; the final full-width-`cover` + height-scaling approach has neither problem by
  construction. Desktop hero is now taller on wide screens (up to ~900px) as the necessary cost of
  showing the full photo without cropping — a visible layout change, reviewed and approved by the
  user on their own (~1790px) screen. `.nav__cta`/`.section--coral` contrast fast-follow and the
  apple-touch-icon remain open, unrelated to this push.

Fix hero CTA contrast (WCAG AA) and reposition the hero gradient/photo band left

## Description

- Superseded `000_INBOX/ofc-hero-spec-and-fix.md` (AntBrainOS vault) — that un-triaged spec's
  hex values (`#1A1F2A`/`#C25A32`) didn't match this repo's actual tokens (`--navy:#1b2230`,
  `--coral:#D85A30`), and its "widen 47–61%→40–68%" gradient target was superseded by a fresh,
  more specific ask this session: the user provided an annotated screenshot of the live hero and
  asked to move the gradient left into a marked zone (~43–50% of hero width).
- **Button contrast:** added `--coral-fill: #C2512B` (`css/style.css` `:root`), `.btn--coral`
  background/border repointed to it. `--coral` (3.87:1 vs white, fails 4.5:1 AA) is unchanged
  everywhere else (headline `<em>`, etc.). `--coral-fill` measures ~4.66:1, sits between `--coral`
  and `--coral-dark` (5.37:1, still used for `:hover`) so hover still darkens further. Confirmed
  `.btn--lg` (15px/weight 600) doesn't qualify as WCAG "large text," so the stricter 4.5:1 applies.
- **Gradient shifted left (low-risk lever, applied first):** `.hero::after`'s 10-stop overlay
  gradient stop positions compressed/shifted left (opacities unchanged): 18→13%, 22→17%, 28→22%,
  34→27%, 40→33%, 46→39%, 52→46%, 60→52%, 100→60%. Concentrates the compression in the old
  gradient's long, nearly-invisible tail rather than uniformly rescaling — peak slope only
  increases ~20% vs. the ~82% a naive rescale would cause.
- **Photo band widened (higher-risk lever, applied after explicit user sign-off):** `--hero-photo-w`
  53%→57% (`.hero::before`), moving the band's hard left edge 47%→43% to reach the rest of the
  user's marked zone that gradient-only tuning structurally cannot reach (no photo pixels exist
  left of the band edge, regardless of overlay opacity). This is the lever this project's history
  flagged as having caused repeat regressions (steepened gradients, narrowed bands, hard seams) —
  only touched after showing the user the gradient-only result and confirming they wanted to close
  the remaining gap.
- Updated two stale explanatory comments above `.hero::before`/`.hero::after` describing the old
  edge/opacity numbers.
- **Found but explicitly out of scope:** `.nav__cta` (header "Get started") and `.section--coral`
  (phone-strip band) have the same coral-fill/white-text contrast pattern as `.btn--coral` — not
  touched this pass.

### Verification (no PIL/ImageMagick available in this environment)

Used `npx --no-install playwright` (chromium already cached locally, no install needed) to
screenshot `index.html` at 1200/1440/700/390px viewports before and after each change, served via
a throwaway local `python3 -m http.server`. Built a small canvas-based pixel sampler (Playwright
chromium navigated to a same-origin HTML page, `<img>` + `<canvas>.getImageData`, images served
over a second local HTTP server to avoid `file://` canvas-tainting) rather than eyeballing
screenshots, since the opacity change is real but visually subtle over a busy photo.

**Button fill**, sampled at the same interior pixel: `(217,97,57)` before → `(196,89,52)` after —
matches the intended `#D85A30`→`#C2512B` shift.

**Gradient-only fix**, sampled along y=460 at 1200px viewport (before → after, `(r,g,b)`):
| x (hero %) | before | after |
|---|---|---|
| 420–564 (≤ old band edge) | identical | identical (0 diff — no photo pixels exist there yet) |
| 600 (50%) | (76,51,49) | (82,53,49) |
| 624 (52%) | (117,78,72) | (125,82,73) |
| 660 (55%) | (139,123,114) | (148,131,120) |
| 900 (75%) | (102,75,54) | (105,77,55) |
| 1050+ | identical | identical (old residual tint already <2%, below rounding) |

Confirms the change is real and lands exactly where the math predicts, and nowhere else.

**Band-widening**, hairline-transition point (dark→lit skin/hair tones) measured across 6 rows
(y=290–340) at 1200px viewport: consistently shifts ~43–45px left (e.g. y=290: x≈710 → x≈667),
matching the intended 4-percentage-point (48px) edge shift closely.

**Face-clearance check** (this project's history flags this as the thing to watch): measured the
gap from the "possible." period's right edge to the hairline's left edge, same rows: **before
~125px (10.4% of 1200px viewport) → after ~80px (6.7%)**. A real reduction, but still above the
~60px/5% gap this project's own prior iteration established as safe (`b195aba`/`ea8b067` era).
Direct zoomed-crop visual inspection (both before/after, same region) showed no hard seam/line at
the new band edge in either version.

**Mobile (390px):** 4 sampled points pixel-identical before/after — confirms the separate mobile
`@media` block (untouched) renders unaffected.

**Environment note:** mid-session, Bash briefly failed entirely with `ENOSPC` (disk full) — even
`df -h` couldn't write its own output log. Resolved itself (or was resolved by the user) partway
through; confirmed via `df -h` showing 7.6Gi free at the low point, 21Gi after cleanup. No repo
files were affected (Edit/Write tool changes to `css/style.css` were confirmed intact via direct
file reads throughout, since those don't depend on the same code path as Bash's output capture).

---

## Summary

Record next action: hero contrast/gradient-fade fix spec waiting in AntBrain inbox

## Description

- No code changed. Updated `docs/project/STATUS.md` and `PROGRESS_NOTE.md` to point the next work
  item at `000_INBOX/ofc-hero-spec-and-fix.md` (AntBrainOS vault) — a pre-written spec covering a
  CTA button contrast fix (coral fill/white text fails 4.5:1 AA) and a gradient fade-seam widening
  fix (~47–61% → ~40–68%)
- The same pointer was also recorded in the vault's `CURRENT_CONTEXT.md`/`HANDOFF_TO_CLAUDE.md` for
  Old Fashion Care so the vault and repo agree on what's next
- No validation needed — documentation-only tracking update, no site files touched

---

## Summary

Complete V3.4 migration: reconcile root v3.3 docs with docs/governance/ and docs/project/

## Description

- The V3.4 baseline + 18-skill suite (installed 2026-07-05, merged to main 2026-07-06) only added
  scaffolding — it never reconciled with this project's existing v3.3 root docs, and the installed
  V3.4 skills hardcode `docs/governance/`/`docs/project/` paths, so they'd only ever see empty
  generic templates without this consolidation
- Moved 8 root docs with real content into `docs/project/`, overwriting empty stubs: STATUS.md,
  CHANGELOG.md, COMMIT_NOTES.md, CONTEXT.md, DECISION_LOG.md, PROJECT_BRIEF.md, RELEASE_NOTES.md,
  ROADMAP.md; authored real content for the new docs/project/ARCHITECTURE.md
- Merged 6 governance file pairs into docs/governance/ (root had real content, V3.4 side was
  generic/empty): CHANGE_CONTROL.md, ROLLBACK_PLAN.md, REPO_HEALTH_CHECK.md, DONE_CRITERIA.md,
  PROJECT_RISK_REGISTER.md (authored real project-specific risks), PHASE_GATES.md (reconciled two
  genuinely different gate taxonomies); deleted all 6 root duplicates
- Rewrote AGENTS.md and CLAUDE.md: kept v3.3-specific content, added V3.4's genuinely new pieces
  (Output Standard report format, no-auto-commit line, skill references)
- Resolved ai/agents/AGENT_REVIEW_GATES.md (kept the real 7-agent table, added Review Gate D);
  added a roster-authority note to ai/agents/SUBAGENT_ROLES.md
- Deleted all 3 remaining .v34_migration_review/ candidates and the now-empty directory
- Updated README.md, START_HERE.md, and 4 live files that still referenced old root paths
  (ai/README.md, .claude/agents/project-steward.md,
  .claude/skills/v33-quality-gate-enforcer/SKILL.md, docs/WORKFLOW.md)
- Verified: git status matches the planned scope exactly (17 deletions, 25 modifications);
  v34_validate.py PASS; local href/src resolution sweep clean; no dangling references to deleted
  files in any actively-referenced doc
- Follow-up: hi-res hero photo swap and dedicated apple-touch-icon.png remain open (pre-existing,
  unrelated items)

---

## Summary

SEO audit fixes + Production Readiness Skills deployment (V3.4 baseline + 18-skill suite)

## Description

- SEO metadata audit across all 6 HTML pages: titles, descriptions, canonicals, Open Graph, and
  Twitter Card tags, plus sitemap.xml and robots.txt — all already correct, no changes needed
- Fixed: `og:image`/`twitter:image` on all 6 pages pointed to `images/og-default.png` (does not
  exist) — repointed to the existing `images/hero-ai.jpg`; corrected `og:image:width`/`:height`
  from placeholder 1200×630 to the file's actual 1100×934
- Fixed: `<link rel="apple-touch-icon">` on all 6 pages pointed to `images/apple-touch-icon.png`
  (does not exist) — removed the tag; `favicon.svg` remains as fallback
- Installed Project Starter Kit V3.4 baseline (migrate mode, additive only) and the 18-skill
  production-readiness suite into `.claude/skills/` (22 skills total); `v34_validate.py` passed
- 3 real conflicts with existing files (`AGENTS.md`, `CLAUDE.md`, `ai/agents/AGENT_REVIEW_GATES.md`)
  were quarantined into `.v34_migration_review/` rather than overwritten
- Fixed a leftover cross-client ("Smart Learning Solutions") reference found in one installed
  skill's description text during verification
- Verified: no remaining references to either missing SEO asset; every local href/src across the
  6 pages resolves to an existing file; no existing tracked repo file was modified or deleted by
  either skills installer
- Follow-up: consider a dedicated 180×180 apple-touch-icon.png; reconcile `docs/project/*.md` with
  existing root tracking docs; review the 3 quarantined `.v34_migration_review/` candidates

---

## Summary

Fix RepoBackups path in tracking docs — correct volume is DataHub_2TB

## Description

- PROGRESS_NOTE.md and PROGRESS_NOTES.md: replaced `/Users/ant/WorkSync/Projects/RepoBackups` with `/Volumes/DataHub_2TB/WorkSync/Projects/RepoBackups` — the drive is mounted on an external volume, not under the home directory
- No site files changed; no validation required
- Remaining: actual snapshot folders on disk may still be at the wrong path — user to verify or move manually

---

## Summary

Add .gitignore; untrack root .DS_Store; suppress ChatGPT source PNGs

## Description

- Created `.gitignore` with `**/.DS_Store` and `images/ChatGPT*` patterns
- `git rm --cached .DS_Store` to untrack the root .DS_Store that was previously committed
- `ai/.DS_Store`, `ai/sessions/.DS_Store`, and `images/ChatGPT*.png` were untracked and are now ignored
- No site files changed; GitHub Desktop changes panel should be clean after this commit

---

## Summary

Add handoff workflow prompt to Prompts/ directory (version-controlled)

## Description

- Added `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` to version control; previously untracked
- File defines the repo push / handoff / snapshot / tag workflow used after each work session
- No site files changed
- No verification required — static file addition only

---

## Summary

Add refined handoff prompt: snapshot naming rules and RepoBackups path confirmation

## Description

- Added `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` to repo
- Refines Section 9 of the original handoff prompt: forbids bare version-only snapshot folder names, requires full `vX.Y.Z__description__commit-HASH` naming, requires user to confirm RepoBackups path per session
- Updated STATUS.md, PROGRESS_NOTES.md, COMMIT_NOTES.md to reflect this push
- No site files changed; workflow tooling only
- No remaining risk

---

## Summary

Handoff: STATUS, PROGRESS_NOTES updated; hero iterations ea8b067→b195aba→c04e819 documented; tag + snapshot for b195aba complete

## Description

- Updated STATUS.md and PROGRESS_NOTES.md to reflect three hero CSS iterations since last handoff (ea8b067, b195aba, c04e819)
- Tag `v1.5.0__hero-smoother-gradient-headline-fix__commit-b195aba` pushed to GitHub; snapshot saved to RepoBackups
- No site files changed in this commit — tracking docs only
- Remaining: user visual review of hero on live/local site; hi-res original photo still needed for images/hero-ai.jpg

---

## Iteration: smoother gradient, headline fix, sub barely-covers-hand

- `.hero__headline { max-width: 565→620px }`: fixed "We make that possible." wrapping to 2 lines;
  now consistently 1 line at desktop.
- `.hero::after` gradient: smoother 10-stop curve (0.94@18%, 0.86@22%, 0.72@28%, 0.56@34%,
  0.40@40%, 0.26@46%, 0.14@52%, 0.05@60%) — near-uniform ~0.02/% rate, visible fade begins at
  'h' in "where" (~22%/x≈340). Seam at 46% covered at 0.26.
- `.hero__sub`: font-size 0.97→0.94rem, max-width 648→580px — lines just barely reach the
  caregiver's wrist/hand; shoulder stays visible and uncovered. 4 lines preserved.
- Verified: desktop 1554×900 + mobile 390×820. Headline 1 line, 4-line sub, smooth gradient,
  hand barely covered, mobile clean.

---

## Iteration: hero gradient further left + wider sub overlap (B-period anchor, hand coverage)

- `.hero::after` gradient stops shifted further left: at the B-period position (x≈510, 33%) opacity
  is now ≈0.50 (was ≈0.86) — photo/women show through noticeably starting there. Seam edge at 47%
  kept at 0.20, sufficient with mask feather. Text over lighter gradient confirmed readable.
- `.hero__sub` max-width 582→648px: right edges of lines 1–3 now clearly overlap the caregiver's
  arm/shoulder/hand; "choose between" (line 1), "certified," (line 2), "want" (line 3) sit over hand.
  4-line count preserved. Mobile max-width is irrelevant (resets to full-width on mobile).
- Verified: desktop 1554×900 and mobile 390×820 screenshots confirmed.

---

## Iteration: hero copy↔people micro-alignment (4-line sub, period↔earlobe, gradient left, hand overlap)

Approved plan-mode change; `css/style.css` hero rules only (no markup, no photo change).

- (1) `.hero__sub`: font-size 1.05rem→0.97rem, max-width 480px→582px → paragraph now 4 lines (was 5),
  smaller and legible.
- (2) `.hero > .container`: added `margin-top: -5.2rem` (reset to 0 on mobile) so the orange "possible."
  period bottom (~y355 @1554×900) lines up with the older woman's earlobe bottom (~y348). Photo NOT moved.
- (3) `.hero::after` first linear-gradient pulled left (stops shifted earlier; e.g. 0.60@44, 0.38@50,
  0.20@55, 0.09@61) so more of the photo/women reveal, band left edge stays ≈0.50 (no seam), text readable.
- (4) The wider sub max-width pushes the lower lines' right edges (~"choose"/"support") subtly onto the
  caregiver's fingertips — hand and shoulder remain clearly recognizable, faces clear.
- Verification: headless Chromium screenshots at 1554×900 (desktop) and 390×820 (mobile); zoom crops via
  sips confirmed period↔earlobe within ~8px, subtle hand overlap, no seam, mobile unaffected.
- Decision: gradient ambiguity resolved with user — "pull dark toward the left edge / reveal more photo."

---

## Summary

Promote featured-photo hero to production homepage + 3-variant preview

## Description (production promotion)

- css/style.css ONLY (index.html markup unchanged); affects homepage hero exclusively
  (verified class="hero" is used only on index.html)
- Added :root token --navy: #1b2230
- .hero: solid charcoal -> navy background-color + images/hero-ai.jpg (background-size cover,
  right center), flex centering, min-height 600px
- .hero::before: repurposed from coral radial circle to a left->right navy scrim for text contrast
- Mobile (max-width 640px): vertical (top-dark) gradient + min-height 520px + background-position
- Photo is a REAL image from the client's own photo collection (NOT AI-generated, despite the
  preview button label / hero-ai.jpg filename). Current file is a 714x558 crop from a 1554px mockup
  screenshot -> soft when full-bleed; recommend dropping the client's ORIGINAL hi-res photo at
  images/hero-ai.jpg before launch (no code change)
- Validation: server -> / 200, css/style.css 200, images/hero-ai.jpg 200; other pages do not reuse
  base .hero (no regression)

## Iteration: shift copy + gradient left; clear button from elbow (CURRENT)

- Last step's right-shift put "See our services" ~30px INTO the older woman's arm/elbow. Per user, moved
  the whole hero composition left:
  - `.hero > .container` padding-left 4.5rem -> 0.75rem: shifts eyebrow/headline/"possible."/paragraph/
    buttons left together (~60px). "See our services" right edge now ~712px, her elbow ~746px -> ~34px
    comfortable gap ("a bit more breathing room"). Mobile still resets padding-left to 1.5rem.
  - `.hero::after` gradient stops shifted left ~4-5% (0.84@40->36, 0.42@47->0.50@43, etc.) so the dark
    copy-zone moves left and more of the photo/people shows; kept ~0.40 at the band edge (47%) so NO seam.
  - `.hero__sub` max-width 512 -> 480 (the hand-overlap widening is no longer needed).
- Accepted trade-off: "possible." now has a larger gap from her head; paragraph no longer overlaps the hand.
- Verified desktop(1554) + mobile(390) + zoom of the button/elbow gap. Photo untouched.

## Iteration: align hero copy to the people (committed 778099e)

- Per user, adjusted the words in relation to the people (photo unchanged; rollback tag
  v1.4.0__hero-clean-photo-hand-visible__commit-41bffc4 protects the prior state):
  - Shifted the hero copy right: `.hero > .container` padding-left 1.5rem -> 4.5rem (mobile reset to
    1.5rem). Result: "We make that possible." now sits a nice ~30px gap from the older woman's head,
    aligned toward her ear; the paragraph moves toward the caregiver's hand.
  - Widened the paragraph: `.hero__sub` max-width 480 -> 512 so its right edge slightly OVERLAYS the hand.
- Verified desktop(1554) + mobile(390) + zoom of both relationships (possible↔head/ear, paragraph↔hand).

## Iteration: swap in the CLEAN original photo — hand now clearly visible (committed 41bffc4)

- User supplied the clean original photograph (no baked website text):
  images/ChatGPT Image Jun 28, 2026 at 08_03_49 PM.png (1536x1024). This was the real blocker — the
  previous hero-ai.jpg was cropped from a full-page MOCKUP screenshot, with the hand ~85% cropped off and
  the paragraph text baked over the rest, so no CSS could recover it.
- Re-cropped images/hero-ai.jpg from the clean source: sips crop x=330..1536 (full height), resampled to
  1100px wide, JPEG q86 (~248KB). The crop keeps a dark margin LEFT of the hand, so the band's left edge
  sits in dark background (no seam) and the caregiver's hand sits INSIDE the frame at ~hero 49-59%.
- No CSS change needed — the committed gradient/feather already frame it correctly. Result: hand on the
  shoulder reads clearly as a soft detail fading into the gradient; paragraph's right edge lands on it;
  faces clear on the right; smooth gradient, no seam. Verified desktop(1554) + mobile(390).
- The clean source PNG is left in images/ (untracked) as the re-crop source.

## Iteration: reveal the hand-on-shoulder (soft fade) — committed cf7404a

- Goal: caregiver's hand on the older woman's shoulder + that shoulder read as a SOFT detail that fades
  into the gradient (user: "clearly visible, just not as visible as the people's faces"), matching the
  "There." reference mockup (images/ChatGPT Image Jun 27, 2026 at 06_39_46 AM.png).
- Root cause confirmed: the hand sits at the photo's FAR-LEFT edge (source x~0-45, y~360-450), which is
  exactly the right-anchored band's left edge — the same strip that must stay dark/feathered to hide the
  seam. No clean photo data exists left of the hand (the file was cropped from the text-baked mockup), so
  the hand is welded to the band edge and can only ever be a soft fade from this source.
- Changes (css/style.css, hero):
  - .hero::before: thin left-edge feather mask (linear-gradient 90deg, transparent 0, #000 6%) — softens
    the band edge (no hard line) without reaching the hand body. Band width 53%, position left center.
  - .hero::after: lightened the hand/shoulder zone while keeping the copy zone dark and the slope smooth:
    0.97@0, 0.94@30, 0.84@40, 0.42@47, 0.26@53, 0.15@62, 0.07@75, 0.02@88, 0@100 (+ vertical vignette).
  - Copy unchanged from the approved reference-match (headline 565 one-line orange, sub 480).
- Verified desktop(1554) + mobile(390) + upscaled zoom of the hand zone: hand reads as a soft fade,
  shoulder visible, gradient smooth, NO seam/line; framing matches the target.
- Limit: the hand stays soft because the source is a low-res, dim crop with the hand at the very edge; a
  clean full-frame original photo would let it read more clearly (no code change for the swap).

## Iteration: match the user's reference mockup exactly (committing together)

- User supplied the exact target screenshot. Rebuilt the hero to match it precisely:
  - .hero::before: right-anchored band restored, width var(--hero-photo-w, 53%), background-position
    left center; added a left-edge feather (mask-image linear-gradient(90deg, transparent 0, #000 11%))
    so the band dissolves into navy with NO vertical seam/line (the "line" the user flagged)
  - .hero::after: smooth multi-stop gradient — 0.97@0, 0.94@30, 0.84@41, 0.60@50, 0.36@60, 0.18@72,
    0.05@87, 0.00@100 (+ softened vertical vignette). Band edge (~47%) hides under the feather+gradient
  - Copy widened to match the reference: .hero__headline max-width 430 -> 565 (orange "We make that
    possible." now ONE line); .hero__sub max-width 350 -> 480 (4-line paragraph), color 0.58 -> 0.68,
    line-height 1.75 -> 1.7
  - Result vs reference: women on the right, older woman's FACE fully clear of the copy, smooth gradient
    with no line, lamp+caregiver bright on the right, hand-on-shoulder in the blend zone
  - Mobile (max-width 640): .hero::before mask reset to none (keeps full-bleed mobile photo unfaded)
  - Verified desktop(1554) + mobile(390) headless screenshots against the reference
- Image still the soft 746x558 placeholder crop; client's hi-res original would sharpen it (no code change)

## Iteration: restored smooth gradient (superseded by the reference-match above)

- Prior step wrongly steepened .hero::after to expose the hand, flattening the premium gradient
  (against the user's explicit instruction). Restored a smooth, gradual multi-stop gradient:
  0.96@0, 0.93@28, 0.84@40, 0.66@50, 0.46@60, 0.30@74, 0.18@100 (+ vertical vignette)
- Hand/shoulder kept visible via FRAMING not gradient hacks: --hero-photo-w 53% -> 50% (pushes the
  hand into the gradient's lighter zone); background-position left center
- Re-cropped image (offset 808) retained; copy unchanged (headline 430 / sub 350)
- Verified desktop(1440)+mobile(390): smooth premium gradient + shoulder visible, hand softly present

## Iteration: ROOT-CAUSE fix — re-crop image to include hand+shoulder (image fix retained)

- Root cause of repeated failures: images/hero-ai.jpg had been cropped from the composite at x=840,
  which sliced OFF the caregiver's hand (x~830) and older woman's outer shoulder (x~815). No CSS could
  reveal detail absent from the file.
- Re-cropped images/hero-ai.jpg from the composite at offset x=808 (746x558) — hand + full shoulder now
  present; cropped just right of the baked-in text so none intrudes.
- CSS retune so hand/shoulder sit right of the copy and read clearly:
  - --hero-photo-w 60% -> 53%; background-position left center (keeps the far-left hand; trims the
    far-right lamp — acceptable per priority)
  - .hero::after gradient lightened across the hand zone: 0.95@0, 0.94@37, 0.54@45, 0.26@53, 0.20@74,
    0.16@100 (+ vignette) so the hand/shoulder (~46-52%) are visible, not muddy
  - copy stays narrow: .hero__headline max-width 430, .hero__sub 350 (ends ~37%, ~10% gap to people)
- Verified desktop(1440)+mobile(390) screenshots: hand/shoulder in frame & right of copy; left zone clean
- Caveat: the hand is in a naturally dim/shadowed part of this AI crop; a full-res original at
  images/hero-ai.jpg would render it more clearly
- Knobs: crop offset, --hero-photo-w, background-position, .hero::after stops, copy max-widths

## Iteration: copy clears the hand-on-shoulder boundary (superseded — image lacked the hand)

- Problem: hand/shoulder contact sits in the LEFT of the photo crop, so the copy still overlapped it
- Fix = pull copy in + keep photo wide enough to render hand/shoulder uncropped (don't shrink band):
  - .hero__headline max-width 680 -> 470 (orange line now wraps to two lines, stays in left zone)
  - .hero__sub max-width 560 -> 430
  - .hero::before width var 54% -> 60% (width-bound => no left-crop) ; background-position right -> left
    (keep the far-left hand/shoulder visible; trims ~3-4% of the far-right lamp)
  - .hero::after re-aligned: 0.97@0, 0.95@40, 0.74@48, 0.42@58, 0.28@76, 0.20@100 (+ vignette)
- Result: copy fully in left dark zone; older woman shoulder + caregiver on the right, warm; lamp still
  visible; verified desktop(1440)+mobile(390) headless screenshots
- Knobs: .hero__headline/.hero__sub max-width, --hero-photo-w, .hero::before position, .hero::after stops

## Iteration: protected copy zone, people on the right (superseded by the above)

- Reversed the over-wide band: people back on the right, left ~48% is a protected text-safe zone
- .hero::before width var(--hero-photo-w) 84% -> 54%; background-position center -> right center
- .hero::after: strong left->right protect gradient — 0.97@0, 0.95@44%, 0.72@52%, 0.42@60%,
  0.30@76%, 0.22@100% (+ vertical vignette). Opaque left (0-44%) also hides the band edge
- Older adult sits center-right (face clears the orange line), caregiver far right, lamp far right
- Mobile: .hero::before reset to full-bleed (left/right/width 100%, background-position 60% 28%) so
  the people stay present under the stronger top-down overlay; readability prioritized
- Verified desktop (1440) + mobile (390) via headless-chromium screenshots
- Tuning knobs: --hero-photo-w (framing), .hero::after stops (readability), .hero min-height (660px)

## Iteration: integrated photo + multi-stop gradient (superseded by the above)

- Goal: hero reads as one composition (not a dark text panel beside a photo); older adult nearer
  centre & partially in the blend, caregiver clear on the right, warm lamp far right
- .hero::before: widened photo band 58% -> var(--hero-photo-w, 84%) (bleeds into centre); removed the
  mask (its hard left edge is now hidden under the opaque-left gradient zone -> still seamless)
- .hero::after: replaced empty desktop scrim with a 6-stop navy gradient
  (~0.95 left -> 0.62 @38% -> 0.30 @56% -> 0 @100%) + a subtle vertical vignette
- .hero min-height 600 -> 660 (more vertical presence)
- Mobile: photo full-band + stronger top-down navy overlay (readability first), faces clear of headline
- Verified via headless-chromium screenshots at 1440 and 390 widths (desktop + mobile)
- Tuning knobs: --hero-photo-w (framing), .hero::after opacity stops (readability), .hero min-height
- NOTE: not yet committed

## Iteration: seamless photo->navy blend (whole-photo zoom kept)

- Fixed visible seam from background-size:contain (hard right-anchored edge that shifted with viewport
  + a compounding diagonal scrim)
- .hero::before now a right-anchored band (width 58%, background-size cover) whose mask feathers the
  band's own left edge -> dissolves into navy seamlessly at any width
- .hero::after desktop scrim removed (left is solid navy); kept only as the mobile vertical scrim
- Mobile: .hero::before reset to full-bleed (left/right/width 100%, mask none)
- Validation: captured full-page screenshot via cached headless Chromium
  (~/Library/Caches/ms-playwright/.../chrome-headless-shell, --screenshot, 1440x1500) -> seam gone,
  text readable, whole photo visible, page unchanged below the hero

---

## Earlier in this branch: 3-variant hero preview (no production change)

## Description

- New preview-only files; production index.html and css/style.css untouched:
  - hero-preview.html — reuses prod nav/head, 3 isolated hero sections, variant switcher
  - css/hero-variants.css — scoped treatments for .hero--ai / .hero--real-single / .hero--real-montage
  - js/hero-preview.js — segmented-control + ?hero= query-param switcher; AI-asset existence check
- Variant 1 (AI Image): images/hero-ai.jpg now in place — cropped (via sips) from the user's AI
  composite at ~/Downloads/"ChatGPT Image Jun 27, 2026 at 06_39_46 AM.png" to isolate just the
  embrace photo (714x558, ~96KB, placeholder-grade). JS still shows a labeled fallback if the file is
  ever missing. Recommend swapping in a hi-res clean export (~2400x1400 .webp) for production.
- Variant 2 (Real / Single): care-04.jpg + charcoal left gradient
- Variant 3 (Real / Soft Montage): blurred low-opacity blend of care-04 + care-10 + care-05, dark left
- Excluded watermarked care-01.jpg / care-02.jpg (visible iStock watermarks)
- Validation: `python3 -m http.server` — hero-preview.html 200; all CSS/JS/images 200;
  images/hero-ai.jpg 404 as expected (placeholder path); git status shows only 3 new files
- Follow-up: user to add images/hero-ai.jpg; decide which variant (if any) to promote to production;
  separately fix watermarked care-01 already live in production photo grid

## Known flags

- care-01.jpg is watermarked AND currently live in the production "Real Care, Real People" grid
- Large source images (2+ MB) should be compressed/resized for production use
