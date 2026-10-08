# Agentic IDE — 6-Month Day-by-Day Plan (3 People)

**Team:** Person 1 = Frontend/UI/Auth (Tauri, Monaco, React) · Person 2 = Day-Review Agent + Git checkpointing · Person 3 = Instructions Agent + PocketBase DB

## Standing weekly rhythm (applies every week)

- **Mon 30m** — planning: each person picks this week's demo-able increment
- **Wed 15m** — mid-week unblock / descope
- **Fri 60m** — demo + retro; one person dogfoods a full day loop while others log friction
- **Daily** — PR review < 4h; standup async in team channel


## Week 0 — Setup & skeletons

**Friday gate:** All skeletons run on all 3 machines; openapi.yaml v0 locked

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Install Node LTS, pnpm, Rust, Tauri CLI, Git; verify versions | Install Python 3.11+, uv, Git; verify | Install Python 3.11+, uv, Git; verify |
| Tue | Create GitHub repo; add collaborators; branch protection on main (PR + CI required) | Download PocketBase; run ./pocketbase serve; confirm admin UI on :8090 | Scaffold services/instructions-agent (FastAPI) with /health; uvicorn runs |
| Wed | Push root skeleton: README, .gitignore, docs/IDEA.md stub, initial CI workflow | Create collections: users, projects, days, violations, feedback, eval_runs | Scaffold CI for Python services (ruff + pytest) in GitHub Actions |
| Thu | Scaffold Tauri app (React+TS) in apps/desktop; npm run tauri dev opens window | Write pocketbase/README.md (setup steps); verify P1 and P2 can run identical DB locally | Write docs/ARCHITECTURE.md decision note: git2-rs vs shell git; define 'day checkpoint' semantics |
| Fri | Branch p1/desktop-skeleton; open PR; merge after review | Branch p2/review-agent-skeleton; open PR; merge | Seed services/evals/ with harness + 10 sample diffs (5 good/3 buggy/2 paste) with expected outputs; branch p3/...; PR; merge |

## Week 1 — Core loop — first slice

**Friday gate:** Fri: real IDEA.md renders as instructions.md in the app (integration checkpoint)

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Embed Monaco Editor; verify editor.onDidPaste() fires and logs | Build /review endpoint stub (accepts diff, returns mock {notes, mistakes, solutions, violation: false}) | Write IDEA.md -> instructions.md prompt v1 (single LLM call, no structure) |
| Tue | Build file tree component via Tauri fs API | Sketch real day-reviewer prompt: required context = diff + expected day tasks from instructions.md | Test prompt against 3 sample IDEA.md files; log inconsistencies |
| Wed | Add tabbed editing (Monaco models, multiple open files) | Start Rust git.rs: init_project_repo + commit_checkpoint (pair 1h with P1) | Add structured day-by-day JSON output via instructor + FastAPI response_model |
| Thu | Sidebar renders instructions.md from a local mock JSON file | Implement get_diff_since_last_checkpoint in Rust | Add error handling + retries; add 5 edge-case IDEA.md samples to eval set |
| Fri | Integration checkpoint: wire real instructions-agent; real IDEA.md -> rendered instructions.md end-to-end | Support integration checkpoint; verify checkpoint/diff flow on a scratch repo | Support integration checkpoint; deploy instructions-agent locally for P1's app |

## Week 2 — Core loop — full loop

**Friday gate:** Fri GATE: full loop works — idea -> instructions -> fake Day 1 -> review -> paste -> reset + violation logged; evals >= 70%

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | PocketBase Auth: signup/login screens; session persistence in Tauri | Finish reset_to_checkpoint Rust command; test against real repo with real commits | Day-plan validation: reasonable length, mandatory stack-selection + directory-structure steps |
| Tue | Auth continued: token refresh, logout, guarded routes | Build real day-reviewer agent: diff + expected tasks -> structured output per OpenAPI contract | Schema evolution as real data flows: projects.completed, violations.count, timestamps |
| Wed | Profile UI shell (empty states, no real data yet) | Day-reviewer agent continued; self-eval against the 10-diff eval set | Wire eval harness into CI: eval smoke runs on every PR to either Python service |
| Thu | Paste -> Rust flag_violation -> review-agent call -> git reset via diff | Pair with P1 on violation/reset wiring; handle edge cases (empty diff, binary files) | Eval gate: instructions-agent scores >= 70% on eval set; iterate on prompt |
| Fri | Full-loop demo: drive the entire flow live; fix blockers on the spot | Eval gate: review-agent scores >= 70% on eval set; fix or note failures | Full-loop support + post-gate retro notes |

