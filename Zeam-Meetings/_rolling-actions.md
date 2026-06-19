# Zeam Rolling Action List

*Last updated: 2026-06-19 (Fri) · Scope: May 8, May 15, May 22, May 29, June 5, and June 12 calls; Zeam Telegram group archive through 2026-06-18; GitHub/repository spot-check on 2026-06-19.*

This file tracks work that survived the weekly calls, Telegram follow-ups, and repository state. Closed items are listed once so they do not keep reappearing as stale action items.

**Legend:** ✅ done · 🔥 hot / active · 🔄 carry · 🆕 new since June 5 · 📌 planning/deferred · ⚠️ blocked/watch

---

## ✅ Recently closed / resolved

### DevNet 5 baseline and releases
- ✅ `blockblaz/zeam#918` — DevNet 5 aggregated block proof / single Type-2 `SignedBlock.proof` merged on June 9.
- ✅ DevNet 4 final pre-DevNet-5 release shipped: `#982` / `v0.4.36` from commit `0009ea9`.
- ✅ DevNet 5 release train started and continued: `#984` / `v0.5.0`, `#993` / `v0.5.1`, `#1000` / `v0.5.2`, `#1005` / `v0.5.3`.
- ✅ June 12 call: all-Zeam DevNet 5 with four subnets reportedly ran with justification/finality for 24h+, but with delayed block publishing/proof generation work still active.

### DevNet 5 correctness / fork-choice / sync fixes
- ✅ `#987` — current-slot att_data proof on interval-2 tick merged.
- ✅ `#988` — proposal-path layering, publish latency, and coverage report fix merged.
- ✅ `#989` — stale low-target att_data / subnet-0 proposal drop issue closed by `#990`.
- ✅ `#990` — prefer fresh high-target proposal attestations merged.
- ✅ `#995` — skip `min_slot` filter on `blocks_by_root` path merged.
- ✅ `#1001` — trigger catch-up on finalization gap, not only head gap, merged.
- ✅ `#1002` — finalized-from-canonical-head + gossip-validation hardening + head-slot pruning for leanSpec `#1001`/`#1020` merged.
- ✅ `#1004` — enforce attestation checkpoint ancestry merged.

### Observability / metrics / tooling
- ✅ `#996` — `zeam_block_proof_merge_time_seconds` metric for Type-2 merge latency merged.
- ✅ `#998` — block proposal metrics and logs merged.
- ✅ `#975` — shadow-rebase failure Telegram message includes conflict file name.
- ✅ `#981` / `#983` — release automation follow-ups: release from PR head commit and escape Telegram release notification backticks.

### Older DevNet 4 context retained
- ✅ `leanEthereum/leanSpec#717` — DevNet 5 aggregated block proof merged; Zeam implementation landed through `#918`.
- ✅ `blockblaz/zeam#931` — `shadow` branch auto-rebase + release status task merged/closed.
- ✅ `blockblaz/zeam#972` — leanMultisig `multisig-glue` dependency bump merged.
- ✅ `blockblaz/zeam#946`, `#948`, `#915`, `#900`, `#902`, `#903`, `#914`, `leanSpec#735`, `zeam#876`, `zeam#898` remain closed historical items.

---

## 🔥 Hot list — after June 5 + June 12 calls and June 18 Telegram

