# Zeam Rolling Action List

*Last updated: 2026-05-08 (Fri) · Scope: previous week (Apr 24 call) + current week (May 1 call → May 8 call)*

Items closed during the May 1 call are excluded. Everything raised on May 1 is included.

**Legend:** 🔄 carry · 🆕 new (May 1) · 🔥 hot for this week · 📌 planning/deferred

---

## 👤 Gajinder

**Previous week (Apr 24 → still open going into May 1):**
- 🔄 Take a look at the **parallelization PR** w.r.t. block byte-code caching, confirm everything needed is covered downstream
- 🔄 Review **PR #754** (Kai) — fork-choice / attestation tolerance / spectest fixes; verify "drop time-based block queue" decision; check that future-block processing also advances local fork choice
- 🔄 **DevNet 6 planning** — execution integration + validator life-cycle; goal: long-running public devnet by EOY
- 🔄 Decide PBS vs **ePBS** for DevNet 6 (research dep, may shift if EF research proposes alternative APS)
- 🔄 **DevNet 5** — look at Goldfish for `safe_target` calculation

**Current week (raised May 1):**
- 🔥 **Review PR #812** (Partha — multi-subnet subscription fix) — blocks today's multi-subnet run
- 🔥 Review **PR #715** (Kai — new spectest kinds, lint blocked); merge order: lint fix → #715 → #754
- 🆕 Review **Anshal's `hashtree_root` cache PR** on `ssz.zig`
- 🆕 Re-review **mutex / parallelization strategy doc** after Anshal + Kai comments
- 🆕 Look into **Lighthouse** to verify whether running business logic on rust-libp2p / libxev event-loop threads is actually unsafe (settle Kai's concern)
- 🆕 Confirm current **subnet subscription behavior** in `node.zig` vs `eth-libp2p.zig` — is the bug Partha describes real or already mitigated by node.zig-level subscription?
- 🆕 Review and merge **Partha's PR #803/#804** (mutex doc / borrowed-state) — agreed mostly OK on the call
- 🆕 Approve **lean-quick-start DevNet 4 PR** (Partha waiting)

---

## 👤 Parthasarathy (Partha)

**Previous week:**
- 🔄 **File spec PR** changing `safe_target` to be calculated on `new` votes (not merged with `known`) — Gajinder confirmed Zeam is the correct version; raise as discussion vehicle on lean-spec
- 🔄 **Debug-log rollover** script — instead of overwriting per restart, back up; copy debug logs to Loki so they survive across runs
- 🔄 Investigate **`gossip duplicate` warning** on aggregator with multi-subnet listeners — likely benign from libp2p layer; suppress to debug if so
- 🔄 **LMDB issue** (self-assigned, mentioned tail-end Apr 24)

**Current week (raised May 1):**
- 🔥 **PR #812 — multi-subnet subscription fix** — needs Gajinder/Anshal/Kai review; blocks today's run
- 🆕 **Preferential peering on attnets ENR field** — Zeam reads it but doesn't use it for peering selection; raise as separate PR
- 🆕 **Multi-subnet stability runs** — primary focus this week; scale up subnets after current Zeam release stabilises
- 🆕 Update **hash glue** to latest leanmulti DevNet 4 commit (AVX improvement) — coordinating with Anshal who'll do it
- 🆕 Re-review **mutex strategy** as Zclaw generates code from doc; chime in on PR #803/#804
- 🆕 **Identified improvements to lean-quick-start DevNet 4 PR** — will raise as separate follow-up PRs after merge
- 🆕 Confirm with Gajinder whether **PR #813 / #812** (gossip submesh / multi-subnet) merge together with #804 mutex doc

---

## 👤 Anshal

**Previous week:**
- 🔄 **Refactor `mock.zig`** into standalone simulation-test package with a queue events/RPC calls flow into; remove `is_schedule` parameter
- 🔄 **`hashtree_root` cache PR** on `ssz.zig` — Merkle dirty-bit caching, default to SHA-256, raise after test cases land
- 🔄 **Refactor `libxev` / mock.zig** — slip from Apr 24, still pending

**Current week (raised May 1):**
- 🔥 **Land Zig 0.16 upgrade PR** on zeam (today goal) — must merge before mutex-strategy follow-up PRs to avoid conflicts
- 🆕 **Optimistic / aggressive aggregation** when this node is known proposer — threshold-based (aggregate when fan-out > N or threshold met); discussed Apr 24 with FA, picking up now
- 🆕 **Update hash glue** to latest leanmulti DevNet 4 commit — recompile binaries for Python specs, push, update on lean-spec
- 🆕 **Remove hashsig CLI** dependency from zeam repo entirely (was using Chaitanya's fork via Caitana); reintroduce after hashing decision firms up
- 🆕 Review **Gajinder's mutex / parallelization strategy doc**; coordinate with Partha on follow-up PRs
- 🆕 **DevNet 5 / proof deconstruction** — Emile shared APIs; Anshal to read and reply positively by Tuesday (this week)
- 🆕 File **DevNet 5 spec PR** by Wednesday this week
- 🆕 Investigate **mutex issues seen during DevNet 4 multi-subnet runs** (libxev thread-pool / worker-pool ownership transfer to blockblaz)

---

## 👤 Kai

**Previous week:**
- 🔄 **PR #754** — attestation tolerance + fork-choice spectest fixes; investigated "attestation too far" + clock-skew angle, refreshed to follow new spec change
- 🔄 **Raise in PQ-interop channel**: why does gossip-attestation tolerance allow `current_slot + 1`? Tag Gajinder; status unclear
- 🔄 Verify **future-block processing** also advances local fork choice (don't process future block without ticking local fork choice forward)

**Current week (raised May 1):**
- 🔥 Get **PR #754 + PR #715 merged** — fix lint on #715 first (it's blocking)
- 🆕 **Review PR #812** (Partha — multi-subnet subscription)
- 🆕 Review **mutex strategy doc** — Kai suggested reducing mutex count; Gajinder pushed back (clarity-of-borrowed-state matters more than count); continue review
- 🆕 Investigate **rust-libp2p thread + libxev main thread** — both should avoid heavy business logic; check if assumption is correct (action shared with Partha + Anshal)
- 🆕 Help refactor Zeam to **avoid heavy logic on libxev timer thread** (would delay next slot tick)
- 📌 **Pure-Zig libp2p / ETH-P2P** WIP — deferred; no funding/timeline; revisit later

---

## 👤 Katya

**Previous week:**
- 🔄 **Investigate Grafana retry-on-delivery-failure** semantics — does Grafana retry failed Telegram webhooks?
- 🔄 **Restart-notification glitch** — Zclaw was DMing Noopur instead of posting to test group; corrected but watch for regressions

**Current week (raised May 1):**
- 🔥 **Brush up alerts** so they stop spam-firing during head-grow / no-finalization — DevNet currently running, needs fixing live
- 🔥 **Telegram delivery failures** — investigate the 400-Bad-Request from contact point, fix message format / parse mode
- 🆕 **New gossip metric** (Partha's request) — open draft spec PR after May 1 details discussion
- 🆕 **Add `tick_interval` metric to lean-spec** (Zeam side merged, now upstream it; Partha confirmed it was useful for finding aggregator-fall-behind)
- 🆕 **Upgrade Grafana to v13 on lean-quick-start** — blocked behind Partha's DevNet 4 PR merge
- 🆕 **Auto-investigation on alert events** — wire Zclaw to investigate Loki logs when alerts fire; reduce hallucination rate; output goes to `zclawz` topic only

---

## 👤 Noopur

**Previous week:**
- 🔄 **Fix Zclaw routing**: Grafana alerts must go to `zclawz` topic, not DM — corrected once but Katya still seeing 400s / glitches
- 🔄 **Configure Zclaw to keep a learnings file** in `blockblaz/zclawz` (push corrections + learnings automatically); confirm openclaw is actually self-learning
- 🔄 **Auto-feed call transcripts into Zclaw** so issues / action items can be generated on demand

**Current week (raised May 1):**
- 🔥 **Zclaw PR-approval permission** — currently TOOLS.md / AGENTS.md only allow review-not-approve; team agreed Zclaw can approve when explicitly asked by team member and PR isn't consensus-touching. Update Zclaw config in `blockblaz/zclawz` repo
- 🆕 **360-degree integration** — surface chat-channel discussion points into a unified per-call agenda (so meeting prep is automatic)
- 🆕 **Twitter weekly-update accuracy** — auto-post is reporting stale info (e.g. "Katya on DevNet 2/3"). Check the job
- 🆕 **First-issue / good-first-PR labels** for external contributors so they don't pick up actively-worked items
- 🆕 **Verify openclaw self-learning** behavior and make sure corrections persist across sessions
- 🆕 **Watch for hallucinations post-upgrade** — openclaw became more secure but more hallucinatory after upgrade; double-check critical actions

---

## 🔥 Hot list — week of May 1 → May 8

1. **Gajinder reviews:** #812, #754, #715, hashtree-root cache
2. **Anshal:** Zig 0.16 upgrade lands first (avoid merge conflicts on follow-up PRs)
3. **Partha:** multi-subnet stability runs + file `safe_target` spec PR
4. **Kai:** lint #715 → merge #754
5. **Katya:** alert hygiene + Telegram 400 + Grafana retry semantics
6. **Noopur:** Zclaw approve-PR config + transcript auto-ingest + DM-regression watch
