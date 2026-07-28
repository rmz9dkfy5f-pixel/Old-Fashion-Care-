# Backlog

Use this for work queue control.

Do not let `BACKLOG.md` become the active plan. The active plan belongs in `PLAN.md`.

---

## Build Now

Required for the current phase.

- [ ] Nothing blocking — V3.4 migration is complete; pick from Build Next.

---

## Build Next

Likely needed after the current slice.

- [ ] Design review — visual polish and layout improvements across pages
- [ ] Mobile layout review — verify all pages look great on small screens
- [ ] Add `plausible()` custom events for contact-form success/failure (`js/main.js`) — no
      business-side visibility exists today when the form fails (2026-07-19, R-009)
- [ ] Add `Strict-Transport-Security` (HSTS) header to `netlify.toml` / VPS nginx config
      (2026-07-19, R-010)

---

## Build Later

Useful but not needed yet.

- [ ] Replace the homepage "trust" section's 3 first-party statements with real client
      testimonials, once collected (2026-07-19: the section previously showed unfilled
      bracket-placeholder text; reframed to honest, non-fabricated statements in the meantime —
      see `docs/project/DECISION_LOG.md` 2026-07-19)
- [ ] Structured data (Schema.org LocalBusiness) — improves Google local search appearance
- [ ] Internal linking audit — ensure pages link to each other effectively for SEO
- [ ] Review whether care-07 through care-11 should replace any current grid photos
- [ ] Configure Web3Forms for the contact form (`contact.html`) — vendor decided 2026-07-22 as
      Web3Forms, not Formspree (see `docs/project/DECISION_LOG.md`); the form now fails/succeeds
      honestly (fixed 2026-07-19) but still can't deliver a message anywhere until this is
      configured. **Blocked on the client finalizing the purchase** — not agent-actionable until
      then. Implementation: swap the form `action` to `https://api.web3forms.com/submit` and add a
      real `access_key` hidden field once issued.
- [ ] Get real-device iOS Safari / cross-browser check — this environment only has a Chromium
      binary available for automated verification
- [ ] Decide whether/how to delete the untracked 15MB `care giver pics/` folder (raw candidate
      stock photos, unreferenced anywhere on the site — found during the 2026-07-19
      production-readiness audit)

---

## Maybe

Interesting but not proven to be needed yet.

- [ ] Tips or resources page — content for caregivers or families (could help SEO and trust)
- [ ] FAQ expansion — build on the existing questions.html page

---

## Do Not Build

Rejected or out of scope.

- [ ] Online booking / scheduling system
- [ ] User accounts or client portal
- [ ] Payment processing
- [ ] CMS or backend
- [ ] Blog

---

## Completed

- [x] Add photo / social proof section to site — homepage photo grid added (care-01 through care-06)
- [x] Regina Booker founder photo on about.html — rectangular portrait, replacing initials placeholder
- [x] Project Starter Kit v3.3 migration — control docs, agents, ai/, docs/, plans/ installed
- [x] Fill in README.md, docs/project/PROJECT_BRIEF.md, docs/project/STATUS.md, BACKLOG.md with
      real project content
- [x] Audit and improve on-page SEO metadata across all 6 HTML pages — metadata already correct;
      fixed broken og-image/twitter-image and apple-touch-icon references (2026-07-05)
- [x] Deploy Production Readiness Skills (V3.4 baseline + 18-skill suite) into `.claude/skills/` (2026-07-05)
- [x] Merge migration/project-starter-v3-3 branch into main (2026-07-06, tag v2.0.0)
- [x] Complete V3.4 migration — reconciled root docs with docs/governance/docs/project, resolved
      all quarantined candidates (2026-07-07)
- [x] Swap in the client's hi-res original photo for `images/hero-ai.jpg` (2026-07-12)
- [x] Fix contact form silently faking a success message on every submit regardless of whether
      anything actually sent — now does a real submission attempt with honest success/error
      states (2026-07-19)
- [x] Fix `.nav__cta`/`.section--coral`/`.phone-strip` WCAG AA contrast failures — migrated to the
      already-established `--coral-fill` token (2026-07-19)
- [x] Enlarge mobile nav hamburger touch target to 44×44px; add open/closed icon state
      (2026-07-19)
- [x] Add Escape-key handling to close the mobile nav (2026-07-19)
- [x] Fix homepage testimonials section showing literal placeholder text (2026-07-19)
- [x] Add automated uptime monitoring (`.github/workflows/uptime-check.yml`, 30-min interval,
      checks the VPS mirror) — none existed before (2026-07-19)
- [x] Deploy this session's fixes to the confirmed-real VPS deployment target, verified live
      (2026-07-19)
- [x] Complete a first full pass of the 18/20-skill production-readiness suite (13 skills run
      against `main`) — see `docs/governance/audits/production-readiness-2026-07-19.md` and
      `seo/audits/hygiene-2026-07-19.md` (2026-07-19)
- [x] Image optimization — compress the 4 live, referenced `care-*.jpg` photos (care-03/04/05/06)
      from 3507–5760px native resolution to ~800×533px (landscape) / 533×800px (portrait), reducing
      page weight by 93.9% (6.96 MB → 0.428 MB). The 5 unreferenced dead files (care-07–11) remain
      uncompressed pending a separate decision. (2026-07-23)
- [x] Create a dedicated 180×180 `apple-touch-icon.png` from the existing `favicon.svg` mark
      (Playwright render); added the `<link rel="apple-touch-icon">` tag to all 6 pages (2026-07-28)

---

## Scope Decision Rules

Every new idea must be placed into one category:

```text
Build now
Build next
Build later
Maybe
Do not build
```

If it does not support the current validation question, it should usually not be in `Build Now`.
