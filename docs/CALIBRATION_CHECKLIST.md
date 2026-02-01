# Calibration Checklist - Yumi

Complete calibration checklist for achieving BambuLab-level print quality and reliability.

## Status Legend

- `[x]` Completed
- `[ ]` Pending
- `[~]` Needs verification/redo

---

## Mechanical Calibrations

### Belt Tension Check
- **Status:** [x] Completed (2026-01-28)
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
- **Status:** [x] Completed (2026-01-28)
- **Priority:** High
- **Command:** `PROBE_ACCURACY`
- **Procedure:**
  1. Heat bed to printing temperature (60°C for PLA)
  2. Wait 10 minutes for thermal expansion to stabilize
  3. Run: `PROBE_ACCURACY SAMPLES=10`
- **Expected:** Range < 0.025mm, Standard deviation < 0.010mm
- **Results (2026-01-28):**
  - Maximum: 1.147000, Minimum: 1.134500
  - Range: 0.0125mm (excellent, well below 0.025mm threshold)
  - Average: 1.144000, Median: 1.144500
  - Standard deviation: 0.0035mm (excellent, well below 0.010mm threshold)
- **Current sensor:** GL-8H inductive
- **Notes:** GL-8H performs well on PEI surface, no need to replace
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
- **Status:** [x] Completed (2026-02-01 via Calistar)
- **Current value:** 39.920mm
- **Calibration method:** Calistar calibration cube
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
- **Current value:** 20.316mm (gear_ratio 66:22)
- **Expected:** 100mm commanded = 100mm extruded (±0.5mm)
- **Redo when:** Extruder gear change, gear wear

### Input Shaper
- **Status:** [x] Completed (2026-01-29)
- **Priority:** HIGH - Biggest quality improvement
- **Command:** `SHAPER_CALIBRATE`
- **Hardware:** KUSBA v2.1 (ADXL345 via USB)
- **Procedure:**
  1. Mount ADXL345 securely on toolhead
  2. Configure SPI connection in printer.cfg
  3. Run: `SHAPER_CALIBRATE AXIS=X`
  4. Run: `SHAPER_CALIBRATE AXIS=Y`
  5. Save with `SAVE_CONFIG`
- **Results (2026-01-29):**
  | Axis | Shaper | Frequency |
  |------|--------|-----------|
  | X | mzv | 54.8 Hz |
  | Y | mzv | 23.0 Hz |
  | Z | 3hump_ei | 77.6 Hz |
- **Recommended max_accel:** 1550 mm/s²
- **Notes:**
  - Y frequency is lower than X, likely due to bed mass on Y axis (bed slinger design)
  - Z shaper optional but configured
- **Benefits:** Eliminates ringing/ghosting, allows higher speeds
- **Redo when:** After major mechanical changes

### Max Velocity/Acceleration Test
- **Status:** [ ] Pending
- **Priority:** Low (reliability > speed)
- **Reference:** [Ellis Guide - Max Speeds](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_max_speeds_accels.html)
- **Procedure:**
  1. Complete Input Shaper first
  2. Set `max_velocity` to a known safe value
  3. Use `TEST_SPEED` macro increasing acceleration until skipping
  4. Run extended test (50 iterations) near the limit
  5. Set final value 15% below maximum found
  6. Repeat process for velocity using Prusa calculator
- **Notes:**
  - Heat printer fully before testing
  - Maximum values may not be practical for daily printing
  - Input Shaper already provides recommended max_accel
- **Redo when:** After Input Shaper, mechanical upgrades

---

## Extrusion Calibrations (Per Filament)

