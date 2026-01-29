# Changelog

All notable hardware and software modifications to Yumi are documented here.

## [Unreleased]
<!-- Add new modifications here before they are committed -->

## 2026-01-29 - Input Shaper Calibration

### Hardware
- KUSBA v2.1 accelerometer mounted on toolhead (ADXL345 via USB)

### Calibrations
- Input Shaper completed for all axes:
  - X: mzv @ 54.8 Hz
  - Y: mzv @ 23.0 Hz
  - Z: 3hump_ei @ 77.6 Hz
- max_accel updated: 1550 mm/s² (was 1000)
- max_z_accel updated: 1550 mm/s² (was 100)

### Notes
- Y frequency lower than X due to bed mass (bed slinger design)
- Ringing/ghosting should be significantly reduced
- Ready for Pressure Advance calibration next

## 2026-01-28 - Belt Tensioning

### Hardware
- Tensioned all belts: X, Y, and Z axes

### Calibrations
- Probe Accuracy test: PASSED (range 0.0125mm, std dev 0.0035mm)
  - GL-8H inductive sensor confirmed reliable on PEI surface

### Notes
- Belt tension was the last pending hardware item in Phase 1
- Rotation Distance X/Y (40.115mm) and Skew (0.00767) should be re-verified after belt changes
- Input Shaper calibration (Phase 2) should be done after this mechanical change

## 2026-01-26 - Documentation & Roadmap

### Software
- Added `docs/CALIBRATION_CHECKLIST.md` - Complete calibration procedures and status
- Added `docs/ROADMAP.md` - Phased plan to BambuLab experience
- Updated `hardware-notes/modifications.md` with calibration status table
- Updated `README.md` with roadmap progress
- Updated `CLAUDE.md` with docs links and calibration status

### Notes
- Calibration ~45% complete (PID, E-steps, Z-offset, Bed Mesh, Skew done)
- Priority next: Input Shaper (needs ADXL345 mount), Probe Accuracy test

---

## 2026-01-26 - Initial Setup

### Hardware
- PEI magnetic bed installed
- Linear rails on X and Y axes
- Z sensor: GL-8H inductive probe
- Z axis: upgraded leadscrew couplers (TH3D style) + anti-backlash nuts

### Software
- Klipper installed on Raspberry Pi
- Mainsail web interface
- KAMP adaptive meshing enabled
- Custom purge line macro
- Calibrations: rotation distance, PID, skew correction

---

## Format

```
## YYYY-MM-DD - Brief description

### Hardware
- What was added/changed/removed

### Software
- Config changes, new macros, calibrations

### Notes
- Any relevant observations or issues
```
