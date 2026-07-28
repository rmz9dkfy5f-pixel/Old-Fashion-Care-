# Status

**Last updated:** 2026-07-28

---

## Latest — Add Dedicated apple-touch-icon.png (180×180) (2026-07-28, branch `main`, verified and staged)

**Context:** Long-open, well-documented gap — R-006 in `docs/governance/PROJECT_RISK_REGISTER.md`,
an open `BACKLOG.md` item, and a repeat finding in two SEO hygiene audits under `seo/audits/`. All 6
pages previously fell back to `favicon.svg` for iOS home-screen bookmarks; no apple-touch-icon
existed anywhere in the repo.

**What happened in this batch:**
1. Rasterized the repo's own `favicon.svg` lettermark (rounded charcoal square, centered coral "O")
   to a 180×180 PNG via a throwaway Playwright render (HTML wrapper, SVG inlined at fixed 180×180,
   background matched to the SVG's `#2A2825` fill so transparent corner slivers render as identical
   charcoal instead of a white halo) — a mechanical conversion of an already-approved mark, not new
   design work. Script not committed, scratch-only.
2. Verified output dimensions (`sips -g pixelWidth -g pixelHeight`, 180×180 confirmed) and visual
   correctness (solid rounded charcoal square, centered coral "O", no artifacts) before promoting
   into the repo at `images/apple-touch-icon.png`.
3. Added `<link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon.png">` to all
   6 pages' `<head>`, directly below the existing favicon `<link>` — textually identical on all 6.
   `hero-preview.html` (noindex, nofollow, dev-only) intentionally left unchanged.
4. Recorded a short `docs/project/DECISION_LOG.md` entry clarifying this doesn't contradict the
   2026-07-05 decision to remove the broken apple-touch-icon tag (that decision's rejected
   alternative concerned cropping a photo, not rasterizing the site's own vector mark).
5. Updated all required tracking docs (BACKLOG.md, PROJECT_RISK_REGISTER.md R-006→R-C09,
   DECISION_LOG.md, PROGRESS_NOTE.md, PROGRESS_NOTES.md, COMMIT_NOTES.md, CHANGELOG.md,
   SLICE_REVIEWS.md, this file).

**Placement rationale:** `images/apple-touch-icon.png`, not repo root — automatically inherits the
existing `netlify.toml` `/images/*` 1-year immutable cache rule with zero `netlify.toml` changes;
CSP `img-src 'self' data:;` already permits it.

**Verification:** `sips` confirmed exact 180×180 dimensions. Grep sweep confirmed exactly 6 files
contain `rel="apple-touch-icon"`, textually identical, and `hero-preview.html` contains zero.
`git diff --stat` confirmed scope: 6 HTML files + 1 new PNG + tracking docs, nothing else;
`netlify.toml` untouched. No automated test suite exists for this static site (documented,
R-001) — verification was manual/scripted.

**Not done, and not in this batch:** no commit, tag, snapshot, or push — staged only, per this
repo's phase-gate convention. All other open items unchanged (Web3Forms configuration blocked on
client purchase, form-analytics events, HSTS header, `care giver pics/` folder decision,
care-07–11 review, iOS Safari check).

**Commit status:** not yet committed — file staged and tracking docs updated; user will request
commit as a separate step.

---

## Previous — Image Optimization: 4 Live `care-*.jpg` Photos Compressed (2026-07-23, branch `main`, verified and staged)

**Context:** The standing user-confirmed next task from the prior two session-end closeouts (2026-07-22 
and 2026-07-23 recovery audit). R-007 (performance risk, quantified at 2026-07-19 production-readiness 
audit) identified 9 oversized `care-*.jpg` files. Exploration found this splits into: 4 genuinely 
referenced in `index.html`'s photo grid (real visitor-facing page-weight impact) and 5 unreferenced 
dead assets (hygiene-only impact). User confirmed this slice should compress only the 4 live files, 
leaving the 5 dead files for a separate decision already tracked in BACKLOG.md.

**What happened in this batch (image compression only, no HTML/CSS/copy changes):**
1. Resized 4 photos via macOS `sips -Z 800 -s format jpeg -s formatOptions 82` (this repo's 
   established precedent tool) to ~800×533px (landscape files: care-04/05) / 533×800px (portrait 
   files: care-03/06). Aspect ratios preserved, no stretching or EXIF-rotation artifacts.
2. Resized to scratch location (`/tmp/ofc-image-opt/`) first, inspected each file's dimensions/
   integrity via `sips -g`, promoted into place via `cp` once all 4 passed — never one-shot in-place.
3. Confirmed scope via `git diff --stat`: exactly the 4 image files changed (binary), no other 
   modifications.
4. Updated all 8 required tracking docs (BACKLOG.md, PROJECT_RISK_REGISTER.md, PROGRESS_NOTE.md, 
   PROGRESS_NOTES.md, COMMIT_NOTES.md, CHANGELOG.md, SLICE_REVIEWS.md, this file).

