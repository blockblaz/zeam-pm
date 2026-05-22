# Zeam Rolling Action List

*Last updated: 2026-05-22 (Fri) · Scope: May 8 + May 15 calls, Zeam Telegram group archive through 2026-05-22 09:55 UTC, and GitHub state checked on 2026-05-22.*

This file tracks work that survived the weekly calls / community discussion. Closed items are listed once so they do not keep reappearing as stale action items.

**Legend:** ✅ done · 🔥 hot / active · 🔄 carry · 🆕 new since May 15 · 📌 planning/deferred · ⚠️ blocked/watch

---

## ✅ Recently closed / resolved

### Specs, metrics, and DevNet 5
- ✅ `leanEthereum/leanSpec#717` — **DevNet 5 aggregated block proof** merged.
- ✅ `leanEthereum/leanSpec#735` — aggregate coverage metrics with subnet labels merged.
- ✅ `blockblaz/zeam#876` — attestation subnet coverage logging merged.
- ✅ `blockblaz/zeam#898` — aggregate coverage metrics labelled by slot/subnet merged.
- ✅ `blockblaz/zeam#914` — block proposal attestation build metrics merged.

### DevNet 4 / aggregation / hot-path work
- ✅ `blockblaz/zeam#812` / issue `#811` — selective attestation subnet subscription fix merged/closed.
- ✅ `blockblaz/zeam#882` — invalid `blocks_by_range` / `blocks_by_root` request handling merged.
- ✅ `blockblaz/zeam#883` — slot-driver watchdog + chain `onBlock` substep histogram merged.
- ✅ `blockblaz/zeam#884` — lock-free public-key cache merged.
- ✅ `blockblaz/zeam#886` — slot-driver starvation under gossip flood fix merged.
- ✅ `blockblaz/zeam#900` — limit aggregate signature builds to active slot merged.
- ✅ `blockblaz/zeam#902` — aggregator publish counter + gossip signature coverage metrics merged.
- ✅ `blockblaz/zeam#903` — ThinLTO / Rayon threads / leanMultisig bump merged.

### Older review backlog
- ✅ `blockblaz/zeam#715` — SSZ roundtrip spectest runner merged.
- ✅ Zig 0.16 upgrade was reported merged in the May 8 call.

---

## 🔥 Hot list — May 22 onward

1. **Aggregation performance remains the top technical risk.** Issue `blockblaz/zeam#899` is still open: Zeam aggregate build was observed at ~2–3s vs ethlambda ~0.4s on DevNet 4.
2. **Run/compare the latest aggregation fixes** (`#900`, `#902`, `#903`, `#914`) on DevNet/shadow sims and decide the next optimization path.
3. **Default topology:** lean-quickstart / DevNet runs should use **one aggregator per subnet by default**. Multi-subnet “super aggregator” remains a manual/high-hardware test mode.
4. **Shadow simulations:** define exactly what questions the shadow sims should answer: aggregator hardware profile, subnet count per aggregator, redundancy, aggregation window, and whether recursive/second-level aggregation is viable.
5. **DevNet 5 implementation:** spec is merged; client implementation and CI fixture practicality are now the active blockers.
6. **Zclaw automation:** automatic issue/PR command polling appears unreliable; verify and fix so GitHub/Telegram commands are processed without manual prompting.
7. **Release automation:** release PR + DevNet 4 tagged image workflow should report whether the `shadow` branch rebased successfully.

---

## 👤 Gajinder

### Active
- 🔥 Continue reviewing / steering **aggregation performance** work from `#899` and follow-on PRs.
- 🔥 Drive the default **one aggregator per subnet** topology for DevNet runs; keep “super aggregator” as a manual override for heavier machines.
- 🔥 Use the latest metrics to decide whether the remaining bottleneck is signature aggregation, libxev/libp2p thread blocking, peer/fetch behavior, or block proposal aggregation.
- 🔄 Continue Z-to-Z / Beam-style runs and add logging/metrics where the cause is still opaque.
- 🔄 Review DevNet 5 implementation alignment: single block signature reuse, proof deconstruction/splitting behavior, and whether proposer behavior matches fork-choice justified/finalized state.
- 📌 DevNet 6 planning: execution integration is the likely DevNet 6 focus. PQ heartbeat is **out of DevNet 5** and likely **out of DevNet 6** unless a clear proposal lands first.
- 📌 Keep execution integration aligned with the latest Engine API fork; fork-by-fork bumps are acceptable.

