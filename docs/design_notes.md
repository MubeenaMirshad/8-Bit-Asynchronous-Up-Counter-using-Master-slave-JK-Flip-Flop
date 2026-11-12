# Design Notes

## Project

8-bit asynchronous up counter using master–slave JK flip-flops.

## Master–slave JK flip-flop

The flip-flop is implemented using NAND-gate logic, with 2-input and 3-input NAND gates.

Items to verify from the original schematic:
- NAND-gate count and interconnections
- Master and slave latch connections
- Clock and complementary-clock polarity
- Active triggering edge
- J and K input connections

## Counter architecture

Items to verify from the complete counter schematic:
- External clock connection
- Inter-stage clock connection for each stage
- Whether Q or Q-bar drives the next stage
- Output-bit order (LSB to MSB)
- Reset or initial-state mechanism

## Verification checklist

- [ ] Confirm the output sequence starts from the intended initial state.
- [ ] Confirm the sequence increments by one at each count event.
- [ ] Confirm the counter wraps from 11111111 to 00000000.
- [ ] Confirm output frequency division across stages.
- [ ] Record propagation delay and any transient glitches.
- [ ] Record the exact Cadence Virtuoso simulation settings.
