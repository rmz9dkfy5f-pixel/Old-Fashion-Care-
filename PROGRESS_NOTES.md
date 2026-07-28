# Progress Notes

Historical project progress notes.

Current active progress belongs in `PROGRESS_NOTE.md`.

---

## 2026-07-28 — Add dedicated apple-touch-icon.png (180×180)

**Work completed:**
- Rasterized the repo's own `favicon.svg` lettermark (rounded charcoal square, centered coral "O")
  to a 180×180 PNG via a throwaway Playwright render (HTML wrapper, SVG inlined at fixed 180×180,
  background matched to the SVG's `#2A2825` fill for seamless corners); script not committed,
  scratch-only.
- Verified output dimensions (`sips -g pixelWidth -g pixelHeight`, 180×180 confirmed) and visual
  correctness before promoting into the repo at `images/apple-touch-icon.png`.
- Added `<link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon.png">` to all
  6 pages' `<head>`, directly below the existing favicon `<link>` — textually identical on all 6.
- Recorded a short `docs/project/DECISION_LOG.md` entry clarifying this doesn't contradict the
  2026-07-05 decision to remove the broken apple-touch-icon tag.

**Files or areas changed:** New `images/apple-touch-icon.png` (binary). Modified: `index.html`,
`about.html`, `services.html`, `how-it-works.html`, `questions.html`, `contact.html` (one line
each); `BACKLOG.md`, `docs/governance/PROJECT_RISK_REGISTER.md` (R-006 closed as R-C09),
`docs/project/DECISION_LOG.md`, `docs/project/CHANGELOG.md`, `SLICE_REVIEWS.md`,
`docs/project/COMMIT_NOTES.md`, `PROGRESS_NOTE.md`, `docs/project/STATUS.md`.

**Validation performed:** Grep sweep confirmed exactly 6 files contain the new tag, textually
identical, and `hero-preview.html` (dev-only, noindex) contains zero. `git diff --stat` confirmed
scope: 6 HTML files + 1 new PNG + tracking docs, nothing else; `netlify.toml` untouched. No
automated test suite exists for this static site; verification was manual/scripted.

**Notes for the next agent:** no commit, tag, snapshot, or push in this batch — staged only, per
this repo's phase-gate convention, awaiting explicit user go-ahead. All other open items unchanged
(Web3Forms configuration blocked on client purchase, form-analytics events, HSTS header, `care
giver pics/` folder decision, care-07–11 review, iOS Safari check).

---

## 2026-07-23 — Image optimization: compress the 4 live `care-*.jpg` photos

**Work completed:**
- Resized 4 oversized, live-referenced photos (`care-03`/`04`/`05`/`06`) from native 3507–5760px to
  ~800×533px (landscape) / 533×800px (portrait) via macOS `sips -Z 800 -s formatOptions 82`.
- Resized to `/tmp/ofc-image-opt/` scratch location first, inspected each file's dimensions/EXIF
  orientation, promoted into place via `cp` once all 4 passed verification.
- Confirmed scope: `git diff --stat` showed exactly the 4 image files changed (binary), no other
  modifications.

**Files or areas changed:** `images/care-03.jpg`, `images/care-04.jpg`, `images/care-05.jpg`,
`images/care-06.jpg` (binary image files only); `BACKLOG.md` (moved image optimization to
Completed, kept the separate `care-07`–`11` review decision untouched), `PROJECT_RISK_REGISTER.md`
(R-007 marked Partial), `PROGRESS_NOTE.md`, `COMMIT_NOTES.md`, `CHANGELOG.md`, this file,
`SLICE_REVIEWS.md`.

**Validation performed:** Dimension checks via `sips -g pixelWidth -g pixelHeight` on both original
and resized files (confirmed 533×800 for portrait, 800×533 for landscape, matching the resize plan).
EXIF orientation check via `sips -g orientation` (all nil, no unexpected rotation). File size
comparison (before: 6.96 MB total; after: 0.428 MB total; 93.9% reduction). `git diff --stat`
confirmed scope (exactly 4 image files, binary diffs, no other changes). No automated test suite
exists for this static site; Playwright visual verification was not available in this environment,
but inspection covered dimension/orientation integrity.

**Notes for the next agent:** the 5 unreferenced dead files (`care-07`–`11`, ~5.5 MB) were
deliberately left untouched per user scope decision — separate `BACKLOG.md` item ("Review whether
care-07–11 should replace any current grid photo") remains open and unstarted. All other open
backlog items unchanged from prior sessions (Web3Forms configuration, form-analytics events, HSTS
header, apple-touch-icon, `care giver pics/` folder decision, iOS Safari check). With this task
complete, no new agent-actionable items remain open.

---

## 2026-07-22 — Contact form vendor decision: Web3Forms

**Work completed:**
- User directly decided the contact form's eventual delivery vendor: Web3Forms, not Formspree.
  `contact.html`'s form action remains the never-configured placeholder
  `https://formspree.io/f/REPLACE_WITH_FORMSPREE_ID` — that placeholder will not be filled with a
  real Formspree ID. Once the client finalizes the purchase of this site, the form will be switched
  to Web3Forms (`https://api.web3forms.com/submit` + a real `access_key` hidden field) instead.
- Docs-only batch, no code changed: recorded the decision in `docs/project/DECISION_LOG.md`
  (including implementation notes for the future swap — the existing `js/main.js` submit handler is
  vendor-agnostic), reworded `docs/governance/PROJECT_RISK_REGISTER.md` R-008/R-009, `BACKLOG.md`'s
  matching item, and `docs/governance/RELEASE_GATE.md`'s Functionality checklist + Release Decision
  notes from Formspree to Web3Forms.

**Files or areas changed:** `docs/project/DECISION_LOG.md`, `docs/governance/PROJECT_RISK_REGISTER.md`,
`BACKLOG.md`, `docs/governance/RELEASE_GATE.md`, `docs/project/STATUS.md`, `PROGRESS_NOTE.md`,
`docs/project/COMMIT_NOTES.md`.

**Validation performed:** `grep -rln "Formspree"` re-run after edits confirmed every tracking-doc
reference either now names Web3Forms or is an untouched historical record of already-shipped past
work; `contact.html`/`js/main.js` intentionally untouched (no access key to configure yet).

**Notes for the next agent:** the actual Web3Forms swap is not agent-actionable — no account or
access key exists yet, and none should be created speculatively. Blocked on the client finalizing
the purchase. All other open backlog items unchanged (image optimization remains the standing
user-confirmed next pick; see `BACKLOG.md` "Build Next").

---

## 2026-07-22 — Hash placeholder backfill and vault baton refresh

**Work completed:**
- A `REPO_SESSION_START_RECOVERY_AUDIT.md` run found two staleness gaps: 3 unfilled "hash once
  committed" placeholders (`docs/project/STATUS.md` ×2 for v2.9.0 and the previously-missed
  v2.6.0 entry, `PROGRESS_NOTE.md` ×1), and a 3-day-stale vault baton entry in AntBrainOS's
  `00_START_HERE/AGENT_HANDOFF.md` still naming an already-completed audit re-run as the open
  next task.
- Backfilled all 3 hash placeholders with the real short hashes (`10ee3d0`, `dd88bf4`) via a
  trailing docs-only, untagged commit (`f75de99`), matching this repo's established
  `8e61909`/`358e0b9` precedent.
- Prepended a new dated `AGENT_HANDOFF.md` entry recording the actual v2.8.0/v2.9.0 audit
  completion, without rewriting the stale entry it supersedes (vault's no-rewrite-of-history
  convention). Refreshed a matching stale pointer in this project's own vault
  `HANDOFF_TO_CLAUDE.md`.

**Files or areas changed:** `docs/project/STATUS.md`, `PROGRESS_NOTE.md` (repo); AntBrainOS
`00_START_HERE/AGENT_HANDOFF.md` and `03_PROJECTS/Active/Old_Fashion_Care/HANDOFF_TO_CLAUDE.md`
(vault, outside this repo).

**Validation performed:** `git diff --stat` confirmed only the 2 intended repo files changed
before the trailing commit; `git show --stat` confirmed it was docs-only; a fresh vault snapshot
was taken and verified (987/987 files, 0 file-list diff) before any vault edit; re-read of the
new `AGENT_HANDOFF.md` entry confirmed the old entry was left untouched.

**Notes for the next agent:** no active task remains — this batch only closed the two staleness
conditions the recovery audit named. The optional/user-owned follow-ups from the prior push
(Formspree ID, form-analytics events, image optimization, HSTS header, apple-touch-icon, the
`care giver pics/` folder decision, iOS Safari check) are unchanged and still open — see
`BACKLOG.md` and `docs/governance/PROJECT_RISK_REGISTER.md` (R-007–R-013).

---

## 2026-07-19 — Production-readiness audit complete: uptime check added, fixes deployed live to VPS

**Work completed:**
- Ran the 4 skills left over from the entry below: `observability-analytics-readiness` (found
  zero custom analytics events and zero uptime monitoring anywhere), `rollback-risk-register`
  (found, via fresh `curl`, that the VPS mirror was still serving pre-fix code — the deploy gap
  below), `production-readiness-audit` (capstone — overall verdict: Conditionally ready),
  `client-handoff-pack` (non-technical summary for the business owner).
- Fixed the observability gap: added `.github/workflows/uptime-check.yml` (checks the VPS mirror
  every 30 minutes for HTTP 200 + expected content), verified via a real triggered GitHub Actions
  run — not just a local simulation. Committed/pushed as `8588925`.
- User explicitly authorized deploying this session's fixes to the VPS mirror
  (`old-fashion-care.craftandconscious.com`). Built the exact deploy file set matching the
  existing live footprint (not a raw rsync filter), dry-ran the sync, applied it, then
  independently re-verified live via fresh `curl`: new `Content-Length`/`Last-Modified`, new
  testimonials copy present and old placeholder gone, new contact-form JS logic present, new
  `--coral-fill` contrast values present on `.nav__cta` and `.phone-strip`.
- Wrote both consolidated audit report files (`seo/audits/hygiene-2026-07-19.md`,
  `docs/governance/audits/production-readiness-2026-07-19.md`) — previously the findings existed
  only in the conversation, not on disk.
- Reconciled `docs/governance/{PROJECT_RISK_REGISTER,SECURITY_BASELINE,COMPATIBILITY_MATRIX,
  ROLLBACK_PLAN,RELEASE_GATE,PHASE_GATES,AGENT_RUN_LOG}.md` and `BACKLOG.md` against every finding
  from all 13 skill-runs — including finally closing the already-stale R-005 entry (hero image —
  had been marked Open despite being fixed back in 2026-07-12) and adding 7 newly-tracked risks
  (R-007 through R-013: image oversizing, Formspree ID, missing form-analytics events, missing
  HSTS, the `care giver pics/` folder, unverified certbot renewal, unverified Netlify status).

**Files/areas changed:** `.github/workflows/uptime-check.yml` (new); `docs/project/{STATUS,
CHANGELOG,COMMIT_NOTES}.md`, `PROGRESS_NOTE.md`, `PROGRESS_NOTES.md`, `BACKLOG.md`,
`docs/governance/{PROJECT_RISK_REGISTER,SECURITY_BASELINE,COMPATIBILITY_MATRIX,ROLLBACK_PLAN,
RELEASE_GATE,PHASE_GATES,AGENT_RUN_LOG}.md`, `seo/audits/hygiene-2026-07-19.md`,
`docs/governance/audits/production-readiness-2026-07-19.md`. No further site-code changes beyond
what the entry below already covers (this batch is the audit's own paperwork, plus the uptime
check and the deploy of already-fixed code).

**Validation performed:** uptime-check workflow confirmed via a real GitHub Actions run (log shows
`OK: https://old-fashion-care.craftandconscious.com/ is up`). VPS deploy confirmed via fresh
`curl` evidence post-deploy (not assumed from a successful `rsync` exit code alone) — new content
length, new `Last-Modified`, new testimonials copy, new JS, new CSS all directly verified present
on the live site.

**Notes for the next agent:** the production-readiness audit is now genuinely complete — don't
re-run it from scratch; if new work is needed, treat it as a fresh, smaller, targeted task. Real
follow-ups worth picking up (all optional/user-owned, none urgent): a real Formspree ID, custom
form-analytics events, image optimization, an HSTS header, a dedicated apple-touch-icon, a
decision on `care giver pics/`, and a real-device iOS Safari check — see `BACKLOG.md` and
`docs/governance/PROJECT_RISK_REGISTER.md`.

---

## 2026-07-19 — Contact form, contrast, touch target, and content fixes (production-readiness audit, in progress)

**Work completed:**
- Ran a `REPO_SESSION_START_RECOVERY_AUDIT.md` at session start — confirmed same-agent resumption
  (Claude Code → Claude Code), `PASS`, matched the vault's `AGENT_HANDOFF.md` exactly.
- User asked whether re-running the 18/20-skill production-readiness suite (installed 2026-07-15,
  never actually executed) would add value. Researched via 3 parallel Explore agents plus a Plan
  agent; produced a scoped plan (`main` branch only, 13 of ~20 skills, several explicitly skipped
  as structurally inapplicable to a static no-backend site); user approved full scope.
- Ran 9 of the 13 planned skills against `main`: `repo-safety-preflight`,
  `v33-quality-gate-enforcer`, `component-inventory`, `seo-hygiene-check`, `accessibility-pass`,
  `performance-budget-pass`, `responsive-browser-matrix`, `content-conversion-readiness`,
  `deployment-nginx-https-readiness`. Notable findings: the contact form always fakes success
  regardless of actual submission (critical); 9 of 14 homepage photos ship at 5-8x the resolution
  they're displayed at (~14.2MB avoidable); homepage testimonials section shows literal
  bracket-placeholder text (critical); a 15MB unreferenced `care giver pics/` folder is tracked in
  git; `oldfashioncare.com`'s hosting mismatch re-confirmed unchanged.
- Fixed 5 of the surfaced issues, each explicitly approved before implementation: the contact
  form's fake-success bug (real `fetch()` submission with honest success/error states); WCAG AA
  contrast on `.nav__cta`/`.section--coral`/`.phone-strip` (migrated to `--coral-fill`; the
  `.phone-strip` cascade collision was only found because verification caught the
  `.section--coral` fix silently not rendering); the mobile nav hamburger's undersized touch
  target (38×31px → 44×44px, plus an open/closed X-icon state); missing Escape-key handling on the
  mobile nav; the homepage testimonials section's placeholder text (reframed to 3 honest,
  first-party trust statements via a short Plan Mode cycle — see `docs/project/DECISION_LOG.md`).