### Pressure Advance
- **Status:** [ ] Pending
- **Priority:** HIGH - Better corners and reduced stringing
- **Reference:** [Ellis Guide - Pressure Advance](https://ellis3dp.com/Print-Tuning-Guide/articles/pressure_linear_advance/introduction.html)
- **Command:** `SET_PRESSURE_ADVANCE ADVANCE=X`
- **Methods (in order of recommendation):**
  1. **Pattern Method** (most accurate) - Print test pattern, measure best section
  2. **Tower Method** (easier) - Print tower with varying PA values
  3. ~~Lines Method~~ (deprecated)
- **Procedure (Pattern Method):**
  1. Set starting PA to 0
  2. Print PA pattern from Klipper docs or Ellis guide
  3. Find section with sharpest corners, no bulging
  4. Calculate PA from line number
  5. Test with `SET_PRESSURE_ADVANCE ADVANCE=X`
  6. Add to printer.cfg or filament profile in slicer
- **Expected range:** 0.02-0.08 for direct drive, 0.4-1.0 for bowden
- **What PA fixes:**
  - Bulging at corners (PA too low)
  - Gaps at corners (PA too high)
  - Inconsistent line width during speed changes
- **Factors affecting PA:** Filament type, nozzle size, temperature, input shaper
- **Notes:** Different for each filament type, save per-filament in slicer
- **Redo when:** Different filament, nozzle change, hotend change

### Extrusion Multiplier (Flow Rate)
- **Status:** [ ] Pending
- **Priority:** HIGH - Dimensional accuracy and surface quality
- **Reference:** [Ellis Guide - Extrusion Multiplier](https://ellis3dp.com/Print-Tuning-Guide/articles/extrusion_multiplier.html)
- **Procedure (Ellis Method):**
  1. Slice 30x30x3mm cubes with EM variations of 1-2% (e.g., 0.96, 0.98, 1.00)
  2. Settings:
     - Infill: 30%+ at moderate speed
     - Top layers: maximum possible with 2+ solid infill layers below
     - Top pattern: "Monotonic" (SuperSlicer) or "Lines" (Cura)
     - Top solid speed: low/moderate (~60mm/s)
     - Fan: moderate-high
  3. Print all cubes together
  4. Evaluate top surface by touch and sight
  5. Correct EM: surface feels smooth, no gaps between lines
  6. If needed, repeat with 0.5% increments
- **Evaluation tips:**
  - Inspect CENTER of cubes, not edges
  - "A bit too high is better than a bit too low"
  - Look for: gaps (too low), rough/bumpy (too high)
- **Expected:** Smooth top surface, typical final value 0.95-0.98
- **Notes:** Different for each filament brand/type/color
- **Redo when:** Different filament brand/type

### Retraction
- **Status:** [ ] Pending
- **Priority:** Medium - Stringing prevention
- **Reference:** [Ellis Guide - Retraction](https://ellis3dp.com/Print-Tuning-Guide/articles/retraction.html)
- **Starting values:**
  - Direct drive: 0.5mm @ 35mm/s
  - Bowden: 1.0mm @ 35mm/s
- **Procedure (Ellis Method):**
  1. Clean nozzle thoroughly
  2. Set fan to 80-100%
  3. Use SuperSlicer "extruder retraction calibration" or similar tower
  4. Configure test:
     - Start temp: 10°C higher than normal
     - Increment: 0.1mm (direct) or 0.5mm (bowden)
     - Print 3 towers at different temps (10°C apart)
  5. Arrange towers from hottest to coldest on bed
  6. Print and analyze
- **Evaluation:**
  - Count rings from base where stringing stops
  - Choose value 1-2 rings ABOVE where stringing disappears (safety margin)
- **Expected range:**
  - Direct drive: 0.5-1.5mm distance, 30-35mm/s speed
  - Bowden: 1.0-6.0mm distance, 30-35mm/s speed
- **Notes:**
  - Temperature affects stringing significantly
  - Manage in slicer for per-filament control
- **Redo when:** Different filament, bowden tube changes

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

### Maximum Volumetric Flow Rate
- **Status:** [ ] Pending
- **Priority:** HIGH - Prevents under-extrusion at high speeds
- **Reference:** [Ellis Guide - Max Volumetric Flow](https://ellis3dp.com/Print-Tuning-Guide/articles/determining_max_volumetric_flow_rate.html)
- **What it is:** Maximum plastic volume (mm³/s) your hotend can melt
- **Why it matters:** Exceeding this limit causes under-extrusion, grinding, skipping
- **Procedure:**
  1. Heat hotend to printing temperature
  2. Mark 100mm of filament with tape
  3. Extrude at increasing speeds (mm/min = mm/s × 60)
  4. At each speed, verify ~100mm enters extruder
  5. Note speed where it starts falling short
  6. Convert to volumetric: `mm³/s = mm/s × 2.4` (for 1.75mm filament)
- **Typical values:**
  - Stock hotend (V6 style): 8-12 mm³/s
  - High-flow hotend (Volcano, Dragon HF): 15-25 mm³/s
  - CHT nozzle: +30-50% over standard
- **How to use:**
  - Set in slicer as "Max volumetric speed" limit
  - Or calculate max print speed: `speed = volumetric_flow / (layer_height × line_width)`
- **Notes:**
  - Higher temps = higher flow (with tradeoffs)
  - Use value slightly below test results
- **Redo when:** Hotend change, nozzle change, different filament type

---

## Cooling Calibrations

### Cooling and Layer Times
- **Status:** [ ] Pending
- **Priority:** Medium - Prevents overheating on small parts
- **Reference:** [Ellis Guide - Cooling](https://ellis3dp.com/Print-Tuning-Guide/articles/cooling_and_layer_times.html)
- **Signs of overheating:**
  - Curling/warping on small features
  - Poor overhangs
  - Blobby surfaces on detailed areas
- **Solutions:**
  1. **Increase fan speed:**
     - PLA: "more is better" - 100% usually fine
     - ABS: needs cooling even in heated chamber
     - PETG: moderate (50-70%)
  2. **Minimum layer time:**
     - Set in slicer (usually 10-15 seconds)
     - Slows down print on small layers
     - ABS: minimum 15 seconds recommended
  3. **Print multiple objects:**
     - Spread across bed
     - Gives each object time to cool between layers
- **Slicer settings:**
  - "Minimum layer time" or "Slow down if layer print time is below"
  - "Lift head" option for very small layers
- **Notes:**
  - Keep fan speed consistent to avoid banding
  - Small/tall objects need more cooling time
- **Redo when:** Different filament type, chamber temperature changes

---

## Slicer Calibrations

### Infill/Perimeter Overlap
- **Status:** [ ] Pending
- **Priority:** Low - Cosmetic improvement
- **Reference:** [Ellis Guide - Overlap](https://ellis3dp.com/Print-Tuning-Guide/articles/infill_perimeter_overlap.html)
- **Problem:** Small gaps/pinholes where top infill meets perimeters
- **Solutions:**
  1. **Preferred:** Use "Not connected" top infill (SuperSlicer)
     - Works with default 25% overlap
  2. **Alternative:** Increase overlap setting
     - PrusaSlicer: "Infill/perimeter overlap"
     - SuperSlicer: "Infill/perimeters encroachment"
     - Increase until pinholes disappear (~40%)
- **Notes:**
  - This is cosmetic, not a flow/PA calibration issue
  - Different slicers handle this differently
- **Redo when:** Slicer change, major profile changes

### First Layer Settings
- **Status:** [ ] Pending
- **Priority:** Medium - Bed adhesion and print success
- **Typical settings:**
  - First layer height: 0.2-0.3mm (or 75% of nozzle)
  - First layer speed: 20-30mm/s
  - First layer flow: 100-105%
  - First layer line width: 120% of nozzle
- **Procedure:**
  1. Print first layer test pattern
  2. Verify even squish across entire bed
  3. Lines should be slightly transparent, well adhered
  4. No gaps, no elephant foot
- **Notes:**
  - Adjust Z offset for fine-tuning, not flow
  - Bed cleanliness is critical (IPA 90%+)
- **Redo when:** After Z offset or bed mesh changes

---

## Dimensional Calibrations

### Skew Calibration
- **Status:** [x] Completed (2026-02-01 via Calistar)
- **Command:** `SET_SKEW`
- **Procedure:**
  1. Print skew calibration square
  2. Measure diagonals AC and BD
  3. Calculate skew value
  4. Add to config: `SET_SKEW XY=value`
- **Current value:** xy_skew=0.00787409559382246
- **Calibration method:** Calistar calibration cube
- **Expected:** Diagonals within 0.5mm of each other
- **Redo when:** Frame adjustments, mechanical changes

---

## Calibration Order (Recommended)

For new setup or major changes, follow this order:

### Phase 1: Machine Setup (One-time)
1. **Mechanical checks** - Belt tension, frame square
2. **PID tuning** - Stable temperatures first
3. **Probe accuracy** - Verify sensor reliability
4. **Z offset** - Critical for first layer
5. **Bed mesh** - Compensate for bed irregularities
6. **E-steps** - Correct extrusion volume
7. **Input Shaper** - Motion quality
8. **Skew** - Dimensional accuracy
9. **Max Volumetric Flow** - Know your hotend's limit

### Phase 2: Per-Filament Tuning
10. **Temperature tower** - Find optimal temp range
11. **Pressure Advance** - Sharp corners, consistent extrusion
12. **Extrusion Multiplier** - Dimensional accuracy
13. **Retraction** - Eliminate stringing
14. **Cooling settings** - Prevent overheating

### Phase 3: Fine-tuning (Optional)
15. **Max velocity/accel test** - For speed optimization
16. **First layer calibration** - Perfect adhesion
17. **Infill/perimeter overlap** - Cosmetic improvements

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

# Pressure Advance test
SET_VELOCITY_LIMIT SQUARE_CORNER_VELOCITY=1 ACCEL=500
SET_PRESSURE_ADVANCE ADVANCE=0
# Then print PA pattern and find best value
SET_PRESSURE_ADVANCE ADVANCE=0.04  # Example value

# Test extrusion for max volumetric flow
G1 E100 F300  # 5mm/s - start slow
G1 E100 F600  # 10mm/s - increase gradually

# Save all changes
SAVE_CONFIG
```

## Useful Test Models

- **Pressure Advance:** [Klipper PA Pattern](https://www.klipper3d.org/Pressure_Advance.html)
- **Retraction:** SuperSlicer built-in tower or [Retraction Calibration](https://www.thingiverse.com/thing:2563909)
- **Temperature Tower:** [Smart Temperature Tower](https://www.thingiverse.com/thing:2729076)
- **Flow/EM Test:** 30x30x3mm cube with top surface inspection
- **First Layer:** [First Layer Test](https://www.thingiverse.com/thing:2187071)

---

## Related Documentation

- [Klipper Config Reference](https://www.klipper3d.org/Config_Reference.html)
- [Ellis' Print Tuning Guide](https://ellis3dp.com/Print-Tuning-Guide/)
- [Pressure Advance Guide](https://www.klipper3d.org/Pressure_Advance.html)
- [Input Shaper Guide](https://www.klipper3d.org/Resonance_Compensation.html)
