# Decision Log

Use this file for important decisions.

Do not use it for tiny task notes.

---

## 2026-07-24 — Mobile Hero: Image-Right Overlay With "Big Headline, Photo Overlaps," Not a Narrower Clean Split

### Decision
For the mobile (≤820px) hero on `design/editorial-sage-hero-split-depth`, mirror desktop's
image-right + left-arch-into-cream composition via an **overlay** layout (photo absolutely
positioned as a right-anchored band, text overlaid on the cream left), and — given a real tradeoff
between headline size and text/photo separation on a narrow phone — keep the headline at its full
display size, letting it extend rightward over the mask's *cream-dissolved* zone (never the opaque
photo), rather than shrinking the headline to guarantee it never crosses into the dissolve zone at
all.

### Reason
Presented the user two concrete options with ASCII-preview mockups: "big headline, photo overlaps"
(headline full-size, may extend over the cream-dissolved zone) vs. "smaller headline, clean split"
(headline shrunk so every line stays fully left of the photo, no overlap at all). User chose the
former — matching the desktop hero's own character more closely (desktop's headline is prominent
and the photo is the dominant right-side element) rather than optimizing purely for a strict,
smaller-scale text/photo separation on phones.

### Alternatives Considered
- Smaller headline, clean split (offered as the alternative option; not chosen — user found it
  undersells the desktop's confident headline scale).
- Keep the stacked/top-arch layout from earlier the same session (`cb73699`) — rejected on review;
  the user wanted the photo beside the text, not below it, to feel consistent with desktop.

### Consequences
On the narrowest phones tested (360–390px), the sage italic accent line ("We make that possible.")
wraps onto solid cream to stay legible (sage-on-light-dissolve reads low-contrast, unlike the dark
ink headline/sub which read fine over the light dissolve) — this constraint was discovered via
measurement, not assumed. Only the care recipient's face is visible in the photo crop below ~430px;
both figures show from tablet width (768px) up. Future tuning of this hero should preserve the
"headline may reach the dissolve zone, sub/accent/buttons stay on solid cream" rule established
here rather than re-deriving it from scratch.

### Status
Accepted and implemented 2026-07-24.

---

## 2026-07-08 — Auto-Chain CLAUDE_CODE_SESSION_END After the Repo Push/Handoff Prompt

### Decision
`Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` now automatically
continues into the AntBrainOS vault's `CLAUDE_CODE_SESSION_END` SOP in the same turn (new Section
13), feeding it the just-established commit hash/tag/snapshot path. This is also applied to the
vault's canonical master copy of the prompt (`09_PROMPTS/Claude_Code_Prompts/04_Prompts/...`), so
every project using that prompt gets the same auto-chaining going forward.

### Reason
On 2026-07-07/08, `CLAUDE_CODE_SESSION_END` was run once, then a further repo push landed
afterward, leaving the vault's `CURRENT_CONTEXT.md`/`HANDOFF_TO_CLAUDE.md` "Last Known Good State"
one push stale until a separate correction pass caught it (see `06_LESSONS_LEARNED/Claude_Code/
2026-07-08_session_end_must_be_the_literal_last_action.md`). Chaining the two prompts into one
automatic sequence prevents this class of drift structurally, rather than relying on remembering
to run session-end manually as the literal last action every time.

### Alternatives Considered
- Keep the two prompts separate and just be more disciplined about running session-end last
  (rejected — this is exactly the discipline that already failed once)
- Only fix this repo's local prompt copy, leave the vault master untouched (rejected — user wanted
  a permanent, systemic fix across all projects, not a one-off local patch)

### Consequences
Every push now triggers a full vault session-end close-out (`CURRENT_CONTEXT.md`, `SESSION_LOG.md`,
`HANDOFF_TO_CLAUDE.md`, lessons learned, context packet, `last_run:` updates) even for small
pushes. This is intentional — keeping the vault always current is preferable to an occasional
larger drift.

---

## 2026-07-07 — Consolidate to docs/governance/docs/project as Single Source of Truth

### Decision
Move real content from 14 root v3.3 docs into `docs/governance/`/`docs/project/` (the paths the
installed V3.4 skills hardcode), deleting the root duplicates rather than keeping both or leaving
`docs/governance/`/`docs/project/` as unpopulated generic templates.

### Reason
The installed V3.4 skills (`v34-execution-loop`, `v34-production-readiness`) never read root
files — they only ever look at `docs/governance/`/`docs/project/`. Leaving root as canonical would
mean those skills always see empty generic templates instead of real project state, making the
V3.4 adoption cosmetic rather than functional.

### Alternatives Considered
- Keep root canonical, leave `docs/governance/`/`docs/project/` unpopulated (rejected — user chose
  full functional adoption over a partial/cosmetic one)

### Consequences
`AGENTS.md`, `CLAUDE.md`, `README.md`, and several actively-used skill/agent files needed their
path references updated to match. `docs/governance/PROJECT_RISK_REGISTER.md` and
`docs/project/ARCHITECTURE.md` now have real, project-specific content for the first time.

---

## 2026-07-07 — Keep the 7 Real Sub-Agents Authoritative Over Generic V3.4 Roles

### Decision
`ai/agents/SUBAGENT_ROLES.md`'s generic V3.4 role taxonomy (Planner/Implementer/Verifier/Security
Reviewer/Migration Reviewer/Documentation Reviewer/Release Reviewer) does not replace the existing
7 named sub-agents (`repo-cartographer`, `project-steward`, `slice-planner`, `debugger`,
`test-verifier`, `security-reviewer`, `docs-promoter`). Added a note to `SUBAGENT_ROLES.md`
pointing back to the real roster as authoritative instead.

