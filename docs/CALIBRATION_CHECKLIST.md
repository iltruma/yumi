# Calibration Checklist - Yumi

Complete calibration checklist for achieving BambuLab-level print quality and reliability.

## Status Legend

- `[x]` Completed
- `[ ]` Pending
- `[~]` Needs verification/redo

---

## Mechanical Calibrations

### Belt Tension Check
- **Status:** [ ] Pending
- **Priority:** High
- **Command:** Manual measurement
- **Procedure:**
  1. Power off printer
  2. Pluck each belt like a guitar string
  3. Both X and Y should have similar tension (low hum, ~100-110Hz)
  4. Adjust tensioners if needed
- **Expected:** Firm but not overtightened, similar frequency on both axes
- **Redo when:** After any work on X/Y mechanics, if print quality degrades

### Probe Accuracy Test
- **Status:** [ ] Pending
- **Priority:** High
- **Command:** `PROBE_ACCURACY`
- **Procedure:**
  1. Heat bed to printing temperature (60°C for PLA)
  2. Wait 10 minutes for thermal expansion to stabilize
  3. Run: `PROBE_ACCURACY SAMPLES=10`
- **Expected:** Range < 0.025mm, Standard deviation < 0.010mm
- **Current sensor:** GL-8H inductive
- **Notes:** If range > 0.05mm, consider BTT Microprobe V2
- **Redo when:** Sensor change, after crashes, if first layer inconsistent

---

## Temperature Calibrations

### PID Tuning - Hotend
- **Status:** [x] Completed
- **Command:** `PID_CALIBRATE HEATER=extruder TARGET=200`
- **Procedure:**
  1. Run PID calibration at typical printing temp
  2. Save with `SAVE_CONFIG`
- **Current values:**
  - Kp: 18.730
  - Ki: 0.821
  - Kd: 106.760
- **Expected:** Stable temperature within ±0.5°C
- **Redo when:** Hotend change, heater cartridge replacement

### PID Tuning - Bed
- **Status:** [x] Completed
- **Command:** `PID_CALIBRATE HEATER=heater_bed TARGET=60`
- **Procedure:**
  1. Run PID calibration at typical bed temp
  2. Save with `SAVE_CONFIG`
- **Current values:**
  - Kp: 39.046
  - Ki: 0.264
  - Kd: 1445.179
- **Expected:** Stable temperature within ±1°C
- **Redo when:** Bed heater replacement, major thermal system changes

---

## Z Calibrations

### Z Offset
- **Status:** [x] Completed
- **Command:** `PROBE_CALIBRATE` then `TESTZ`
- **Procedure:**
  1. Home all axes: `G28`
  2. Run: `PROBE_CALIBRATE`
  3. Use paper method with `TESTZ Z=-0.1` increments
  4. Save with `SAVE_CONFIG`
- **Current value:** 1.107mm
- **Expected:** First layer squish without scraping
- **Redo when:** Nozzle change, bed surface change, sensor adjustment

### Bed Mesh
- **Status:** [x] Completed
- **Command:** `BED_MESH_CALIBRATE`
- **Procedure:**
  1. Heat bed to printing temperature
  2. Wait 10 minutes for thermal expansion
  3. Run: `BED_MESH_CALIBRATE PROFILE=default`
  4. Save with `SAVE_CONFIG`
- **Expected:** Mesh variance < 0.3mm
- **Notes:** KAMP handles adaptive meshing per-print
- **Redo when:** After bed removal, leveling adjustments, or periodic maintenance

---

## Motion Calibrations

### Rotation Distance X/Y
- **Status:** [x] Completed (via Califlower)
- **Current value:** 40.115mm
- **Expected:** Movement matches commanded distance
- **Redo when:** Belt/pulley changes

### E-steps (Rotation Distance)
- **Status:** [x] Completed
- **Command:** Mark and extrude test
- **Procedure:**
  1. Heat hotend to 200°C
  2. Mark filament 120mm from extruder
  3. Extrude 100mm: `G1 E100 F100`
  4. Measure remaining distance
  5. Calculate: `new_rd = old_rd * (100 / actually_extruded)`
- **Current value:** 20.925mm
- **Expected:** 100mm commanded = 100mm extruded (±0.5mm)
- **Redo when:** Extruder gear change, gear wear

### Input Shaper
- **Status:** [ ] Pending
- **Priority:** HIGH - Biggest quality improvement
- **Command:** `SHAPER_CALIBRATE`
- **Requirements:**
  - ADXL345 accelerometer (owned, needs mounting)
- **Procedure:**
  1. Mount ADXL345 securely on toolhead
  2. Configure SPI connection in printer.cfg
  3. Run: `SHAPER_CALIBRATE AXIS=X`
  4. Run: `SHAPER_CALIBRATE AXIS=Y`
  5. Save with `SAVE_CONFIG`
