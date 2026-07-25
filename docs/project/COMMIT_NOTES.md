# Commit Notes

This file is GitHub-specific tracking and must be kept unconditionally.

Use it to prepare commits before they are made.

---

## Summary

fix: mobile hero reworked to image-right overlay, mirroring desktop (branch
`design/editorial-sage-hero-split-depth`, commit `aa02b33`, supersedes `cb73699` same session)

## Description

- **What changed:** Rewrote the `@media (max-width: 820px)` hero rules in `css/editorial-sage.css`
  from a stacked (photo-below-text) layout into an overlay matching the desktop composition: photo
  on the right as an absolutely-positioned, right-anchored full-height band (58% wide), its left
  edge dissolving into `--es-cream` via the **same left-anchored radial `mask-image` desktop
  uses** (never a color overlay — honors the 2026-07-15 decision), widened so the dissolve spans to
  ~screen-72%; text content overlaid (`z-index:2`) on the cream left. `object-position` on the
  photo pushed left (`38% 28%`) so the care recipient is framed correctly in the narrower crop.
  Headline (`clamp(2.1rem, 5.4vw, 3.4rem)`, `max-width:88%`) and the sage accent
  (`max-width:56%`, wraps onto solid cream — it's the one low-contrast color against the light
  dissolve) tuned against **measured** rendered text widths (a Playwright width-probe, not
  estimates) so nothing illegible lands over the opaque photo. Buttons stack in the cream column.
  The prior commit's mobile-only vignette override removed (inherits desktop's, correct now that
  the photo is a right band again). `--es-ink-rgb` token (added by the prior commit) retained.
- **Why:** desktop already places the photo on the right with an arch dissolving into cream on its
  left; mobile previously abandoned that mask entirely for a plain rounded card stacked below the
  text (commit `cb73699`, this same session), which the user found flat/underwhelming once live.
  User explicitly asked mobile to mirror the desktop composition and picked the "big headline,
  photo overlaps" tradeoff (over "smaller headline, clean split") via an `AskUserQuestion` with
  ASCII-preview options.
- **Validation:** local Playwright screenshots at 360/390/430/768/1440px — photo prominent with a
  smooth arch dissolve, all text readable on cream, both figures visible at tablet+ (narrow phones
  show only the care recipient — the band is physically too narrow for both faces at readable
  size, accepted as emotionally apt), zero horizontal overflow at any width, desktop (1440px)
  pixel-unchanged. After deploy: `curl` confirmed the new rule served and the prior commit's
  top-anchor rule gone; a live Playwright screenshot of the actual production URL matched the
  local render exactly.
- **Risk/follow-up:** none new. Not separately tagged/snapshotted as its own milestone — folded
  into this session's push-workflow tag (see `STATUS.md`'s Latest Push entry).

---

## Summary

fix: mobile hero rotated to top-anchored arch, full-bleed stacked layout (branch
`design/editorial-sage-hero-split-depth`, commit `cb73699` — superseded same session by `aa02b33`
above; kept for history, do not treat as current)

## Description

- **What changed:** desktop's `.es-hero__media` mask (a left-anchored `radial-gradient` dissolving
  the photo into cream, tall ellipse) had no mobile equivalent — the existing
  `@media (max-width: 820px)` block killed the mask (`mask-image: none`) and boxed the photo in a
  rounded card (`border-radius`, `overflow:hidden`) stacked below the text. Rotated the same mask
  formula 90° (anchor `0% 50%` → `50% 0%`, ellipse radii swapped between axes) since mobile stacks
  the photo below rather than beside the text; removed the rounded-card treatment for a full-bleed
  photo matching desktop's flush-edges-except-the-seam look; added a mobile-only rotation of the
  `::after` depth vignette to match; changed `aspect-ratio` `5/4` → `4/5` for room to read the new
  top curve; added an `--es-ink-rgb` token consolidating a value hardcoded 2× (now 4×) in the
  vignette.
- **Why:** client-reported the mobile hero was "not attractive enough... a compromise" versus the
  desktop arch treatment.
- **Validation:** local Playwright at 390/430/768/1440px — genuine arch dissolve at the photo's
  top edge, full-bleed, no card remnants, no overflow; desktop pixel-unchanged. Deployed live,
  independently re-verified via `curl` + live Playwright.
- **Risk/follow-up:** superseded same session (see `aa02b33` above) after the user reviewed it live
  and preferred a desktop-mirroring layout instead of the stacked treatment.

---

## Summary

Install AntBrainOS kit tooling — EngKit, TradeKit, handoff-repository (branch
`design/editorial-sage-hero-split-depth`)

