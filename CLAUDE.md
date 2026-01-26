# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Yumi** (弓) - the Japanese bow, made of bamboo. The name bridges *Artillery* (the weapon) and *Bambu* (the material): precision, elegance, balance.

Project to convert an Artillery Sidewinder X1 into a reliable, precise, and silent 3D printer, inspired by the BambuLab experience (A1 mini / P1S).

**Main goals:**
- Dimensional accuracy and print quality
- "Press-print-and-forget" reliability
- Silent operation
- Extreme speed is NOT a priority

## Hardware Setup

**Base printer:** Artillery Sidewinder X1

**Modifications:**
- PEI magnetic bed
- Linear rails
- Inductive Z sensor GL-8H (evaluating BTT Microprobe V2)

**Controller:** Raspberry Pi with Klipper

## Repository Structure

```
yumi/
├── klipper/           # Klipper configurations
│   ├── printer.cfg    # Main printer config
│   ├── moonraker.cfg  # Moonraker config
│   ├── macros/        # Custom G-code macros
│   └── scripts/       # Utility scripts
├── orca-slicer/       # Orca Slicer profiles
│   ├── printer/       # Printer profiles
│   ├── filament/      # Filament profiles
│   └── process/       # Print process profiles
└── hardware-notes/    # Hardware modification notes
```

## Development Guidelines

### Klipper Configs
- Always comment sections explaining the "why"
- Use conservative values for accelerations and speeds (priority: reliability)
- Separate macros into dedicated files by category
- Always test on dry-run before applying critical changes

### Security
- **ALWAYS check for sensitive information before committing:**
  - Passwords, tokens, API keys
  - Private IP addresses (specific, not RFC1918 ranges)
  - SSH keys or credentials
  - Scan configs with: `grep -ri "password\|token\|api_key\|secret" klipper/config/`
- Never commit files containing secrets or credentials

### Orca Slicer Profiles
- Name profiles with `yumi_` prefix to distinguish them
- Document differences from stock profiles
- Filament profiles must include tested temperatures

### Conventions
- Language: English for all documentation, code, and configs
- Chat/output with user: Italian
- Units: mm for dimensions, mm/s for speed, °C for temperatures
- Commit messages: English, descriptive

## Common Tasks

```bash
# Validate printer.cfg syntax (run on Raspberry)
klipper/scripts/validate-config.sh

# Backup configs from Raspberry
klipper/scripts/backup-from-pi.sh
```

## Notes

- GL-8H inductive sensor requires specific offset for PEI
- Linear rails may require recalibration after installation