**Why this scope, not all 9:** The 4 live files drive real visitor-facing page-weight savings (6.7 MB 
→ 0.428 MB combined, 93.9% reduction per file). The 5 unreferenced dead files (`care-07`–`11`) 
represent hygiene-only savings (~5.5 MB total) but aren't downloaded by any visitor, and their use 
is blocked on a separate, already-tracked user decision ("should we use these as photo-grid 
replacements?") — see BACKLOG.md "Review whether care-07–11 should replace any current grid photo."

**Size reduction:** care-03 1.2 MB → 102 KB; care-04 989 KB → 93 KB; care-05 2.1 MB → 106 KB; 
care-06 2.4 MB → 117 KB. Total: 6.96 MB → 0.428 MB (93.9% smaller).

**Verification:** Dimension checks (portrait 533×800, landscape 800×533, both correct per aspect 
ratio). EXIF orientation checks (all nil, no unexpected rotation). File integrity confirmed via 
`sips` read-back. Scope confirmation via `git diff --stat`. No automated test suite exists for this 
static site; Playwright visual verification not available in this environment, but dimension/
orientation integrity was verified. The resized JPEG images remain valid.

**Not done, and not in this batch:** the 5 unreferenced files remain uncompressed pending the 
separate user decision. All other open items unchanged from prior closeouts (Web3Forms configuration 
blocked on client purchase, form-analytics events, HSTS header, apple-touch-icon, `care giver pics/` 
folder decision, iOS Safari check).

**Commit status:** not yet committed — images are staged and tracking docs updated; user will request 
commit as a separate step per this repo's convention (preparation before the actual commit).

---

## Previous — Contact Form Vendor Decided: Web3Forms (2026-07-22, branch `main`, not yet committed)

**Context:** user directly decided the contact form's eventual delivery vendor: **Web3Forms, not
Formspree**. `contact.html`'s form action is still the never-configured placeholder
`https://formspree.io/f/REPLACE_WITH_FORMSPREE_ID` — that placeholder will not be filled in with a
real Formspree ID. Once the client finalizes the purchase of this site, the form will be switched to
Web3Forms instead.

**What happened in this batch (docs only, no code change):**
1. Recorded the decision in `docs/project/DECISION_LOG.md`, including implementation notes for
   whoever performs the future swap (the existing `js/main.js` submit handler is vendor-agnostic;
   only `contact.html`'s form `action` and hidden fields need to change, once a real Web3Forms
   access key exists).
2. Reworded `docs/governance/PROJECT_RISK_REGISTER.md` R-008 (and its R-009 cross-reference),
   `BACKLOG.md`'s matching item, and `docs/governance/RELEASE_GATE.md`'s Functionality checklist +
   Release Decision notes — all from Formspree to Web3Forms.
3. Updated this repo's own required tracking docs (`PROGRESS_NOTE.md`, `docs/project/COMMIT_NOTES.md`,
   this file) to record the batch.

**Why:** direct user instruction — "when the client is ready to finalize the purchase, we will be
using this. Web3Forms document that, and we can pretty much close out."

**Not done, and not agent-actionable right now:** the actual vendor swap. No Web3Forms account or
access key exists yet; implementation is blocked on the client finalizing the purchase.

**With this decision recorded, no agent-actionable item remains open.** Every remaining
`BACKLOG.md`/`PROJECT_RISK_REGISTER.md` entry is either user-owned (this vendor swap, the
`oldfashioncare.com` domain question) or optional polish not yet prioritized by the user (image
optimization — the standing confirmed pick from the prior closeout — HSTS header, apple-touch-icon,
analytics events, the `care giver pics/` folder decision, iOS Safari check).

**Verified:** `grep -rln "Formspree"` re-run after edits confirmed every tracking-doc reference to
Formspree either now names Web3Forms or is an untouched historical record of already-shipped past
work; `contact.html`/`js/main.js` intentionally untouched (no key to configure yet, out of scope for
a docs-only decision record).

**Commit status:** not yet committed — staged for the next push at the user's direction.

---

## Previous Push — Hash Placeholder Backfill and Vault Baton Refresh (2026-07-22, branch `main`)

```text
Branch: main
Tag:    v2.10.0__hash-placeholder-backfill-vault-baton-refresh__commit-<pending>
        (filled in below once this commit is made and tagged)
```

**Context:** a `REPO_SESSION_START_RECOVERY_AUDIT.md` run found two staleness gaps left over from
the prior push: (1) `docs/project/STATUS.md`'s v2.9.0 entry and `PROGRESS_NOTE.md`'s "Commit
status" section still carried "hash once committed" placeholders instead of the real resulting
hash (`10ee3d0`), and a much older, previously-missed placeholder in this same file's v2.6.0
founder-photo entry was never backfilled either (`dd88bf4`); (2) the AntBrainOS vault's
`00_START_HERE/AGENT_HANDOFF.md` baton entry for this project was three days stale, still naming
the (already-completed) 18-skill audit re-run as the open next task.

**What happened in this batch:**
1. Backfilled all 3 hash placeholders — `docs/project/STATUS.md` (×2: v2.9.0, v2.6.0) and
   `PROGRESS_NOTE.md` (×1) — via a small trailing docs-only, untagged commit (`f75de99`),
   following this repo's own established `8e61909`/`358e0b9` precedent for this exact fix pattern.
2. Prepended a new dated entry to the vault's `00_START_HERE/AGENT_HANDOFF.md`, superseding
   (without rewriting) the stale 2026-07-18 entry, recording the actual v2.8.0/v2.9.0 audit
   completion and that no active task remains.
3. Refreshed a matching stale "in progress" pointer in this project's own vault
   `HANDOFF_TO_CLAUDE.md` to cite the real pushed hash.
4. Took a fresh AntBrainOS vault snapshot before any vault edit, per that vault's own snapshot SOP
   (987/987 files verified, 0 file-list diff; 3 checksum mismatches traced to unrelated live edits
   made after the snapshot copy — documented in that snapshot's own `SNAPSHOT_MANIFEST.md`).
5. Updated this repo's own tracking docs (`STATUS.md`, `PROGRESS_NOTES.md`, `COMMIT_NOTES.md`,
   `PROGRESS_NOTE.md`, `CHANGELOG.md`) to record this batch before this push's commit/tag.

**Why:** the session-start recovery audit's `PASS WITH CONDITIONS` verdict named both stale spots
as explicit conditions; the user then asked to fix them.

**Verified:** `git diff --stat` before the trailing commit confirmed only the 2 intended files
changed; `git show --stat` on that commit confirmed it was docs-only; re-read of the new
`AGENT_HANDOFF.md` entry confirmed the old entry was left untouched and ordering preserved.

**Known follow-up, not part of this batch:** none new — this batch only closes the two staleness
conditions named by the recovery audit. The optional/user-owned items from the prior push (real
Formspree ID, form-analytics events, image optimization, HSTS header, apple-touch-icon, the
`care giver pics/` folder decision, iOS Safari check) remain open, tracked in `BACKLOG.md` and
`docs/governance/PROJECT_RISK_REGISTER.md` (R-007–R-013).

