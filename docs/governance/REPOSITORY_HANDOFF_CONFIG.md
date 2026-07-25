# Repository Handoff Configuration

Project-local configuration for the `handoff-repository` skill (see
`../../.claude/skills/handoff-repository/SKILL.md`). Values below are real and confirmed for this
repository as of 2026-07-15. Store operational coordinates only, never credentials.

## Repository Identity

- Project name: Old Fashion Care (in-home elder-care marketing site, Raleigh NC)
- Repository root: `/Users/ant/Projects/GitHub/Old-Fashion-Care`
- Canonical remote: `https://github.com/rmz9dkfy5f-pixel/Old-Fashion-Care.git`
- Default branch: `main`
- Canonical handoff file: `docs/project/COMMIT_NOTES.md` + `PROGRESS_NOTE.md` (repo-side, kept
  unconditionally); cross-agent baton lives in the AntBrainOS vault
  (`03_PROJECTS/Active/Old_Fashion_Care/HANDOFF_TO_CLAUDE.md`).

## Validation Contract

- Install command: none (static HTML/CSS/JS site — no package manager, no `package.json`)
- Focused test commands: none (no test framework)
- Full test command: none
- Lint/type-check commands: none configured
- Production build command: none (static site served as-is; Netlify publishes repo root)
- Runtime smoke test: serve locally (`python3 -m http.server`) and load the 6 pages, or Playwright
  headless load of `index.html` — assert 0 console errors and 0 horizontal overflow
- Manual or device checks: visual review of the hero + responsive check at 1440 / 768 / 390

## Snapshot Contract

- Snapshot required: conditional (before tag/push handoff and before high-risk changes)
- Naming rule: `vX.Y.Z__short-work-description__commit-REALHASH` (tag the code commit, not a
  trailing docs-only commit — established precedent: `v1.5.0`/`b195aba`, `v2.4.0`/`2727395`)
- Exclusions: `.DS_Store`, `images/ChatGPT*` (raw source images, gitignored)
- Verification method: compare snapshot file list against `git ls-tree -r <commit>`
- Checksum requirement: `shasum` spot-check on a snapshot copy when feasible
- Retention policy: keep per-tag snapshots; no automatic pruning
- Restore/rollback procedure: `git checkout <tag>` / restore files from the named snapshot folder

### Snapshot Destination by Machine

```bash
scutil --get ComputerName 2>/dev/null || hostname
```

| Machine | Detection | Snapshot destination | Notes |
|---|---|---|---|
| Ant's Mac Mini | `scutil --get ComputerName` → `Ant’s Mac Mini (4)` | `/Volumes/AntNVMe1TB/WorkSync/Projects/RepoBackups/Old-Fashion-Care/` | Correct volume is AntNVMe1TB, NOT DataHub_2TB (stale root) |
| Ant's MacBook Air | `scutil --get ComputerName` → `Ant’s MacBook Air` | `/Users/ant/WorkSync/Projects/RepoBackups/Old-Fashion-Care/` | Added 2026-07-24 — found missing when a session ran from this machine; path confirmed to already hold this project's full snapshot history back to `v1.3.0` |

If the current machine does not match a row above, **stop and ask the user** for the correct
destination — do not guess a path pattern.

## Deployment Contract

- Deployment in scope: yes (two independent targets)
- VPS/server alias: `ssh -i ~/.ssh/jones_vps root@74.208.9.49` (shared Ionos box; client-preview
  subdomains only)
- Deployment root: Netlify (production, from `main`); VPS webroots under
  `/var/www/old-fashion-care*/` (preview branches)
- Deployment branch or artifact: `main` → Netlify auto-deploy (`oldfashioncare.com`); design/hero
  branches → `craftandconscious.com` preview subdomains via `git archive` + explicit file-list rsync
- Service/container names: nginx + Let's Encrypt on the VPS; Netlify managed for production
- Read-only health checks: `curl -I` / DNS-over-HTTPS cross-check (local resolver cache is
  unreliable for freshly-changed VPS DNS — verify via DoH before concluding)
- Log locations: nginx logs on the VPS; Netlify deploy log in the Netlify dashboard
- Rollback target: previous tagged commit / previous Netlify deploy
- Actions requiring approval: any merge to `main`, any change to the live Netlify site, any VPS
  nginx/TLS change

## Safety Boundaries

- Protected paths: `main` branch, `netlify.toml` (CSP headers — security review required),
  `index.html`/`css/`/`js/` on `main` (live site)
- Secret-bearing files: none in-repo; VPS key `~/.ssh/jones_vps` lives outside the repo
- Prohibited actions: committing `.DS_Store` or `images/ChatGPT*`; adding LLM co-author lines to
  commit messages; `background-size: contain` on the `main` desktop hero (reverted 2026-07-12)
- Commit/push authorization rule: do not commit or push unless the user explicitly asks
- Tag/release authorization rule: tag the code commit per the naming rule above; snapshot before tag
- Deploy/merge authorization rule: no merge to `main` or live-site change without explicit user
  authorization
