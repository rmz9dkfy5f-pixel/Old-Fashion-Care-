# Progress Note

This file is GitHub-specific tracking and must be kept unconditionally.

Use this as the current active progress note.

For historical notes, copy completed entries into `PROGRESS_NOTES.md`.

---

## Latest — Tagged + RepoBackups-Snapshotted (2026-07-27)

This branch was pushed to `origin` on 2026-07-15 and deployed live to the VPS preview
(`old-fashion-care.sage.hero.cream.immersive.craftandconscious.com`), but never tagged or
RepoBackups-snapshotted — unlike its sibling `design/editorial-sage-hero-split-depth`. Closed that
gap today: applied annotated tag
`v2.14.0__cream-immersive-hero-full-bleed-gradient-scrim__commit-4182844` to the design commit
`4182844` (not this docs commit, not the branch's later tooling-only tip `25647aa` — matches this
repo's established "tag the code commit" rule), and created a RepoBackups snapshot at
`/Users/ant/WorkSync/Projects/RepoBackups/Old-Fashion-Care/v2.14.0__cream-immersive-hero-full-bleed-gradient-scrim__commit-4182844`.
Re-verified the live preview URL before writing this entry: `200 OK`, `Last-Modified` unchanged
since the original 2026-07-15 deploy. No site code changed — tracking docs + tag + snapshot only.

---

## Previous — AntBrainOS Kit Tooling Install (2026-07-15)

Installed dev-tooling kits across all branches (user request). This branch gained **EngKit**
(`.claude/skills/eng/`), **TradeKit** (`.claude/tradekit/`), and **handoff-repository**
(`.claude/skills/` + `.agents/skills/` + filled `docs/governance/REPOSITORY_HANDOFF_CONFIG.md`);
SEOKit was already present. **No site files changed** — rendered site and live Netlify deploy are
byte-identical. EcomKit/VideoKit skipped (no surface); MKTKit skipped (previously rolled back). See
`docs/project/COMMIT_NOTES.md` for the full entry.

---

## Current Progress

Date:

```text
2026-07-27
```

Phase:

```text
Phase 2 — Hero variant branch `design/editorial-sage-hero-cream-immersive` (cut from
`design/editorial-sage-elder-friendly`; does NOT touch main). Pushed 2026-07-15, deployed live;
now (this commit) tagged v2.14.0 and RepoBackups-snapshotted. No active implementation slice.
```

Current slice:

```text
Bring the Editorial Sage homepage hero "to life" like the live/main design (full-bleed photo +
gradient scrim), explored as separate branches rather than editing the baseline. THIS branch =
"cream immersive": full-bleed `images/hero-ai.jpg` + a cream→transparent gradient scrim (the live
hero's technique, recolored to the sage palette so the hero stays light), ink/sage copy over it.
Sibling branch `design/editorial-sage-hero-split-depth` (keep the split, add depth) is the other
variant. Baseline `design/editorial-sage-elder-friendly` left unchanged for comparison.
```

Completed (this branch):

- [x] `index.html`: moved `.es-hero__media` out of `.es-hero__inner` to a full-bleed layer (kept the
      semantic `<img>` + alt/width/height)
- [x] `css/editorial-sage.css`: rewrote the `.es-hero*` block — full-bleed photo, cream horizontal
      scrim (opaque left, clears ~66%) + soft vignette, single-column copy over it; deleted the
      cream-ellipse curve
- [x] Responsive: ≥1200px left-fading scrim (copy left / faces right); ≤1199px top-weighted scrim
      (copy over cream top / embrace below) so fixed-width copy never overlaps the faces
- [x] Verified 1440/1280/1024/768/390 — legible (ink-soft/cream ≈ 6.9:1), no seam, faces visible,
      0 overflow, 0 console errors (Playwright-measured)

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

- [ ] None — both hero-variant branches (cream-immersive, split-depth) are now built, pushed,
      deployed live, tagged, and snapshotted. Waiting on the client's design-direction decision.

Blocked:

- [ ] Nothing blocking. Choice of variant and any merge-to-main decision remain the user's call.

Next action:

> Wait for the client to review the live hero/design variants and decide. If a variant is chosen,
> decide on folding it in / merging to `main` — a separate, explicit, high-blast-radius decision.

Checks run:

```bash
# Playwright (Brave engine) at 1440/1280/1024/768/390:
#   scrollWidth == innerWidth at every width  → 0 horizontal overflow
#   0 console errors
#   ink-soft (#43586a) over cream (#fbf7ee) ≈ 6.9:1  → passes WCAG AA
# Static site — no build/test/lint scripts.
# 2026-07-27: curl -I on the live preview URL → 200 OK, Last-Modified unchanged since 2026-07-15.
```

Commit status:

```text
Pushed to origin/design/editorial-sage-hero-cream-immersive 2026-07-15, deployed live. Tagged
v2.14.0__cream-immersive-hero-full-bleed-gradient-scrim__commit-4182844 and RepoBackups-snapshotted
2026-07-27 (this commit is the docs-only record of that ceremony; the tag itself points at the
earlier design commit 4182844, per this repo's "tag the code commit" rule). Baseline
design/editorial-sage-elder-friendly and main both untouched.
```

Approval status:

```text
User approved the plan (ExitPlanMode) to create two hero-variant branches, then explicitly
confirmed (2026-07-27) tagging + RepoBackups-snapshotting this branch as the next task.
```