## Description

- **What changed:** dev-tooling only. Added **EngKit** (`.claude/skills/eng/`, `/eng` dispatcher +
  26 subcommands), **TradeKit** (`.claude/tradekit/`, 7 `/tk` command cards + adapters + templates),
  and the cross-tool **handoff-repository** skill (`.claude/skills/handoff-repository/`,
  `.agents/skills/handoff-repository/`, plus a filled
  `docs/governance/REPOSITORY_HANDOFF_CONFIG.md` with this repo's real Netlify/VPS/snapshot values).
  SEOKit was already present on this branch. **No site files touched** — zero changes to any
  `.html`/`css`/`js`/`images`/`netlify.toml`, so the rendered site and live Netlify deploy are
  unaffected.
- **Why:** user asked to install as many fit-appropriate kits as possible across all branches.
  EcomKit/VideoKit skipped (no ecommerce/video surface); MKTKit skipped (previously evaluated and
  rolled back here as a poor fit for a static business site).
- **Verified:** `git status` shows only `.claude/`, `.agents/`, `docs/governance/`, and tracking
  docs; a site-file guard grep returned clean. EngKit tree = 35 files; TradeKit = 7 cards; handoff
  skill + agent + config all present.
- **Scope:** this branch's tooling only; live site and design unchanged. Part of a 5-branch install
  pass (same tooling added to every branch; SEOKit additionally added to `main` + migration).

---

## Summary

Tag + snapshot split-depth hero's final curved photo-mask dissolve (branch
`design/editorial-sage-hero-split-depth`)

## Description

- **What changed:** Handoff/tracking docs only (`docs/project/STATUS.md`, `PROGRESS_NOTES.md`, this
  file) — no site code in this commit. Formalizes the split-depth hero work already committed and
  deployed: the photo now dissolves into the cream page along an organic curve via a `mask-image`
  on `.es-hero__media`, after two earlier attempts (feathered radial fill, then a widened version of
  the same) were both correctly rejected by the user as a hard line, then a milky/unprofessional
  haze.
- **Why it changed:** closing out this push per the repo's push/handoff/snapshot/tag workflow —
  user approved the live result and asked to commit, push, snapshot, and tag.
- **What was verified:** Playwright/Brave against the **live HTTPS URL**
  (`old-fashion-care.sage.hero.split.depth.craftandconscious.com`, DNS forced to the VPS IP) — 200,
  0 horizontal overflow, 0 console errors, zoomed seam crop confirmed smooth curved dissolve in
  production.
- **Remaining risk / follow-up:** sibling branch `design/editorial-sage-hero-cream-immersive` is
  pushed to GitHub but not yet tagged/snapshotted — separate step, pending user review of that
  variant. Merge of either branch to `main` remains a separate, un-made decision.

---

## Summary

Split-depth hero: curved photo-mask dissolve (branch `design/editorial-sage-hero-split-depth`)

## Description

- **What changed:** `css/editorial-sage.css` `.es-hero__media` mask. The prior mask (a large-radius
  radial off to the right) flattened into a nearly-straight edge and dropped the curve — the
  opposite of the request. Reshaped the `mask-image` to a **tall left-side ellipse**
  (`radial-gradient(46% 130% at 0% 50%, …)`) with a wide, evenly-stepped feather, so the photo
  dissolves into the cream **along a gentle convex curve** — keeping BOTH the organic curve AND a
  smooth gradient, with no cream-fog overlay. Mobile mask still off (clean rounded card).
- **Why:** user feedback — the previous version wrongly removed the curve and the gradient. This
  restores the curve and the smooth dissolve while keeping the clean image-mask technique (no haze).
- **Verified:** Playwright/Brave at 1440 (full + 2× zoomed seam) + 390 — visible organic curve,
  smooth gradual dissolve, no line, no haze, faces clear, mobile card clean, 0 overflow, 0 console
  errors. Redeployed + re-verified live.
- **Scope:** split-depth branch + subdomain only; cream-immersive, baseline, `main`, nginx, TLS
  untouched.

---

## Summary

Split-depth hero: professional edge via photo mask (branch `design/editorial-sage-hero-split-depth`)

## Description

- **What changed:** `css/editorial-sage.css`. Replaced the cream-fog technique entirely. The prior
  approach painted a semi-opaque cream radial *over* the photo (`.es-hero__media::before`), which —
  even widened — read as a milky haze / unprofessional smudge. Removed that pseudo-element and
  instead put a `mask-image` (off-centre radial) on `.es-hero__media` so the **photo's own left
  edge dissolves cleanly into the cream page** — a controlled, smooth soft edge with no overlay
  colour. Mobile disables the mask (`mask-image: none`) so the stacked photo stays a clean rounded
  card.
