# Changelog

All notable changes to the Old Fashion Care website will be documented here.

---

## [branch `design/editorial-sage-hero-split-depth`] — 2026-07-24

Mobile hero reworked to mirror desktop's image-right composition. Live at
`old-fashion-care.sage.hero.split.depth.craftandconscious.com`.

### Changed
- Mobile (`@media (max-width: 820px)`) hero photo is now a right-anchored full-height band whose
  left edge dissolves into cream via the same `mask-image` technique desktop uses (never a color
  overlay), instead of a plain rounded card stacked below the text (`aa02b33`).
- Headline and sage accent sizing tuned so no text lands over the opaque part of the photo at any
  phone width tested.
- (Superseded intermediate step, same day: `cb73699` rotated the mask to a top-anchored, full-bleed
  stacked layout — replaced by the above after live review.)

### Notes
- Desktop (`>820px`) hero unchanged; verified pixel-identical before/after.

---

## [tooling: AntBrainOS kit install — branch `design/editorial-sage-hero-split-depth`] — 2026-07-15

Dev-tooling only. **No site files changed** — zero `.html`/`css`/`js`/`images`/`netlify.toml` edits,
so the rendered site is byte-identical and the live Netlify site is unaffected.

### Added
- **EngKit** — `/eng` dispatcher skill at `.claude/skills/eng/` (26 subcommands).
- **TradeKit** — `/tk` command cards + adapters at `.claude/tradekit/`.
- **handoff-repository** — cross-tool skill at `.claude/skills/handoff-repository/` +
  `.agents/skills/handoff-repository/`, with a filled `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`.
- (SEOKit was already installed on this branch.)

### Notes
- EcomKit / VideoKit skipped (no ecommerce or video surface); MKTKit skipped (previously rolled back
  here as a poor fit for a static business site).

---

## [branch `design/editorial-sage-hero-split-depth`] — 2026-07-15

Isolated hero-variant branch cut from `design/editorial-sage-elder-friendly`. Does **not** change
`main` or the live site. One of two new hero variants; the low-disruption one (keeps the split).

### Changed
- Homepage hero given more **depth** while keeping the two-column split — **CSS-only**
  (`css/editorial-sage.css` `.es-hero*`), no markup change. The crisp cream-ellipse curve
  (`.es-hero__media::before`) became a feathered radial fill so the photo dissolves into the cream
  column, and a new low-opacity ink vignette (`.es-hero__media::after`) makes the photo read as a
  lit scene rather than a flat rectangle (faces kept bright). Mobile keeps the stacked rounded photo
  card with the same subtle vignette.

### Notes
- Sibling variant: `design/editorial-sage-hero-cream-immersive` (full-bleed photo + cream gradient
  scrim). Baseline `design/editorial-sage-elder-friendly` unchanged. All hero copy preserved.

---

## [v2.3.0 — branch `design/editorial-sage-elder-friendly`] — 2026-07-14

Isolated branch. Does **not** change `main` or the live site.

### Changed
- Font loading on all 6 pages: replaced the render-blocking Google Fonts `@import` inside
  `css/editorial-sage.css` with `<link rel="preconnect">` (googleapis + gstatic) + a direct font
  `<link rel="stylesheet">` in each page `<head>`, so the browser discovers and fetches fonts
  immediately instead of after the CSS parses.

### Fixed
- `index.html` founder photo now has `loading="lazy"`, matching `about.html` (was eagerly loaded).

### Added
- SEOKit installed under `.claude/` (`commands/seo/` ×19, `skills/seo-*.md` ×4, `agents/seo-auditor.md`
  + `serp-researcher.md`) with `seo/` context files; `/seo hygiene` audit at
  `seo/audits/hygiene-2026-07-14.md`.
- `docs/marketing/cta-proposal-2026-07-14.md` — homepage CTA proposal (MKTKit output; kit itself
  evaluated then removed as a poor fit for a static business site).

### Notes
- No live-site behavior change beyond the two performance tweaks; all copy/metadata unchanged.
- Deferred: contact-form Formspree `action` placeholder; og:image dims, sitemap lastmod,
  `LocalBusiness` JSON-LD on services/contact/how-it-works, and `apple-touch-icon.png`.

---

## [Unreleased — branch `design/editorial-sage-elder-friendly`] — 2026-07-13

Isolated alternate design branch. Does **not** change `main` or the live site.

### Added
- New `css/editorial-sage.css` — a complete "Editorial Sage" design system: warm cream / sage / ink
  / sand light editorial theme, Lora + Source Sans 3 typography, light sticky cream header with a
  botanical leaf mark, organic curved hero split, sage trust band, 5 line-icon service cards, sand
  process strip, Meet Regina split, dignified references statement, rounded final contact panel, and
  all interior-page components (page hero, prose, service detail, pull quote, values grid, FAQ,
  contact form).
- `docs/design/EDITORIAL_SAGE_DESIGN_SPEC.md` and `docs/design/reference/` reference image.
- Accessibility: skip link + `#main-content` on all six pages; `:focus-visible` outlines;
  `prefers-reduced-motion` handling; Escape-to-close + focus-return on the mobile menu.

### Changed
- All six pages (`index`, `about`, `services`, `how-it-works`, `questions`, `contact`) rebuilt onto
  the new editorial shell and now load `css/editorial-sage.css` instead of `css/style.css` (on this
  branch only). All `<head>` metadata (title/description/canonical/OG/Twitter/JSON-LD/Plausible/
  favicon) preserved verbatim; all verified copy retained.
- `js/main.js` mobile-menu handling upgraded (Escape/focus-return/aria) with backward-compatible
  selectors; FAQ accordion, scroll-reveal, and contact-form success handler unchanged.

### Fixed
- Content no longer begins hidden: the legacy scroll-triggered `.reveal` (opacity:0 until scrolled
  into view) is neutralized so all sections are visible on load, per the plan's accessibility/motion
  rules. Caught on the Meet Regina page, where the "The difference is Regina" values grid and the
  "Ready to talk?" CTA rendered blank until scrolled to; fix applies site-wide.

### Notes
- No fabricated testimonials: the reference's AI-generated quote cards were replaced with a dignified
  "references available during consultation" statement (user decision).
- `css/style.css` retained untouched as the prior-design reference. Not committed/pushed; `main`
  remains the live design.

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