- Along the way, a sub-agent investigating the testimonials fix's scope reported encountering text
  in tool output mimicking a fake system-reminder. Investigated thoroughly (grepped the entire
  repo — tracked files, untracked files, and full git history) and found zero evidence of actual
  injected content anywhere; most likely the sub-agent saw genuine harness-level messages that
  don't semantically apply to its own restricted tool context and flagged them out of appropriate
  caution. No tampering found, no unauthorized action taken.

**Files/areas changed:** `js/main.js`, `css/style.css`, `contact.html`, `index.html` (site code);
`docs/project/{STATUS,CHANGELOG,DECISION_LOG,COMMIT_NOTES}.md`, `PROGRESS_NOTE.md`,
`PROGRESS_NOTES.md`, `BACKLOG.md` (this repo's tracking docs); the linked AntBrainOS vault project
folder (`03_PROJECTS/Active/Old_Fashion_Care/`).

**Validation performed:** no build/lint/test scripts exist (static site). All 5 fixes verified via
local Playwright — computed-style contrast checks, mocked and real `fetch` responses for the
contact form's success/error paths, keyboard/focus checks for the Escape handler, bounding-box
measurement for the touch target, and viewport screenshots for the testimonials section (after
correctly scrolling it into view to trigger its `.reveal` animation before capturing). Zero
console errors, zero horizontal overflow across every check.

**Notes for the next agent:** the production-readiness audit is **not finished** — 4 of 13 planned
skill-runs remain (`observability-analytics-readiness`, `rollback-risk-register`,
`production-readiness-audit` capstone, `client-handoff-pack`), and neither of the two planned
consolidated report files nor the tracking-doc reconciliation (`PROJECT_RISK_REGISTER.md`,
`SECURITY_BASELINE.md`, `COMPATIBILITY_MATRIX.md`, `RELEASE_GATE.md`, `AGENT_RUN_LOG.md`) has been
written yet. Don't mistake this push for a completed audit — it's 5 real fixes found along the way,
with the audit's own paperwork still open. The Formspree ID is still a placeholder (user-owned).
The `oldfashioncare.com` mismatch is user-confirmed expected/known — don't re-flag it as a blocker
unless the user raises it again.

---

## 2026-07-18 — Fixed a real staleness gap in the push workflow itself

**Work completed:**
- A follow-up session found `PROGRESS_NOTE.md` was stale after the founder-photo push below —
  still describing 2026-07-15/07-12 state, missing the fix entirely. Corrected its content.
- Root-caused why: this repo's own push prompt
  (`Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`) told the agent
  `PROGRESS_NOTE.md` and `PROGRESS_NOTES.md` were alternatives ("prefer `PROGRESS_NOTES.md`"), so
  the previous push updated this file but silently skipped the singular one, despite `CLAUDE.md`/
  `AGENTS.md`/`REPOSITORY_HANDOFF_CONFIG.md` all marking it required unconditionally.
- Fixed the push prompt: `PROGRESS_NOTE.md` is now a mandatory every-push update, the ambiguous
  guidance was removed, and a `git diff --stat` cross-check was added before every commit so a
  required file can no longer silently fall out of a push (Section 6).
- Added a new Section 2a requiring the linked AntBrainOS vault project folder
  (`03_PROJECTS/Active/Old_Fashion_Care/`) to be updated on every push, not only at session end, so
  the vault-side record can never drift more than one push behind.
- The same class of bug was confirmed present, and fixed the same way, in the AntBrainOS vault's own
  canonical copy of this prompt (no mechanical sync exists between a repo's local copy and the
  vault template — this was a real, separate gap in its own right), plus a new independent
  cross-check step added to the vault's `CLAUDE_CODE_SESSION_END.md` so a future closeout can't
  record a repo as "clean/current" purely by trusting a prior push-prompt step.

**Files/areas changed:** `PROGRESS_NOTE.md`, `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`,
`docs/project/STATUS.md`, `docs/project/CHANGELOG.md`, `docs/project/DECISION_LOG.md`,
`LESSONS_LEARNED.md`, `SLICE_REVIEWS.md`, `docs/project/COMMIT_NOTES.md`. No site code.

**Validation performed:** no build/lint/test scripts exist for this static site; verification was a
manual review confirming the push prompt's sections are internally consistent (no remaining
"prefer the other file" language) and that this push's own `git diff --stat` includes every file
this repo's required-tracking list names.

