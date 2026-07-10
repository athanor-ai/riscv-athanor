# Athanor RISC-V Frontier

This repository is the umbrella index for Athanor's open RISC-V optimization
work. It tracks what has evidence today, what is still exploratory, and what
evidence is needed before an exploratory result can be described as impact.

Method: forward-deployed agents plus Kairos. Agents choose targets, apply
cross-run memory and hardware judgment, and use Kairos as the evidence engine.
Every row must link to its evidence and state exactly what scope that evidence
supports.

## Current Public Status

Today, public RISC-V impact is Ibex-only. The accepted Ibex evidence consists
of five artifacts: two whole-core results and three module-level results.

## How To Read This Page

An **accepted artifact** is an optimization result that has a linked evidence
package. The evidence package records the design version, the change, the
measured result, and the checks that were run.

**RTL** means register-transfer-level hardware source code, the Verilog or
SystemVerilog design that synthesis tools turn into gates.

A **whole-core result** means the candidate was evaluated on the full
`ibex_top` design, not only on the smaller block where the edit was made. A
whole-core result is the level that can support a headline about the Ibex core.

An **accepted whole-core result** is an optimization that stayed beneficial
after integration into the whole processor and passed the required checks:
area, timing, activity, formal equivalence, and independent replay. Many local
edits improve a small block but disappear or regress when the entire core is
rebuilt; those are not whole-core results.

A **module-level artifact** means the result is real for one named RTL module,
but it is not a claim about the entire Ibex core. These rows are still useful
engineering results, but they are deliberately separated from whole-core rows.

**Area** is a synthesis size estimate, where lower is usually better. **WNS**
means worst negative slack, a timing metric where improvement means more timing
margin. **Data arrival** and **max delay** are timing measurements where lower
delay or better margin is better. **Liberty cells** are cells counted after
mapping to a standard-cell library. **Toggle** is a switching-activity count
used as a power proxy; "flat" means the checked count did not increase. **SAIF**
is the switching-activity file format used for the toggle check. **Yosys
equivalence** is a formal check that the optimized RTL preserves the original
behavior for the checked scope. A **miter** is another formal equivalence check
that connects the original and changed designs and proves their relevant outputs
match. A **bad mutant** is an intentionally wrong version used to prove that the
check would fail if behavior changed. **OSS-FV** is the hosted open-source
formal-verification workflow used by the Ibex evidence runs. **Cold replay**
means the receipt was rerun from a fresh checkout or fresh environment rather
than relying on a warm local state.

A **receipt** is the linked machine-readable evidence file or hosted run for a
row. It is the starting point for independently checking the claim.

A **selected-flow** run evaluates a candidate in the same synthesis/timing flow
we use for that target before spending time on deeper proof or activity checks.
**Generic-Yosys** and **no-ABC** are early synthesis screens; they can identify
possible leads, but they are not enough by themselves for an accepted
optimization claim.

A **detector** is an automated search rule that looks for a possible
optimization. **Preflight** means setup work before a usable baseline exists.

Timing group names describe the kind of path being checked: **in2out** is an
input-to-output combinational path, **in2reg** is input-to-register, **reg2out**
is register-to-output, and **reg2reg** is register-to-register. A lower slack
number is worse.

A **rejected tradeoff** is a candidate that improved at least one checked axis,
such as area or timing, but failed another required axis. These rows are not
optimization claims. They are included so readers can see what was tried, what
improved, and why the candidate was not accepted. Candidates that fail every
axis stay in the internal lead bank instead of cluttering this page.

Do not read the Ibex module-level rows as whole-core Ibex wins, and do not read
the non-Ibex exploratory rows as optimization claims. They are listed so the current
state and the next evidence gate are visible.

