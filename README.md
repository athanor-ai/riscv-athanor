# Athanor RISC-V Frontier

This repository is the umbrella index for Athanor's open RISC-V optimization
work. It tracks what is proven, what is only scouted, and what gate must clear
next before a result can be described as impact.

Method: forward-deployed agents plus Kairos. Agents choose targets, apply
cross-run memory and hardware judgment, and use Kairos as the evidence engine.
Rows here must stay receipt-indexed and scope-honest.

## Current Customer-Safe Status

Today, public RISC-V impact is Ibex-only. The accepted Ibex evidence consists
of three artifacts: two module-level artifacts and one whole-core top-level
survivor.

| Core | Artifact | Status | Scope | Evidence |
| --- | --- | --- | --- | --- |
| Ibex | `ibex_if_stage / expanded_predicate_factor` | Accepted top-level survivor | Whole-core `ibex_top`: selected-toolchain area and timing improve, toggle flat, formal equivalence closed, cold replay reproduced | [`ibex-athanor/athanor_artifacts/if_stage_expanded_predicate_factor`](https://github.com/athanor-ai/ibex-athanor/tree/main/athanor_artifacts/if_stage_expanded_predicate_factor) |
| Ibex | `ibex_multdiv_slow / greater_equal_xor_shape` | Accepted module artifact | Module-scope area/timing positive, toggle flat, formal equivalence closed; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_slow_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/main/athanor_artifacts/multdiv_slow_greater_equal_xor_shape) |
| Ibex | `ibex_multdiv_fast / greater_equal_xor_shape` | Accepted module artifact | Module-scope timing positive, cell/toggle flat, formal equivalence closed; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_fast_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/main/athanor_artifacts/multdiv_fast_greater_equal_xor_shape) |

Investor/customer-safe wording:

> We have three accepted Ibex optimization artifacts, including one whole-core
> top-level survivor.

Do not claim three whole-core Ibex optimizations, an aggregate whole-core Ibex
percentage, or non-Ibex RISC-V impact until the relevant receipt exists.

## Scout Queue

These rows are reconnaissance, baselines, or pre-spend leads. They are not
optimization claims.

| Core | Current state | Best current lead | Next honest gate |
| --- | --- | --- | --- |
| Ibex | Active public design with accepted receipts | Dexter re-review of remaining candidate queue before leaving Ibex | Post either a quick-win candidate or a near-exhausted verdict with receipts |
| CV32E40P | Non-Ibex scout work exists | `cv32e40p_id_stage` hazard-tail factoring: bounded module generic-Yosys cells `14959 -> 14917`; whole-core no-ABC `40858 -> 40852`; no selected-flow/formal/toggle/cold replay yet | Run selected-flow area/timing review before any formal/toggle spend |
| PicoRV32 | Selected-flow baseline and residual replay exist | Replay found two residual rows; both cheap-killed (`0 == mem_wstrb` formal/assertion-only, `pcpi_rs1 - pcpi_rs2` area neutral) | Treat as negative transferability receipt unless a new detector family appears |
| SERV | Selected-flow baseline exists | Detector replay found zero spend-ready leads | Treat as negative transferability receipt |
| ultraembedded/riscv | Selected-flow baseline exists | Source-only constant-prop leads disappeared after selected-flow residual replay | Treat as negative transferability receipt |
| VexRiscv | Preflight only | Generator toolchain blocked locally; no pinned generated CPU RTL baseline | Provision/import a pinned generated RTL artifact before any detector or PPA work |

Machine-readable status lives in [`frontier-status.json`](frontier-status.json).