**Notes for the next agent:** the push prompt now has a Section 2a — update the linked vault
project folder on every push, not just at session end. Don't skip it because a session-end closeout
feels like it will "catch up" later; that's the exact assumption that let this gap form in the
first place.

---

## 2026-07-18 — Fixed distorted founder photo on `about.html` (branch `main`)

**Work completed:**
- Root-caused the compressed/distorted Regina Booker photo on `about.html`: a leftover
  `.founder-photo` placeholder-box rule in `css/style.css` was forcing `aspect-ratio: 3/4`
  (desktop) / `16/9` (mobile) onto the real `<img>`, and with no `object-fit` set the browser
  stretched the real 354×514 portrait to fill those mismatched boxes.
- Fix: added `aspect-ratio: auto;` to the `img.founder-photo` rule so the photo renders at its
  natural ratio, matching how the equivalent photo already looked on the Editorial Sage design
  branches (`.es-founder__photo` never forced an `aspect-ratio`).
- Files/areas changed: `css/style.css` only — no markup change, no other page affected (grepped
  all `.html` files; `about.html` is the only `founder-photo` usage).
- Also re-synced the VPS webroot behind `old-fashion-care.craftandconscious.com` (the URL the
  user actually checks) since it's a manually-updated mirror of `main`, not Netlify — see
  `docs/project/STATUS.md`'s 2026-07-18 entry for the full deployment-topology finding
  (`oldfashioncare.com` currently serves an unrelated third-party "Jottful" site, not this
  repo).

