# Changelog

All notable changes to the Old Fashion Care website will be documented here.

---

## [feat: add dedicated apple-touch-icon.png (180×180) — `main`] — 2026-07-28

### Added
- `images/apple-touch-icon.png` — 180×180 PNG rasterized from the existing `favicon.svg` lettermark
  (rounded charcoal square, centered coral "O") via a throwaway Playwright render; a mechanical
  conversion of the already-approved vector mark, not a new design
- `<link rel="apple-touch-icon" sizes="180x180" href="/images/apple-touch-icon.png">` added to all
  6 pages' `<head>`, directly below the existing `<link rel="icon">` favicon tag

### Notes
- Closes `docs/governance/PROJECT_RISK_REGISTER.md` R-006 (now `R-C09`) and the matching
  `BACKLOG.md` item
- Placed under `images/` (not repo root) so it automatically inherits the existing `netlify.toml`
  `/images/*` 1-year immutable cache rule — no `netlify.toml` change needed
- No CSP change needed — `img-src 'self' data:;` already permits a same-origin image
- `favicon.svg` remains the `<link rel="icon">` fallback for non-Apple contexts; unchanged

---

## [fix: compress the 4 live `care-*.jpg` photos (93.9% size reduction) — `main`] — 2026-07-23

### Fixed
- `images/care-03.jpg` resized from 3507×5261 (1.2 MB) to 533×800 (102 KB)
- `images/care-04.jpg` resized from 5310×3540 (989 KB) to 800×533 (93 KB)
- `images/care-05.jpg` resized from 5184×3456 (2.1 MB) to 800×533 (106 KB)
- `images/care-06.jpg` resized from 3840×5760 (2.4 MB) to 533×800 (117 KB)
- Total combined reduction: 6.96 MB → 0.428 MB (93.9% smaller)
- Aspect ratios preserved, no unexpected EXIF rotation, JPEG quality q82

