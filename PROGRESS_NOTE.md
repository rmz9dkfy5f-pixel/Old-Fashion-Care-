# Progress Note

This file is GitHub-specific tracking and must be kept unconditionally.

Use this as the current active progress note.

For historical notes, copy completed entries into `PROGRESS_NOTES.md`.

---

## Latest — Mobile Hero Reworked to Image-Right Overlay, Deployed Live (2026-07-24)

Two sequential mobile-hero passes this session (see `docs/project/COMMIT_NOTES.md` /
`docs/project/DECISION_LOG.md` for full detail). Desktop already places the photo on the right with
an arch dissolving its left edge into cream (a `mask-image`, never a color overlay). Mobile had no
equivalent — it fell back to a plain rounded photo card stacked below the text.

1. `cb73699` (superseded same session): rotated the desktop mask 90° for a full-bleed,
   photo-below-text stacked layout.
2. `aa02b33` (current, live): after live review, reworked mobile into an **overlay** matching the
   desktop composition directly — photo as a right-anchored full-height band (58% wide), the
   desktop's left-anchored mask restored and widened so the dissolve spans to ~screen-72%, text
   overlaid on the cream left. User chose the "big headline, photo overlaps" balance over a
   "smaller headline, clean split" alternative. Headline/accent sizing tuned against measured
   rendered text widths (not estimated) so no text lands over the opaque photo.

**Deployed** to the live VPS preview (`old-fashion-care.sage.hero.split.depth.craftandconscious.com`)
via a single-file `scp` swap each time; live-verified via `curl` + production Playwright screenshots.
CSS-only (`css/editorial-sage.css`), desktop rules untouched both passes.