**Validation performed:** Local Playwright screenshots at 390/768/1440px (before/after via
`git stash`), 0 console errors, bounding-box ratios matched the image's real intrinsic ratio
after the fix. Live re-verification after deploy: curled the deployed CSS for the fix line and
re-screenshotted the live page at mobile width.

**Notes for the next agent:** `oldfashioncare.com` is not currently wired to this repository
(Netlify or otherwise) despite being the domain baked into every page's canonical URL,
`sitemap.xml`, `robots.txt`, and the Plausible analytics `data-domain`. That's a pre-existing
condition surfaced during this fix, not something resolved here — needs a hosting/DNS decision
from the user.

---

## 2026-07-15 — AntBrainOS kit tooling installed across all 5 branches

**Work completed:**
- Installed dev-tooling kits from the AntBrainOS `20_TOOLS/KITS` collection onto all 5 branches
  (one commit each): SEOKit (added to `main` + `migration`; already present on the 3 design
  branches), EngKit (`/eng` dispatcher), TradeKit (`/tk` cards + adapters), and the cross-tool
  handoff-repository skill with a filled `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`.
- Skipped EcomKit + VideoKit (no ecommerce/video surface) and MKTKit (previously rolled back here).

**Files/areas changed (per branch):** `.claude/skills/eng/`, `.claude/tradekit/`,
`.claude/skills/handoff-repository/`, `.agents/skills/handoff-repository/`,
`docs/governance/REPOSITORY_HANDOFF_CONFIG.md`, (`+ .claude/commands/seo/`, `.claude/skills/seo-*`,
`.claude/agents/seo-*`, `seo/` on `main` + `migration`), and tracking docs. **No site files.**