## Week 3 — Real data + hardening

**Friday gate:** Profile live on real data; git edge cases handled

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Profile page wired to real PocketBase data (projects, violation history) | Handle uncommitted changes at reset time (stash/abort policy) | Instructions quality across 3 more idea types (API, CLI tool, game) |
| Tue | Editor save flow + unsaved-changes indicator | Corrupted-repo recovery path (reinit from last good checkpoint) | Add 'regenerate day plan' option (per-day and full-plan) |
| Wed | Keyboard shortcuts (save, command palette stub) | First-day-with-no-prior-commit edge case | Grow instructions eval set to 25+ IDEA.mds |
| Thu | Loading + error states on every API call surface | Grow review eval set to 20 diffs; rerun evals | PocketBase backup/export script for local data |
| Fri | Review P2's git reset UX; polish violation feedback UI | Pair-review P3's prompt changes against eval regressions | Weekly eval report written to docs/EVALS.md |

## Week 4 — Month 1 dogfood

**Friday gate:** GATE: 2 clean full-day dogfood runs; evals >= 80%; all must-fixs ticketed

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Fix dogfood friction batch 1 (UI blockers) | Fix dogfood friction in review agent (wrong-note patterns) | Fix dogfood friction in instructions agent (unclear day plans) |
| Tue | Fix dogfood friction batch 2 | Fix dogfood friction in git checkpointing | Fix dogfood friction in schema/queries |
| Wed | Internal dogfood: P1 plays student through a real mini-project day | Internal dogfood: P2 plays student; log friction | Internal dogfood: P3 plays student; log friction |
| Thu | Log friction for others' areas; fix own-area bugs found | Fix own-area bugs; update eval set with dogfood failures as new cases | Fix own-area bugs; eval set update from dogfood failures |
| Fri | Month-1 retro; write docs/FRICTION.md summary; gate review vs 80% eval target | Gate review: eval scores, retro input | Gate review + Month-1 closeout notes |

## Week 5 — Multi-project + flywheel kickoff

**Friday gate:** Multiple concurrent projects per user works end-to-end

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Multi-project dashboard UI: create, switch, archive projects | Review-agent accuracy sprint: run against 10 intentionally buggy sample submissions | Schema: multi-project support in PocketBase (relations, cascade rules) |
| Tue | Project switcher state management; per-project editor state | Tune mistake-detection prompt from accuracy results | Data migration for existing single-project users |
| Wed | Multi-project UI polish; empty states | Thumbs-up/down + free-text feedback capture on every review output (API side) | Regenerate-option improvements from flywheel plan |
| Thu | Telemetry v1: PostHog funnel signup -> first IDEA.md -> first completed day | Feedback storage schema (eval_runs collection wiring with P3) | CI matrix expansion: both Python services + contract tests on every PR |
| Fri | Fri demo: multi-project flow | Fri demo: improved review accuracy numbers | Fri demo: schema + CI |

## Week 6 — Agent quality flywheel + telemetry

**Friday gate:** Flywheel live: user feedback lands in eval sets weekly

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Wire review feedback UI (thumbs up/down + comment) to review-agent API | Labeling pipeline: 20 user/team samples labeled into eval set | Instructions-agent prompt iteration v2 from labeled data |
| Tue | Telemetry dashboards: day-loop completion funnel live | Regression-blocking CI: eval score drop fails the PR | Labeling pipeline for instructions (20 samples) |
| Wed | Feedback UI polish; error states | Review-agent prompt iteration v2 from labeled data | Sentry on instructions-agent + shared alerting |
| Thu | Sentry integration on desktop app | Cost tracking: log tokens per review; flag outliers | LLM cost dashboard for both agents |
| Fri | Fri demo: flywheel UI + dashboards | Fri demo: eval trend chart | Fri demo: instructions eval trend |

## Week 7 — Accuracy + reliability sprint