Separately this session (not this branch's own history): the local `main` checkout used this
session was 17 commits behind `origin/main` with an orphaned uncommitted draft blocking `git pull` —
resolved (draft discarded per user decision, fast-forwarded clean) before any hero work began.

---

## Previous — AntBrainOS Kit Tooling Install (2026-07-15)

Installed dev-tooling kits across all branches (user request). This branch gained **EngKit**
(`.claude/skills/eng/`), **TradeKit** (`.claude/tradekit/`), and **handoff-repository**
(`.claude/skills/` + `.agents/skills/` + filled `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`);
SEOKit was already present. **No site files changed** — the rendered site and live Netlify deploy
are byte-identical. EcomKit/VideoKit skipped (no surface); MKTKit skipped (previously rolled back).
See `docs/project/COMMIT_NOTES.md` for the full entry.

---

## Current Progress

Date:

```text
2026-07-24
```

Phase:

```text
Phase 2 — Mobile hero rework on `design/editorial-sage-hero-split-depth` complete, pushed, and
deployed live. No active implementation slice.
```

Current slice:

```text
None active. Mobile hero now mirrors desktop (image-right overlay, arch dissolving into cream)
per user request; live and verified. Next step is not agent-actionable until the user picks from
the ranked next-task list presented at this session's own closeout (see push-workflow report).
```

Completed (this branch):

- [x] `css/editorial-sage.css` `.es-hero*` only (no markup change): feathered the cream curve
      (`.es-hero__media::before` solid → radial fill) so the photo dissolves into the cream column
- [x] Added `.es-hero__media::after` depth vignette (low-opacity ink, top/bottom + outer edges;
      faces kept bright); `isolation: isolate` on the media for self-contained layering
- [x] Mobile keeps the stacked rounded photo card, now with the same subtle vignette
- [x] Verified 1440/1024/768/390 — 0 overflow, 0 console errors; no text-over-photo, no new contrast risk
- [x] **2026-07-15 follow-up (user feedback):** the cream/photo seam read as a hard line — the fade
      was back-loaded. Rewrote the `.es-hero__media::before` radial to a wide, evenly-stepped fade
      (28%→100% of radius) so the cream dissolves gradually with no perceptible edge; re-verified
      (zoomed seam crop) and redeployed to the VPS split-depth subdomain.
- [x] **2026-07-15 follow-up #2 (user feedback):** the widened cream fade "was not smooth or
      professional-looking" (milky haze). Changed technique entirely: removed the cream-fog
      `::before` and added a `mask-image` on `.es-hero__media` so the **photo's own edge** dissolves
      into the cream (clean "image bleeds into page" look, no overlay colour). Mask disabled on
      mobile (clean rounded card). Re-verified + redeployed.

Prior slice (Editorial Sage redesign) — still complete, unchanged this push:

- [x] Full-site Editorial Sage design on this branch (see PROGRESS_NOTES.md history)

Completed (earlier redesign detail retained for reference):

- [x] Slice 0/1 — new `css/editorial-sage.css` design system (tokens, Lora + Source Sans 3, cream
      sticky header, mobile menu, footer, base + accessibility primitives); `docs/design/` spec +
      reference image; skip link + `#main-content` added
- [x] Slice 2 — homepage hero (organic curved cream/photo split, ~50/50) + sage 4-item trust band
- [x] Slice 3 — 5 line-icon service cards (responsive 5→3→2→1) + sand "How It Works" process strip
      + edge botanical foliage
- [x] Slice 4 — Meet Regina split (real `regina.jpg`) + dignified references statement (no
      fabricated testimonials) + rounded pale-sage final contact panel + deep-sage footer
- [x] Homepage verified across 7 viewports (no overflow, no console errors, mobile menu w/ Escape);
      fixed a hero headline/accent/CTA clip found on screenshot (rebalanced split + accent size)
- [x] User reviewed the homepage and approved continuing
- [x] Slice 5 — all 5 secondary pages (`about`, `services`, `how-it-works`, `questions`, `contact`)
      rebuilt onto the shared shell + design language; `<head>` metadata preserved verbatim; FAQ
      accordion + Formspree contact form preserved
- [x] Slice 6 — verification across all 6 pages at 1440/768/390 (18 combos: 0 overflow, 0 console
      errors); FAQ click+keyboard, contact-form success + Formspree action preserved; 124-ref local
      link sweep clean; one `<h1>`/page + all SEO metadata intact
- [x] Fixed a content-visibility bug (user-caught on the Meet Regina page): the carried-over
      scroll-`.reveal` started sections at opacity:0 until scrolled to, so `about.html`'s values grid
      + CTA rendered blank on load. Neutralized `.reveal` site-wide (content visible by default) per
      the plan's §8.4/§13 rules. Re-verified: 0 hidden reveals on fresh load across all 6 pages, no
      overflow/console regressions.

In progress:

- [ ] None — mobile hero rework complete, pushed (`aa02b33`), and deployed live.

Blocked:

- [ ] Nothing blocking this branch. Client review of the live hero/design variants and any
      merge-to-main decision remain the user's call (see Next Best Actions in the vault's
      `CURRENT_CONTEXT.md`).

Next action:

> User-confirmed at this session's own closeout (see push-workflow Final Output / Section 6) — not
> asserted here.

Checks run:

```bash
# Local Playwright at 360/390/430/768/1440px (both this session's hero passes):
#   scrollWidth == innerWidth at every width → 0 horizontal overflow
#   photo prominent, smooth arch dissolve into cream, all text readable, desktop pixel-unchanged
# Live re-verification via curl (new CSS content/Last-Modified) + a production Playwright
#   screenshot of the actual deployed URL, matching local renders.
# Static site — no build/test/lint scripts.
```

Commit status:

```text
design/editorial-sage-hero-split-depth @ aa02b33, pushed, matches origin, hard-clean (pre-this-
push-workflow-run). Deployed live to the VPS preview via a single-file scp swap, independently
re-verified. main untouched by this branch's own work (separately fast-forwarded to origin/main
this session — see Latest Push entry above).
```

Approval status:

```text
Both hero passes explicitly approved by the user before implementation (Plan Mode + ExitPlanMode
each time); the second pass's design tradeoff ("big headline, photo overlaps" vs. "smaller
headline, clean split") was a direct user pick via AskUserQuestion. Redeploy to the live preview
explicitly authorized both times ("redeploy" / "approved").
```
