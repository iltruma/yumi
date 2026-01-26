# Yumi 弓

> **Yumi** (弓) is the traditional Japanese bow, made of bamboo.
> A weapon that bridges *Artillery* and *Bambu* — precision, elegance, balance.

Project to convert an **Artillery Sidewinder X1** into a reliable, precise, and silent 3D printer, inspired by the BambuLab user experience.

## Goals

- Dimensional accuracy and print quality
- "Press-print-and-forget" reliability
- Silent operation
- Easy maintenance

## Stack

- **Firmware:** Klipper on Raspberry Pi
- **Interface:** Mainsail/Fluidd
- **Slicer:** Orca Slicer

## Hardware Modifications

- [x] PEI magnetic bed
- [x] Linear rails (X and Y)
- [x] Inductive Z sensor (GL-8H)
- [ ] ADXL345 mount (for Input Shaper)
- [ ] BTT Microprobe V2 (evaluating)

## Roadmap to BambuLab Experience

```
Phase 1: Foundation     [=========-] 90%  <- Current
Phase 2: Motion Quality [----------]  0%
Phase 3: Extrusion      [==---------] 20%
Phase 4: Automation     [----------]  0%
Phase 5: Refinements    [----------]  0%
```

**Next milestones:**
1. Input Shaper calibration (ADXL345)
2. Pressure Advance tuning
3. Webcam + notifications

See [Roadmap](docs/ROADMAP.md) for details.

## Repository Structure

- `klipper/` - Klipper, Moonraker configs and macros
- `orca-slicer/` - Printer, filament, and process profiles
- `hardware-notes/` - Hardware modification notes
- `CHANGELOG.md` - Chronological log of all modifications

## License

MIT