### Watch / backlog
- 🔄 `blockblaz/zeam#754` remains open — attestation handling / leanSpec alignment.
- 🔄 `blockblaz/zeam#817` remains open — async response handling / lock-free RPC serving; still needs value/mergeability decision.
- 🔄 `blockblaz/zeam#915` remains open — minor cleanup, requested review/approval in Telegram.

---

## 👤 Parthasarathy (Partha / @grapebaba)

### Active
- 🔥 Keep leading DevNet 4 multi-subnet / high-validator testing.
  - May 15 status: 64-validator run existed; Hetzner quota increase was granted; next step was provisioning for 128-validator runs before/around the May 22 call.
- 🔥 Validate latest aggregation fixes in a long run and update `#863` / `#899` with whether slot-driver stalls, lock contention, or signature build cost still dominate.
- 🔥 Ensure lean-quickstart defaults to **one aggregator per subnet**, not aggregators listening to all subnets. Manual edits can still test multi-subnet “super aggregator” behavior.
- 🆕 Create/write the **shadow simulation outcomes document** requested May 21:
  - expected aggregator hardware profile,
  - how many subnets a typical aggregator should cover,
  - required aggregator redundancy,
  - whether capable nodes should auto-upgrade into aggregators,
  - whether recursive / second-level aggregation is worth its extra latency,
  - aggregation window target: <0.4s compute budget when propagation needs the rest of the 0.8s slot.
- 🔄 Continue work around issue `#863` — slow slot interval / event-loop starvation. PRs `#883`, `#884`, `#886` landed, but the issue remains open pending validation.
- 🔄 Keep issue `#910` in view: make `blocks_by_root` peer selection slot-aware. This is separate from the checkpoint-peer-affinity PR.

### Watch / recently discussed
- ⚠️ macOS simtest / node3 finalization timeout was discussed May 22. PR `#914` reportedly addressed it and is merged; keep watch for flakiness/regressions.
- 🔄 `blockblaz/lean-quickstart#151` — shadow network simulator automation remains open.

---

## 👤 Anshal

### Active
- 🔥 DevNet 5: spec PR is merged, but CI fixture generation is still too slow. Follow-ups from May 15:
  - coordinate with Toma on heavier GitHub runners / CI plan,
  - keep max attestations configurable,
  - use lower max attestations for tests if needed without changing target behavior,
  - consider macOS runner only if it is the practical unblocker.
- 🔥 Zeam DevNet 5 implementation / proof logic:
  - verify block proposal behavior when fork-choice justified/finalized has moved but split/imported aggregate payloads have not arrived yet,
  - rely on aggregator splitting via gossip, but ensure proposer failure/skip behavior is correct,
  - add a spec test or Hive test for this behavior if possible.
- 🔥 Aggregation optimization:
  - revisit threading architecture,
  - keep aggregation off hot/libxev paths,
  - explore parallel/tree aggregation where useful,
  - make sure async aggregation has a timeout/window and does not block slot progression.
- 🔄 Address comments on the PR removing scheduled gossip-network calls from the libxev thread.
- 🔄 Continue resolving comments on the `ssz.zig` / Merkle root caching PR.
- 🔄 Collate performance TODOs with Partha and Kai into a single performance issue/checklist so they can be burned down systematically.

### Recently clarified
- ✅ Existing compact-attestation/proposal metrics were found, but May 20–21 discussion required more detailed metrics and upstream naming. Zeam `#914` and leanSpec `#735` are now merged; validate that they cover the requested proposal aggregation dimensions in live dashboards.

---

## 👤 Kai

### Active
- 🔥 Start / continue Zeam **DevNet 5 implementation** now that leanSpec `#717` is merged.
- 🔥 Validate threading changes in DevNet runs:
  - avoid over-fine-grained `num_cpu - 3` style tuning,
  - prefer simpler `num_cpu` sizing where threads are needed,
  - compare Kai’s current changes vs full `num_cpu` worker counts and revert/tune only if the run shows regression.
- 🔄 Review / help land remaining attestation-handling work (`#754` still open).
- 🔄 Review aggregation-related PRs and regressions, especially where macOS simtest/finality timeout is involved.
- 🔄 Check whether `cache = false` in CI/YAML is actually needed; try removing/changing it if CI remains green.

