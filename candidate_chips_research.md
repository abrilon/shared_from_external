# Candidate Analog Chip Research

Quick reference for the four chips named in the architecture doc. Verify specs against current datasheets before committing hardware spend.

## Mythic M1

- **Type:** Analog compute-in-memory (CIM) tiles using flash-based weights.
- **Claimed throughput:** 176 TOPS (INT8-class) at roughly 6W.
- **Why it fits Chip 2:** Dense matrix-vector work in the analog domain; good for the recurrent/associative core.
- **Watch-outs:** Flash cell endurance and weight-update granularity; analog drift over time; SDK maturity.
- **Links:** mythic.ai (product pages), Mythic M1 datasheet via sales.

## Syntiant NDP (Neural Decision Processor)

- **Type:** Ultra-low-power digital-ish neural accelerator with analog-ish front end; aimed at always-on wake/keyword workloads.
- **Power:** Microwatt-to-milliwatt class.
- **Why it fits Chip 3:** The always-on decision layer that should barely sip power.
- **Watch-outs:** Fixed model support; not a general-purpose analog compute fabric.
- **Links:** syntiant.com

## Aspinity AML (analogML)

- **Type:** Event-driven analog front end; processes signals in analog before any ADC.
- **Why it fits Chip 1:** Ingest raw streams without digitizing first — exactly the perception-layer job.
- **Watch-outs:** Narrow application focus; eval-board availability and toolchain access.
- **Links:** aspinity.com

## TetraMem MLX300

- **Type:** Analog in-memory compute using ReRAM crossbars; multi-layer stacking.
- **Why it fits Chips 1 and 2:** Crossbar arrays do matrix math natively; stacking gives depth for recurrent context.
- **Watch-outs:** ReRAM variability, sneak paths, and the training-to-hardware mapping problem (see Open Question 1 in the architecture doc).
- **Links:** tetramem.com

## Comparison Snapshot

| Chip | Domain | Power Class | Role Fit |
|---|---|---|---|
| Mythic M1 | Analog CIM | ~6W | Recurrent core |
| Syntiant NDP | Mixed / always-on | µW–mW | Decision layer |
| Aspinity AML | Analog front end | Low mW | Perception |
| TetraMem MLX300 | Analog CIM (ReRAM) | Low W | Perception + recurrent |

## Action Items

- [ ] Request eval boards / SDKs from Aspinity and TetraMem.
- [ ] Get Mythic M1 throughput-per-watt numbers on a matrix-vector microbenchmark.
- [ ] Confirm Syntiant NDP can run a small custom model, not just vendor keywords.
