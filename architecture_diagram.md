# Architecture Diagram

ASCII overview of the analog-hybrid AGI compute design. For a rendered figure, export this as SVG or draw it in Excalidraw / diagrams.net.

## System View

```
+---------------------------------------------------------------+
|                     DIGITAL COPRESSOR                         |
|   RISC-V / small ARM                                          |
|   - instruction sequencing                                    |
|   - weight loading orchestration                              |
|   - inter-chip protocol + addressing                          |
|   - error correction                                          |
|   - bit-exact tasks                                           |
|   - digital shadow state (checkpointing)                      |
+---------------------------+-----------------------------------+
                            | mixed-signal sidebands
                            | (sync, addressing, control)
   +------------------------+------------------------+
   |                        |                        |
   v                        v                        v
+------------------+ +------------------+ +------------------+
|  CHIP 1          | |  CHIP 2          | |  CHIP 3          |
|  PERCEPTION      | |  RECURRENT CORE  | |  DECISION LAYER  |
|  Front-end       | |  Associative     | |  Output /        |
|  analog array    | |  memory          | |  scoring         |
+------------------+ +------------------+ +------------------+
   ^                        ^                        |
   | raw sensor / data      | analog feedback        | continuous
   | streams (no ADC)       | loops (no SRAM)       | confidence
   +------------------------+------------------------+
```

## Data Flow

1. **Raw input** enters Chip 1 directly in analog. No digitization at the boundary.
2. Chip 1's matrix-vector results feed Chip 2's recurrent crossbars as analog currents/voltages.
3. Chip 2's feedback loops hold temporal context; its output drives Chip 3.
4. Chip 3 produces continuous confidence scores (softmax-like competition), not hard labels.
5. The coprocessor monitors via sidebands and only pulls a value into digital when it needs an exact number or a branch decision.
6. Weight updates and training signals flow the opposite direction: coprocessor -> chips.

## Power Flow (target)

```
Chip 1  < 50 mW
Chip 2  < 200 mW
Chip 3  < 20 mW
Coprocessor < 500 mW
-------------------
Total   < 1 W
```

## Interconnect Detail

- **Analog domain:** current-mode or voltage-mode links between crossbars. Signal stays analog end-to-end inside the chip trio.
- **Digital sidebands:** low-speed, for sync, chip addressing, and command packets.
- **Promotion rule:** analog -> digital only on explicit coprocessor request.

## Suggested Visual Layout (for Excalidraw)

- Three analog chips as rounded rectangles in a horizontal row, Chip 1 left, Chip 3 right.
- Digital coprocessor as a taller rectangle above the row, centered.
- Solid arrows: analog data flowing left -> right through the chips.
- Dashed arrows: sideband control from coprocessor down to each chip.
- A thin arrow from Chip 3 back up to the coprocessor labeled "confidence scores".
- Power budget callout box in the bottom-left corner.