**Friday gate:** Evals >= 85% trending; CI fully green across all surfaces

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Cross-OS smoke test setup (Windows + macOS + Linux VMs/hosts) | Accuracy deep-dive: failure taxonomy (missed bugs / false positives / vague notes) | Instructions failure taxonomy (vague days / missing steps / wrong stack) |
| Tue | Fix top 5 UI papercuts from friction log | Fix top failure class; add 10 eval cases for it | Fix top failure class; add 10 eval cases |
| Wed | Error/loading state audit completion | Review latency optimization: parallelize diff chunking | Schema index audit; slow-query fixes |
| Thu | Performance pass: editor startup, file-tree load on large folders | p95 latency < 30s verified on 100-sample run | Contract test coverage to 100% of openapi.yaml endpoints |
| Fri | Fri demo: cross-OS evidence | Fri demo: latency + accuracy numbers | Fri demo: eval scores + contract coverage |

## Week 8 — Month 2 dogfood + gate

**Friday gate:** GATE: Month-2 must-fixs = 0; evals >= 85%; flywheel produced measurable prompt gains

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Month-2 dogfood fixes batch 1 | Dogfood: review-agent friction fixes | Dogfood: instructions-agent friction fixes |
| Tue | Month-2 dogfood fixes batch 2 | Dogfood: git hardening fixes from student run | Dogfood: DB fixes from student run |
| Wed | Dogfood: P1 student run; friction log | Dogfood: P2 student run; friction log | Dogfood: P3 student run; friction log |
| Thu | Fix own bugs; telemetry funnel review | Eval set now 40+ diffs; regression suite timing < 10 min | Eval set now 50+ IDEA.mds; regression suite timing < 10 min |
| Fri | Month-2 retro; gate review | Gate review + retro | Gate review + Month-2 closeout |

## Week 9 — Profile completion + violation defense-in-depth (pt 1)

**Friday gate:** Badges + certificates live; clipboard-polling fallback detecting pastes

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Badges engine UI (earned/locked states) | Clipboard polling fallback implementation (Rust, throttled) | Certificate/badge data model + rules engine in PocketBase |
| Tue | Completion certificate UI (per project, shareable) | Unified violation detector: editor paste + context-menu + shortcut + polling, single decision point | Award logic hooks on project completion |
| Wed | Violation history with dates on profile | False-positive tuning session on unified detector | Settings screen schema (theme, font size, keybindings) |
| Thu | Clipboard polling fallback research in Tauri (read_text availability per OS) | Unit tests for all 4 paste vectors | Settings API endpoints |
| Fri | Fri demo: profile completion | Fri demo: 4-vector detection | Fri demo: badges backend |

## Week 10 — OS parity + settings + onboarding

**Friday gate:** Violation detection verified on Win/Mac/Linux; settings screen live

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Settings screen UI: theme, font size, keybindings | Cross-OS Tauri behavior fixes from W10 testing | Keybindings storage + conflict detection |
| Tue | Settings persistence + live application | Violation hardening: OS-specific edge cases documented | Onboarding content: writing your first IDEA.md (guided) |
| Wed | Cross-OS violation testing: Windows (paste vectors) | Settings consumption on desktop (Tauri store) | Onboarding API: sample project template for guided flow |
| Thu | Cross-OS violation testing: macOS + Linux | Onboarding step 1: first-launch walkthrough shell | time-to-first-instructions instrumentation |
| Fri | Fri demo: settings + OS matrix results | Fri demo: OS fixes | Fri demo: onboarding backend |

## Week 11 — Onboarding + scale planning

**Friday gate:** New user reaches instructions.md in < 5 min (telemetry-verified)

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Onboarding flow wired end-to-end in app | Review-agent robustness: malformed/empty diffs, huge diffs (chunking) | PocketBase -> Postgres migration plan written (schema mapping, migration scripts) |
| Tue | Onboarding polish from self-run observations | Review-agent robustness: timeout + retry UX contract with P1 | Migration plan review with P2 + P1 |
| Wed | Empty-state and first-run UX polish | Load test review-agent: 20 concurrent requests | Cost controls: caching identical diffs, per-user daily caps |
| Thu | Review onboarding funnel metrics; iterate | Fix load findings; latency budget re-verified | Cost regression tests in CI |
| Fri | Fri demo: < 5 min time-to-first-instructions | Fri demo: robustness evidence | Fri demo: migration plan + cost controls |

