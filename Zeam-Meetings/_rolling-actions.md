# Zeam Rolling Action List

*Last updated: 2026-06-05 (Fri) · Scope: May 8, May 15, May 22, and May 29 calls; Zeam Telegram group archive through 2026-06-04; GitHub spot-check on 2026-06-05.*

This file tracks work that survived the weekly calls / community discussion. Closed items are listed once so they do not keep reappearing as stale action items.

**Legend:** ✅ done · 🔥 hot / active · 🔄 carry · 🆕 new since May 22 · 📌 planning/deferred · ⚠️ blocked/watch

---

## ✅ Recently closed / resolved

### DevNet 5 / shadow / CI
- ✅ `leanEthereum/leanSpec#717` — DevNet 5 aggregated block proof merged; Zeam implementation work moved into `blockblaz/zeam#918`.
- ✅ `blockblaz/zeam#931` — CI task for `shadow` branch auto-rebase + release status merged/closed.
- ✅ `blockblaz/zeam#972` — leanMultisig `multisig-glue` dependency bump merged.
- ✅ `blockblaz/zeam#946` — DevNet 4 Hive client-interop release branch fix closed.

### DevNet 4 / aggregation / robustness
- ✅ `blockblaz/zeam#915` — minor cleanup closed; May 22 discussion had requested review/approval.
- ✅ `blockblaz/zeam#948` — robustness/metrics PR connected to `#942` closed; May 29 call treated it as part of the snappy/decode recovery investigation.
- ✅ Earlier aggregation/metrics fixes remain closed: `#900`, `#902`, `#903`, `#914`, leanSpec `#735`, Zeam `#876`, Zeam `#898`.

### Older merged items kept for context
- ✅ `blockblaz/zeam#812` / issue `#811` — selective attestation subnet subscription fix merged/closed.
- ✅ `blockblaz/zeam#882`, `#883`, `#884`, `#886` — request handling, slot-driver watchdog, PK cache, and gossip flood/starvation fixes landed.
- ✅ Zig 0.16 upgrade was reported merged in earlier calls.

---

## 🔥 Hot list — after May 22 + May 29 calls

1. **DevNet 4 multi-subnet finality/stability remains the top technical risk.** `blockblaz/zeam#899` is still open for aggregation performance; `#942` is still open for all-Zeam-node stalls around snappy/decode/gossip behavior.
2. **Aggregation path needs priority + deadline semantics.** May 22 call: process attestation data matching local fork-choice first; publish each aggregate immediately when ready; expose/configure aggregation parallelization factor; avoid waiting for all compactions if block/proposal deadline is hit.
3. **Block proposal should stop at deadline and publish what is ready.** If compaction misses the window, do not keep building a doomed block; propose with compacted payloads available by the deadline and let later slots fill gaps.
4. **DevNet 4 diagnostics:** use subnet aggregate coverage + gossip attestation coverage metrics to identify which subnets/clients are missing attestations/aggregates; other clients should add comparable metrics where possible.
5. **Snappy/decode stall investigation (`#942`):** rename scary “corrupt” language to invalid/bad snappy input where appropriate; inspect debug logs slot-by-slot; understand why Zeam stops receiving gossip while peers remain connected; confirm whether `blocks_by_root` should or should not trigger for rejected bad blocks.
6. **Validation matrix requested May 29:** run all-Zeam nodes, run without ethlambda, preserve/copy logs before restarts, then reintroduce other clients once stability/P95 improves.
7. **DevNet 5 implementation and shadow readiness:** `blockblaz/zeam#918` is open; need CI simtest practicality via larger slot time/config, eLambda interop finalization screenshot, and shadow branch readiness for Kamil/PQ interop.
8. **Shadow simulations:** start with one aggregator per subnet, realistic bandwidth/latency topologies, artificial compute sleeps for aggregation/verification, and later visualization/overnight “auto research” runs.
9. **Zclaw / project tracking automation:** keep transcripts + Telegram + GitHub as the three sources of truth; continue updating this repository and rolling list after meetings.

---

## 👤 Gajinder

### Active
- 🔥 Continue steering DevNet 4 aggregation and stall investigations: `#899`, `#942`, and follow-up PRs.
- 🔥 Review/debug DevNet 4 logs slot-by-slot, especially around slots where gossip stops or head/finalized state diverges.
- 🔥 Push for deadline-based block proposal / aggregation behavior: prioritize local fork-choice attestation data; publish partial-ready aggregates/blocks before the deadline.
- 🔥 Keep default topology at **one aggregator per subnet** for baseline runs; treat multi-subnet / super-aggregator modes as manual experiments after baseline stability.
- 🔄 Review DevNet 5 implementation alignment in `#918`: aggregated block proof, proof deconstruction/splitting behavior, proposer/fork-choice behavior, and CI fixture practicality.
- 🔄 Coordinate public/interop milestone messaging once Kai provides eLambda finalization screenshot.
- 📌 Continue DevNet 6 / execution integration planning; keep PQ heartbeat out unless a clear proposal lands.