### Notes
- Used macOS `sips -Z 800 -s format jpeg -s formatOptions 82` (this repo's established tool precedent)
- Resized to scratch location first, visually inspected each file, promoted into place once all 4 passed
- No HTML/CSS changes, same filenames (transparent to all page references)
- The 5 unreferenced dead files (`care-07`–`11`, ~5.5 MB) were deliberately left out per user scope 
  decision — separate `BACKLOG.md` item ("Review whether care-07–11 should replace any current grid 
  photo") remains open; see that decision before compressing or deleting them
- Addresses `docs/governance/PROJECT_RISK_REGISTER.md` R-007 (performance), marked Partial closure

---

## [docs: hash placeholder backfill + vault baton refresh — `main`] — 2026-07-22

### Fixed
- Backfilled 3 stale "hash once committed" placeholders now that each real commit/tag exists:
  `docs/project/STATUS.md`'s v2.9.0 entry (`10ee3d0`), `docs/project/STATUS.md`'s
  previously-missed v2.6.0 founder-photo entry (`dd88bf4`), and `PROGRESS_NOTE.md`'s "Commit
  status" section (`10ee3d0`).

### Changed
- Prepended a new dated entry to the AntBrainOS vault's `00_START_HERE/AGENT_HANDOFF.md`,
  recording the actual v2.8.0/v2.9.0 audit completion and superseding a 3-day-stale entry that
  still named an already-completed audit re-run as the open next task, without rewriting it.
- Refreshed a matching stale "in progress" pointer in this project's own vault
  `HANDOFF_TO_CLAUDE.md`.

### Notes
- No site code changed in this batch. Triggered by a `REPO_SESSION_START_RECOVERY_AUDIT.md` run
  that resolved `PASS WITH CONDITIONS`, naming both staleness gaps as explicit conditions.

---

## [feat: uptime monitoring + audit completion; deployed live — `main`] — 2026-07-19

### Added
- `.github/workflows/uptime-check.yml` — checks the VPS mirror
  (`old-fashion-care.craftandconscious.com`) every 30 minutes for HTTP 200 plus a basic
  content-sanity check, failing the job otherwise. Verified via a real triggered GitHub Actions
  run. GitHub's default behavior emails repo watchers on failure.

### Changed
- **Deployed this session's fixes (below) live to the VPS mirror**, the confirmed-real deployment
  target, per explicit user authorization. Independently re-verified via fresh `curl` evidence
  (not assumed from a successful deploy command alone).
- Completed the production-readiness audit begun earlier the same day: ran the 4 remaining skills
  (`observability-analytics-readiness`, `rollback-risk-register`, `production-readiness-audit`
  capstone, `client-handoff-pack`), wrote both consolidated report files
  (`seo/audits/hygiene-2026-07-19.md`, `docs/governance/audits/production-readiness-2026-07-19.md`),
  and reconciled `docs/governance/{PROJECT_RISK_REGISTER,SECURITY_BASELINE,COMPATIBILITY_MATRIX,
  ROLLBACK_PLAN,RELEASE_GATE,PHASE_GATES,AGENT_RUN_LOG}.md` and `BACKLOG.md` against every finding.
- Closed the already-stale `PROJECT_RISK_REGISTER.md` R-005 entry (hero image — had remained
  marked Open despite being fixed 2026-07-12).

### Notes
- 7 new risks added to `PROJECT_RISK_REGISTER.md` (R-007 through R-013): image oversizing,
  unconfigured Formspree ID, missing form-analytics events, missing HSTS header, the unreferenced
  `care giver pics/` folder, unverified certbot renewal automation, unverified Netlify status.
- No further site-code changes beyond what's in the entry below — this entry is the audit's own
  completion plus the uptime check and the deploy of already-fixed code.

---

## [fix: contact form, contrast, touch target, content — `main`] — 2026-07-19

### Fixed
- **Contact form silently failed while claiming success.** `js/main.js`'s submit handler
  unconditionally called `preventDefault()` and showed the "message sent" success state on every
  submit, regardless of whether anything was actually sent — the form never called Formspree at
  all. Rewrote it to perform a real `fetch()` POST and only show success on an actual `2xx`
  response; added a matching honest error state (`#form-error` in `contact.html`, reusing the
  existing `.form-success` visual pattern) that appears on failure, re-enables the submit button,
  and keeps the form open for retry.
- `.nav__cta` and `.section--coral` used `--coral` (~3.87:1 on white, below WCAG AA's 4.5:1 for
  normal text) — migrated both to the already-established `--coral-fill` (~4.66:1) token, the same
  fix already applied to `.btn--coral` in `v2.1.0`. Verification found a second, related bug:
  `.phone-strip`'s own `background: var(--coral)` (declared later in the stylesheet) was overriding
  `.section--coral`'s fix on the one element that actually uses that class — fixed `.phone-strip`
  too.
- Mobile nav hamburger button was 38×31px, below the 44×44px minimum touch-target guideline —
  enlarged to a proper 44×44px hit area. Bundled a small UX improvement on the same element: the
  three-bar icon now morphs into an "X" when the menu is open (CSS-only, driven by the existing
  `aria-expanded` attribute).
- Escape key now closes the mobile nav and returns focus to the hamburger button (previously only
  closed on nav-link click).
- Homepage "Testimonials" section was showing literal, unfilled bracket-placeholder text (e.g.
  `"[Testimonial to be added — contact a current client for a quote before launch.]"`) under a
  heading claiming "Real words from real families." Reframed to three honest, first-party trust
  statements — each restating a claim already published elsewhere on the site (personal
  coordination by Regina, certified/vetted caregivers, transparent no-hidden-fee pricing) — no
  fabricated names, quotes, or attributions. Section heading and `aria-label` updated to match;
  visual structure (`section--dark`, 3-column card grid) unchanged.

### Notes
- These fixes surfaced from an in-progress production-readiness audit (9 of 13 planned skill-runs
  from the installed-but-never-executed 18/20-skill suite completed this session; the remainder,
  the two consolidated report files, and tracking-doc reconciliation are still pending — see
  `docs/project/STATUS.md`).
- `oldfashioncare.com`'s hosting mismatch (serves an unrelated third-party site, not this repo) is
  confirmed by the user to be expected/known right now — not a blocker, no action taken.
- The Formspree ID (`contact.html`) is still the placeholder `REPLACE_WITH_FORMSPREE_ID` — the form
  now fails/succeeds honestly, but won't actually deliver a message anywhere until a real ID is
  configured (requires the user's own Formspree account).
- All five fixes verified via local Playwright (mocked and real fetch responses, computed-style
  contrast checks, keyboard/focus checks, viewport screenshots) — see `docs/project/COMMIT_NOTES.md`
  for the full verification detail.

---

## [fix: push-workflow tracking-file gap — `main`] — 2026-07-18

### Fixed
- `Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` told the agent
  `PROGRESS_NOTE.md` and `PROGRESS_NOTES.md` were alternatives ("prefer `PROGRESS_NOTES.md`"), so a
  real push silently skipped `PROGRESS_NOTE.md` despite this repo's own governance docs marking it
  required unconditionally. `PROGRESS_NOTE.md` moved into the mandatory every-push list; the
  ambiguous guidance removed.
- `PROGRESS_NOTE.md` content corrected to reflect actual current repo state.

### Changed
- Added a `git diff --stat` cross-check to the push prompt's validation step (Section 6): every
  required tracking file is now checked by name before commit, not just assumed updated.
- Added Section 2a to the push prompt: the linked AntBrainOS vault project folder
  (`03_PROJECTS/Active/Old_Fashion_Care/`) must be updated on every push, not only at session end.

### Notes
- The same bug was confirmed present, and fixed the same way, in the AntBrainOS vault's own
  canonical copy of this prompt (no auto-sync exists between a repo's local copy and the vault
  template). The vault's `CLAUDE_CODE_SESSION_END.md` also gained an independent cross-check step
  (Step 18a) so a future closeout can't record a repo as clean/current purely by trusting a prior
  push-prompt step. See `docs/project/DECISION_LOG.md` this date, and the vault's own
  `08_DECISIONS/DECISION_LOG.md` #36.
- No site code changed; live site unaffected.

---

## [fix: founder photo aspect ratio — `main`] — 2026-07-18

### Fixed
- `about.html` founder photo (`images/regina.jpg`) was rendering visibly stretched/compressed,
  worst on mobile. A leftover `.founder-photo` placeholder-box rule in `css/style.css` was
  forcing a mismatched `aspect-ratio` (3/4 desktop, 16/9 mobile) onto the real photo with no
  `object-fit` override, so the browser stretched the image to fill those boxes. Added
  `aspect-ratio: auto;` to `img.founder-photo` so the photo renders at its natural ratio.

### Notes
- Re-synced the VPS preview at `old-fashion-care.craftandconscious.com` (manually mirrored from
  `main`, not Netlify). Surfaced that `oldfashioncare.com` — this repo's documented Netlify
  production domain — is not currently serving this repository at all (see
  `docs/project/STATUS.md`); unresolved, flagged for the user.

---

## [tooling: AntBrainOS kit install — `main`] — 2026-07-15

Dev-tooling only, on `main`. **No site files changed** — zero `.html`/`css`/`js`/`images`/
`netlify.toml` edits. `netlify.toml` publishes the repo root with no served route to `.claude/`,
so the live Netlify site's appearance, behavior, CSP headers, and redirects are all unchanged
(verified: `git diff` on `*.html`/`css`/`js`/`images`/`netlify.toml` between the pre- and
post-install commits is empty).

### Added
- **SEOKit** — `.claude/commands/seo/` (19), `.claude/skills/seo-*.md` (4),
  `.claude/agents/{seo-auditor,serp-researcher}.md`, and the `seo/` site context (reused from the
  Editorial Sage branch — same business).
- **EngKit** — `/eng` dispatcher skill at `.claude/skills/eng/` (26 subcommands).
- **TradeKit** — `/tk` command cards + adapters at `.claude/tradekit/`.
- **handoff-repository** — cross-tool skill at `.claude/skills/handoff-repository/` +
  `.agents/skills/handoff-repository/`, with a filled `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`.

### Notes
- EcomKit / VideoKit skipped (no ecommerce or video surface); MKTKit skipped (previously rolled back
  on the design branches as a poor fit for a static business site).
- Installing tooling onto `main` was explicitly authorized; it does not constitute a design/merge
  change to the live site.

---

## [Unreleased] — 2026-07-12

### Changed
- Homepage hero photo composition reworked: photo band widened (`--hero-photo-w` 57%→75%) and the
  `.hero::after` overlay compressed so solid navy is confined to the leftmost ~25% of the hero
  (was ~43-47%), with the image visible across the full right 75%.
- Homepage hero photo (`images/hero-ai.jpg`) replaced with a sharper resample of the client's
  clean hi-res original photo (was a soft, low-res 1100x934 crop).
- Desktop (≥1024px) hero height now scales with viewport width
  (`clamp(660px, 50vw, 900px)`) so the full photo frame — the entire lamp, both people's full
  heads, and more of the left curtain — displays without cropping. Hero is visibly taller on wide
  screens as a result. Tablet and mobile layouts unaffected.

### Fixed
- An interim `background-size: contain` zoom-out attempt (since reverted) had caused the hero's
  navy/photo boundary to grow with viewport width and reintroduced a hard vertical seam on wide
  monitors; final fix avoids both by using full-width `cover` with height scaling instead.

---

## [v2.1.0] — 2026-07-12

### Fixed
- Homepage hero CTA button (`.btn--coral`) failed WCAG AA contrast (3.87:1 vs the 4.5:1 required
  for normal-size text). Added `--coral-fill: #C2512B` (~4.66:1 vs white), used only for the
  button's fill/border; `--coral` itself is unchanged everywhere else.

### Changed
- Homepage hero gradient (`.hero::after`) and photo band (`--hero-photo-w`, 53%→57%) repositioned
  left per a fresh user-provided reference screenshot, so the photo reads through noticeably
  earlier/more clearly. Verified via Playwright screenshot + pixel sampling; no hard seam
  introduced; mobile hero (separate `@media` rules) unaffected. Known tradeoff: the
  headline-text-to-hairline clearance shrank from ~125px to ~80px at a 1200px viewport — still
  above this project's own historical ~60px safe minimum, but worth knowing for future tuning.

---

## [v2.0.1] — 2026-07-07

### Changed
- Completed the V3.4 migration: reconciled root v3.3 docs with `docs/governance/` and
  `docs/project/` (the paths the installed V3.4 skills actually read from). Moved 8 root docs
  with real content into `docs/project/`; merged 6 governance file pairs into `docs/governance/`;
  authored real content for the new `docs/project/ARCHITECTURE.md` and
  `docs/governance/PROJECT_RISK_REGISTER.md` (neither had real project-specific content before).
- Rewrote `AGENTS.md`/`CLAUDE.md` to keep this project's specific rules while adding V3.4's
  genuinely new pieces (structured report format, skill references).
- Resolved all 3 remaining `.v34_migration_review/` candidates; directory removed.
- No HTML/CSS/JS site files changed — documentation and tooling only.

---

## [Unreleased] — 2026-07-05

### Fixed
- `og:image`/`twitter:image` on all 6 pages pointed to a nonexistent `images/og-default.png`,
  breaking social-share link previews site-wide — repointed to the existing `images/hero-ai.jpg`
  and corrected the declared image dimensions (1200×630 → actual 1100×934)
- `apple-touch-icon` link tag on all 6 pages pointed to a nonexistent `images/apple-touch-icon.png`
  — removed; `favicon.svg` remains as fallback

### Audited
- Meta titles, descriptions, canonical URLs, Open Graph tags, Twitter Card tags, `sitemap.xml`,
  and `robots.txt` across all 6 pages — confirmed correct, no changes needed

### Added
- Project Starter Kit V3.4 baseline (migrate mode) and the 18-skill production-readiness suite
  installed under `.claude/skills/` (22 skills total: 4 V3.4 + 18 PR suite), `.agents/`,
  `docs/governance/`, `docs/project/` — dev-tooling/workflow scaffolding only, no site-facing change

---

## [v1.5.0] — 2026-06-30

### Changed
- Hero gradient smoother 10-stop curve (uniform ~0.02/% rate); visible fade begins at 'h' in "where in the home" (~22% width)
- Hero gradient seam coverage at 46% strengthened to 0.26 opacity (was 0.20)
- Hero orange headline max-width 565→620px — "We make that possible." restored to 1 line (regression fix from v1.4.1)
- Hero sub font-size 0.97→0.94rem, max-width 648→580px — text lines barely reach caregiver's hand, shoulder stays uncovered
- No markup, image, or content changes — `css/style.css` hero rules only

---

## [v1.4.1] — 2026-06-29

### Changed
- Hero sub-paragraph reduced to 4 lines (font-size 1.05rem→0.97rem, max-width 480→582px)
- Hero copy column nudged upward (margin-top -5.2rem, reset on mobile) so the orange "possible." period aligns with the older woman's earlobe
- Hero gradient pulled left (transition starts ~5–8% earlier) to reveal more of the photo; seam-preventing opacity floor maintained
- No markup, image, or content changes — `css/style.css` hero rules only

---

## [v1.3.1] — 2026-06-18

### Changed
- GitHub repository renamed from `Old-Fashion-Care-` to `Old-Fashion-Care` (trailing dash removed)
- Local git remote URL updated to match new repo name

---

## [v1.3.0] — 2026-06-15

### Added
- Caregiver photo social proof section on homepage ("Real Care, Real People" — responsive grid of 6 photos)
- Regina Booker founder photo on about.html, replacing the "RB" initials placeholder (rectangular portrait)
- images/ folder with 12 photos: 11 stock caregiver photos (care-01.jpg — care-11.jpg) + regina.jpg
- Project Starter Kit v3.3 control structure: .claude/agents/, ai/, docs/, plans/
- 7 Claude sub-agents: debugger, docs-promoter, project-steward, repo-cartographer, security-reviewer, slice-planner, test-verifier
- 23 root-level project control docs (STATUS.md, BACKLOG.md, PLAN.md, ROADMAP.md, DECISION_LOG.md, etc.)
- Quality gate docs: DONE_CRITERIA.md, CHANGE_CONTROL.md, REPO_HEALTH_CHECK.md, ROLLBACK_PLAN.md, PROJECT_RISK_REGISTER.md
- .starter-kit/ migration workspace with install log, pre-migration inventory, and conflicts archive

### Changed
- README.md rewritten with real project description, tech stack, and setup notes
- PROJECT_BRIEF.md filled in with business goal, user profile, success criteria, and out-of-scope items
- STATUS.md and BACKLOG.md updated with real current state and work queue
- css/style.css: added .photos-grid responsive layout (3-col/2-col/1-col) and img.founder-photo rules

---

## [v1.2.0] - 2026-05-04 - Regina

### Added
- Open Graph metadata across public pages for richer social link previews
- Twitter/X summary card metadata across public pages
- Site favicon references and SVG favicon asset
- Plausible analytics script for oldfashioncare.com
- `robots.txt` with sitemap discovery
- `sitemap.xml` covering the public site pages
- `netlify.toml` with security headers, cache headers, content security policy, and `/index.html` redirect

### Changed
- Added robots indexing directives to secondary pages
- Improved production deploy readiness for static hosting on Netlify

---

## [v1.1.0] - 2026-05-01 - Regina

### Added
- `RELEASE.md` with v1.0.0 release notes
- `CHANGELOG.md` to track notable website changes by version
- `COMMITS.md` with GitHub commit-style release notes and commit metadata

---

## [v1.0.0] - 2026-04-30 - Regina

### Added
- Homepage with hero section, value proposition, trust signals, and call-to-action
- Services page covering personal care, home support, meal preparation, grooming, and companionship
- About page featuring founder Regina Booker with structured Person schema
- How It Works page with three-step process from initial call to first caregiver visit
- Questions page with comprehensive FAQ and FAQPage schema for rich search snippets
- Contact page with lead generation form and phone-first option for urgent cases
- Mobile-first responsive layout with hamburger navigation menu
- Custom design system — charcoal, coral, amber, and cream palette
- Typography system using Fraunces serif (headings) and Plus Jakarta Sans (body)
- FAQ accordion with single-open behaviour per category
- Active link highlighting across all pages
- LocalBusiness schema for local search visibility
- Canonical URLs and meta descriptions on all pages
- ARIA labels and semantic HTML for accessibility
- Vanilla JavaScript for navigation, mobile menu, and FAQ interactions (3 KB)