- **Why:** user feedback — the widened cream fade "was not smooth or professional-looking."
  Masking the image (rather than fogging cream over it) is the standard high-end "image bleeds into
  the page" technique and reads clean.
- **What was verified:** Playwright/Brave at 1440/390 + a 2× zoomed seam crop — clean gradual
  dissolve, no haze, no line, faces + the caregiver's arm read naturally, mobile card clean, 0
  overflow, 0 console errors. Redeployed to the VPS split-depth webroot and re-verified live.
- **Scope:** split-depth branch + its subdomain only. cream-immersive, baseline, `main`, nginx, TLS
  untouched.

---

## Summary

Split-depth hero: smooth the cream/photo curve (branch `design/editorial-sage-hero-split-depth`)

## Description

- **What changed:** `css/editorial-sage.css`, `.es-hero__media::before` radial-gradient only. The
  cream curve's fade was back-loaded (opaque to 56% of radius, then 0.72→0 crammed into the 72–90%
  tail), which read as a hard arc where the cream met the darker part of the photo. Replaced with a
  wide, evenly-stepped fade (28%→100% of radius, ~0.12–0.18 opacity steps) so the cream dissolves
  gradually with no perceptible line. Also pulls the opaque cream back slightly, keeping both faces
  clear.
- **Why:** user feedback — the split-depth seam looked like a hard line; they wanted it much smoother.
- **What was verified:** Playwright/Brave at 1440/1024/390 + a 2× zoomed crop of the seam — smooth
  dissolve, no line, faces clear, 0 horizontal overflow, 0 console errors. Then redeployed to
  `/var/www/old-fashion-care-hero-split-depth/` and re-verified the live HTTPS URL.
- **Scope:** split-depth branch + its subdomain only. cream-immersive, baseline, `main`, nginx, TLS
  all untouched.

---

## Summary

Hero variant "split + depth": feathered curve + vignette (branch
`design/editorial-sage-hero-split-depth` — NOT main, NOT the baseline branch)

## Description

- **What changed:** `css/editorial-sage.css`, `.es-hero*` block only — **no markup change**, the
  two-column hero and all copy are kept.
  - `.es-hero__media::before` (the cream "organic curve") changed from a solid `var(--es-cream)`
    ellipse to a **feathered** radial fill (cream → transparent) so the photo dissolves into the
    cream text column instead of ending on a crisp arc.
  - New `.es-hero__media::after` — a low-opacity ink (`rgb(23,50,77)`) **vignette**: a top/bottom
    linear band plus an outer radial, giving the photo dimension while keeping the faces bright.
  - `.es-hero__media` gets `isolation: isolate` so the photo (bottom), vignette (z-index 1), and
    cream curve (z-index 2) layer self-contained. Mobile keeps the stacked rounded photo card, now
    carrying the same subtle vignette (the curve `::before` stays hidden when stacked).
- **Why it changed:** the low-disruption option of the "bring the hero to life" request — adds
  depth/integration without leaving the two-column split. Sibling variant:
  `design/editorial-sage-hero-cream-immersive` (full-bleed photo + gradient scrim).
- **What was verified:** Playwright (Brave engine) at 1440/1024/768/390 — `scrollWidth ==
  innerWidth` at every width (0 horizontal overflow) and 0 console errors. Text stays in its own
  column (no text-over-photo), so no new contrast risk. No build/test/lint scripts exist.
- **Remaining risk / follow-up:** local branch only — not pushed. Baseline branch and `main`
  untouched. Choosing a variant, pushing branches, and any merge to `main` are separate user
  decisions.

---

## Summary

SEO/performance hygiene pass + SEOKit install on branch `design/editorial-sage-elder-friendly` (NOT main)

## Description

- **What changed:**
  - Performance: removed the render-blocking Google Fonts `@import` from `css/editorial-sage.css`
    and added `<link rel="preconnect">` (googleapis + gstatic) + a direct font `<link rel="stylesheet">`
    to all 6 page `<head>`s; added `loading="lazy"` to the `index.html` founder photo.
  - Tooling: installed SEOKit — `.claude/commands/seo/` (19 commands), `.claude/skills/seo-*.md` (4),
    `.claude/agents/seo-auditor.md` + `serp-researcher.md`, and `seo/` context files
    (`SITE.md` filled with verified facts, `KEYWORD_MAP.md`, `REVENUE.md`).
  - Audits: `seo/audits/hygiene-2026-07-14.md` (from `/seo hygiene`) and
    `docs/marketing/cta-proposal-2026-07-14.md` (MKTKit output, preserved after the kit was removed).