1. **DevNet 5 is now the primary technical track.** DevNet 4 should be treated as mostly wrapped except for published stats / learnings; current releases and testing are DevNet 5.
2. **Late block publishing / Type-2 proof merge latency is the top DevNet 5 risk.** Blocks can be selected/produced on time, but the block is not publishable until `buildBlockProof` finishes the recursive STARK / Type-2 merge; Telegram and June 12 call estimate roughly 0.5–1.5s in common cases, with only ~20–30% publishing in interval 0 in one test.
3. **Proposal, signing, and publishing architecture must stay separated.** Telegram June 10–12: restore/keep validator-client proposal flow (`maybeDoProposal` semantics), move signing/proposal code out of chain where appropriate, avoid re-processing STF for locally produced blocks, and cache post-state correctly.
4. **Bound block-proof work without hiding correctness cases.** Active design: add metrics/logs, use publish budgets / attestation-group caps, start Type-1 aggregation in the last interval of the previous slot, and add hard limits so stale proposal/attestation work does not overlap indefinitely.
5. **Type-1 / Type-2 / splitting metrics are needed to guide optimization.** June 12 call specifically requested logs/metrics for Type-1 aggregation, Type-2 merge, split/signature work, delayed-publish intervals, and whether proposal/attestation duties are missing deadlines.
6. **Pure-Zig networking is now a serious review track.** `blockblaz/zeam#968` replaces `rust-libp2p-glue` with `zig-libp2p`; Partha reported 12h+ interop progress with several clients and moved `zig-libp2p` under `blockblaz`. Needs review and wider client/shadow testing.
7. **Shadow simulations remain required for DevNet 5 scale testing.** Use the 96GB shadow server; test 32+ nodes / 4 subnets, artificial Type-2 merge time, one-aggregator-per-subnet baseline, and realistic topologies. Current watch item: shadow/fake-merge-time runs reported a failure resembling closed leanSpec `#1000`; confirm whether Zeam has the corresponding fixes and test coverage.
8. **Spec alignment is moving fast.** leanSpec changes around `MultiMessageAggregate` / Type-2 naming, validator sync-lag duty gating, source votes from head-chain justified checkpoint, and tiered block production need active tracking in Zeam.
9. **Release automation remains in the loop.** Telegram requests continued through June 18 for DevNet 5 release PR + tagged Docker images; keep release PRs observable and include shadow status where possible.
10. **PM automation / rolling list remains a standing task.** Keep `zeam-pm` current from the three sources of truth: meetings, Telegram, and repository/GitHub state.

---

## 👤 Gajinder

### Active
- 🔥 Drive the DevNet 5 late-publish resolution: clarify when block selection, Type-1 aggregation, Type-2 merge, signing, and publish happen; insist on logs/metrics that make the timeline visible.
- 🔥 Keep proposal/validator architecture clean: proposal creation, signing, and publishing should stay separated between node/chain and validator client; no accidental local STF reprocessing for locally produced blocks.
- 🔥 Review active DevNet 5 optimization PRs: `#992`, `#997`, `#1003`, and follow-ups from `#996`/`#998` metrics.
- 🔥 Keep pressure on hard limits: proposal and attestation work should be singleton/bounded; if previous proposal work is still running, the implementation needs a clear discard/continue strategy.
- 🔄 Coordinate DevNet 5 public stats/milestone messaging now that DevNet 5 has releases and all-Zeam finality reports.
- 🔄 Continue pushing for shadow simulations on the 96GB server, including realistic network assumptions and fake merge/compute delays.
- 🔄 Track leanSpec changes (`#1166`, `#1165`, `#1149`, `#791`, earlier `#799` discussion) and ensure Zeam semantics match.
- 📌 DevNet 6 / execution integration remains planning: decide whether to put an EPF-style project around engine API / execution integration.

### Watch / backlog
- 🔄 `blockblaz/zeam#754` remains open — attestation handling / leanSpec alignment.
- 🔄 `blockblaz/zeam#817` remains open — async response handling / lock-free RPC serving; still needs value/mergeability decision.
- 🔄 `blockblaz/zeam#863` / `#867` remain open — slow slot interval / xev loop spikes.
- 🔄 `blockblaz/zeam#910` remains open — slot-aware `blocks_by_root` peer selection, though `#995` fixed the `min_slot` path issue.

---

## 👤 Parthasarathy (Partha / @grapebaba)

### Active
- 🔥 Continue DevNet 5 testing and release validation; June 12 baseline was stable all-Zeam 4-subnet finality for 24h+, but late publishing remains unresolved.
- 🔥 Validate current DevNet 5 release images (`v0.5.x`) after merged fixes `#988`, `#990`, `#995`, `#996`, `#998`, `#1001`, `#1002`, `#1004`.
- 🔥 Continue the pure-Zig networking path in `#968`: finish wider interop tests (ethlambda, lantern, gean done/reported; grandine, ream, qlean planned), then get review/merge readiness.
- 🔥 Use the shadow server for DevNet 5 local/shadow runs with other clients; verify block production/publish timing under fake merge time and realistic client mix.
- 🔄 Help quantify Type-2 merge / block-proof bottlenecks with metrics/logs; June 12 specifically asked for mean STARK generation/merge time and component breakdown.
- 🔄 Review/iterate `#992` publish-budget cap and `#976` cgroup-aware CPU worker pool sizing.
- 🔄 Recreate/update the key-store EIP draft in an accessible repo and start the EIP PR process as a draft.
- 📌 Consider whether an EPF project proposal should cover engine API / DevNet 6 integration work.

### Recently discussed
- ⚠️ Type-2 merge appears more expensive than Type-1 aggregation in current observations; splitting may also be significant and needs measurement.
- ⚠️ Earlier DevNet 4 snappy/decode/gossip issue `#942` remains open, but current attention has shifted to DevNet 5 late publishing and proof merge timing.

