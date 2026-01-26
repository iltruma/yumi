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
- [x] Linear rails
- [x] Inductive Z sensor (GL-8H)
- [ ] Evaluating BTT Microprobe V2

## Repository Structure

- `klipper/` - Klipper, Moonraker configs and macros
- `orca-slicer/` - Printer, filament, and process profiles
- `hardware-notes/` - Hardware modification notes

## License

MIT