### Deferred
- 📌 Pure-Zig libp2p / ETH-P2P remains deferred: current fork has Lighthouse interop issues and lacks dedicated resourcing.

---

## 👤 Katya

### Active / still relevant from prior rolling list
- 🔄 Grafana / alert hygiene: avoid noisy head-grow/no-finalization alerts and route only useful alerts to the intended destination.
- 🔄 Keep Grafana webhook routing behavior under watch after prior Telegram 400 / DM-vs-topic glitches.
- 🔄 Upstream useful Zeam-specific metrics to leanSpec when they become protocol-level rather than implementation-specific.

---

## 👤 Noopur / Zclaw / automation

### Active
- 🔥 Fix or diagnose **Zclaw GitHub/Telegram command polling**. May 15 call noted that commands on PRs/comments were not being picked up automatically and needed manual prompting.
- 🔥 Confirm model-selection commands work not only in Telegram prompts but also in GitHub issue descriptions/comments. If not, configure/support that flow.
- 🔥 Post/pin a short Zclaw usage guide in the Zclaw or main channel once model-selection behavior is confirmed.
- 🆕 Add a CI task for the `shadow` branch:
  - auto-rebase `shadow` on new tips,
  - fail visibly if rebase fails,
  - include `shadow` rebase success/failure in DevNet release messages.
- 🔄 Release workflow: `@zeam_eth_bot create a release pr and a devnet4 tagged docker image as per the release readme document` was requested repeatedly in Telegram; make sure this command path is reliable and observable.
- 🔄 Continue transcript ingestion into `zeam-pm` and use Telegram + transcripts + GitHub as the three sources for this rolling list.
- 🔄 Explore cost reduction / local-model options for Zclaw/OpenClaw. May 8 discussion mentioned local GPU/server options, LocalAI-style setups, Llama, and needing better project memory to reduce token cost.

### Watch / policy
- ⚠️ If Zclaw is asked to approve PRs, keep the existing safety distinction: review/comment freely, approve only when explicitly asked by an authorized team member and not for consensus-critical/governance-sensitive changes.
- ⚠️ Continue double-checking critical claims before filing/commenting. Prior lesson: do not claim a fix is included in a build/image without verifying merge/build state.

---

## 📋 Next Call Agenda — May 22, 2026

### 1. DevNet 4 aggregation performance / issue `#899`
- Current aggregate build time after `#900`, `#902`, `#903`, `#914`.
- Is Zeam still 5–7x slower than ethlambda in the latest official image?
- Which bottleneck remains: recursive aggregation, Rayon/thread count, block proposal compacting, libxev/libp2p blocking, or something else?

### 2. Shadow simulations
- Review the desired outcomes doc.
- Decide aggregator hardware assumptions and subnet-per-aggregator target.
- Decide whether second-level/recursive aggregation is viable under the 0.8s slot / ~0.4s compute budget.
- Confirm `shadow` branch workflow and auto-rebase CI requirement.

### 3. DevNet 5
- leanSpec `#717` merged: Zeam implementation status.
- CI fixture generation problem: runner size, max attestations, macOS vs Linux, and test-specific configs.
- Proof deconstruction / splitting / proposer behavior test plan.

### 4. Topology and tooling
- Confirm lean-quickstart default: one aggregator per subnet.
- Confirm high-validator run status: 64 → 128 validator provisioning and results.
- Confirm local tooling remains preferred unless Kubernetes/Curtosis tooling is clearly better.

### 5. Zclaw / automation
- Why command polling stopped or became unreliable.
- Whether `/model` or equivalent model selection works in GitHub issues/comments.
- Release PR + DevNet 4 tagged image automation status.
- Transcript / Telegram / GitHub rolling-action maintenance process.

### 6. Open PR/issue triage
- `#754` attestation handling alignment.
- `#817` async response handling / lock-free RPC — decide value/merge path.
- `#915` minor cleanup.
- `#863`, `#899`, `#910` open issues.

---

## Source notes

- Meeting transcripts: `Zeam-Meetings/May-08.md`, `Zeam-Meetings/May-15.md`.
- Telegram archive: `Telegram/zeam-group/2026-05-13.jsonl` through `2026-05-22.jsonl`.
- GitHub state checked for referenced PRs/issues on 2026-05-22 before this update.
