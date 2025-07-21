# G-code Non-Planar Modulation Script

This Python script adds non-planar modulation to G-code files for 3D printing, creating wave-based variations in the Z-axis to produce enhanced visual effects and improved layer adhesion.

## Features

- **Multiple Wave Functions**: sine, triangle, trapezoidal, and sawtooth waves
- **Slicer Support**: Compatible with PrusaSlicer, OrcaSlicer, and BambuStudio
- **Selective Modulation**: Apply to infill, internal perimeters, external perimeters, or any combination
- **Layer-Aware Scaling**: Automatically adjusts amplitude based on proximity to solid infill layers
- **Configurable Parameters**: Extensive command-line options for fine-tuning

## Usage

### Basic Usage
```bash
python3 scripts/gcode_nonplanar_modulation.py input_file.gcode [options]
```

### Examples

1. **Apply sine wave modulation to walls only:**
   ```bash
   python3 scripts/gcode_nonplanar_modulation.py print.gcode -include-perimeters -include-external-perimeters
   ```

2. **Apply triangle wave to infill with custom parameters:**
   ```bash
   python3 scripts/gcode_nonplanar_modulation.py print.gcode -include-infill -infill-function triangle -infill-amplitude 0.5 -infill-frequency 2.0
   ```

3. **Complex modulation with alternating loops:**
   ```bash
   python3 scripts/gcode_nonplanar_modulation.py print.gcode \
     -include-perimeters -include-external-perimeters -include-infill \
     -wall-amplitude 0.2 -wall-frequency 1.5 -wall-direction xy \
     -infill-amplitude 0.3 -infill-frequency 1.0 -infill-direction x \
     -perimeter-function sine -infill-function triangle \
     -alternate-loops -resolution 0.1
   ```

## Command-Line Options

### Required
- `input_file`: Path to the input G-code file (will be modified in-place)

### Modulation Control
- `-include-infill`: Apply modulation to infill regions
- `-include-perimeters`: Apply modulation to internal perimeters
- `-include-external-perimeters`: Apply modulation to external perimeters

### Wall Parameters
- `-wall-amplitude FLOAT`: Amplitude for wall modulation (default: 0.3mm)
- `-wall-frequency FLOAT`: Frequency for wall modulation (default: 1.1)
- `-wall-direction {x,y,xy,negx,negy,negxy}`: Direction of wave for walls (default: x)
- `-perimeter-function {sine,triangle,trapezoidal,sawtooth}`: Wave function for perimeters (default: sine)

### Infill Parameters
- `-infill-amplitude FLOAT`: Amplitude for infill modulation (default: 0.3mm)
- `-infill-frequency FLOAT`: Frequency for infill modulation (default: 1.1)
- `-infill-direction {x,y,xy,negx,negy,negxy}`: Direction of wave for infill (default: x)
- `-infill-function {sine,triangle,trapezoidal,sawtooth}`: Wave function for infill (default: sine)

### Advanced Options
- `-max-step-size FLOAT`: Maximum amplitude increase per layer as percentage (0.0-1.0, default: 0.1)
- `-alternate-loops`: Alternate wave phase on successive wall loops
- `-resolution FLOAT`: Resolution of wave segments in mm (default: 0.2)

## Wave Functions

### Sine Wave
Classic smooth sinusoidal modulation, ideal for gentle transitions and organic-looking surfaces.

### Triangle Wave
Sharp, linear transitions between peaks and valleys, creating faceted surface effects.

### Trapezoidal Wave
Combines flat regions with linear transitions, useful for creating stepped surface effects.

### Sawtooth Wave
Linear ramp pattern, creating directional surface textures.

## Direction Options

- `x`: Wave varies along X-axis
- `y`: Wave varies along Y-axis
- `xy`: Wave varies along both X and Y axes (diagonal pattern)
- `negx`: Inverted X-axis variation
- `negy`: Inverted Y-axis variation
- `negxy`: Inverted diagonal pattern

## Integration with OrcaSlicer

### As a Post-Processing Script

1. **In OrcaSlicer GUI:**
   - Go to Process Settings → Others → Post-processing scripts
   - Add the full path to the script: `/path/to/scripts/gcode_nonplanar_modulation.py`
   - Add your desired parameters after the script path

2. **Example Configuration:**
   ```
   python3 /path/to/OrcaSlicer/scripts/gcode_nonplanar_modulation.py [output_file_placeholder] -include-perimeters -wall-amplitude 0.2
   ```

### Manual Processing

1. Slice your model normally in OrcaSlicer
2. Save the G-code file
3. Run the script on the saved G-code file
4. The modified file can be printed directly

## Important Notes

### Printer Requirements
- **Z-axis precision**: Your printer must have sufficient Z-axis resolution and accuracy
- **Firmware compatibility**: Ensure your firmware can handle frequent Z-axis movements
- **Mechanical considerations**: The printer's Z-axis should be capable of rapid small movements

### Safety Considerations
- **Start with small amplitudes** (0.1-0.3mm) to test your printer's capabilities
- **Monitor first layers** carefully to ensure proper bed adhesion
- **Check for Z-axis binding** or overheating during long prints

### Performance Impact
- **Increased file size**: The script significantly increases G-code file size due to segmentation
- **Print time**: May increase print time due to additional moves
- **Processing time**: Large files may take several minutes to process

## Troubleshooting

### Common Issues

1. **Script doesn't detect slicer type:**
   - The script will fall back to PrusaSlicer markers
   - This usually works fine for most slicers

2. **No modulation applied:**
   - Ensure you've specified at least one `-include-*` option
   - Check that your G-code contains the expected type markers

3. **Excessive Z-axis movement:**
   - Reduce the amplitude values
   - Increase the resolution to create smoother transitions

4. **Print quality issues:**
   - Reduce amplitude near solid infill layers (automatic with max-step-size)
   - Experiment with different wave functions
   - Adjust frequency for your model size

### Debug Information
The script creates a `gcode_debug.log` file with detailed processing information. Check this file if you encounter issues.

## Technical Details

### Layer Detection
The script automatically detects solid infill layers and reduces modulation amplitude near these layers to maintain print quality and structural integrity.

### Segmentation
Long moves are broken into smaller segments based on the resolution parameter. Each segment can have individual Z-modulation applied.

### Extrusion Compensation
The script adjusts extrusion values to account for 3D path length changes due to Z-axis modulation.

## License

This script is released under the GNU General Public License v3.0, same as OrcaSlicer.

## Credits

Copyright (c) [2025] [Roman Tenger]

Integrated into OrcaSlicer repository for enhanced post-processing capabilities.