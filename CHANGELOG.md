# Changelog

All notable hardware and software modifications to Yumi are documented here.

## [Unreleased]
<!-- Add new modifications here before they are committed -->

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