### Watch / backlog
- 🔄 `blockblaz/zeam#754` remains open — attestation handling / leanSpec alignment.
- 🔄 `blockblaz/zeam#817` remains open — async response handling / lock-free RPC serving; still needs value/mergeability decision.
- 🔄 `blockblaz/zeam#863` remains open — slow slot interval / event-loop starvation; previous fixes landed but validation remains.
- 🔄 `blockblaz/zeam#910` remains open — slot-aware `blocks_by_root` peer selection.

---

## 👤 Parthasarathy (Partha / @grapebaba)

### Active
- 🔥 Lead DevNet 4 multi-subnet / high-validator testing and preserve logs before restarts.
- 🔥 For `#942`, validate whether PR `#948` image prevents crashes/stalls; if not, capture debug logs and isolate why Zeam uniquely stops receiving gossip.
- 🔥 Run the May 29 requested matrix:
  - all-Zeam network,
  - network without ethlambda,
  - then mixed-client network after stability improves.
- 🔥 Target P95 under 1s on the current setup before expanding scale; May 29 goal was to work toward P95 `<1s` over the weekend.
- 🔥 Update issues `#899` / `#942` with whether the remaining bottleneck is aggregation compute, gossip coverage, block proposal compaction, peer/fetch behavior, or fork-choice/head divergence.
- 🔄 Track subnet aggregate coverage + gossip attestation coverage metrics in live dashboards and identify which subnet/client data is missing.
- 🔄 Continue `#863` validation; distinguish “missed/orphaned slots” from “node behind/slow” in logs.
- 🔄 Keep `#910` in view: slot-aware `blocks_by_root` peer selection remains separate from checkpoint-peer-affinity work.
- 📌 For shadow, provide/estimate server needs once Kamil’s scripts/topologies are ready; 64GB was considered enough for ~100-node first runs.

### Recently discussed
- ⚠️ May 29: snappy/decode errors were observed when ethlambda proposed; other clients rejected and continued, Zeam stalled. Need prove whether this is decode recovery, gossip subscription/mesh, peer rejection, or sync/fetch logic.
- ⚠️ May 22: multi-subnet fork-choice/head divergence was observed across clients; this may amplify aggregation failures and finality stalls.

---

## 👤 Anshal

### Active
- 🔥 Aggregation optimization follow-ups from May 22:
  - validate/extend parallel aggregation work (`#915` discussion is closed, but design follow-ups remain),
  - add configurable aggregation parallelization factor,
  - prioritize attestation data matching local fork-choice / latest justified view,
  - publish each aggregate immediately on completion,
  - start compaction earlier when enough signatures are available instead of waiting for the interval boundary.
- 🔥 Add block proposal deadline behavior: compact until the time budget is hit, then propose with whatever compacted payloads are ready.
- 🔥 Perf/profile aggregation on real DevNet setup: measure CPU/core utilization, thread pool behavior, and whether underlying leanMultisig already parallelizes enough.
- 🔄 Continue SSZ clone / Merkle root caching work:
  - verify variable-type lists and embedded variable lists,
  - consider caching composite-type hashing where benchmarking shows benefit,
  - make type parameters comptime where needed for size/min/max calculations.
- 🔄 Review Kai’s DevNet 5 implementation / aggregation branch updates and coordinate on leanMultisig breaking API changes.
- 🔄 Investigate async/concurrency options after Zig 0.16; Kai warned evented async IO may not be ready and suggested `c-io` / moving away from libxev hacks.
- 🔄 Collate performance TODOs with Partha and Kai into a single performance issue/checklist.

### Recently clarified
- ✅ May 22: aggregation bottleneck is not just raw signature build time; prioritization, deadlines, fork-choice divergence, and block proposal compaction all matter.
- ✅ Underlying leanMultisig may parallelize aggregation internally, so extra outer parallelism should be configurable and measured rather than assumed.

---

## 👤 Kai

### Active
- 🔥 Continue Zeam DevNet 5 implementation in `blockblaz/zeam#918`.
- 🔥 Fix DevNet 5 CI/simtest practicality:
  - configure CI simtest with larger slot time (8s or 12s if needed),
  - ensure slot-time config is picked up from the DevNet spec / beam command,
  - avoid disabling simtest unless truly unavoidable.
- 🔥 Provide eLambda DevNet 5 interop/finalization screenshot so Gajinder can post the milestone.
- 🔥 Move/prepare the `shadow` branch from DevNet 5 port to DevNet 5 and announce readiness in the shadow/PQ interop channel.
- 🔄 Pull latest leanMultisig aggregation updates into DevNet 5 work; handle breaking API change where errors are returned instead of panics.
- 🔄 Investigate Zeam DevNet 5 memory use (~2GB observed vs eLambda ~500MB) and catch-up stability under forks/low memory.
- 🔄 Help Kamil add shadow-simulation knobs to Zeam:
  - artificial sleeps for signature aggregation / verification / start aggregation,
  - env var or CLI flag control,
  - default zero sleeps in production.