## Week 12 — Month 3 dogfood + pre-hardening

**Friday gate:** GATE: all Month-3 features demoed; internal bug bash pass 1 done

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Month-3 dogfood fixes | Bug bash pass 1 in review agent + git areas | Bug bash pass 1 in instructions agent + DB areas |
| Tue | Internal bug bash pass 1 (whole app, as outsider) | Fix P2-area P0/P1s | Fix P3-area P0/P1s |
| Wed | File bugs; fix P1-area P0/P1s | Fix P2-area P2s; robustness backlog burn | Fix P3-area P2s; cost-control verification |
| Thu | Fix P1-area P2s; settings/onboarding polish | Eval set 50+ difcs; document known limitations | Backup/restore drill: recover PB data from backup |
| Fri | Month-3 retro; gate review | Gate review + retro | Gate review + Month-3 closeout |

## Week 13 — Bug bash x3 + security red team

**Friday gate:** Bug bash passes 1-3 complete; red-team findings classified

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Bug bash pass 2 (cross-test P2's areas as outsider) | Bug bash pass 2 in own areas; fix findings | Bug bash pass 2 in own areas; fix findings |
| Tue | Bug bash pass 3 (cross-test P3's areas as outsider) | Security red team: file edits outside editor (bypass via external editor) | Security red team: API abuse (replay, injection in diff payloads) |
| Wed | Security red team: fast-typing simulation attack | Security red team: git history tampering attempts | Security red team: auth/session weaknesses |
| Thu | Security red team: OS automation + clipboard tool attacks | Write defend-now fixes plan; start top 3 | Start defend-now fixes (API hardening) |
| Fri | Classify findings: defend-now vs known-limitation; demo security log | Gate: findings classified; fixes underway | Gate: API findings triaged |

## Week 14 — Security fixes + performance budgets

**Friday gate:** Red-team defend-now fixes shipped; perf budget suite exists

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Ship UI-side security fixes (paste vectors hardened) | Ship git-layer security fixes | Ship API security fixes (rate limits, payload validation) |
| Tue | Performance: Monaco with 10k-line files — budget defined | Performance: git ops < 500ms on 500-commit history | Performance: review-agent p95 < 30s under load |
| Wed | Performance: editor startup < 2s budget | Perf harness in Rust; baseline captured | Perf harness for services; baseline captured |
| Thu | Fix perf findings batch 1 | Fix perf findings batch 1 | Fix perf findings batch 2 |
| Fri | Fri demo: security + editor perf | Fri demo: git perf numbers | Fri demo: service perf numbers |

## Week 15 — Performance enforcement + docs

**Friday gate:** Perf budgets enforced in CI; stranger-test docs drafted

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Perf budgets in CI (editor startup, render) | Perf budgets in CI for git ops | Perf budgets in CI for services (latency, throughput) |
| Tue | Fix perf regressions caught by CI | Fix perf regressions caught by CI | PocketBase load test: 20 concurrent users; fix findings |
| Wed | Docs: desktop setup + troubleshooting guide | Docs: git checkpointing internals + recovery guide | Docs: services setup + ops runbook |
| Thu | Docs: user guide (first project walkthrough) | Docs: known limitations doc (honest security boundaries) | Docs: full-stack setup guide (stranger-test draft) |
| Fri | Fri demo: perf CI gates | Fri demo: git perf gates | Fri demo: service perf gates |

## Week 16 — Month 4 gate: stranger test + scale decision

**Friday gate:** GATE: stranger runs full stack in < 30 min; Postgres go/no-go ADR signed

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Recruit 1 stranger; observe setup; fix doc gaps found | Stranger-test support; fix recovery-guide gaps found | Stranger-test support; fix services-doc gaps found |
| Tue | Second stranger run; time it; fix remaining gaps | Postgres migration decision input (git/ops side) | Postgres go/no-go ADR authored (waitlist > 200 = migrate) |
| Wed | Prepare launch-readiness checklist template | Rollback plan draft (checkpoint/restore story) | Migration scripts dry-run if GO |
| Thu | Polish from stranger feedback | Sign off launch-readiness checklist (git portion) | Sign off launch-readiness checklist (services portion) |
| Fri | Month-4 retro; gate review | Gate review + retro | Gate review + Month-4 closeout |

## Week 17 — Beta infrastructure + recruitment

**Friday gate:** Waitlist live; 100+ signups; beta build stable

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Waitlist landing page + referral links | Feedback ingestion API: reports routed to labeled eval candidates | Invite codes + beta cohort schema in PocketBase |
| Tue | Beta onboarding flow (invite code, first-run tuning) | Beta triage process doc (must-fix vs later) | Cohort 1 recruitment (20 users): CS discords, university clubs |
| Wed | In-app feedback report button (one-tap, context attached) — UI | Review-agent stability under real traffic; hot-patch path | Analytics: cohort funnels defined |
| Thu | Beta build stabilization: top crash fixes | Beta support rotation schedule | Beta build DB migration rehearsal |
| Fri | Fri demo: waitlist + feedback UI | Fri demo: triage pipeline | Fri demo: cohort setup |

## Week 18 — Beta cohort 1

**Friday gate:** 20 active users; first weekly triage done; must-fixs flowing

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Monitor cohort 1 funnels daily; UI crash triage | Fix top 3 review-agent must-fixs from cohort 1 | Fix top 3 instructions-agent must-fixs from cohort 1 |
| Tue | Fix top 3 UI must-fixs from cohort 1 | Label 20 new real samples into eval set | Label 20 new real samples into eval set |
| Wed | Feedback-button iteration from usage data | Review accuracy on real data vs evals; gap analysis | DB health monitoring: slow queries, growth |
| Thu | Session recordings review (opt-in); friction notes | Support rotation: answer user questions | Support rotation: answer user questions |
| Fri | Fri: cohort-1 retro + triage | Fri: triage review | Fri: triage review |

## Week 19 — Beta cohort 2 + platform bets kickoff

**Friday gate:** 40 cumulative users; platform bet spikes scoped

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Cohort 2 onboarding (20 more users) | Cohort 2 support; fix review must-fixs | Cohort 2 support; fix instructions must-fixs |
| Tue | Platform bet spike: VS Code extension — read-only review companion (scope: OAuth-less MVP) | Platform bet spike: study rooms — presence + cursor sync (WebSocket) | Platform bet spike: agent marketplace — persona prompt packs (schema + CRUD) |
| Wed | Extension spike: fetch reviews from API, render in webview | Spike: rooms backend (auth, rooms, cursor events) | Spike: 3 seed personas (strict interviewer, gentle mentor, code golf) |
| Thu | Kill-list analysis v1: feature usage stats | Fix spike blockers; assess feasibility scorecard | Kill-list analysis: flag < 5% usage features |
| Fri | Fri demo: extension spike | Fri demo: rooms spike | Fri demo: marketplace spike |

## Week 20 — Beta cohort 3 + bets continue

**Friday gate:** 60-100 cumulative users; bets have go/no-go evidence

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Cohort 3 (20-40 users); funnel review | Cohort 3 support; review-agent prompt v3 from flywheel | Cohort 3 support; instructions-agent prompt v3 |
| Tue | Extension bet: decide GO/NO-GO with evidence | Rooms bet: decide GO/NO-GO with evidence | Marketplace bet: decide GO/NO-GO with evidence |
| Wed | If GO: extension MVP hardening (error states, polish) | If GO: rooms MVP — day-progress sync (not just cursors) | If GO: marketplace MVP — browse + install persona packs |
| Thu | Kill-list flagged features removal or redesign | Eval set now 80+ diffs; publish eval trend report | Cost report: LLM spend per active user; tune caps |
| Fri | Fri demo: extension decision | Fri demo: rooms decision | Fri demo: marketplace decision |

## Week 21 — Beta close + launch scope lock

**Friday gate:** GATE: beta must-fix list = 0; launch scope frozen; Loom script approved

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Beta must-fix fixes (UI) — final pass | Beta must-fix fixes (review) — final pass | Beta must-fix fixes (instructions/DB) — final pass |
| Tue | Launch-scope lock: feature freeze on core app | Launch-scope lock (review features) | Launch-scope lock (services/DB) |
| Wed | Demo video script: idea -> instructions -> coding -> review -> completion | Demo video segment: review + violation-reset moment | Demo video segment: certificates + completion |
| Thu | Demo video recording pass 1 (screen capture) | Hotfix runbook + on-call rotation for launch week | Infra check: backups verified, cost caps set, monitoring dashboards |
| Fri | Fri: launch-readiness review vs checklist | Fri: readiness review | Fri: readiness review + Month-5 closeout |

## Week 22 — Launch polish + video

**Friday gate:** Video final; launch checklist > 90% green

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Polish pass from beta feedback (UI) | Review-agent final eval: must hold >= 85% | Instructions-agent final eval: must hold >= 85% |
| Tue | Demo video final edit + captions | Review-agent launch hardening (timeouts, fallbacks) | Services launch hardening (rate limits, alerts) |
| Wed | 3 short social clips cut from video | Demo video technical review (accuracy of shown reviews) | Landing page copy review + SEO basics |
| Thu | Launch checklist drive: UI items to green | Launch checklist: review service items to green | Launch checklist: DB/infra items to green |
| Fri | Fri: full-team dry run of demo flow | Fri: dry run | Fri: dry run |

## Week 23 — Packaging + distribution prep

**Friday gate:** Signed installers for Win/Mac/Linux; landing page live (unlisted)

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Tauri bundling: Windows installer (NSIS) + code signing setup | Review-agent production config: env separation, secrets | Instructions-agent production config + deploy |
| Tue | macOS bundle + notarization | Deploy runbook final test (staging -> prod) | DB final backup strategy + monitoring alerts |
| Wed | Linux AppImage/deb | Landing page: review accuracy section (real eval numbers) | Landing page live (unlisted); analytics verified |
| Thu | GitHub Release workflow (automated artifacts) | Support FAQ drafted (top 10 beta questions) | Launch-day comms draft (PH/HN/Reddit posts) |
| Fri | Fri: install-from-artifact test on all 3 OSes | Fri: deploy drill | Fri: full-stack prod rehearsal |

## Week 24 — Launch week 1 — public

**Friday gate:** Public launch executed; zero Sev-1 incidents

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Launch: Product Hunt post live; respond to comments | On-call: review service health; hotfix if needed | On-call: DB/services health; hotfix if needed |
| Tue | Monitor telemetry/crashes; hotfix UI bugs | Load monitoring; scale decision if traffic spikes | Cost dashboard watch; cap tuning live |
| Wed | Hacker News thread monitoring; respond | HN/PH technical questions support | Comms support (technical answers) |
| Thu | Reddit r/programming + learnprogramming posts | Fix service bugs; deploy patches | Fix backend bugs; deploy patches |
| Fri | Fri: launch-week retro; incident log review | Fri: retro + service metrics report | Fri: retro + cost/traffic report |

## Week 25 — Launch week 2 — stabilize + celebrate

**Friday gate:** Post-launch stability confirmed; retro actions ticketed

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Triage launch-week bug backlog (UI) | Triage launch-week review-agent bugs | Triage launch-week backend bugs |
| Tue | Stability fixes batch 1 | Stability fixes; eval re-run to confirm no regression | DB scaling watch; migration decision revisited with real numbers |
| Wed | Thank-you + changelog post for early users | Analyze real-world review quality report | Usage stats report (DAU/WAU, completion funnels) |
| Thu | User interview scheduling (5 interviews) | Support interview analysis (review moments) | Support interview analysis (instructions moments) |
| Fri | Fri: launch retro (full team) | Fri: launch retro | Fri: launch retro |

## Week 26 — Closeout + 6-month roadmap

**Friday gate:** 6-month retro done; Months 7-12 roadmap ADR; final e2e on all machines

| Day | Person 1 (Frontend) | Person 2 (Review/Git) | Person 3 (Instructions/DB) |
|---|---|---|---|
| Mon | Final e2e test on all 3 machines (P1's) | Final e2e (P2's machine); git checkpointing sign-off | Final e2e (P3's machine); services sign-off |
| Tue | Months 7-12 roadmap input (companion app, cohort mode) | Roadmap input: study rooms GA, multiplayer hardening | Roadmap input: hosted cloud version, Postgres migration |
| Wed | Write public retrospective blog post draft | Retrospective input: eval flywheel results (scores over 6 months) | Retrospective input: cost + quality metrics journey |
| Thu | Repo hygiene: docs final pass, issue triage | Runbook finalization for handoff/maintainers | docs/ROADMAP.md Months 7-12 written |
| Fri | Final 6-month retro + celebration | Final retro | Final retro + closeout |