**Validation:** per-branch site-file diff (parent..commit) empty on all 5; `main` live-site guard
diff (`fd7543c..7818660`) empty; cross-branch presence matrix uniform; EngKit validator PASSED.

**Notes for next agent:** `main`'s tooling commit is tagged `v2.5.0__install-antbrainos-kit-tooling-
all-branches__commit-7818660`. The 4 non-trunk branch commits were pushed but intentionally not
individually tagged (matches this repo's precedent for non-trunk branches). No live-site or design
change occurred; redesign/hero merges remain separate decisions.

---

## 2026-07-11 — Hero CTA contrast fix + gradient/photo-band repositioned left

**Work completed:**
- Added `--coral-fill: #C2512B` token; repointed `.btn--coral` to it (~4.66:1 vs white, up from
  3.87:1, fixing a WCAG AA failure)
- Shifted `.hero::after` gradient stops left; widened `--hero-photo-w` 53%→57% (band edge
  47%→43%) to reach the zone marked in the user's reference screenshot
- Verified via Playwright screenshots + canvas pixel-sampling at 1200/1440/700/390px — real,
  correctly-directioned, no hard seam, mobile pixel-identical
- Measured the face-clearance tradeoff: text-to-hairline gap 125px→80px at 1200px viewport
  (10.4%→6.7%), still above the ~60px/5% historical safe minimum

**Files changed:**
- `css/style.css`, `docs/project/STATUS.md`, `PROGRESS_NOTE.md`, `docs/project/COMMIT_NOTES.md`,
  `docs/project/CHANGELOG.md`

**Validation:**
- Playwright headless render + canvas pixel-sampling, no PIL/ImageMagick available

**Notes for next agent:**
- Pushed to main in two commits: `d2ffe77` (v2.0.3, pre-existing unrelated policy docs) and
  `b9a2a42` (v2.1.0, this hero fix). Both tagged, snapshotted, pushed.
- Found but out of scope: `.nav__cta`/`.section--coral` share the same coral-fill/white-text
  contrast bug — not fixed this pass.

---

## 2026-07-06 — Record next action: hero spec waiting in AntBrain inbox

