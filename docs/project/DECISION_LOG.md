# Decision Log

Use this file for important decisions.

Do not use it for tiny task notes.

---

## 2026-07-28 — Rasterize the Existing favicon.svg for apple-touch-icon.png Instead of Waiting on New Design Input

### Decision
Generate `images/apple-touch-icon.png` (180×180) by rasterizing the repo's own `favicon.svg` mark.
This is not the alternative rejected on 2026-07-05 ("generate a quick 180×180 PNG from an existing
image") — that rejection was reasoned in the context of cropping a *photo* (would need real design
input, not a quick crop). Rasterizing the site's own already-approved vector lettermark at a fixed
size is a deterministic, mechanical conversion, not a creative/design decision, and is appropriate
for a coding agent to execute directly.

### Reason
`favicon.svg` (rounded charcoal square, centered coral serif "O", using this repo's exact
`--charcoal`/`--coral` tokens) is the only icon-appropriate asset in the repo — `images/` and
`care giver pics/` contain only photographic JPGs, none square or icon-like. No new design work is
introduced; the output is a fixed-size render of an existing, already-approved mark.

### Alternatives Considered
- Commission or generate a new icon design — rejected, unnecessary; the SVG mark is already the
  site's approved brand icon at every other size.

### Consequences
Closes `docs/governance/PROJECT_RISK_REGISTER.md` R-006 (now `R-C09`) and the matching `BACKLOG.md`
item. The 2026-07-05 entry is unchanged and still correct for what it actually decided (removing the
broken tag); this entry only clarifies that its rejected alternative doesn't extend to rasterizing
the repo's own vector mark.

---

## 2026-07-22 — Contact Form Vendor Decided: Web3Forms (Not Formspree), Gated on Client Purchase Finalization

### Decision
The contact form's eventual delivery vendor is **Web3Forms**, not Formspree. `contact.html`'s form
currently points at `https://formspree.io/f/REPLACE_WITH_FORMSPREE_ID` (still a placeholder, never
configured — see `PROJECT_RISK_REGISTER.md` R-008). That placeholder will not be filled in with a
real Formspree ID; instead, once the client finalizes the purchase of this site, the form will be
switched to Web3Forms.

### Reason
User's direct instruction. No further business reasoning was given and none is needed to record —
this is a vendor pick, not a technical tradeoff this project needs to justify.

### Implementation Note (not yet done — recorded for whoever picks this up)
`js/main.js`'s submit handler is vendor-agnostic (`fetch(form.action, {method:'POST', body: new
FormData(form), headers:{Accept:'application/json'}})`, checks `response.ok`) — no JS change is
expected. The HTML-side change, once a real Web3Forms access key exists, is:
- `contact.html`'s `<form action="...">` swaps from `https://formspree.io/f/REPLACE_WITH_FORMSPREE_ID`
  to `https://api.web3forms.com/submit`.
- Add a hidden `<input type="hidden" name="access_key" value="...">` field with the real key.
- The existing Formspree-specific hidden fields (`_subject`, `_next`) are not Web3Forms field names
  (Web3Forms uses `subject` and `redirect`) — confirm against Web3Forms's current docs when this is
  actually implemented, don't assume the old field names carry over unchanged.

### Alternatives Considered
- Formspree (the prior placeholder default) — not chosen; superseded by this decision.

### Consequences
`PROJECT_RISK_REGISTER.md` R-008 and `BACKLOG.md`'s matching item are reworded to name Web3Forms
instead of Formspree. The underlying risk (contact form cannot deliver a message to the business)
is unchanged and still open — only the planned fix changed. This is explicitly **not
agent-actionable now**: no Web3Forms account or access key exists yet, and none should be created
speculatively. Implementation is blocked on the client finalizing the purchase.

**Status:** Accepted, not yet implemented — blocked on the client finalizing the purchase.

---

## 2026-07-19 — Reframe the Placeholder Testimonials Section Instead of Hiding or Fabricating It

### Decision
The homepage's testimonials section (`index.html`) was found showing literal, unfilled
bracket-placeholder text ("[Testimonial to be added...]") under a heading claiming "Real words
from real families." Presented the user three options: hide the section entirely, reframe its
copy to an honest non-quote statement, or supply real client quotes now. User chose **reframe**:
kept the section and its `section--dark` visual role (the white→dark→white rhythm between
"Services" and the caregiver photo grid), replaced the three fake quote+attribution cards with
three first-party trust statements, and updated the heading/`aria-label` to match. No CSS changes
needed — the existing 3-column grid classes were reused as-is.

