# Roadmap - Yumi

Path to achieving "press-print-and-forget" BambuLab-level reliability.

---

## Current Progress

```
Phase 1: Foundation     [=========-] 90%
Phase 2: Motion Quality [==========] 100%
Phase 3: Extrusion      [=======---] 70%
Phase 4: Automation     [----------]  0%
Phase 5: Refinements    [----------]  0%

Overall Progress        [======----] ~55%
```

---

## Phase 1: Foundation

*Hardware stability and basic calibrations*

### Hardware
- [x] PEI magnetic bed
- [x] Linear rails (X and Y)
- [x] Z sensor (GL-8H inductive)
- [x] Upgraded leadscrew couplers
- [x] Anti-backlash nuts
- [x] Belt tension verification (2026-01-28)

### Software
- [x] Klipper installed and configured
- [x] Mainsail interface
- [x] KAMP adaptive meshing
- [x] Custom macros (purge, start/end)

### Calibrations
- [x] PID tuning (hotend + bed)
- [x] Z offset (1.107mm)
- [x] Bed mesh
- [x] E-steps / rotation distance (20.925mm)
- [x] Skew correction (0.00767)
- [x] Probe accuracy test (2026-01-28 - GL-8H: range 0.0125mm, std dev 0.0035mm)
- [ ] First layer test print

### Status: Near Complete
**Blockers:** None
**Next action:** First layer test print

---

## Phase 2: Motion Quality

*Input Shaper and motion optimization*

### Hardware
- [x] KUSBA v2.1 (ADXL345 via USB) mounted and working

### Calibrations
- [x] Input Shaper X axis (mzv @ 54.8 Hz, 2026-01-29)
- [x] Input Shaper Y axis (mzv @ 23.0 Hz, 2026-01-29)
- [x] Max velocity testing (200 mm/s, 2026-02-01)
- [x] Max acceleration testing (2100 mm/s², 2026-02-01)

### Results
- Ringing/ghosting eliminated
- Cleaner vertical surfaces
- max_accel: 2100 mm/s² (15% margin from 2500 tested)
- max_velocity: 200 mm/s

### Status: Complete
**Completed:** 2026-02-01

---

## Phase 3: Extrusion Perfection

*Per-filament tuning for dimensional accuracy*

### Calibrations (per filament type)
- [x] Pressure Advance (PLA: 0.065, 2026-02-01)
- [x] Flow rate (PLA: 0.94, 2026-02-01)
- [~] Retraction settings (PLA: temporary 1.5mm @ 35mm/s — redo with dry filament)
- [ ] Temperature tower
- [x] Max Volumetric Flow (PLA: 18 mm³/s configured, 2026-02-01)

### Filament Profiles
- [~] PLA (generic) — mostly done, retraction + temp tower pending
- [ ] PETG (generic)
- [ ] TPU (if used)

### Status: In Progress (~70%)
**Blockers:** Filament dryer needed for accurate retraction test
**Next action:** Temperature tower (PLA), then redo retraction with dry filament

---

## Phase 4: Automation & Monitoring

*Set-and-forget reliability features*

### Hardware
- [ ] Webcam installation
- [ ] LED lighting (for camera visibility)

### Software
- [ ] Telegram notifications (print start/end/error)
- [ ] Timelapse with Crowsnest
- [ ] Automated config backup script

### Features to Enable
- [ ] Print progress notifications
- [ ] Error alerts (thermal runaway, etc.)
- [ ] Remote monitoring via webcam
- [ ] Automatic nightly backups to repo

### Status: Not Started
**Blockers:** Webcam purchase
**Next action:** Research webcam options compatible with Crowsnest

---

## Phase 5: Refinements

*Polish and optimization*

### Potential Improvements
- [ ] Noise reduction analysis
- [ ] Speed optimization (if desired after reliability proven)
- [ ] Enclosure consideration (for ABS/ASA)
- [ ] Filament runout sensor
- [ ] Power loss recovery testing

### Evaluate Hardware
- [ ] BTT Microprobe V2 (if GL-8H proves unreliable)

### Status: Future
**Blockers:** Complete Phases 1-4 first

---

## Planned Hardware Upgrades (Priority Order)

| Priority | Item | Purpose | Status |
|----------|------|---------|--------|
| ~~1~~ | ~~ADXL345 mount~~ | ~~Input Shaper calibration~~ | ✅ Done (KUSBA v2.1) |
| 1 | Filament dryer | Accurate retraction calibration | Needed |
| 2 | Webcam | Monitoring + timelapse | Planned |
| 3 | LED lighting | Camera visibility | Planned |
| 4 | BTT Microprobe V2 | Better PEI probing accuracy | Not needed (GL-8H excellent) |

---

## Planned Software Features (Priority Order)

| Priority | Feature | Purpose | Dependency |
|----------|---------|---------|------------|
| ~~1~~ | ~~Input Shaper config~~ | ~~Motion quality~~ | ✅ Done |
| 1 | Telegram bot | Notifications | Moonraker config |
| 2 | Backup script | Config safety | None |
| 3 | Timelapse | Documentation | Webcam |

---

## Success Criteria

### Definition of "BambuLab Experience"

A print session is successful when:

1. **One-click start**: Slice -> Upload -> Print (no manual prep)
2. **Zero babysitting**: First layer works every time
3. **Consistent quality**: Same file = same result
4. **Silent operation**: Can run overnight without disturbance
5. **Remote awareness**: Know print status without being present

### Metrics to Track

| Metric | Target | Current |
|--------|--------|---------|
| First layer success rate | >95% | ~80% (estimate) |
| Print completion rate | >98% | ~90% (estimate) |
| Dimensional accuracy | ±0.2mm | Not measured |
| Noise level | <45dB | Not measured |

---

## Timeline

No time estimates provided - progress depends on availability.
Track progress via this document and CHANGELOG.md.

---

## Related Documentation

- [Calibration Checklist](./CALIBRATION_CHECKLIST.md) - Detailed procedures
- [Hardware Modifications](../hardware-notes/modifications.md) - Component status
- [Changelog](../CHANGELOG.md) - Modification history