**Work completed:**
- Discovered `000_INBOX/ofc-hero-spec-and-fix.md` already exists in the AntBrainOS vault — an
  un-triaged, frontmatter-less spec ("Hero Component — Verified Spec & Fixes") covering a CTA
  button contrast fix (coral fill/white text fails 4.5:1 AA) and a gradient fade-seam widening fix
  (~47–61% → ~40–68%, multi-stop gradient), with guardrails (don't change the two confirmed hex
  colors) and a 6-step execution plan
- Recorded this as the project's next work item in `docs/project/STATUS.md` and `PROGRESS_NOTE.md`,
  and mirrored the same pointer into the vault's `CURRENT_CONTEXT.md`/`HANDOFF_TO_CLAUDE.md`
- No code changed — this was a pure note-recording turn

**Files changed:**
- `docs/project/STATUS.md`, `PROGRESS_NOTE.md`, `docs/project/COMMIT_NOTES.md` (this entry's
  companion), `PROGRESS_NOTES.md` (this entry)

**Validation:**
- None required — documentation-only, no site files touched

**Notes for next agent:**
- Before implementing the hero fixes, run `05_SOPS/Obsidian/INBOX_PROCESSING.md` on
  `000_INBOX/ofc-hero-spec-and-fix.md` first (triage/classify/file it properly), then follow its
  6-step execution plan against `css/style.css`'s `.hero*` rules
- Get Phase 1 approval before implementing, per standard workflow

---

## 2026-07-05 — Production Readiness Skills deployment (V3.4 + 18-skill suite)

**Work completed:**
- Ran the `DEPLOY_PRODUCTION_READINESS_SKILLS` SOP against this repo. Found both source packages
  (`project-starter-kit-v3.4`, `production-readiness-skills-v1`) were stranded in an unreviewed
  migration-staging folder in the vault instead of their documented canonical path; fixed that in
  the vault first (separate from this repo), with a verified pre-change snapshot.
- Installed the V3.4 baseline in migrate mode: additive only, `v34_validate.py` → PASS. 3 real
  conflicts with existing files (`AGENTS.md`, `CLAUDE.md`, `ai/agents/AGENT_REVIEW_GATES.md`) were
  quarantined into `.v34_migration_review/*.v34-candidate` rather than overwritten.
- Installed the 18-skill production-readiness suite into `.claude/skills/` — 22 total skills now
  present (4 V3.4 + 18 PR suite), no name conflicts with the existing v3.3 setup.
- Verification caught a leftover cross-client reference ("Smart Learning Solutions") baked into
  `production-readiness-audit/SKILL.md`'s description text; user approved generalizing the wording.

**Files changed:**
- New: `.agents/`, `.claude/skills/` (22 skills), `docs/governance/`, `docs/project/`,
  `ai/agents/SUBAGENT_ROLES.md`, `ai/prompts/*`, `00_MIGRATION_KICKOFF.md`, `MIGRATION_REPORT.md`,
  `V34_INSTALL_REPORT.json`, `.v34_migration_review/*`
- No existing tracked file was modified or deleted by either installer

**Validation:**
- `python3 project-starter-kit-v3.4/scripts/v34_validate.py --target .` → PASS
- `find .claude/skills -name SKILL.md | wc -l` → 22 (expected)
- `grep -rl "Smart Learning" .claude/skills/` → clean after the one-line fix
- `git status --short` reviewed to confirm only new/untracked paths plus the 3 quarantined files

**Notes for next agent:**
- `docs/project/*.md` (from V3.4) duplicates the root-level tracking docs already in active use in
  this repo (STATUS.md, CHANGELOG.md, COMMIT_NOTES.md, etc.) — not yet reconciled, flagged in
  STATUS.md open questions
- The 3 files in `.v34_migration_review/` are candidate replacements for `AGENTS.md`, `CLAUDE.md`,
  and `ai/agents/AGENT_REVIEW_GATES.md` — review before deciding whether to merge any content in
- 22 skills are now available as slash-command-style skills in Claude Code sessions on this repo

---

## 2026-07-02 → 2026-07-05 — SEO audit: metadata verified, two broken asset references fixed

**Work completed:**
- Ran the `CLAUDE_CODE_SESSION_START` SOP to re-establish project context before starting the SEO-audit slice named in the prior handoff.
- Read-only inventory (grep) of `<title>`, meta description, canonical, Open Graph, and Twitter Card tags across all 6 pages (`index.html`, `about.html`, `services.html`, `how-it-works.html`, `questions.html`, `contact.html`), plus `sitemap.xml` and `robots.txt` — all already correct and consistent; no content changes needed.
- Found `og:image`/`twitter:image` on all 6 pages pointed to `images/og-default.png`, which does not exist — broken social-share preview image site-wide. User chose to repoint to the existing `images/hero-ai.jpg`; also corrected the declared `og:image:width`/`:height` from the placeholder 1200×630 to the file's actual 1100×934 (verified via `sips`).
- Found `<link rel="apple-touch-icon" href="/images/apple-touch-icon.png">` on all 6 pages also referenced a nonexistent file — broken iOS "Add to Home Screen" icon. User chose to remove the tag; `favicon.svg` remains as the fallback icon.
- Verified fix: no remaining `og-default`/`apple-touch-icon` references; a scripted sweep of every local `href`/`src` across the 6 pages confirms all referenced local files now exist.