- 🔄 Review aggregation-related PRs and regressions, especially macOS simtest/finality timeout.

### Deferred
- 📌 Pure-Zig libp2p / ETH-P2P remains deferred until dedicated resourcing exists.

---

## 👤 Kamil / Shadow simulations

### Active
- 🔥 Continue leading shadow-simulation setup and topology design.
- 🔥 Baseline achieved by May 22: Zeam + Clean can run under shadow / lean-quickstart; non-aggregator Zeam memory ~400MB/node, aggregator ~600MB/node; 64GB server likely enough for ~100-node first simulations.
- 🔥 Add realistic underlay topologies:
  - EIP-7870-style bandwidth assumptions,
  - geographic latency distributions from Ethereum node country distribution,
  - possible RIPE Atlas-derived latency sampling.
- 🔥 Define and test simulation parameters:
  - node count / validator count per node,
  - one aggregator per subnet baseline,
  - aggregator redundancy,
  - optional multi-subnet/super-aggregator experiments,
  - aggregation/verification artificial compute sleeps.
- 🔄 Prototype visualizations from shadow outputs: attestation propagation, per-slot bandwidth spikes, duplicate gossip, and bottleneck summaries.
- 🔄 Revisit server procurement once scripts/topology/log collection are stable enough to run overnight experiments.

---

## 👤 Katya

### Active / still relevant
- 🔄 Grafana / alert hygiene: avoid noisy head-grow/no-finalization alerts and route only useful alerts to the intended destination.
- 🔄 Keep Grafana webhook routing behavior under watch after prior Telegram 400 / DM-vs-topic glitches.
- 🔄 Upstream useful Zeam-specific metrics to leanSpec when they become protocol-level rather than implementation-specific.

---

## 👤 Noopur / Zclaw / automation

### Active
- 🔥 Keep `zeam-pm` updated from the three sources of truth: Zeam Telegram group, Zeam meeting transcripts, and GitHub state.
- 🔥 Maintain this rolling action list after each call; Noopur noted on May 22 that this rolling list was created by Zclaw and should continue accumulating across the three sources.
- 🔥 Fix or diagnose Zclaw GitHub/Telegram command polling reliability; previous calls noted manual prompting was still needed in some cases.
- 🔄 Confirm model-selection commands work in GitHub issue descriptions/comments as well as Telegram prompts.
- 🔄 Post/pin a short Zclaw usage guide once model-selection behavior is confirmed.
- 🔄 Release workflow reliability: release PR + DevNet tagged image commands should be observable and include `shadow` rebase status.
- 🔄 Explore cost reduction / local-model options for Zclaw/OpenClaw where feasible.

### Watch / policy
- ⚠️ If Zclaw is asked to approve PRs, keep the existing safety distinction: review/comment freely, approve only when explicitly asked by an authorized team member and not for consensus-critical/governance-sensitive changes.
- ⚠️ Continue double-checking critical claims before filing/commenting. Do not claim a fix is included in a build/image without verifying merge/build state.

---

## 📋 Next Call Agenda — June 5, 2026

### 1. DevNet 4 stall / snappy decode / gossip (`#942`)
- Did PR `#948` prevent crash/stall in current images?
- What do debug logs show slot-by-slot when gossip stops?
- Does all-Zeam or no-ethlambda run still stall?
- Should the decode error naming be changed from “corrupt” to invalid/bad snappy input?

### 2. Aggregation performance and proposal deadlines (`#899`)
- Current P95 after latest leanMultisig / aggregation changes.
- Priority ordering for attestation data matching local fork-choice.
- Configurable parallelization factor and immediate publish-on-complete behavior.
- Block proposal deadline: stop compacting and publish ready payloads.

### 3. Metrics / observability
- Subnet aggregate coverage + gossip attestation coverage dashboards.
- Which subnets/clients are missing data?
- Rename misleading `behind` terminology to `missed` / orphaned where appropriate.

### 4. DevNet 5 (`#918`)
- Zeam implementation status.
- eLambda interop finalization screenshot / milestone.
- CI simtest slot-time config and runner/resource plan.
- Memory usage investigation.

### 5. Shadow simulations
- Kamil’s topology/script status.
- Zeam artificial sleep CLI/env knobs.
- First realistic topology target and server procurement timing.
- Visualization/log collection plan.

### 6. Zclaw / PM automation
- Confirm transcripts landed in `zeam-pm`.
- Review rolling action maintenance process.
- Review command polling / release automation reliability.

---

## Source notes

- Meeting transcripts: `Zeam-Meetings/May-08.md`, `Zeam-Meetings/May-15.md`, `Zeam-Meetings/May-22.md`, `Zeam-Meetings/May-29.md`.
- Telegram archive: `Telegram/zeam-group/2026-05-13.jsonl` through `2026-06-04.jsonl`.
- GitHub spot-check on 2026-06-05 for referenced issues/PRs: `#754`, `#817`, `#863`, `#899`, `#910`, `#915`, `#931`, `#942`, `#946`, `#948`, `#972`; open PR list also checked.
