# OrcaSlicer Scripts Directory

This directory contains utility scripts and post-processing tools for OrcaSlicer.

## Post-Processing Scripts

### gcode_nonplanar_modulation.py
A comprehensive G-code post-processing script that adds non-planar modulation for enhanced 3D printing effects.

**Features:**
- Multiple wave functions (sine, triangle, trapezoidal, sawtooth)
- Configurable modulation for walls and infill
- Layer-aware amplitude scaling
- Support for all major slicers (PrusaSlicer, OrcaSlicer, BambuStudio)

**Usage:** See `README_gcode_nonplanar_modulation.md` for detailed documentation.

## Profile Management Scripts

### orca_extra_profile_check.py
Validates 3D printer profiles for consistency and checks for common configuration issues.

### orca_filament_lib.py
Utilities for managing filament profiles and libraries.

### generate_presets_vendors.py
Generates vendor preset configurations for different printer manufacturers.

### check_unused_setting_id.py (in resources/profiles/)
Identifies unused setting IDs in profile configurations.

## Other Utilities

### HintsToPot.py
Extracts hint messages for internationalization and localization.

### pack_profiles.sh
Shell script for packaging and organizing profile configurations.

## Usage

Most scripts can be run directly with Python 3:
```bash
python3 script_name.py [arguments]
```

For post-processing integration with OrcaSlicer, add the script path to the "Post-processing scripts" section in the Process Settings.