---

## 👤 Anshal

### Active
- 🔥 Own aggregation/proposal timing follow-ups from June 12:
  - start Type-1 aggregation in interval 4 / previous slot (`#1003` open),
  - add/verify logs and metrics for Type-1 aggregation, Type-2 merge, splitting, and delayed interval transitions,
  - apply hard limits so Type-1 does not run past its budget and proposal/attestation duties do not overlap indefinitely.
- 🔥 Review current DevNet 5 optimization path and Kai/Partha PRs; June 17 Telegram specifically asked Anshal and Kai to look into eLambda block-building optimization proposals and Zeam-specific optimizations.
- 🔄 Continue SSZ / hash-tree-root alignment for leanSpec changes; June 5 Telegram flagged `MultiMessageAggregate` / ByteList vs Container HTR differences from leanSpec `#799`.
- 🔄 Keep EIP work moving: aggregator-role EIP / EIP-892 draft was said to be ready enough to share, with access to create the topic now fixed.
- 🔄 Draft or help draft EPF / DevNet 6 proposal around engine API, CI caching/perf work, or execution integration if the team chooses to submit it.
- 🔄 Continue reviewing DevNet 5 implementation / aggregation branch updates and leanMultisig API changes.

### Recently clarified
- ✅ June 12: delayed block production should not automatically mean skipping the block in every case; side-branch / finality-moving cases may justify longer work, but logs/metrics must make the delay explicit.
- ✅ Type-1 aggregation is likely less expensive after recent fixes; Type-2 merge and splitting need real measurements before choosing optimization strategy.

---

## 👤 Kai

### Active
- 🔥 Continue DevNet 5 proof/merge instrumentation and fixes. `#996` metric is merged; June 12 requested an additional clear log around `buildBlockProof` / aggregate proof production.
- 🔥 Confirm leanSpec `MultiMessageAggregate` / proof type changes are name-only vs SSZ-structure changes; update Zeam naming if it is only a rename, and flag if HTR semantics changed.
- 🔥 Continue shadow branch maintenance and DevNet 5 shadow runs. June 12: shadow branch was updated to latest code; 32-node runs with fake Type-2 merge time were underway.
- 🔄 Help test / review `#1003` Type-1 previous-interval aggregation and `#992` publish-budget behavior.
- 🔄 Keep DevNet 5 simtest / CI practical with appropriate slot time/config and avoid disabling simtests unless unavoidable.
- 🔄 Investigate memory and catch-up stability under forks/low memory as DevNet 5 scales.
- 🔄 Coordinate with Partha on pure-Zig networking impacts where `#968` touches DevNet 5 behavior.

---

## 👤 Kamil / Shadow simulations

### Active
- 🔥 Continue shadow-simulation setup against current DevNet 5 / `shadow` branch.
- 🔥 Use the 96GB / ~28-core server as the main near-term test machine; assume roughly 90GB available for actual runs.
- 🔥 Run 32-node / 4-subnet and larger experiments with fake Type-2 merge time, one aggregator per subnet baseline, and realistic bandwidth/latency topologies.
- 🔥 Track and reproduce the fake-merge-time issue referenced in Telegram; it was compared to closed `leanEthereum/leanSpec#1000`, so confirm whether the same fork-choice fix/test coverage applies in Zeam.
- 🔄 Add/keep knobs for artificial sleeps: aggregation, verification, proposal/block proof, and splitting as needed; production defaults should stay zero.
- 🔄 Continue visualization/log collection from shadow outputs: attestation propagation, late-publish slots, bandwidth spikes, duplicate gossip, and bottleneck summaries.

---

## 👤 Katya

### Active / still relevant
- 🔄 Grafana / alert hygiene remains a watch item: avoid noisy head-grow/no-finalization alerts and route only useful alerts to the intended destination.
- 🔄 Upstream Zeam-specific metrics to leanSpec only when they become protocol-level rather than implementation-specific.
- 🔄 Watch DevNet 5 metrics additions (`#996`, `#998`, future Type-1/Type-2/splitting metrics) for dashboard usefulness.

---

## 👤 Noopur / Zclaw / automation

