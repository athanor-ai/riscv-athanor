# Athanor RISC-V Frontier

This repository is the umbrella index for Athanor's open RISC-V optimization
work. It tracks what is proven, what is only scouted, and what gate must clear
next before a result can be described as impact.

Method: forward-deployed agents plus Kairos. Agents choose targets, apply
cross-run memory and hardware judgment, and use Kairos as the evidence engine.
Rows here must stay receipt-indexed and scope-honest.

## Current Customer-Safe Status

Today, public RISC-V impact is Ibex-only. The accepted Ibex evidence consists
of five artifacts: three module-level artifacts and two whole-core top-level
survivors.

| Core | Artifact | Status | Scope | Evidence |
| --- | --- | --- | --- | --- |
| Ibex | `ibex_if_stage / expanded_predicate_factor` | Accepted top-level survivor | Whole-core `ibex_top`: area `108428.9920 -> 108397.7120`; all recorded WNS groups improve; toggle flat `311729 -> 311729`; Yosys equiv `1956/1956`; cold replay `6/6` | [`ibex-athanor/athanor_artifacts/if_stage_expanded_predicate_factor`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/if_stage_expanded_predicate_factor) |
| Ibex | PR #31: `ibex_top / no_bp_prefetch_direct` | Accepted top-level survivor | Whole-core `ibex_top`: area `108441.5040 -> 108373.9392`; WNS deltas `+13.7942/+13.7942/+13.7924/+0.3407/+0.1761ns`; toggle flat `311729 -> 311729`; Yosys equiv `1956/1956`; hosted OSS-FV green; cold review passed | [`ibex-athanor` PR #31 top-level receipt](https://github.com/athanor-ai/ibex-athanor/blob/ea0e5bc50e2322369a5cee166161acadbda417f0/athanor_artifacts/if_stage_no_bp_prefetch_direct/top_level_first/top_level_first_receipt.json) |
| Ibex | `ibex_multdiv_slow / greater_equal_xor_shape` | Accepted module artifact | Module-scope area `10339.9168 -> 10333.6608`; data arrival `8.13ns -> 7.25ns`; toggle flat `6117 -> 6117`; Yosys equiv `411/411`; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_slow_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/multdiv_slow_greater_equal_xor_shape) |
| Ibex | `ibex_multdiv_fast / greater_equal_xor_shape` | Accepted module artifact | Module-scope cell metric flat `3306 -> 3306`; max delay `10.85ns -> 10.57ns`; toggle flat `7657 -> 7657`; Yosys equiv `772/772`; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_fast_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/multdiv_fast_greater_equal_xor_shape) |
| Ibex | `ibex_fetch_fifo / err_unaligned_factored` | Accepted module-local artifact | Module-scope generic cells `396 -> 395`; liberty cells `456 -> 451` (`-1.0965%`); timing flat at `6.32ns`; SAIF transition-count flat `34031 -> 34031`; relation-aware sequential miter closes; not a whole-core headline | [`ibex-athanor/athanor_artifacts/fetch_fifo_err_unaligned_factored`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/fetch_fifo_err_unaligned_factored) |

Investor/customer-safe wording:

> We have five accepted Ibex optimization artifacts, including two whole-core
> top-level survivors and three module-level artifacts.

Do not claim five whole-core Ibex optimizations, an aggregate whole-core Ibex
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
