# Progress Notes

Historical project progress notes.

Current active progress belongs in `PROGRESS_NOTE.md`.

---

## 2026-07-27 — Tag + RepoBackups Snapshot (branch `design/editorial-sage-hero-cream-immersive`)

**Work completed:**
- This branch was pushed to `origin` on 2026-07-15 and deployed live to the VPS preview, but was
  never tagged or RepoBackups-snapshotted — unlike its sibling `design/editorial-sage-hero-split-depth`,
  which received both the same day. Closed that gap, replicating the split-depth ceremony exactly.
- Applied annotated tag `v2.14.0__cream-immersive-hero-full-bleed-gradient-scrim__commit-4182844`
  directly to the historical design commit `4182844` — not to this docs commit, and not to the
  branch's later tooling-only tip `25647aa` — per this repo's own established rule ("tag the code
  commit, not a trailing docs-only commit"; see `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`,
  precedent `v1.5.0`/`b195aba`, `v2.4.0`/`2727395`).
- Created a RepoBackups snapshot at
  `/Users/ant/WorkSync/Projects/RepoBackups/Old-Fashion-Care/v2.14.0__cream-immersive-hero-full-bleed-gradient-scrim__commit-4182844`
  (this machine, "Ant's MacBook Air" — path confirmed via `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`'s
  Snapshot Destination table).

**Files changed:** `PROGRESS_NOTE.md`, `PROGRESS_NOTES.md` (this entry), `docs/project/COMMIT_NOTES.md`,
`docs/project/STATUS.md`. No site code (`.html`/`.css`/`.js`) touched.

**Validation:**
- Live preview URL re-checked before writing this entry: `curl -I
  https://old-fashion-care.sage.hero.cream.immersive.craftandconscious.com` → `200 OK`,
  `Last-Modified: Wed, 15 Jul 2026 07:58:33 GMT` — unchanged since the original 2026-07-15 deploy.
- Snapshot verified: file count matches `git ls-tree -r --name-only 4182844` (297 files), a sorted
  file-list `diff` against the same tree was empty, and `shasum -a 256` spot-checks on `index.html`
  and `css/editorial-sage.css` matched the tree exactly.
- `git tag --points-at 4182844` confirms the new tag; hard-clean check passed after push.

**Notes for next agent:**
- Both hero-variant branches (`hero-cream-immersive`, `hero-split-depth`) are now equally
  tagged/snapshotted. No new agent-actionable item from this task itself.
- Standing open items unchanged: the client's hero/design-direction decision; any merge-to-`main`
  decision (separate, explicit, high-blast-radius); the `oldfashioncare.com`/Jottful hosting
  question; `migration/project-starter-v3-3` deletion decision; `main`-only BACKLOG.md follow-ups.

---

## 2026-07-14 — SEO/performance hygiene pass + SEOKit install (branch `design/editorial-sage-elder-friendly`)

**Work completed:**
- Answered the user's "can any of the kits add value?" question with concrete work rather than a
  survey: evaluated the 22 installed `.claude/skills/` and the 6 external kits at
  `/Volumes/AntNVMe1TB/.../20_TOOLS/KITS/` against the actual static site.
- Ran `seo-hygiene-check` + installed **SEOKit** and ran `/seo hygiene` → `seo/audits/hygiene-2026-07-14.md`.
- Ran `performance-budget-pass`; applied 2 low-risk fixes: (1) removed the render-blocking Google
  Fonts `@import` from `css/editorial-sage.css` and added `<link rel="preconnect">` + direct font
  `<link>` to all 6 page `<head>`s; (2) added `loading="lazy"` to the `index.html` founder photo.
- Evaluated **MKTKit**, judged it a poor fit for a static business site, and rolled it back;
  preserved its one useful output at `docs/marketing/cta-proposal-2026-07-14.md`.

**Files changed:**
- Modified: `index.html`, `about.html`, `services.html`, `how-it-works.html`, `contact.html`,
  `questions.html`, `css/editorial-sage.css`
- Added: `.claude/commands/seo/*` (19), `.claude/skills/seo-*.md` (4), `.claude/agents/seo-auditor.md`,
  `.claude/agents/serp-researcher.md`, `seo/{SITE,KEYWORD_MAP,REVENUE}.md`,
  `seo/audits/hygiene-2026-07-14.md`, `docs/marketing/cta-proposal-2026-07-14.md`

**Validation:**
- Source inspection + grep across all 6 pages (font links present ×3/page, `@import` removed, lazy
  attr present). No build/test/lint scripts in this static repo; browser smoke-test not re-run.

**Notes for next agent:**
- Deferred (user's call): Formspree `action` still `REPLACE_WITH_FORMSPREE_ID` in `contact.html`.
- Not yet applied (low effort, from the audits): og:image dims 1100×934→1400×933, sitemap `lastmod`
  refresh, `LocalBusiness` JSON-LD on services/contact/how-it-works, dedicated `apple-touch-icon.png`.
- `care-03..11.jpg` (1–2.5MB, up to 5760px) are unreferenced — resize before wiring into any page.
- SEOKit's `/seo quick-wins` needs GSC/CSV data (not connected) before it can produce real rankings.
- Prior v2.x pushes did not create RepoBackups snapshots; only this v2.3.0 snapshot exists there.

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
