# Starter CLAP Plugin

A minimal but complete CLAP audio plugin template with proper architecture and build system.

## Features

- **Complete CLAP Implementation**: Parameters, state saving, audio processing
- **Parameter Smoothing**: Anti-zipper noise smoothing system
- **Denormal Protection**: Flush-to-zero to prevent CPU spikes
- **Cross-Platform Build**: Automated builds for Linux, macOS (Intel/ARM), Windows
- **Clean Architecture**: Separated DSP, parameters, and main plugin code
- **Simple DSP**: Basic lowpass filter with gain and dry/wet controls
- **Safety Clipper**: Prevents digital clipping with soft limiting

## Parameters

| Parameter | Range | Description |
|-----------|--------|-------------|
| **Gain** | 0-2x | Output gain multiplier |
| **Frequency** | 20Hz-20kHz | Lowpass filter cutoff frequency |
| **Dry/Wet** | 0-100% | Mix between processed and dry signal |

## Getting Started

1. **Clone and customize**:
   - Change `PLUGIN_ID`, `PLUGIN_NAME`, `PLUGIN_VENDOR` in `plugin.h`
   - Update project name in `CMakeLists.txt`
   - Modify GitHub Actions workflow name

2. **Add your DSP**:
   - Replace the simple lowpass filter in `dsp.cpp`
   - Add new parameters in `plugin.h` and `params.cpp`
   - Update parameter count and enum

3. **Build locally**:
   ```bash
   cmake -B build -DCMAKE_BUILD_TYPE=Release
   cmake --build build --config Release
   ```

## File Structure

- **plugin.h**: Main header with constants and structures
- **plugin.cpp**: CLAP interface implementation and plugin lifecycle
- **dsp.cpp**: Digital signal processing and parameter smoothing
- **params.cpp**: Parameter management and state saving/loading
- **CMakeLists.txt**: Cross-platform build configuration
- **.github/workflows/build.yml**: Automated CI/CD pipeline

## Key Patterns

### Parameter Smoothing
All parameters use exponential smoothing to prevent zipper noise:
```cpp
param.current += (param.target - param.current) * smooth_coeff;
```

### Coefficient Updates
Filter coefficients only recalculate when parameters change:
```cpp
if (p->coefficients_need_update) {
    update_filter_coefficients(p);
}
```

### State Management
Complete save/restore with version checking and bounds validation.

### Cross-Platform Compatibility
- Universal binaries for macOS (Intel + ARM64)
- Proper compiler optimization flags
- SIMD alignment considerations

## Building

The included GitHub Actions workflow automatically builds for all platforms when you push a tag:

```bash
git tag v0.1.0
git push origin v0.1.0
```

## Installation

Copy the `.clap` file to your plugin directory:
- **Linux**: `~/.clap`
- **macOS**: `~/Library/Audio/Plug-Ins/CLAP`
- **Windows**: `%COMMONPROGRAMFILES%\CLAP`

## License

Public domain template - use however you want.

## Credits

- All coding by Claude AI
- Based on patterns from the Evans Harmonic Isolator project and CLAP documentation
