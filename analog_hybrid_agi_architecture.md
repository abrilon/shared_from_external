# Analog-Hybrid AGI Compute Architecture

## Core Concept

A 3-analog-chip front end feeding a digital coprocessor. Analog compute handles the bulk of matrix math and signal processing at near-zero energy cost per operation; the digital coprocessor handles sequencing, control flow, memory management, and anything requiring exact arithmetic or branching logic.

This mirrors biological neural tissue: analog for the heavy lifting, sparse digital events for coordination.

## The Three Analog Chips

### Chip 1 — Perception / Front-End Analog Array
- Continuous-time analog matrix-vector multiplication for early feature extraction.
- Subthreshold or charge-domain circuits for ultra-low power.
- Designed to ingest raw sensor or data streams without digitizing first.
- Candidate silicon: **Aspinity AML** (analogML, event-driven analog front end), **TetraMem MLX300** (analog in-memory compute, ReRAM crossbars).

### Chip 2 — Associative / Recurrent Analog Core
- Recurrent connections implemented as analog feedback loops rather than clocked SRAM.
- Holds working memory and temporal context in the analog domain.
- Candidate silicon: **TetraMem MLX300** (multi-layer analog crossbar stacks), **Mythic M1** (analog compute-in-memory tiles, 176 TOPS at ~6W).

### Chip 3 — Output / Decision Analog Layer
- Analog scoring and softmax-like competition circuits.
- Produces continuous confidence values rather than hard classifications.
- Candidate silicon: **Syntiant NDP** (ultra-low-power neural decision processor, wake-word class devices) for the always-on decision path.

## Digital Coprocessor

- Standard RISC-V or small ARM core.
- Owns: instruction sequencing, weight loading orchestration, inter-chip communication protocol, error correction, and any task needing bit-exact results.
- Acts as the "conductor" — the analog chips are the orchestra.
- Keeps a compact digital shadow of critical state for checkpointing and recovery.

## Interconnect

- Analog chips communicate via mixed-signal links: current-mode or voltage-mode signaling between crossbars, with digital sidebands for sync and addressing.
- Avoid full digitization between stages. Only promote to digital when the coprocessor explicitly requests it.

## Power Budget (target)

| Stage | Est. Power |
|---|---|
| Analog front end | < 50 mW |
| Analog recurrent core | < 200 mW |
| Analog decision layer | < 20 mW |
| Digital coprocessor | < 500 mW |
| **Total** | **< 1 W** |

Compare: a GPU doing equivalent throughput burns 300W+. This is the pitch — three orders of magnitude.

## Open Questions

1. How do we train weights that map cleanly onto analog crossbar non-idealities (drift, noise, limited precision)?
2. What's the right protocol for the digital coprocessor to "query" an analog state without collapsing it?
3. Can we do online learning in the analog domain, or must all plasticity route through the digital side?
4. Thermal stability of the analog arrays over long deployments.

## Next Steps

- [ ] Prototype Chip 1 on Aspinity AML eval board.
- [ ] Benchmark Mythic M1 vs TetraMem MLX300 for the recurrent core workload.
- [ ] Define the mixed-signal interconnect spec.
- [ ] Write the digital coprocessor ISA.