### Reason
Fabricating client quotes/names was never an option (dishonest, and this project's own content
rules explicitly forbid inventing reviews). Hiding the section was the simplest fix but would have
collapsed the page's visual rhythm (confirmed via a scoped investigation that `section--dark` is
what provides the only dark break between two white sections). Reframing preserves that rhythm
without waiting on real testimonials, and every replacement claim restates something already
published elsewhere on the site (hero copy, `about.html`, `questions.html`'s cost FAQ) rather than
inventing anything new.

### Alternatives Considered
- Hide/remove the section until real testimonials exist — rejected by the user; would flatten the
  page's visual structure in the meantime.
- Wait and ask the user to supply real testimonials now — offered as an option; user did not have
  them ready, chose to reframe instead rather than block on collecting them.

### Consequences
The section no longer overclaims. A future session can swap the three trust statements for real
client testimonials once collected, without needing any structural change — see `BACKLOG.md`.

**Status:** Accepted and implemented 2026-07-19.

---

## 2026-07-18 — Fix the Push-Prompt Ambiguity That Let `PROGRESS_NOTE.md` Go Stale, Not Just the File Itself

### Decision
Found `PROGRESS_NOTE.md` stale (missing the founder-photo push below entirely) and root-caused it to
`Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` itself, which told the
agent `PROGRESS_NOTE.md` and `PROGRESS_NOTES.md` were alternatives ("prefer `PROGRESS_NOTES.md`").
Rather than only fixing the file's content, fixed the prompt: moved `PROGRESS_NOTE.md` into the
mandatory every-push section, removed the ambiguous guidance, and added a `git diff --stat`
cross-check before every commit. Also added a new Section 2a requiring the linked AntBrainOS vault
project folder to be updated on every push, not only at session end.

### Reason
This repo's own `CLAUDE.md`/`AGENTS.md`/`REPOSITORY_HANDOFF_CONFIG.md` all mark `PROGRESS_NOTE.md`
"kept unconditionally" — the push prompt directly contradicted that. Fixing only the stale content
would have left the same mechanism free to produce the identical gap on the next push. The user
separately asked whether the fix was mirrored in the AntBrainOS vault (it wasn't — the vault's
canonical copy of this prompt carried the byte-identical bug) and asked for "no gaps... system to
system, or session to session," which extended this decision to the vault template and to
`CLAUDE_CODE_SESSION_END.md` (new independent Step 18a: cross-check a repo's required tracking files
before recording a closeout as clean, rather than trusting the push prompt alone).

### Alternatives Considered
- Fix only `PROGRESS_NOTE.md`'s content this one time (rejected — leaves the actual bug in place for
  the next push).
- Make `PROGRESS_NOTE.md` unconditionally mandatory in the vault's *generic* template (rejected —
  not every repo that adopts that template has or needs both a singular and plural progress file;
  the generic fix instead tells the agent to check that specific repo's own governance docs).
- Rely on prose guidance alone with no mechanical check (rejected — a prose-only rule, phrased
  slightly wrong, is exactly what caused the original bug).

### Consequences
`Prompts/repo_push_handoff_snapshot_tag_prompt_snapshot_naming_refined.md` (this repo) and its
vault canonical counterpart are now hardened against this failure shape; `CLAUDE_CODE_SESSION_END.md`
gained an independent verification layer. See the vault's own `08_DECISIONS/DECISION_LOG.md` #36 for
the full cross-project write-up (this entry is this repo's own record of the same decision).

**Status:** Accepted and implemented 2026-07-18.

---

## 2026-07-15 — Install AntBrainOS kit tooling on all 5 branches; skip poor-fit kits; include `main`

### Decision
Install the fit-appropriate AntBrainOS kits (SEOKit, EngKit, TradeKit, handoff-repository) as
dev-tooling onto **all 5 branches** including `main`, as a set of tooling-only commits that touch no
site files. Skip EcomKit, VideoKit, and MKTKit. Tag `main`'s tooling commit as the canonical
`v2.5.0` checkpoint; push all 5 branches but do not individually tag the 4 non-trunk branches.

### Reason
User asked to "use as many kits as possible on as many branches as possible," then refined it to
"install the tooling, value-driven (skip poor fit), across all 5 branches." Installing tooling only
adds files under `.claude/`/`.agents/`/`seo/`/`docs/` — `netlify.toml` publishes repo root with no
served route to those, so even installing onto `main` leaves the live Netlify site byte-for-byte
unchanged (verified by an empty site-file guard diff). This let "include `main`" be honored safely
without it being a design change or a merge.

### Alternatives Considered
- **Run the kits for outputs** instead of installing tooling — not chosen (user picked "install").
- **Install all 7 kits** including EcomKit/VideoKit/MKTKit — rejected: no ecommerce/video surface,
  and MKTKit was already evaluated and rolled back here (`docs/project/STATUS.md`) as a poor fit.
- **Design branches only / current branch only** — rejected: user explicitly chose all 5.
- **Tag every branch commit** — not chosen: redundant for 5 near-identical tooling commits; this
  repo's own precedent leaves non-trunk branches (e.g. cream-immersive) untagged.

### Consequences
- `main` history advances by a tooling commit (+ a trailing docs commit) with zero live-site impact.
- All 5 branches now carry a consistent SEOKit/EngKit/TradeKit/handoff-repository toolset, available
  for future work on any branch.
- `migration/project-starter-v3-3` (pending a deletion decision) also received the install; if that
  branch is later deleted, its tooling commit goes with it — accepted as low-cost.

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