**Next recommended step (user-confirmed at this push's own session-end closeout gate):** image
optimization — compress the 9 oversized `care-*.jpg` files (~14.2MB avoidable). Presented as a
ranked 7-candidate list; this was the explicit pick, not inferred.

---

## Previous Push — Production-Readiness Audit Complete: Uptime Check, VPS Deploy, Full Reconciliation (2026-07-19, branch `main`)

```text
Branch: main
Tag:    v2.9.0__production-readiness-audit-complete-uptime-check-vps-deploy__commit-10ee3d0
```

**Context:** follow-on to the push immediately below (same day, continuing the same session). The
production-readiness audit that push left at "9 of 13 skills done" is now **complete**: all 13
planned skill-runs finished, the two consolidated report files are written, and all 5 planned
governance/tracking docs are reconciled — plus `BACKLOG.md` and `PHASE_GATES.md`.

**What happened in this batch:**
1. Ran the 4 remaining skills: `observability-analytics-readiness` (found zero custom
   analytics events and zero uptime monitoring — see fix below), `rollback-risk-register` (found
   the VPS mirror was still serving pre-fix code — see deploy below), `production-readiness-audit`
   (capstone — overall verdict: Conditionally ready), `client-handoff-pack` (non-technical summary
   produced for the business owner).
2. **Fixed:** added `.github/workflows/uptime-check.yml` — checks the VPS mirror every 30 minutes,
   verified via a real triggered GitHub Actions run (not just local simulation). Committed/pushed
   as `8588925`.
3. **User explicitly authorized deploying this session's fixes to the VPS mirror.** Built the
   exact deploy file set (matching the existing live footprint, not a raw rsync filter), dry-ran
   the sync, applied it, then verified live via fresh `curl`: new `Content-Length`/`Last-Modified`,
   new testimonials copy present, old placeholder text gone, new contact-form JS logic present,
   new contrast values (`--coral-fill`) present on `.nav__cta` and `.phone-strip`.
4. Wrote `seo/audits/hygiene-2026-07-19.md` and
   `docs/governance/audits/production-readiness-2026-07-19.md` (the full audit findings, previously
   only in chat).
5. Reconciled `docs/governance/{PROJECT_RISK_REGISTER,SECURITY_BASELINE,COMPATIBILITY_MATRIX,
   ROLLBACK_PLAN,RELEASE_GATE,PHASE_GATES,AGENT_RUN_LOG}.md` and `BACKLOG.md` against every finding
   from all 13 skill-runs — including correcting the already-stale R-005 entry (closed properly
   this time) and adding 7 new tracked risks (R-007 through R-013).

**Why:** the prior push's own "Remaining Follow-Up" said this was still open; the user then asked
to continue ("Finish the production-readiness audit" — confirmed at that push's own session-end
Step 4a gate), then separately authorized the VPS deploy once the rollback-risk-register step
surfaced that gap.

**Verified:** uptime-check workflow confirmed working via a real GitHub Actions run (not simulated).
VPS deploy confirmed via fresh `curl` evidence post-deploy, not just a successful rsync exit code.

**Known follow-up, not fixed this batch:** Formspree ID still a placeholder (user-owned); 9
oversized images not yet optimized; no HSTS header yet; no custom Plausible events for
form success/failure yet; WebKit/Firefox/iOS Safari still untested. All tracked in `BACKLOG.md`
and `PROJECT_RISK_REGISTER.md`.

---

## Completed Previously — Contact Form, Contrast, Touch Target, and Content Fixes (2026-07-19, branch `main`)

```text
Branch: main
Tag:    v2.8.0__contact-form-contrast-touch-target-content-fixes__commit-e6d1363
```

**Context:** the user asked whether re-running the 18/20-skill production-readiness suite
(installed 2026-07-15, never actually executed) would add value. Researched via parallel Explore
agents + a Plan agent, produced a scoped plan (`main` only, 13 of ~20 skills — several were
structurally inapplicable to a static no-backend site and explicitly skipped), user approved full
scope. **9 of 13 planned skill-runs completed this session**: `repo-safety-preflight`,
`v33-quality-gate-enforcer`, `component-inventory`, `seo-hygiene-check`, `accessibility-pass`,
`performance-budget-pass`, `responsive-browser-matrix`, `content-conversion-readiness`,
`deployment-nginx-https-readiness`. **Remaining, not yet run:** `observability-analytics-readiness`,
`rollback-risk-register`, `production-readiness-audit` (capstone), `client-handoff-pack`. The two
planned consolidated report files (`seo/audits/hygiene-2026-07-19.md`,
`docs/governance/audits/production-readiness-2026-07-19.md`) and full tracking-doc reconciliation
(`PROJECT_RISK_REGISTER.md`, `SECURITY_BASELINE.md`, `COMPATIBILITY_MATRIX.md`, `RELEASE_GATE.md`,
`AGENT_RUN_LOG.md`) are **also not yet done** — this push covers only the real code fixes the audit
surfaced along the way, not the audit's own paperwork. All of that remains open follow-up.

**What changed (5 real fixes, all on `main`):**
1. **Contact form silently failed while claiming success** — `js/main.js`'s submit handler
   unconditionally faked the success state regardless of whether anything was actually sent.
   Rewrote to a real `fetch()` POST with honest success/error handling; added a matching
   `#form-error` state in `contact.html`.
2. **WCAG AA contrast failures** on `.nav__cta`, `.section--coral`, and (found during
   verification) `.phone-strip` — all migrated to the already-established `--coral-fill` token.
3. **Mobile nav hamburger touch target** enlarged from 38×31px to 44×44px; bundled an open/closed
   "X" icon animation (CSS-only, driven by the existing `aria-expanded` attribute).
4. **Escape key** now closes the mobile nav and returns focus to the hamburger button.
5. **Homepage testimonials section** was showing literal, unfilled bracket-placeholder text —
   reframed to three honest, first-party trust statements (see
   `docs/project/DECISION_LOG.md` 2026-07-19).

**Why:** all 5 surfaced directly from the accessibility, content-conversion, and responsive audit
passes above — not pre-planned, but each was small, scoped, low-risk, and explicitly approved by
the user before implementation (either inline or via a short Plan Mode cycle for the testimonials
content decision).

**Verified:** no build/lint/test scripts exist (static site). Each fix verified locally via
Playwright: contrast via computed-style checks against the `--coral-fill` RGB value; the form via
mocked `fetch` responses (200 → success shown, 500 → error shown, button re-enabled) *and* a real
request against the actual placeholder Formspree URL (confirms it now fails honestly instead of
faking success); Escape-key/focus-return behavior; hamburger touch-target bounding-box measurement
and X-icon transform values; testimonials section screenshots at 390px/1440px (after correctly
scrolling the section into view to trigger its `.reveal` animation before capturing — an initial
screenshot attempt captured the pre-animation `opacity:0` state, caught and corrected). Zero
console errors, zero horizontal overflow at any tested viewport across all fixes.

**Known follow-up, not fixed this push:** `contact.html`'s Formspree ID is still the placeholder
`REPLACE_WITH_FORMSPREE_ID` — the form now fails/succeeds honestly, but won't actually deliver a
message until a real ID is configured (needs the user's own Formspree account). WebKit/Firefox/iOS
Safari testing was not performed — only a Chromium binary is available in this environment.
`oldfashioncare.com`'s hosting mismatch (confirmed again this session, still serves an unrelated
live "Jottful" application) — **user confirmed this is expected/known right now, not a blocker.**

---

## Completed Previously — Repo Push-Workflow Governance Fix (2026-07-18, branch `main`)

```text
Branch: main
Tag:    v2.7.0__push-workflow-tracking-file-gap-fix__commit-8ed5902
```

**What changed:** `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` and
`PROGRESS_NOTE.md` only — no site code. A follow-up session running
`REPO_SESSION_START_RECOVERY_AUDIT.md` found `PROGRESS_NOTE.md` was stale (still describing
2026-07-15/07-12 state, missing the founder-photo fix above entirely). Root cause: this repo's own
push prompt told the agent `PROGRESS_NOTE.md` and `PROGRESS_NOTES.md` were alternatives ("prefer
`PROGRESS_NOTES.md`"), so the previous push updated the plural historical log and silently skipped
the singular current-state file — despite `CLAUDE.md`/`AGENTS.md`/`REPOSITORY_HANDOFF_CONFIG.md`
all marking it required "unconditionally."

**Fixed:** (1) `PROGRESS_NOTE.md` content corrected to the actual current state. (2) The push
prompt: `PROGRESS_NOTE.md` moved into the mandatory "every push" list, the ambiguous
"prefer `PROGRESS_NOTES.md`" guidance removed, a name-by-name `git diff --stat` cross-check added
before commit (Section 6), and a new Section 2a added requiring the linked AntBrainOS vault project
folder (`03_PROJECTS/Active/Old_Fashion_Care/`) to be updated on **every** push, not only at session
end — closing a gap where the vault-side record could otherwise drift for days across many pushes.

**Why:** the user asked whether the repo-local fix was mirrored in AntBrainOS (it wasn't — the
vault's canonical copy of this same prompt carried the byte-identical bug, confirming no mechanical
sync exists between a repo's local prompt copy and the vault template), then asked for "no gaps...
system to system, or session to session." The vault's canonical prompt template and
`CLAUDE_CODE_SESSION_END.md` (new independent Step 18a cross-check) were hardened in parallel — see
`docs/project/DECISION_LOG.md` 2026-07-18 for this repo's side of that decision, and the vault's own
`08_DECISIONS/DECISION_LOG.md` #36 for the full cross-project write-up.

**Verified:** no build/lint/test scripts exist (static site) — verification here is a manual
line-by-line review confirming (a) `PROGRESS_NOTE.md`'s "Latest"/"Current Progress" sections match
this push's actual state, (b) the push prompt's Section 2/2a/4/6 read consistently with no
remaining "prefer the other file" language, (c) `git diff --stat` includes every file this repo's
own required-tracking list names for this push (see Validation section below).

---

## Completed Previously — Fix Distorted Founder Photo on `about.html` (2026-07-18, branch `main`)

```text
Branch: main
Tag:    v2.6.0__founder-photo-aspect-ratio-fix__commit-dd88bf4
```

**What changed:** `css/style.css` only — added `aspect-ratio: auto;` to `img.founder-photo`.
The class was still carrying `aspect-ratio: 3/4` (desktop) / `16/9` (mobile) from an older
`.founder-photo` placeholder-box rule that predates the real photo. Since nothing reset
`aspect-ratio` on the actual `<img>`, the browser's default `object-fit: fill` stretched the
real 354×514 portrait of Regina Booker to fit those mismatched boxes — worst on mobile, where
a portrait photo was forced into a 16:9 landscape box.

**Why:** user reported the photo looked compressed on their phone at
`old-fashion-care.craftandconscious.com/about.html`.

**Verified:** locally via Playwright at 390/768/1440 — image renders at its natural ~0.69
ratio at every width, 0 console errors. Bounding-box measurements matched the image's real
intrinsic ratio exactly (before: 1.78 mobile / 0.75 desktop; after: 0.689 at both). Re-verified
against the live deployed URL post-push (see Deployment note below).

**Deployment note (important, unrelated to the photo bug):** this repo's own canonical
URLs/sitemap/`REPOSITORY_HANDOFF_CONFIG.md` name `oldfashioncare.com` as the Netlify
production domain, but it currently resolves to an unrelated third-party site built on
"Jottful" — not this repository, on Netlify or otherwise. The URL the user actually checks,
`old-fashion-care.craftandconscious.com`, is a separate VPS/nginx deployment of this repo kept
in sync via manual `git archive` + rsync (same mechanism as the hero-variant preview
subdomains) — it was frozen at commit `fd7543c` (2026-07-12, `main`'s last site-affecting
commit before this one) until this push's re-sync. The `oldfashioncare.com` domain mismatch is
flagged here for visibility; it is a pre-existing condition, not introduced or fixed by this
push.

---

## Completed Previously — AntBrainOS Kit Tooling Install Across All 5 Branches (2026-07-15)

```text
Trunk tag: v2.5.0__install-antbrainos-kit-tooling-all-branches__commit-7818660 (on main's tooling commit)
```

Installed dev-tooling kits (from the AntBrainOS `20_TOOLS/KITS` collection) onto **all 5 branches**,
one commit each, at the user's request:

```text
main                                        7818660  + SEOKit, EngKit, TradeKit, handoff-repository
migration/project-starter-v3-3              190caf4  + SEOKit, EngKit, TradeKit, handoff-repository
design/editorial-sage-elder-friendly        4da3df3  + EngKit, TradeKit, handoff-repository (SEOKit already there)
design/editorial-sage-hero-cream-immersive  25647aa  + EngKit, TradeKit, handoff-repository (SEOKit already there)
design/editorial-sage-hero-split-depth      6645f6e  + EngKit, TradeKit, handoff-repository (SEOKit already there)
```

- **Value-driven kit selection:** installed the kits with genuine fit (SEOKit, EngKit, TradeKit,
  handoff-repository). **Skipped** EcomKit and VideoKit (no ecommerce/video surface on a static
  elder-care site) and MKTKit (previously evaluated and rolled back here as a poor fit).
- **Live site provably unchanged:** every commit is dev-tooling only — no `.html`/`css`/`js`/
  `images`/`netlify.toml` edits. The `main` live-site guard diff (`fd7543c..7818660` over site
  paths) is **empty**; `netlify.toml` publishes repo root with no served route to `.claude/`, so the
  Netlify deploy is byte-for-byte unchanged. Same site-file diff was empty on every branch.
- **Not a design change or a merge** — the redesign/hero-variant branches remain separate, un-made
  decisions; this only adds tooling.

**Validation:** cross-branch matrix confirmed each branch carries eng=35 files, tradekit=7 cards,
handoff skill+agent+config, seo=19 commands; EngKit validator PASSED (26 commands); staging guards
confirmed no site/local-only files were ever committed. Static site — no build/test/lint scripts.

---

## Current Phase

```text
Phase 2 — production-readiness audit COMPLETE (13 of 13 planned skill-runs done; 6 real fixes
landed, verified, and deployed live to the VPS mirror; both report files written; all 6
governance/tracking docs reconciled). No active implementation slice — next steps are the
user-owned/optional follow-ups listed in BACKLOG.md and PROJECT_RISK_REGISTER.md.
```

---

## Current Branch

```text
main
```

---

## Current Slice

```text
None active — the production-readiness audit pass on `main` (2026-07-19) is complete. All 13
planned skills ran: repo-safety-preflight, v33-quality-gate-enforcer, component-inventory,
seo-hygiene-check, accessibility-pass, performance-budget-pass, responsive-browser-matrix,
content-conversion-readiness, deployment-nginx-https-readiness, observability-analytics-readiness,
rollback-risk-register, production-readiness-audit (capstone), client-handoff-pack. 6 real issues
were fixed, verified, and deployed live (not left as a findings list only): the contact form's
fake-success bug, 3 coral-contrast WCAG AA failures, the mobile nav hamburger's undersized touch
target (+ icon-state improvement), missing Escape-key handling, the homepage's placeholder
testimonials section, and missing uptime monitoring. Both consolidated report files are written
(seo/audits/hygiene-2026-07-19.md, docs/governance/audits/production-readiness-2026-07-19.md) and
all 6 governance/tracking docs reconciled.
```

---

## Current Goal

> No active goal — see `BACKLOG.md` and `docs/governance/PROJECT_RISK_REGISTER.md` (R-007 through
> R-013) for the remaining, mostly user-owned or optional follow-ups: configuring Web3Forms in
> `contact.html` (decided 2026-07-22, blocked on the client finalizing the purchase),
> image optimization, HSTS header, custom form-analytics events, a dedicated apple-touch-icon, a
> decision on the `care giver pics/` folder, and a real-device iOS Safari check. None are urgent;
> ask the user which (if any) to pick up next.

---

## Completed This Push — Hero Photo Composition Rework (2026-07-12)

Three related changes to `css/style.css` + `images/hero-ai.jpg`, all verified via Playwright
headless rendering + canvas pixel-sampling across 1024/1440/1790/2560px desktop widths plus
768px tablet and 390px mobile (no PIL/ImageMagick available in this environment):

- **25%-navy / 75%-image ratio enforced:** widened the photo band (`--hero-photo-w` 57%→75%,
  band's left edge 43%→25%) and compressed the `.hero::after` overlay gradient stops so solid
  navy ends by ~18-25% instead of the prior ~43-47%. Pixel-sampled confirmation: navy now ends at
  ~25% at every desktop width (was ~43-50% before), image visible across the full right 75%.
- **Hi-res photo swap:** replaced the old soft, low-res `images/hero-ai.jpg` (1100x934, a crop of
  a mockup) with a resample of the client's clean hi-res original (1400x933, q85, comparable file
  weight). Source PNG kept locally at `images/ChatGPT Image Jul 12, 2026 at 05_07_20 AM.png`
  (untracked, matches the existing `.gitignore` `images/ChatGPT*` rule).
- **Full-frame desktop display, no seam:** added `@media(min-width:1024px){ .hero{ min-height:
  clamp(660px, 50vw, 900px) } }` so the photo band's aspect ratio matches the photo's own (~1.5),
  letting `cover` show the entire frame (full lamp, full heads, more left curtain) instead of
  cropping the top on wide screens. Reverted an earlier `background-size: contain` attempt at the
  same goal after it proved to grow the navy column with viewport width and reintroduce a hard
  seam on wide monitors — full-width `cover` has neither problem by construction.

**Known tradeoff, flagged for awareness, not blocking:** the desktop hero is now taller on wide
screens (up to ~900px, e.g. ~895px at 1790px width) — the cost of showing the full photo without
cropping. Tablet (641-1023px) and mobile (<=640px) layouts are completely unchanged (pixel-verified
identical before/after).

---

## Completed Previously — Hero Contrast + Gradient Fix (2026-07-11)

Two changes to `css/style.css`, both verified via Playwright-driven pixel sampling (no
ImageMagick/PIL available in this environment — used `npx --no-install playwright` with chromium
already cached locally, driving a canvas-based pixel sampler instead):

- **Button contrast:** added `--coral-fill: #C2512B` (new token, ~4.66:1 vs white — `--coral`
  alone measures 3.87:1, below the 4.5:1 AA threshold for normal-size text; confirmed `.btn--lg`
  at 15px/weight 600 does not qualify as WCAG "large text"). `.btn--coral` background/border now
  use `--coral-fill`; `--coral` itself is untouched (still used for the headline `<em>`). Verified
  by sampling the rendered button pixel: `(217,97,57)` → `(196,89,52)`, matching the intended
  token shift. Hover (`--coral-dark`) unchanged, still darkens further on hover.
- **Gradient/photo band repositioned left:** `.hero::after`'s 10-stop overlay gradient stops
  compressed/shifted left (e.g. 46%→39%, 52%→46%, 60%→52%, 100%→60% — full table in commit notes),
  and `--hero-photo-w` widened 53%→57% (band's hard left edge 47%→43%) to reach the zone the user
  marked in their reference screenshot. Verified by pixel-sampling before/after screenshots at
  1200/1440/700/390px: real, correctly-directioned change exactly where the math predicts (zero
  diff left of the old band edge, consistent lightening from x≈564px rightward at 1200px
  viewport), zero diff on mobile (390px, untouched media query).
- **Known tradeoff, flagged for awareness, not blocking:** widening the band moved the photo (and
  the older woman's face) left along with it. Measured headline-text-end-to-hairline gap shrank
  from ~125px (10.4% of a 1200px viewport) to ~80px (6.7%) — still above the ~60px/5% minimum this
  project's own history established as safe (see `docs/project/COMMIT_NOTES.md`), but a real
  reduction worth knowing about if future sessions touch this again.
- Fixed two stale explanatory comments above `.hero::before`/`.hero::after` that described the
  pre-change opacity/edge numbers.
- Found but explicitly out of scope: `.nav__cta` and `.section--coral` have the same coral-fill/
  white-text contrast pattern as the button fix — not touched this pass.

---

## Completed Previously — V3.4 Doc Consolidation (2026-07-07)

Reconciled the V3.4 scaffolding (installed 2026-07-05, merged to `main` 2026-07-06) with this
project's real v3.3 root docs, since the installed V3.4 skills (`v34-execution-loop`,
`v34-production-readiness`) hardcode `docs/governance/` and `docs/project/` paths and would
otherwise only ever see empty generic templates:

- Moved 8 root docs with real content into `docs/project/` (overwriting empty stubs): `STATUS.md`,
  `CHANGELOG.md`, `COMMIT_NOTES.md`, `CONTEXT.md`, `DECISION_LOG.md`, `PROJECT_BRIEF.md`,
  `RELEASE_NOTES.md`, `ROADMAP.md`. Authored real content for `docs/project/ARCHITECTURE.md` (new,
  no root equivalent).
- Merged 6 governance file pairs into `docs/governance/` (root had real/customized content, the
  V3.4 versions were generic unpopulated templates): `CHANGE_CONTROL.md`, `ROLLBACK_PLAN.md`,
  `REPO_HEALTH_CHECK.md`, `DONE_CRITERIA.md`, `PROJECT_RISK_REGISTER.md` (authored real,
  project-specific risks — neither side had any before), and `PHASE_GATES.md` (the two sides had
  genuinely different gate taxonomies — kept V3.4's Gate 0–5 structure, populated with this
  project's real completion signals). Deleted all 6 root duplicates once merged.
- Rewrote `AGENTS.md` and `CLAUDE.md`: kept the v3.3-specific content (phase-gate language, the
  7-sub-agent workflow, the required-tracking-files list — now pointing at `docs/project/*` where
  files moved) and added the V3.4 candidates' genuinely new, non-conflicting pieces (structured
  "Output Standard" report format, explicit no-auto-commit line, references to the now-installed
  V3.4/production-readiness skills).
- Resolved `ai/agents/AGENT_REVIEW_GATES.md`: kept the real 7-agent-specific table, added the
  candidate's new "Review Gate D — Migration Safety" section.
- Added a pointer note to `ai/agents/SUBAGENT_ROLES.md` clarifying the 7 real sub-agents are
  authoritative, not the generic V3.4 role names.
- Deleted all 3 remaining `.v34_migration_review/*.v34-candidate` files (content merged) and
  removed the now-empty `.v34_migration_review/` directory.
- Updated `README.md`'s Project Control Files table and added a migration note to the top of
  `START_HERE.md` (legacy v3.1 doc) pointing at the new file locations.
- Fixed 4 live, actively-referenced files that still pointed at the old root paths:
  `ai/README.md`, `.claude/agents/project-steward.md`,
  `.claude/skills/v33-quality-gate-enforcer/SKILL.md`, `docs/WORKFLOW.md`.

---

## Completed Previously — Merge (2026-07-06)

Merged `migration/project-starter-v3-3` into `main` (`--no-ff`, real merge commit, not a
fast-forward). The two branches had diverged since 2026-06-30: `main` had picked up its own commit
(`76ea00e`, tagged `v1.6.0`/`v1.6.1`, adding the refined handoff-prompt file) while the migration
branch continued independently with 4 more commits ending in `5a7abc8` (SEO audit + Production
Readiness Skills deployment, tag `v1.5.5`).

- `git merge-tree` dry run (read-only, performed before merging) showed exactly 3 real conflicts:
  `COMMIT_NOTES.md`, `PROGRESS_NOTES.md`, `STATUS.md` — all append-style logs both branches had
  added different entries to. No conflicts in any HTML/CSS file or in `.claude/skills/`, `.agents/`,
  `docs/governance/`, `docs/project/` (main didn't have those paths at all — clean additions).
  `LESSONS_LEARNED.md` and the handoff-prompt file auto-merged cleanly (only one side had changed
  each).
- Resolved the 3 conflicts by combining both sides' entries (no content dropped) rather than
  picking one side.
- Pre-merge safety snapshot of `main` taken at
  `/Volumes/AntNVMe1TB/WorkSync/Projects/RepoBackups/Old-Fashion-Care/v1.6.1__pre-v2-merge-snapshot__commit-76ea00e`
  (223 files, verified against `git ls-tree`).

**Production Readiness Skills deployment (now on main):**
- Project Starter Kit V3.4 baseline (migrate mode) — additive only; `v34_validate.py` → PASS.
  3 real conflicts (`AGENTS.md`, `CLAUDE.md`, `ai/agents/AGENT_REVIEW_GATES.md`) quarantined into
  `.v34_migration_review/*.v34-candidate` for manual review — existing files untouched.
- 18-skill production-readiness suite installed into `.claude/skills/` (22 total skills: 4 V3.4 +
  18 PR suite). No name conflicts.
- New top-level additions: `.agents/`, `.claude/skills/`, `docs/governance/`, `docs/project/`,
  `ai/agents/SUBAGENT_ROLES.md`, `ai/prompts/*`, `00_MIGRATION_KICKOFF.md`, `MIGRATION_REPORT.md`,
  `V34_INSTALL_REPORT.json`.
- Known overlap: `docs/project/*.md` (new, from V3.4) duplicates the root-level tracking docs
  already in active use in this repo — not yet reconciled.

**SEO audit (now on main):**

SEO audit across `index.html`, `about.html`, `services.html`, `how-it-works.html`,
`questions.html`, `contact.html`:

- Titles, meta descriptions, canonical URLs, Open Graph tags, Twitter Card tags, `sitemap.xml`,
  and `robots.txt` were already correct and consistent across all 6 pages.
- **Fixed:** `og:image` / `twitter:image` on all 6 pages pointed to `images/og-default.png`, which
  does not exist in the repo (broken link-preview image on Facebook/LinkedIn/iMessage/X for every
  page). Repointed to the existing `images/hero-ai.jpg` and corrected `og:image:width`/`:height`
  from the placeholder 1200×630 to the file's actual 1100×934.
- **Fixed:** `<link rel="apple-touch-icon" href="/images/apple-touch-icon.png">` on all 6 pages
  referenced a file that does not exist (broken iOS "Add to Home Screen" icon). Removed the tag;
  browsers fall back to the existing `favicon.svg`.
- Verified: no other local `href`/`src` references across the 6 pages point to missing files.

---

## Completed Previously (c04e819 — 2026-06-30)

Three hero CSS iterations completed and pushed since last handoff (a2e7c9a):

**ea8b067** — Hero gradient further left (B-period anchor) + wider sub overlap:
- `.hero::after` gradient stops shifted so opacity ≈0.50 at the B-period position (x≈510, 33%)
- `.hero__sub` max-width 582→648px — lower text lines clearly overlap caregiver's arm/shoulder/hand
- Verified desktop 1554×900 + mobile 390×820

**b195aba** — Hero smoother gradient + headline 1-line fix + sub barely-covers-hand:
- `.hero__headline` max-width 565→620px — fixed "We make that possible." regression to 2 lines
- `.hero::after` 10-stop smooth gradient; visible fade anchored at 'h' in "where" (~22%); seam at 46% covered at 0.26 opacity
- `.hero__sub` font-size 0.97→0.94rem, max-width 648→580px — lines barely reach hand, shoulder uncovered
- Verified desktop 1554×900 + mobile 390×820

**c04e819** — Handoff tracking docs (this commit):
- CHANGELOG.md: v1.5.0 entry added
- SLICE_REVIEWS.md: 2026-06-30 entry added
- PROGRESS_NOTE.md: updated to current state

Tag `v1.5.0__hero-smoother-gradient-headline-fix__commit-b195aba` applied to b195aba and pushed.
Snapshot saved to RepoBackups for b195aba.

---

## Current Blocker

> None.

---

## Next Action

> The production-readiness audit is complete — nothing is actively in-progress. Remaining items,
> none urgent, ask the user which to pick up next:
> 1. User-owned, not agent-actionable: configure Web3Forms in `contact.html` (vendor decided
>    2026-07-22 — see `docs/project/DECISION_LOG.md`) — blocked on the client finalizing the
>    purchase.
> 2. Add `plausible()` custom events for contact-form success/failure (small, no dependency).
> 3. Resize/recompress the 9 oversized `care-*.jpg` images (~14.2MB avoidable, quantified).
> 4. Add HSTS header; create a dedicated apple-touch-icon.png (both small, long-queued).
> 5. Decide what to do with the 15MB unreferenced `care giver pics/` folder (found this session).
> 6. Get a real-device iOS Safari / cross-browser check (this environment only has Chromium).
> 7. `oldfashioncare.com` hosting mismatch — user has confirmed this is expected/known right now;
>    no action needed unless the user raises it again.

---

## Last Verified Working State

```text
Contact form, contrast, touch-target, and content fixes on main — 2026-07-19 (5 fixes, verified
via Playwright, not yet committed/tagged/pushed at the time this section was written — see the
Latest Push entry above for the pending tag).
Immediate prior good state: push-workflow governance fix — 2026-07-18, tag v2.7.0 (8ed5902),
trailing docs commit 358e0b9.
Prior good state before that: founder-photo aspect-ratio fix — 2026-07-18, tag v2.6.0 (dd88bf4).
Contact form: real fetch() submission with honest success/error states (was: unconditional fake
success regardless of actual submission).
Contrast: .nav__cta, .section--coral, .phone-strip all now use --coral-fill (~4.66:1) instead of
--coral (~3.87:1, failed WCAG AA for normal text).
Mobile nav: hamburger touch target 44x44px (was 38x31px); open/closed X-icon state added; Escape
key closes the menu and returns focus to the hamburger.
Homepage testimonials section: reframed from literal bracket-placeholder text to 3 honest
first-party trust statements; section--dark visual structure unchanged.
GitHub repo: Old-Fashion-Care — git@github.com:rmz9dkfy5f-pixel/Old-Fashion-Care.git
Confirmed-working deploy target: VPS mirror old-fashion-care.craftandconscious.com (manually
synced, not Netlify). oldfashioncare.com serves an unrelated third-party site — user-confirmed
expected, not a blocker.
```

---

## Validation Performed This Push

```text
Contact form / contrast / touch-target / content fixes (2026-07-19): static site, no build/lint
scripts. All verified via local Playwright (Chromium only — no WebKit/Firefox binaries available
in this environment):
- Contrast: computed getComputedStyle().backgroundColor on .nav__cta and .section--coral resolved
  to rgb(194, 81, 43) (--coral-fill) both before and after finding/fixing the .phone-strip cascade
  collision that was silently overriding the .section--coral fix on the one real element using it.
- Contact form: mocked fetch responses via page.route() — 200 response shows success state and
  hides the form; 500 response shows the new error state, re-enables the submit button, keeps the
  form visible. A fourth run hit the REAL (still-placeholder) Formspree URL with no mock — confirms
  it now fails honestly (error state shown) instead of the old unconditional fake success.
- Escape key: closes the mobile nav, sets aria-expanded=false, returns focus to the hamburger
  button — confirmed via document.activeElement.
- Hamburger touch target: bounding-box measured at exactly 44x44px post-fix (was 38x31px);
  computed transform values on the 3 icon bars confirmed a clean X-morph when aria-expanded=true,
  confirmed visually via screenshot.
- Testimonials section: grep confirms zero remaining "[Testimonial to be added" or "[First name,
  last initial]" placeholder strings. Screenshots at 390px/1440px after scrolling the section into
  view (to trigger its .reveal IntersectionObserver animation before capturing — an initial
  screenshot attempt captured the pre-animation opacity:0 state, caught and corrected).
- Zero horizontal overflow (document.documentElement.scrollWidth === clientWidth) and zero console
  errors confirmed across all of the above, at every viewport tested (360-1920px).
git diff reviewed file-by-file after each fix to confirm scope matched intent.

Hero fix (2026-07-11): static site, no build/lint scripts. Verified via Playwright
(npx --no-install playwright, chromium cached locally — no PIL/ImageMagick available in this
environment) screenshotting index.html before/after at 1200/1440/700/390px viewports, then a
canvas-based pixel sampler (same Playwright chromium instance, images served over a throwaway
local HTTP server to avoid file:// canvas tainting) to numerically confirm: button fill color
shift, gradient lightening exactly in the predicted x-range with zero diff outside it, mobile
pixel-identical before/after, and the text-to-hairline gap measurement (125px -> 80px). No hard
seam found via direct zoomed-crop visual inspection of the new band edge. Hit and resolved an
environment ENOSPC (disk-full) error mid-session that blocked Bash entirely for a few minutes;
confirmed via `df -h` once resolved (7.6Gi free at the low point) and re-ran the blocked steps.

Static site — no build/lint scripts.
V3.4 consolidation (2026-07-07): git status reviewed — 17 deletions (root files consolidated),
25 modifications (docs/governance/, docs/project/, AGENTS.md, CLAUDE.md, README.md, START_HERE.md,
ai/README.md, .claude/agents/project-steward.md, .claude/skills/v33-quality-gate-enforcer/SKILL.md,
docs/WORKFLOW.md, ai/agents/AGENT_REVIEW_GATES.md, ai/agents/SUBAGENT_ROLES.md) — matches the
planned scope exactly, no unexpected changes. v34_validate.py re-run → PASS. Local href/src
resolution sweep re-run across all 6 pages → clean (docs-only change, expected no regression).
grep swept for dangling references to the 6 deleted root governance files across all .md files;
fixed 4 live files that still pointed at old paths (ai/README.md, .claude/agents/project-steward.md,
.claude/skills/v33-quality-gate-enforcer/SKILL.md, docs/WORKFLOW.md) — remaining hits are in
.starter-kit/archive/ (frozen historical install templates) and generic Prompts/*.md templates,
left untouched as out of scope. .v34_migration_review/ confirmed empty and removed.

Merge (2026-07-06): git merge-tree dry run identified exactly 3 conflicts before merging; all
resolved by combining both sides' content (verified no <<<<<<</=======/>>>>>>> markers remain
post-resolution). Pre- and post-merge snapshots taken and verified by file count against
git ls-tree. v34_validate.py re-run on main post-merge. Local href/src resolution sweep re-run
across all 6 pages post-merge.

SEO audit (2026-07-05): grep-based inventory of <title>/meta description/canonical/OG/Twitter tags
across all 6 pages — all correct prior to this session. Confirmed images/og-default.png and
images/apple-touch-icon.png referenced but missing via `ls`. Post-fix: grep confirms zero remaining
references to og-default.png or apple-touch-icon.png; a scripted check of every local href/src
across all 6 pages confirms every referenced local file now exists.

Production Readiness Skills install (2026-07-05): `python3 v34_validate.py --target .` → PASS
(no missing required files, no skill errors). `find .claude/skills -name SKILL.md | wc -l` → 22
(expected). `grep -rl "Smart Learning" .claude/skills/` → clean after fix. `git status --short`
confirms no existing tracked file was modified/deleted by either installer — all changes are new
files plus the 3 quarantined candidates.
```

---

## Open Questions

- [ ] Which photos from care-07 through care-11 should replace any in the current grid?
- [ ] Should a dedicated apple-touch-icon.png (180×180) be created later, or is the favicon.svg
      fallback acceptable long-term?
- [x] ~~How should `docs/project/*.md` be reconciled with the root-level tracking docs?~~ Resolved
      2026-07-07 — real content moved into `docs/project/`/`docs/governance/`, root duplicates
      deleted.
- [x] ~~Review the 3 quarantined `.v34_migration_review/` candidates.~~ Resolved 2026-07-07 — all 3
      reconciled by hand (kept project-specific content, added genuinely new V3.4 pieces).

---

## Approval Status

Phase 1 approved?

```text
Yes
```

Phase 2 approved?

```text
Yes — this push's 5 fixes were each explicitly approved before implementation: the contact-form/
contrast/Escape-key fixes via an inline "let's fix all three" after the Accessibility Pass report;
the hamburger touch-target+icon fix via an "approved" after the Responsive report; the testimonials
reframe via a short Plan Mode cycle (plan written, approved via ExitPlanMode) after the
Content-Conversion-Readiness report. Prior approvals (SEO-audit fixes, Production Readiness Skills
install, migration-branch merge — 2026-07-05/06) remain historical record, superseded by the above
for current-state purposes.
```