### Active
- 🔥 Keep `zeam-pm` updated from the three sources of truth: Zeam Telegram group, Zeam meeting transcripts, and GitHub/repository state.
- 🔥 Maintain the rolling action list after each call; this update incorporates June 5 + June 12 transcripts, Telegram through June 18, and GitHub spot-check on June 19.
- 🔥 Keep release workflow observable: release PRs, DevNet tagged images, and shadow rebase status should be visible in PR/topic updates.
- 🔄 Track `#997` — move proposal signing into validator client — as the Zclaw-created follow-up from the June 12 Telegram TODO.
- 🔄 Track `#999` — spectest/fork-choice runner fix — until it is merged or superseded.
- 🔄 Continue diagnosing command polling / GitHub + Telegram reliability, especially release PR and tagged image commands.
- 🔄 Post/pin a short Zclaw usage guide once model-selection behavior is confirmed.
- 🔄 Explore cost reduction / local-model options for Zclaw/OpenClaw where feasible.

### Watch / policy
- ⚠️ If Zclaw is asked to approve PRs, keep the existing safety distinction: review/comment freely, approve only when explicitly asked by an authorized team member and not for consensus-critical/governance-sensitive changes.
- ⚠️ Continue double-checking critical claims before filing/commenting. Do not claim a fix is included in a build/image without verifying merge/build state.

---

## 📋 Next Call Agenda — June 19, 2026

### 1. DevNet 5 release health
- Which `v0.5.x` image is the current test baseline?
- Is all-Zeam 4-subnet finality still stable after `#1001`, `#1002`, `#1004`, and `#1005`?
- Which mixed-client combinations have been tested since June 12?

### 2. Late block publishing / Type-2 merge
- What are current `zeam_block_proof_merge_time_seconds` mean/P95/P99 values?
- How often does block publishing miss interval 0 after `#996`/`#998`?
- Is `#992` still the right publish-budget strategy, or should it be revised after metrics?
- Do logs clearly separate produce/select, Type-1 aggregation, Type-2 merge, signing, and publish?

### 3. Proposal / validator-client architecture
- Status of `#997` moving proposal signing into validator client.
- Confirm locally produced blocks cache post-state and do not re-run STF unnecessarily.
- Decide singleton/discard strategy when proposal or attestation work overruns into the next duty window.

### 4. Aggregation timing and optimization
- Status of `#1003` Type-1 aggregation in interval 4.
- Which component is actually slow: Type-1 aggregation, Type-2 merge, splitting, signing, or publish path?
- Should Zeam adopt any eLambda block-building optimization ideas?

### 5. Pure-Zig networking / libp2p
- Review status of `#968` and `blockblaz/zig-libp2p`.
- Which clients have passed interop and for how long?
- What remains before replacing `rust-libp2p-glue` on a DevNet 5 test image?

### 6. Shadow simulations
- Latest 32-node / 4-subnet run results on the 96GB server.
- Reproduction/status of fake merge-time issue compared to closed `leanSpec#1000`.
- Next topology target, run duration, and visualization/log outputs.

### 7. EIPs / planning
- Aggregator-role EIP / EIP-892 draft status.
- Key-store EIP recreation status.
- Whether to submit an EPF / DevNet 6 engine API integration project.

### 8. Zclaw / PM automation
- Confirm transcripts landed in `zeam-pm`.
- Review rolling action update cadence.
- Review release command polling / tagged image command reliability.

---

## Source notes

- Meeting transcripts: `Zeam-Meetings/May-08.md`, `Zeam-Meetings/May-15.md`, `Zeam-Meetings/May-22.md`, `Zeam-Meetings/May-29.md`, `Zeam-Meetings/June-05.md`, `Zeam-Meetings/June-12.md`.
- Telegram archive: `Telegram/zeam-group/2026-05-13.jsonl` through `Telegram/zeam-group/2026-06-18.jsonl`, with the June 5–18 entries used for this update.
- GitHub spot-check on 2026-06-19 for referenced Zeam PRs/issues: `#754`, `#817`, `#861`, `#899`, `#910`, `#918`, `#942`, `#968`, `#976`, `#977`, `#982`, `#984`, `#987`, `#988`, `#989`, `#990`, `#991`, `#992`, `#993`, `#995`, `#996`, `#997`, `#998`, `#999`, `#1000`, `#1001`, `#1002`, `#1003`, `#1004`, `#1005`.
- Related repository spot-check on 2026-06-19: `leanEthereum/leanSpec` open PRs `#1166`, `#1165`, `#1149`, `#791`, `#784`, `#753`, `#748`; open issues `#747`, `#697`; `blockblaz/zig-libp2p` open PR `#239` and issues `#235`, `#212`, `#211`, `#210`, `#172`, `#171`, `#170`, `#169`, `#166`.
