# Post-Processing Scripts

Here you can set up post-processing scripts that will be executed after slicing. This allows you to modify the G-code output or perform additional tasks.

## Built-in Scripts

OrcaSlicer includes several post-processing scripts in the `scripts/` directory:

### G-code Non-Planar Modulation
Location: `scripts/gcode_nonplanar_modulation.py`

Adds wave-based non-planar modulation to G-code for enhanced visual effects and improved layer adhesion. Supports multiple wave functions (sine, triangle, trapezoidal, sawtooth) and can be applied to walls, infill, or both.

**Example usage:**
```
python3 scripts/gcode_nonplanar_modulation.py [output_file_placeholder] -include-perimeters -wall-amplitude 0.2 -wall-function sine
```

See `scripts/README_gcode_nonplanar_modulation.md` for complete documentation.

## Custom Scripts

You can also add your own custom post-processing scripts by specifying the full path to the script executable.