- **Expected:** Recommended shaper type and frequency per axis
- **Benefits:** Eliminates ringing/ghosting, allows higher speeds
- **Redo when:** After major mechanical changes

### Max Velocity/Acceleration Test
- **Status:** [ ] Pending
- **Priority:** Low (reliability > speed)
- **Procedure:**
  1. Complete Input Shaper first
  2. Print speed test models increasing velocity
  3. Listen for layer shifts or quality degradation
  4. Set conservative limits below failure point
- **Redo when:** After Input Shaper, mechanical upgrades

---

## Extrusion Calibrations (Per Filament)

### Pressure Advance
- **Status:** [ ] Pending
- **Priority:** HIGH - Better corners and reduced stringing
- **Command:** `SET_PRESSURE_ADVANCE ADVANCE=X`
- **Procedure:**
  1. Print pressure advance tower
  2. Find line with sharpest corners
  3. Calculate PA value from line number
  4. Add to filament profile in slicer
- **Expected range:** 0.02-0.08 for direct drive
- **Notes:** Different for each filament type
- **Redo when:** Different filament, nozzle change

### Flow Rate
- **Status:** [ ] Pending
- **Priority:** Medium - Dimensional accuracy
- **Procedure:**
  1. Print single-wall cube (no infill)
  2. Measure wall thickness
  3. Adjust flow: `new_flow = (expected / measured) * current_flow`
- **Expected:** Wall thickness matches nozzle diameter
- **Notes:** Start at 100%, typical adjustment 95-98%
- **Redo when:** Different filament brand/type

### Retraction
- **Status:** [ ] Pending
- **Priority:** Medium - Stringing prevention
- **Procedure:**
  1. Print retraction tower
  2. Vary distance (0.5-2mm for direct drive)
  3. Vary speed (25-45mm/s)
  4. Find settings with minimal stringing
- **Expected range:** 0.8-1.5mm distance, 35mm/s speed (direct drive)
- **Redo when:** Different filament, bowden changes

### Temperature Tower
- **Status:** [ ] Pending
- **Priority:** Medium
- **Procedure:**
  1. Download/slice temperature tower model
  2. Add temperature changes at layer heights
  3. Print and evaluate each section for:
     - Layer adhesion
     - Stringing
     - Surface quality
     - Overhangs
- **Notes:** Required for each new filament
- **Redo when:** New filament brand/type/color

---

## Dimensional Calibrations

### Skew Calibration
- **Status:** [x] Completed
- **Command:** `SET_SKEW`
- **Procedure:**
  1. Print skew calibration square
  2. Measure diagonals AC and BD
  3. Calculate skew value
  4. Add to config: `SET_SKEW XY=value`
- **Current value:** xy_skew=0.00767
- **Expected:** Diagonals within 0.5mm of each other
- **Redo when:** Frame adjustments, mechanical changes

### First Layer Test
- **Status:** [ ] Pending
- **Procedure:**
  1. Print first layer test pattern
  2. Verify even squish across entire bed
  3. No gaps, no overextrusion
- **Expected:** Consistent lines, slight transparency
- **Redo when:** After Z offset or bed mesh changes

---

## Calibration Order (Recommended)

For new setup or major changes, follow this order:

1. **Mechanical checks** - Belt tension, frame square
2. **PID tuning** - Stable temperatures first
3. **Probe accuracy** - Verify sensor reliability
4. **Z offset** - Critical for first layer
5. **Bed mesh** - Compensate for bed irregularities
6. **E-steps** - Correct extrusion volume
7. **Input Shaper** - Motion quality
8. **Skew** - Dimensional accuracy
9. **Pressure Advance** - Per filament
10. **Flow rate** - Per filament
11. **Retraction** - Per filament
12. **Temperature tower** - Per filament

---

## Quick Reference Commands

```gcode
# PID tuning
PID_CALIBRATE HEATER=extruder TARGET=200
PID_CALIBRATE HEATER=heater_bed TARGET=60

# Probe accuracy (heat bed first!)
PROBE_ACCURACY SAMPLES=10

# Z offset calibration
PROBE_CALIBRATE

# Bed mesh
BED_MESH_CALIBRATE PROFILE=default

# Input shaper (with ADXL345)
SHAPER_CALIBRATE AXIS=X
SHAPER_CALIBRATE AXIS=Y

# Save all changes
SAVE_CONFIG
```

---

## Related Documentation

- [Klipper Config Reference](https://www.klipper3d.org/Config_Reference.html)
- [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/)
- [Pressure Advance Guide](https://www.klipper3d.org/Pressure_Advance.html)
- [Input Shaper Guide](https://www.klipper3d.org/Resonance_Compensation.html)
