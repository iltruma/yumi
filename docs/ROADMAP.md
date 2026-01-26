# Roadmap - Yumi

Path to achieving "press-print-and-forget" BambuLab-level reliability.

---

## Current Progress

```
Phase 1: Foundation     [=========-] 90%
Phase 2: Motion Quality [----------]  0%
Phase 3: Extrusion      [==---------] 20%
Phase 4: Automation     [----------]  0%
Phase 5: Refinements    [----------]  0%

Overall Progress        [===-------] ~25%
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
- [ ] Belt tension verification

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
- [ ] Probe accuracy test
- [ ] First layer test print

### Status: Near Complete
**Blockers:** None
**Next action:** Probe accuracy test to verify GL-8H reliability

---

## Phase 2: Motion Quality

*Input Shaper and motion optimization*

### Hardware Required
- [ ] Mount ADXL345 accelerometer (owned)

### Calibrations
- [ ] Input Shaper X axis
- [ ] Input Shaper Y axis
- [ ] Max velocity testing
- [ ] Max acceleration testing

### Expected Benefits
- Elimination of ringing/ghosting
- Cleaner vertical surfaces
- Foundation for higher speeds (if desired)

### Status: Not Started
**Blockers:** ADXL345 needs mounting bracket
**Next action:** Design/print ADXL345 mount for toolhead

---

## Phase 3: Extrusion Perfection

*Per-filament tuning for dimensional accuracy*

### Calibrations (per filament type)
- [ ] Pressure Advance
- [ ] Flow rate
- [ ] Retraction settings
- [ ] Temperature tower

### Filament Profiles to Create
- [ ] PLA (generic)
- [ ] PETG (generic)
- [ ] TPU (if used)

### Expected Benefits
- Sharp corners without bulging
- Accurate dimensions
- No stringing
- Optimal layer adhesion

### Status: Not Started
**Blockers:** Depends on Phase 2 for clean results
**Next action:** Print PA tower after Input Shaper complete

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
| 1 | ADXL345 mount | Input Shaper calibration | Owned, needs mount |
| 2 | BTT Microprobe V2 | Better PEI probing accuracy | Evaluating need |
| 3 | Webcam | Monitoring + timelapse | Planned |
| 4 | LED lighting | Camera visibility | Planned |

---

## Planned Software Features (Priority Order)

| Priority | Feature | Purpose | Dependency |
|----------|---------|---------|------------|
| 1 | Input Shaper config | Motion quality | ADXL345 mount |
| 2 | Telegram bot | Notifications | Moonraker config |
| 3 | Backup script | Config safety | None |
| 4 | Timelapse | Documentation | Webcam |

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
