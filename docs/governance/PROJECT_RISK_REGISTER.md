# Project Risk Register

Use this to track risks that could affect correctness, security, delivery, maintainability, data
safety, client trust, or deployment stability.

## Risk Rating

| Level | Meaning |
|---|---|
| Low | Minor inconvenience or cleanup issue |
| Medium | Could slow delivery or create visible defects |
| High | Could break core functionality, data, deployment, or trust |
| Critical | Could cause data loss, security exposure, production outage, or irreversible damage |

## Open Risks

| ID | Risk | Area | Impact | Likelihood | Level | Mitigation | Status |
|---|---|---|---|---|---|---|---|
| R-001 | No automated tests, lint, or build check — all QA is manual browser verification | testing/verification | Medium | Medium | Medium | Keep the local `href`/`src` resolution sweep and manual page-load checks as standard practice before every push | Open |
| R-002 | SEO metadata is fully hand-maintained across 6 duplicated `<head>` blocks — easy for one page to drift | documentation drift | Medium | Medium | Medium | Re-run the SEO audit sweep whenever any page's `<head>` is touched | Open |
| R-003 | Snapshot/RepoBackups root has moved drives twice (`/Users/ant/WorkSync` → `DataHub_2TB` → `AntNVMe1TB`) — a future drive change could silently break the snapshot step | deployment / AI workflow | Medium | Low | Medium | Verify the RepoBackups path is still mounted and populated at the start of any session that will snapshot, per the machine-detection lesson in the AntBrainOS vault | Open |
| R-004 | Two parallel doc systems existed side-by-side (root v3.3 files vs `docs/governance`/`docs/project` V3.4 templates) before this consolidation — a future package install could reintroduce the same drift | AI workflow / documentation drift | Medium | Medium | Medium | This consolidation (2026-07-06/07) is the mitigation; re-check for reintroduced duplicates after any future skill/package install | Open |
| R-007 | The 4 live/referenced `care-*.jpg` files (care-03/04/05/06) were oversized at 3507–5760px, wasting ~6.7 MB; the 5 unreferenced dead files (care-07–11, ~5.5 MB) remain a separate decision | performance | Medium | High (confirmed, quantified) | Medium | **Fixed the 4 live files** (2026-07-23): resized to ~800×533 / 533×800 px via `sips -Z 800 -s formatOptions 82`; verified dimensions, aspect ratios preserved, no EXIF rotation artifacts; deployed in place with zero other file changes. **5 unreferenced files untouched** — separate `BACKLOG.md` item ("Review whether care-07–11 should replace any current grid photo") remains open; see that item before deciding whether to compress or delete them. | Partial (4 of 9 fixed, 5 remain) |
| R-008 | `contact.html`'s form has no live delivery endpoint — still points at the placeholder `https://formspree.io/f/REPLACE_WITH_FORMSPREE_ID`. Vendor decided as **Web3Forms, not Formspree** (see `docs/project/DECISION_LOG.md` 2026-07-22), but not yet configured | client content / deployment | High | High (confirmed, form cannot deliver any message) | High | Blocked on the client finalizing the purchase — swap `contact.html`'s form `action` to `https://api.web3forms.com/submit` and add a real `access_key` hidden field once issued (tracked in `BACKLOG.md`; implementation notes in `DECISION_LOG.md`) | Open |
| R-009 | No custom analytics events for contact-form success/failure — a future failure (once the form's delivery vendor is configured) would again be invisible to the business | observability | Medium | Medium | Medium | Add `plausible()` calls on the success/error paths in `js/main.js` | Open |
| R-010 | No `Strict-Transport-Security` (HSTS) header on either deploy path | security / deployment | Low | Low | Low | Add HSTS header to `netlify.toml` / VPS nginx config | Open |
| R-011 | 15MB unreferenced `care giver pics/` folder (raw candidate stock photos) tracked in git at repo root | repo hygiene | Low | Low | Low | Decide whether to delete or move outside the repo (tracked in `BACKLOG.md`) | Open |
| R-012 | Certbot renewal automation on the VPS is unverified (cert valid until 2026-10-12, but auto-renew cron/systemd timer not confirmed via SSH) | deployment | Low | Low | Low | Verify renewal automation before the cert's next renewal window | Open |
| R-013 | Netlify's actual configuration/status for this repo is unverified from outside (no dashboard access) | deployment | Low | Unknown | Low | User to check the Netlify dashboard directly | Open |

## Closed Risks

| ID | Risk | Resolution | Date closed |
|---|---|---|---|
| R-C01 | `og:image`/`twitter:image` referenced a nonexistent `images/og-default.png` on all 6 pages | Repointed to existing `images/hero-ai.jpg`, corrected declared dimensions | 2026-07-05 |
| R-C02 | `apple-touch-icon` link tag referenced a nonexistent file on all 6 pages | Tag removed; `favicon.svg` fallback confirmed working | 2026-07-05 |
| R-C03 | `main` and `migration/project-starter-v3-3` had silently diverged (each added the same file independently) | Reconciled via real merge commit combining both branches' tracking-doc entries | 2026-07-06 |
| R-005 | `images/hero-ai.jpg` was a soft, low-res placeholder crop | Swapped for the client's hi-res original (2026-07-12, commit `fd7543c`, tag `v2.2.0`) — this entry had gone stale (still marked Open) until corrected during the 2026-07-19 production-readiness audit | 2026-07-19 (fix landed 2026-07-12) |
| R-C04 | Contact form's JS unconditionally faked a success message regardless of actual submission | Rewrote to a real `fetch()` POST with honest success/error handling; deployed and verified live on the VPS mirror | 2026-07-19 |
| R-C05 | `.nav__cta`/`.section--coral`/`.phone-strip` used `--coral` (~3.87:1, fails WCAG AA) | Migrated to `--coral-fill` (~4.66:1); the `.phone-strip` cascade collision was found and fixed during verification | 2026-07-19 |
| R-C06 | Mobile nav hamburger touch target was 38×31px, below the 44px minimum | Enlarged to 44×44px; added an open/closed X-icon state | 2026-07-19 |
| R-C07 | Homepage testimonials section showed literal, unfilled bracket-placeholder text | Reframed to 3 honest, first-party trust statements | 2026-07-19 |
| R-C08 | No uptime monitoring on either deploy target | Added `.github/workflows/uptime-check.yml` (30-min interval), verified via a real triggered GitHub Actions run | 2026-07-19 |
| R-C09 | No dedicated `apple-touch-icon.png` — relied on `favicon.svg` fallback for iOS home-screen bookmarks | Generated `images/apple-touch-icon.png` (180×180) from the existing `favicon.svg` mark via a throwaway Playwright render (not committed); added `<link rel="apple-touch-icon">` to all 6 pages | 2026-07-28 |

## Risk Categories

- scope drift
- architecture
- data/schema
- imports/exports
- auth/security
- secrets/privacy
- deployment
- dependency/versioning
- testing/verification
- performance
- accessibility
- client content
- documentation drift
- AI workflow/process

## Review Cadence

Review this file:

- before implementation of risky work
- after bugs
- before deployment (merge to `main`)
- before client handoff
- at the end of each major slice
