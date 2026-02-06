# Yumi 弓

> [!IMPORTANT]
> **This project is no longer maintained.** Development stopped in February 2026. The repository is kept as-is for reference.

> **Yumi** (弓) is the traditional Japanese bow, made of bamboo.
> A weapon that bridges *Artillery* and *Bambu* — precision, elegance, balance.

Project to convert an **Artillery Sidewinder X1** into a reliable, precise, and silent 3D printer, inspired by the BambuLab user experience.

## Status

Project discontinued. Calibration reached ~75% completion.

**What was completed:**
- PID tuning, Z-offset, Bed Mesh, E-steps, Skew correction
- Input Shaper (X: mzv 54.8Hz, Y: mzv 23.0Hz)
- Max velocity (200 mm/s) and acceleration (2100 mm/s²)
- Pressure Advance (PLA: 0.065), Flow rate (PLA: 0.94)
- Max Volumetric Flow (PLA: 18 mm³/s)

**What was not completed:**
- Retraction (needs redo with dry filament)
- Temperature Tower, Cooling settings
- PETG calibration
- Automation (webcam, notifications, timelapse)

## Stack

- **Firmware:** Klipper on Raspberry Pi
- **Interface:** Mainsail
- **Slicer:** Orca Slicer

## Hardware Modifications

- PEI magnetic bed
- Linear rails (X and Y)
- Inductive Z sensor (GL-8H)
- KUSBA v2.1 (ADXL345 via USB)
- Upgraded leadscrew couplers + anti-backlash nuts

## Repository Structure

- `klipper/` - Klipper, Moonraker configs and macros
- `orca-slicer/` - Printer, filament, and process profiles
- `hardware-notes/` - Hardware modification notes
- `docs/` - Calibration checklist and roadmap
- `CHANGELOG.md` - Chronological log of all modifications

## License

MIT