### Reason
The generic roles don't map 1:1 onto the existing agents (no `repo-cartographer`/`docs-promoter`
equivalent; introduces "Migration Reviewer"/"Release Reviewer" with no corresponding
`.claude/agents/` file) and the existing 7-agent system is the real, working one already in use.

### Alternatives Considered
- Adopt the generic V3.4 roles and retire the 7 named agents (rejected by user — would discard a
  working, specific system for a generic parallel one)

### Consequences
`ai/agents/AGENT_REVIEW_GATES.md` keeps its agent-specific table and invocation order; only the
new "Review Gate D — Migration Safety" section was adopted from the V3.4 candidate.

---

## 2026-07-05 — Broken og-image Repointed to hero-ai.jpg, Not a New Dedicated Asset

### Decision
Fix the site-wide broken `og:image`/`twitter:image` reference (`images/og-default.png`, which
did not exist) by repointing it to the existing `images/hero-ai.jpg`, correcting the declared
dimensions to the file's actual 1100×934, rather than waiting for a purpose-built 1200×630 social
card image.

### Reason
Every page currently shows a broken social-share preview image; a working (if imperfectly
proportioned) image now is better than a correct one later. `hero-ai.jpg` is the only existing
brand-appropriate image close to a usable social-card shape.

### Alternatives Considered
- Wait for the user to supply a proper 1200×630 asset (rejected for now — leaves the bug live)
- Just remove the og:image tags entirely (rejected — worse than a passable image)

### Consequences
Social share previews will show `hero-ai.jpg` cropped/scaled by each platform rather than a
purpose-built card. A dedicated `og-default.png` remains a good future improvement (tracked in
`BACKLOG.md`).

---

## 2026-07-05 — Removed Broken apple-touch-icon Tag Instead of Creating a Placeholder Asset

### Decision
Remove the `<link rel="apple-touch-icon">` tag (referenced a nonexistent `images/apple-touch-icon.png`)
from all 6 pages rather than generate a placeholder icon file.

### Reason
No apple-touch-icon asset exists yet; `favicon.svg` is a reasonable interim fallback and modern
iOS handles SVG favicons reasonably well. Better to remove a broken reference than commit a
low-effort placeholder icon.

### Alternatives Considered
- Generate a quick 180×180 PNG from an existing image (rejected — would need real design input,
  not a quick crop)

### Consequences
iOS "Add to Home Screen" icon quality depends on the `favicon.svg` fallback until a dedicated
touch icon is created (tracked in `BACKLOG.md`).

---

## 2026-07-05 — Install Both V3.4 Baseline and 18-Skill Suite Despite Existing v3.3 Setup

### Decision
Install the full Project Starter Kit V3.4 baseline plus the 18-skill production-readiness suite
into this repo, even though a v3.3 Starter Kit migration is already partially in place on this
branch, rather than installing only the 18-skill suite.

### Reason
User chose full coverage over minimal footprint. The V3.4 installer's migrate-mode safety rules
(no overwrite, quarantine conflicts) made this low-risk to attempt.

### Alternatives Considered
- 18-skill suite only, skip V3.4 baseline (would have avoided the `docs/project/*` duplication
  with existing root tracking docs, but user preferred full coverage)

### Consequences
- 3 real conflicts (`AGENTS.md`, `CLAUDE.md`, `ai/agents/AGENT_REVIEW_GATES.md`) quarantined into
  `.v34_migration_review/` for manual review rather than applied
- `docs/project/*.md` now duplicates root-level tracking docs already in active use — needs
  reconciliation (tracked in `BACKLOG.md`)

---

## 2026-06-15 — Founder Photo: Rectangular Portrait, Not Circular Crop

### Decision
Display Regina's founder photo as a full rectangular portrait using `width: 100%; height: auto` instead of a circular crop.

### Reason
The circular crop (50% border-radius with fixed 260×260px) cut off too much of the photo and looked unnatural and unprofessional. The rectangular format shows the full image at its natural proportions.

### Alternatives Considered
- Circular crop (rejected — too much cropping, unnatural appearance)
- Fixed aspect-ratio portrait crop with object-fit: cover (not needed — natural proportions preferred)

### Consequences
Photo height is determined by the image's natural proportions. If Regina's photo is replaced with a very tall or very wide image in the future, layout may need adjustment.

---

## 2026-06-15 — Photo Placement: Inline on Homepage, Not Dedicated Gallery Page

### Decision
Add caregiver photos as an inline section on the homepage (between Testimonials and How It Works), not as a separate gallery page.

### Reason
Social proof is most effective on the highest-traffic page without requiring users to navigate away. A dedicated gallery page would reduce exposure.

### Alternatives Considered
- Dedicated gallery/photos page (rejected — requires extra navigation click, reduces social proof impact)
- Both homepage + about page (deferred — possible future enhancement)

### Consequences
The homepage grows by one section. Future gallery expansion would either extend the homepage grid or create a new page linked from the nav.

---

## 2026-06-15 — Migration Branch Strategy: Separate Branch, Not Direct to Main

### Decision
Run Starter Kit v3.3 migration on a dedicated branch (`migration/project-starter-v3-3`) rather than directly on `main`.

### Reason
Migration adds ~40+ new files. Working on a separate branch protects the stable live site on `main` from accidental breakage and allows review before merge.

### Alternatives Considered
- Commit directly to main (rejected — too risky for a 40+ file addition)

### Consequences
Branch must be reviewed and merged to main after SEO audit is complete. Netlify deploys from main, so migration work is not live until merged.
