# Hardware Modifications - Yumi (Sidewinder X1)

## Current State

### PEI Magnetic Bed
- **Status:** Installed
- **Notes:** Works well, requires proper Z offset calibration

### Linear Rails
- **Status:** Installed
- **Axes:** X and Y
- **Notes:** Improved motion quality and reduced play

### Z Sensor
- **Current:** GL-8H (inductive)
- **Evaluating:** BTT Microprobe V2
- **Z Offset:** 1.107 (calibrated)
- **Notes:** Some inconsistency with PEI surface. Inductive sensors can have temperature-related variations and may not trigger consistently on non-metallic surfaces. Microprobe V2 being considered for better accuracy.

### Extruder
- **Type:** Stock Artillery
- **Gear ratio:** 66:22
- **Rotation distance:** 20.925 (calibrated)
- **Notes:** Original extruder, working well

### Stepper Drivers
- **Type:** TMC2208
- **Mode:** Standalone (hardware configuration via jumpers)
- **Notes:** Silent operation, no UART control

### Z Axis Upgrades
- **Leadscrew couplers:** Upgraded flexible couplers (TH3D style)
- **Anti-backlash nuts:** Installed
- **Notes:** Reduces Z wobble and eliminates backlash for better layer consistency

### Webcam
- **Status:** Planned
- **Notes:** Crowsnest pre-configured for future installation

## Software Configuration

### KAMP (Klipper Adaptive Meshing & Purging)
- **Adaptive Mesh:** Enabled - meshes only the print area
- **Purge Line:** Custom macro (not using KAMP purge)
- **Notes:** Adaptive meshing reduces probing time for smaller prints

### Calibrations
- **Rotation distance X/Y:** 40.115 (calibrated via Califlower)
- **PID Hotend:** Kp=18.730, Ki=0.821, Kd=106.760
- **PID Bed:** Kp=39.046, Ki=0.264, Kd=1445.179
- **Skew correction:** Enabled (xy_skew=0.00767)

## Planned Modifications

- [ ] Evaluate BTT Microprobe V2 for better Z probing accuracy with PEI
- [ ] Add webcam for monitoring and timelapse

## References

- [Klipper Docs - Probe](https://www.klipper3d.org/Probe_Calibrate.html)
- [BTT Microprobe V2](https://github.com/bigtreetech/MicroProbe)
- [KAMP](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging)