**Files changed:**
- `index.html`, `about.html`, `services.html`, `how-it-works.html`, `questions.html`, `contact.html` — `og:image`, `og:image:width`, `og:image:height`, `twitter:image` values; `apple-touch-icon` link removed
- `STATUS.md`, `PROGRESS_NOTE.md` — updated to reflect this slice

**Validation:**
- `grep -rn "og-default\|apple-touch-icon" *.html` → no matches
- Scripted check extracting every local `href`/`src` across the 6 pages and confirming the file exists on disk → all resolve

**Notes for next agent:**
- SEO audit is now effectively complete pending commit — proceed to migration branch → main merge review next, per the standing open question in STATUS.md
- Hi-res hero photo swap remains a separate, still-open item
- Consider creating a real 180×180 `apple-touch-icon.png` later rather than relying on the `favicon.svg` fallback indefinitely

---

## 2026-06-30 — Prompts: refined handoff snapshot naming prompt added

**Work completed:**
- Added `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` to repo
- Refines Section 9 of the base handoff prompt: full descriptive tag required for snapshot folder names, user must confirm RepoBackups path per session, bare version-only names explicitly forbidden
- Updated STATUS.md, PROGRESS_NOTES.md, COMMIT_NOTES.md

**Files changed:**
- `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` (new)
- `STATUS.md`, `PROGRESS_NOTES.md`, `COMMIT_NOTES.md`

**Validation:**
- git diff --stat confirmed no site files changed

**Notes for next agent:**
- Use the refined prompt for all future handoffs; it supersedes Section 9 of the base prompt
- RepoBackups path: `/Users/ant/WorkSync/Projects/RepoBackups/Old-Fashion-Care`
- Next steps: user visual review of live hero, then SEO audit across all 6 HTML pages

---

## 2026-06-30 — Branch sync: migration/project-starter-v3-3 merged into main

**Work completed:**
- Diagnosed GitHub Desktop "Newer Commits on Remote" block: local `migration/project-starter-v3-3` was 9 commits behind remote; post-check workflow did not verify local vs. remote parity
- Fast-forward pulled remote `migration/project-starter-v3-3` (98f2478 → f794a80)
- Fast-forward merged `migration/project-starter-v3-3` into local `main` (0806eda → f794a80)
- Pushed `main` to remote — all hero and migration work now on main for the first time
- Updated STATUS.md, PROGRESS_NOTES.md, COMMIT_NOTES.md, LESSONS_LEARNED.md

**Files changed:**
- `STATUS.md`, `PROGRESS_NOTES.md`, `COMMIT_NOTES.md`, `LESSONS_LEARNED.md` — tracking docs only

**Validation:**
- git fast-forward merge confirmed clean (no conflicts)
- git status clean; git branch -vv confirmed main in sync with origin/main

**Notes for next agent:**
- All work is now on `main`; migration branch is no longer the active working branch
- RepoBackups path: `/Users/ant/WorkSync/Projects/RepoBackups/Old-Fashion-Care`
- Next steps: user visual review of hero on live site, then SEO audit across all 6 HTML pages

---

## 2026-06-30 — Hero: smoother gradient, headline fix, sub barely-covers-hand + handoff

**Work completed:**
- Three hero CSS iterations since last handoff: ea8b067 (B-period gradient + wider sub), b195aba (smoother gradient + headline fix + barely-covers-hand), c04e819 (tracking docs)
- `css/style.css` changes (b195aba): `.hero__headline max-width 565→620px` (fixed orange line regression); `.hero::after` 10-stop smooth gradient anchored at 'h' in "where" (22%); `.hero__sub` 0.97→0.94rem, 648→580px (sub barely reaches hand, not shoulder)
- Tracking docs updated: CHANGELOG.md v1.5.0 entry, SLICE_REVIEWS.md 2026-06-30 entry, PROGRESS_NOTE.md, STATUS.md
- Tag `v1.5.0__hero-smoother-gradient-headline-fix__commit-b195aba` created + pushed to GitHub
- Snapshot saved: `/Volumes/DataHub_2TB/WorkSync/Projects/RepoBackups/Old-Fashion-Care/v1.5.0__hero-smoother-gradient-headline-fix__commit-b195aba`

**Files changed:**
- `css/style.css` (b195aba, ea8b067) — hero rules only
- `COMMIT_NOTES.md`, `PROGRESS_NOTE.md`, `PROGRESS_NOTES.md`, `STATUS.md`, `CHANGELOG.md`, `SLICE_REVIEWS.md`