| Core | Artifact | Status | Scope | Evidence |
| --- | --- | --- | --- | --- |
| Ibex | `ibex_if_stage / expanded_predicate_factor` | Accepted whole-core result | Whole-core `ibex_top`: area `108428.9920 -> 108397.7120`; all recorded WNS groups improve; toggle flat `311729 -> 311729`; Yosys equivalence `1956/1956`; cold replay `6/6` | [`ibex-athanor/athanor_artifacts/if_stage_expanded_predicate_factor`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/if_stage_expanded_predicate_factor) |
| Ibex | PR #31: `ibex_top / no_bp_prefetch_direct` | Accepted whole-core result | Whole-core `ibex_top`: area `108441.5040 -> 108373.9392`; WNS deltas `+13.7942/+13.7942/+13.7924/+0.3407/+0.1761ns`; toggle flat `311729 -> 311729`; Yosys equivalence `1956/1956`; hosted OSS-FV green; cold review passed | [`ibex-athanor` PR #31 top-level receipt](https://github.com/athanor-ai/ibex-athanor/blob/ea0e5bc50e2322369a5cee166161acadbda417f0/athanor_artifacts/if_stage_no_bp_prefetch_direct/top_level_first/top_level_first_receipt.json) |
| Ibex | `ibex_multdiv_slow / greater_equal_xor_shape` | Accepted module-level result | Module scope only: area `10339.9168 -> 10333.6608`; data arrival `8.13ns -> 7.25ns`; toggle flat `6117 -> 6117`; Yosys equivalence `411/411`; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_slow_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/multdiv_slow_greater_equal_xor_shape) |
| Ibex | `ibex_multdiv_fast / greater_equal_xor_shape` | Accepted module-level result | Module scope only: cell metric flat `3306 -> 3306`; max delay `10.85ns -> 10.57ns`; toggle flat `7657 -> 7657`; Yosys equivalence `772/772`; not a whole-core headline | [`ibex-athanor/athanor_artifacts/multdiv_fast_greater_equal_xor_shape`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/multdiv_fast_greater_equal_xor_shape) |
| Ibex | `ibex_fetch_fifo / err_unaligned_factored` | Accepted module-level result | Module scope only: generic cells `396 -> 395`; liberty cells `456 -> 451` (`-1.0965%`); timing flat at `6.32ns`; SAIF transition-count flat `34031 -> 34031`; relation-aware sequential miter closes; not a whole-core headline | [`ibex-athanor/athanor_artifacts/fetch_fifo_err_unaligned_factored`](https://github.com/athanor-ai/ibex-athanor/tree/master/athanor_artifacts/fetch_fifo_err_unaligned_factored) |

In short: Athanor currently has five accepted Ibex optimization artifacts,
including two whole-core results and three module-level results.

Do not claim five whole-core Ibex optimizations, an aggregate whole-core Ibex
percentage, or non-Ibex RISC-V impact until the relevant receipt exists.


## Rejected Tradeoffs

These rows explain candidates that looked promising on one evidence axis but
were rejected because another required axis regressed. They are not accepted
optimization artifacts.

| Core | Candidate | Why It Was Rejected | Evidence |
| --- | --- | --- | --- |
| CV32E40P | `cv32e40p_int_controller / reachable_irq_loop_encoder` | In the real controller plus interrupt-controller context, area improved `3365.728000 -> 3299.414400` (`-1.9702%`) and all selected-flow timing groups improved; equivalence proved for both checked `PULP_SECURE` settings and a bad-mutant control failed as expected. It was rejected because exercised toggle activity increased `338551 -> 359771` (`+6.267889%`), which is a power-proxy regression. | [`cv32e40p_int_controller_reachable_irq_loop_encoder_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_int_controller_reachable_irq_loop_encoder_tradeoff.json) |
| CV32E40P | `cv32e40p_load_store_unit / rotated_payload` | In the LSU EX-stage selected-flow parent context, area improved `5950.707200 -> 5512.787200` (`-7.359125%`) and the registered-output timing groups improved. It was rejected because required input timing regressed: in2out `-0.8415 -> -1.3570`, in2reg `1.4147 -> 1.0122`, and overall `-0.8415 -> -1.3570`. The candidate stopped before equivalence, activity, or replay spend. | [`cv32e40p_load_store_unit_rotated_payload_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_load_store_unit_rotated_payload_tradeoff.json) |
| CV32E40P | `cv32e40p_load_store_unit / data_sign_ext_zero_reduce` | In the LSU EX-stage selected-flow parent context, area improved `5950.707200 -> 5933.190400` (`-0.294365%`). It was rejected because all recorded timing groups regressed, including in2out `-0.8415 -> -1.7815` and overall `-0.8415 -> -1.7815`. The candidate stopped before equivalence, activity, or replay spend. | [`cv32e40p_load_store_unit_data_sign_ext_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_load_store_unit_data_sign_ext_tradeoff.json) |
| CV32E40P | `cv32e40p_aligner / held_misaligned32 + fetch_misaligned16 + branch_fetch_hi` | The aligner module itself improved area and timing, and the IF-stage parent context still reduced area `18228.732800 -> 18203.708800` (`-0.137278%`). It was rejected because the parent in2out timing group regressed `1.1039 -> 0.9067`; a module-local win did not clear the consuming-parent timing bar. The candidate stopped before equivalence, activity, or replay spend. | [`cv32e40p_aligner_eq11_if_stage_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_aligner_eq11_if_stage_tradeoff.json) |
| CV32E40P | `cv32e40p_fifo / push_pop_ok_factor` | The enclosing IF-stage selected-flow run reduced area `18228.732800 -> 18204.960000` (`-0.130414%`) and improved all five timing groups, while activity was flat `21739 -> 21739` and a source-local predicate proof closed with three biting negatives. It was rejected because the reusable prefetch-buffer scope regressed four local timing groups: in2out `2.6664 -> 2.6055`, in2reg `0.3554 -> 0.3546`, overall `-0.7369 -> -0.7736`, and reg2reg `-0.7369 -> -0.7736`. The standalone prefetch-buffer reuse caveat stops this from being promoted as an IF-stage-only win. | [`cv32e40p_fifo_push_pop_ok_factor_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_fifo_push_pop_ok_factor_tradeoff.json) |
| CV32E40P | `cv32e40p_mult / mulh_bits` | The MULH slice selected-flow run improved area `13207.667200 -> 13172.633600` (`-0.265252%`) and all five timing groups, and the EX-stage multiplier parent context still reduced area `77151.494400 -> 76960.060800` (`-0.248127%`). It was rejected because parent timing regressed in2reg `-5.7015 -> -6.7705`, reg2out `-7.6692 -> -8.8364`, and reg2reg `-5.9449 -> -7.0463`. The candidate stopped before equivalence, activity, or replay spend. | [`cv32e40p_mult_mulh_bits_tradeoff.json`](receipts/cv32e40p/rejections/cv32e40p_mult_mulh_bits_tradeoff.json) |

## Exploratory Work

These rows are reconnaissance, baselines, or candidates that still need more
evidence. They are not optimization claims. A **scout lead** or **preclear
candidate** is an early lead that has not yet passed the full evidence path.

| Core | Current state | Best current lead | Next honest gate |
| --- | --- | --- | --- |
| Ibex | Active public design with accepted receipts | Additional candidates are checked only when they have fresh evidence to add or reject | Either add a new accepted result with evidence, or record that a checked candidate did not pass |
| CV32E40P | Active exploration; no accepted optimization yet | Recent module-level discovery has produced both early positives and negatives, but no candidate has cleared selected-flow plus follow-on gates | Promote only a candidate that passes selected-flow area/timing, then formal equivalence, activity checks, and replay |
| PicoRV32 | Selected-flow baseline and residual replay exist | Replay found two residual rows; both were rejected cheaply (`0 == mem_wstrb` was formal/assertion-only, and `pcpi_rs1 - pcpi_rs2` was area neutral) | Treat as evidence that this search path did not transfer unless a new search method appears |
| SERV | Selected-flow baseline exists | Detector replay found zero candidates ready for deeper evaluation | Treat as evidence that this search path did not transfer |
| ultraembedded/riscv | Selected-flow baseline exists | Source-only constant-prop leads disappeared after selected-flow residual replay | Treat as evidence that this search path did not transfer |
| VexRiscv | Preflight only | Generator toolchain blocked locally; no pinned generated CPU RTL baseline | Provision/import a pinned generated RTL artifact before any detector or PPA work |

Machine-readable status lives in [`frontier-status.json`](frontier-status.json).