- **Why it changed:** the user asked whether any of the external kits could add value; this delivers
  the concrete outcome — faster font rendering, an SEO toolkit that maps onto existing BACKLOG items,
  and a documented audit trail — while rejecting the kit (MKTKit) that did not fit.
- **What was verified:** source inspection + grep across all 6 pages confirmed the font links are
  present (3 per page), the `@import` is gone, and the lazy attribute is set. No build/test/lint
  scripts exist in this static repo; browser smoke-test not re-run this pass.
- **Remaining risk / follow-up:** contact-form Formspree `action` still a placeholder (deferred by
  user); 4 low-effort hygiene items outstanding (og:image dims, sitemap lastmod, JSON-LD on 3 pages,
  apple-touch-icon); `care-03..11.jpg` need resizing before use. All on the branch — `main` untouched.

---

## Summary (branch `design/editorial-sage-elder-friendly` — NOT main)

Add a complete, isolated "Editorial Sage" alternate design for the whole site: a warm cream / sage /
ink / sand light editorial theme (Lora + Source Sans 3) replacing the live dark charcoal/coral look,
across all six pages, on a dedicated branch that does not touch `main`.

## Description

- **What changed:**
  - New `css/editorial-sage.css` — full design system: cream/sage/ink/sand tokens, Lora + Source
    Sans 3 typography, light sticky cream header, mobile menu, hero with organic curved split, sage
    trust band, 5 line-icon service cards, sand process strip, Meet Regina split, dignified
    references statement, rounded final contact panel, footer, plus interior components (page hero,
    prose, service detail, pull quote, values grid, detailed steps, FAQ, contact form) and full
    responsive rules.
  - All six HTML pages (`index`, `about`, `services`, `how-it-works`, `questions`, `contact`)
    rebuilt onto the new shell + design language. `<head>` metadata (title, description, canonical,
    OG, Twitter, JSON-LD, Plausible, favicon) preserved verbatim on every page — only the stylesheet
    `<link>` swapped from `css/style.css` to `css/editorial-sage.css`. Added a skip link and
    `id="main-content"` to every page.
  - `js/main.js` — mobile menu upgraded to the new shell with Escape-to-close + focus return +
    `aria-expanded`/`aria-label` sync, while still driving the legacy markup (backward-compatible
    selectors). FAQ accordion, scroll-reveal, and the contact-form success handler unchanged.
  - New `docs/design/EDITORIAL_SAGE_DESIGN_SPEC.md` + `docs/design/reference/` reference image.
  - `css/style.css` left untouched (retained as the prior-design reference; not loaded on this
    branch).
- **Why it changed:** executes the vault's `EDITORIAL_SAGE_REDESIGN_EXECUTION_PLAN.md` (2026-07-13),
  authorized by the user, to produce a faithful implementation of the supplied Editorial Sage concept
  with real content, on an isolated branch for review before any merge decision.
- **What was verified:** Playwright headless rendering across 7 viewports (homepage) and all 6 pages
  at 1440/768/390 — zero horizontal overflow, zero console errors everywhere. Mobile menu
  open/click + Escape-close + aria state confirmed. FAQ accordion works by click and keyboard.
  Contact form hides + shows the success state on submit with the Formspree `action` and all field
  `name`s preserved. Local `href`/`src` sweep across all 6 pages (124 refs) → every referenced file
  exists. Head-integrity check confirmed one `<h1>` per page and all SEO/analytics metadata intact.
- **Content policy:** no fabricated testimonials — the reference's AI-generated quote cards were
  replaced with a dignified "references available during consultation" statement (user decision).
  Regina uses the real `images/regina.jpg`; hero uses the approved `images/hero-ai.jpg`.
- **Remaining risk or follow-up:** not committed or pushed (awaiting user go-ahead). `main` is
  untouched and still live. Botanical/service icons are hand-authored inline SVG line-art (no icon
  font). Formspree endpoint is still the pre-existing `REPLACE_WITH_FORMSPREE_ID` placeholder,
  unchanged by this work.

---

## Previous — Summary

Rework the homepage hero photo composition: enforce a 25%-navy/75%-image ratio, swap in the
client's hi-res original photo, and show the full frame (lamp, heads, curtain) on desktop with no
seam

## Previous — Description

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
