# Design Decisions Log

Living record of choices made for the analog-hybrid AGI architecture. Newest first.

## 2026-11-15 — Initial architecture committed

- **Decision:** Three analog chips (perception, recurrent, decision) plus one digital coprocessor.
- **Rationale:** Split by compute type matches the energy profile — analog for throughput, digital for control.
- **Rejected alternative:** Single monolithic analog array. Too hard to debug, no clean checkpointing story.
- **Open:** Interconnect protocol unspecified; see Open Question 2.

## Template for future entries

```
## YYYY-MM-DD — <short title>

- **Decision:**
- **Rationale:**
- **Rejected alternative:**
- **Open:**
```