**Validation:**
- Headless Chromium at 1554×900 (desktop) and 390×820 (mobile) for both ea8b067 and b195aba
- sips zoom crops confirmed: headline 1 line, 4-line sub, smooth gradient, hand barely covered, shoulder visible, mobile clean

**Notes for next agent:**
- The two source image PNGs in `images/` (ChatGPT Image Jun 27/28) remain untracked intentionally; do not commit unless re-cropping hero-ai.jpg
- Hero alignment tuned at 1554×900; period↔earlobe alignment is viewport-sensitive (cover bg + flex centering)
- RepoBackups path (user-confirmed): `/Volumes/DataHub_2TB/WorkSync/Projects/RepoBackups/Old-Fashion-Care`
- Next steps: user visual review of live hero, then SEO audit, then migration branch → main merge review

---

## 2026-06-29 — Hero copy↔people micro-alignment

**Work completed:**
- Hero sub-paragraph reduced to 4 lines (font-size 1.05→0.97rem, max-width 480→582px)
- Hero copy column nudged up (margin-top -5.2rem, mobile reset) so orange "possible." period bottom aligns with older woman's earlobe bottom (~8px tolerance)
- Hero gradient pulled left (stops shifted ~5–8% earlier) to reveal more of the photo; band edge kept ≥0.50 opacity to prevent seam
- Wider sub paragraph makes lower text lines subtly overlap the caregiver's fingertips; hand/shoulder/faces preserved
- Gradient ambiguity resolved with user: chose "pull dark toward left edge / reveal more photo" direction

**Files changed:**
- Modified: `css/style.css` (hero rules only), `COMMIT_NOTES.md`, `PROGRESS_NOTE.md`
- Untracked (not committed): `.DS_Store`, `images/ChatGPT Image Jun 2[78]*.png`, `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md`

**Validation:**
- Headless Chromium screenshots at desktop 1554×900 and mobile 390×820
- sips zoom crops confirmed period↔earlobe alignment, hand overlap subtlety, no seam line, mobile unaffected

**Notes for next agent:**
- The two source image PNGs in `images/` (ChatGPT Image Jun 27/28) are the clean original photo and the old mockup — untracked intentionally; do not commit them unless re-cropping is needed
- Hero is tuned at desktop 1554×900; period↔earlobe alignment will drift slightly at other viewport sizes (photo is `cover`, text is `align-items:center`)
- Next steps: user visual review, then SEO audit, then migration branch → main merge review

---

## 2026-06-18 — GitHub Repo Rename

**Work completed:**
- Renamed GitHub repository from `Old-Fashion-Care-` to `Old-Fashion-Care` (trailing dash removed)
- Updated local git remote URL to `git@github.com:rmz9dkfy5f-pixel/Old-Fashion-Care.git`
- Updated CONTEXT.md, CHANGELOG.md, STATUS.md, COMMIT_NOTES.md to record rename

**Files changed:**
- Modified: CONTEXT.md, CHANGELOG.md, STATUS.md, COMMIT_NOTES.md, PROGRESS_NOTES.md

**Validation:**
- `git ls-remote origin HEAD` confirmed connectivity to new URL

**Notes for next agent:**
- Repo name is now `Old-Fashion-Care` — use this in all future references
- No site files were changed in this push
- Next priority remains: SEO audit across all 6 HTML pages

---

## 2026-06-15 — Photo Social Proof + Starter Kit v3.3 Migration

**Work completed:**
- Starter Kit v3.3 migration: added .claude/agents/ (7 sub-agents), ai/, docs/, plans/, and 23 root-level project control docs
- Filled in README.md, PROJECT_BRIEF.md, STATUS.md, BACKLOG.md with real Old Fashion Care project content
- Created images/ folder with 12 photos using clean URL-safe filenames (care-01.jpg through care-11.jpg + regina.jpg)
- Added caregiver photo grid section to homepage index.html ("Real Care, Real People" — 6 photos, responsive 3-col/2-col/1-col)
- Replaced "RB" initials placeholder on about.html with Regina Booker's real founder photo (rectangular portrait)
- Updated css/style.css with .photos-grid layout and img.founder-photo rules
- CHANGELOG.md template quarantined in .starter-kit/review/conflicts/ (no overwrite of existing real content)
- Migration branch created: migration/project-starter-v3-3 off main @ 0806eda

**Files changed:**
- Modified: about.html, css/style.css, index.html
- New: images/ (12 photos), .claude/ (7 agents + settings.example.json), ai/, docs/, plans/, all root .md control docs, .starter-kit/ workspace, Prompts/

**Validation:**
- Static site — no build/lint/test scripts
- Manual browser check: all pages load, photos display correctly, no broken images

**Notes for next agent:**
- SEO audit is the next priority — review meta titles, descriptions, Open Graph across all 6 HTML pages
- Migration branch not yet merged to main — review before merging
- care giver pics/ (original photo folder with spaces) kept in place; images/ has clean copies
- Photos care-07 through care-11 available but not yet used in the grid
