# Advanced G-code Post-Processor

This script provides comprehensive G-code post-processing for OrcaSlicer, combining wall shifting and non-planar infill capabilities to enhance 3D printing results.

## Features

### Wall Shifting
- **Z-axis modulation**: Shifts perimeter walls in alternating blocks to different Z heights
- **Extrusion adjustments**: Automatically adjusts extrusion based on layer position:
  - First layer: 1.5x extrusion for better adhesion
  - Last layer: 0.5x extrusion for cleaner finish
  - Regular layers: Configurable multiplier
- **Wall reordering**: Option to print non-shifted walls before shifted walls for better quality

### Non-planar Infill
- **Sine wave modulation**: Applies sinusoidal Z-variation to infill paths
- **Adaptive scaling**: Reduces modulation near solid infill layers to maintain structural integrity
- **Segmentation**: Breaks long moves into smaller segments for smooth curves
- **Extrusion compensation**: Adjusts extrusion for 3D path length changes

### General Features
- **Layer height detection**: Automatically detects layer height from G-code headers
- **Comprehensive logging**: Detailed logging of all processing steps
- **Slicer compatibility**: Works with OrcaSlicer, PrusaSlicer, and similar G-code formats

## Usage

### Basic Usage
```bash
python3 advanced_gcode_processor.py input_file.gcode [options]
```

### Command-Line Arguments

#### Required
- `input_file`: Path to the input G-code file (will be modified in-place)

#### Wall Shifting Options
- `-extrusionMultiplier FLOAT`: Extrusion multiplier for regular layers (default: 1.0)
- `-wallReorder {0,1}`: Enable wall reordering (0=off, 1=on, default: 1)

#### Non-planar Infill Options
- `-nonPlanar {0,1}`: Enable non-planar infill (0=off, 1=on, default: 0)
- `-amplitude FLOAT`: Amplitude of the Z modulation in mm (default: 0.6)
- `-frequency FLOAT`: Frequency of the Z modulation (default: 1.1)

### Examples

#### Wall Shifting Only
```bash
python3 advanced_gcode_processor.py print.gcode -extrusionMultiplier 1.2
```

#### Non-planar Infill Only
```bash
python3 advanced_gcode_processor.py print.gcode -nonPlanar 1 -amplitude 0.4 -frequency 1.5
```

#### Combined Processing
```bash
python3 advanced_gcode_processor.py print.gcode \
  -extrusionMultiplier 1.1 \
  -nonPlanar 1 \
  -amplitude 0.3 \
  -frequency 2.0 \
  -wallReorder 1
```

## Integration with OrcaSlicer

### Post-Processing Script Setup
1. In OrcaSlicer, go to **Process Settings** → **Others** → **Post-processing scripts**
2. Add the script path with desired parameters:
   ```
   python3 /path/to/scripts/advanced_gcode_processor.py [output_file_placeholder] -extrusionMultiplier 1.1 -nonPlanar 1
   ```

### Manual Processing
1. Slice your model in OrcaSlicer
2. Save the G-code file
3. Run the script on the saved file
4. Print the modified G-code

## Technical Details

### Wall Shifting Algorithm
1. **Perimeter detection**: Identifies internal perimeter blocks from G-code comments
2. **Block alternation**: Alternates Z-height between blocks (odd blocks shifted up by 0.5 × layer height)
3. **Extrusion scaling**: Applies layer-specific extrusion multipliers
4. **Buffer management**: Optionally reorders walls for optimal print quality

### Non-planar Infill Algorithm
1. **Infill detection**: Identifies internal infill sections
2. **Segmentation**: Breaks moves into segments of 1mm length
3. **Z-modulation**: Applies sine wave: `Z = Z_base + amplitude × sin(frequency × X) × scaling_factor`
4. **Scaling factor**: Reduces amplitude near solid infill layers to prevent collisions
5. **Extrusion compensation**: Adjusts extrusion for 3D path length

### Layer Height Detection
The script automatically detects layer height from G-code headers using pattern:
```
; layer_height = 0.2
```
Falls back to 0.2mm if not found.

## Safety Considerations

### Printer Requirements
- **Z-axis precision**: Requires accurate Z-axis movement capability
- **Firmware compatibility**: Must handle frequent Z-axis movements
- **Mechanical stability**: Z-axis should handle rapid small movements

### Recommended Settings
- **Start with small amplitudes**: Begin with 0.1-0.3mm for testing
- **Monitor first layers**: Ensure proper bed adhesion
- **Check for binding**: Watch for Z-axis mechanical issues
- **Test incrementally**: Gradually increase parameters

### Print Quality Tips
- **Reduce amplitude for fine details**: Lower amplitude for small features
- **Adjust frequency for model size**: Higher frequency for larger models
- **Use wall reordering**: Enable for better surface quality
- **Monitor extrusion**: Adjust multipliers based on material

## Troubleshooting

### Common Issues

1. **No modifications applied**:
   - Check G-code format compatibility
   - Ensure correct comment markers (`;TYPE:`)
   - Verify parameters are enabled

2. **Excessive Z-movement**:
   - Reduce amplitude values
   - Check printer Z-axis capabilities
   - Lower frequency for smoother motion

3. **Layer adhesion problems**:
   - Adjust extrusion multipliers
   - Reduce amplitude near bed
   - Check first layer settings

4. **Print quality issues**:
   - Enable wall reordering
   - Reduce frequency
   - Adjust segment length

### Debug Information
The script creates `z_shift_log.txt` with detailed processing information:
- Layer detection events
- Perimeter block processing
- Z-height modifications
- Extrusion adjustments

Check this log file to diagnose processing issues.

## File Output

The script modifies the input G-code file in-place. The processed file will contain:
- Additional Z-moves for wall shifting
- Segmented infill moves with Z-modulation
- Modified extrusion values
- Detailed comments explaining modifications

## Performance Notes

- **File size increase**: Segmentation significantly increases G-code size
- **Processing time**: Large files may take several minutes to process
- **Print time impact**: Additional moves may increase print time
- **Memory usage**: Large files require adequate system memory

## License

This script is distributed under the GNU General Public License v3.0, same as OrcaSlicer.

## Credits

Copyright (c) [2025] [Roman Tenger]

Integrated into OrcaSlicer for enhanced post-processing capabilities.