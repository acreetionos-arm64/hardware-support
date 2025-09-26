# GitHub Copilot Instructions - hardware-support

This file provides GitHub Copilot with domain-specific context for the hardware-support submodule of the AcreetionOS ARM64 project.

## Repository Domain

**Specialization**: ARM64 Hardware Support and Platform Configuration
**Primary Function**: Device-specific configurations and hardware support for ARM64 platforms
**Technical Domain**: Device trees, hardware drivers, firmware management, platform optimization

## Code Style and Conventions

### Hardware Configuration Standards
- **Device Tree Sources**: Follow Linux kernel device tree conventions and naming
- **Platform Organization**: Hierarchical structure with common configs and platform-specific overrides
- **Script Automation**: Robust hardware detection, configuration, and validation scripts
- **Firmware Management**: Secure firmware loading with version tracking and validation

### Platform Support Patterns
```bash
# Platform detection pattern
detect_hardware_platform() {
    local model_file="/proc/device-tree/model"
    if [[ -f "$model_file" ]]; then
        local model=$(tr -d '\0' < "$model_file")
        case "$model" in
            "Raspberry Pi 4"*) echo "rpi4" ;;
            "Raspberry Pi 5"*) echo "rpi5" ;;
            *"Pine64"*) echo "pine64" ;;
            *) echo "generic-arm64" ;;
        esac
    else
        echo "unknown"
    fi
}

# Hardware validation pattern
validate_hardware_component() {
    local component="$1"
    local platform="$2"

    case "$component" in
        gpu) validate_gpu_support "$platform" ;;
        audio) validate_audio_system "$platform" ;;
        network) validate_network_interfaces "$platform" ;;
        *) return 1 ;;
    esac
}
```

### Device Tree Development
- **DTS Organization**: Platform-specific device tree sources with clear hardware descriptions
- **Overlay Management**: Modular device tree overlays for optional hardware components
- **Hardware Abstraction**: Standard interfaces for cross-platform compatibility
- **Performance Optimization**: Platform-specific optimizations for CPU, GPU, and peripherals

## Framework Preferences

### Hardware Support Architecture
- **Platform Hierarchy**: Common configurations inherited with platform-specific overrides
- **Modular Design**: Separate modules for GPU, audio, network, and peripheral support
- **Runtime Detection**: Automatic hardware detection and appropriate configuration loading
- **Validation Framework**: Comprehensive testing for hardware functionality and performance

### Supported Platforms Priority
```yaml
Primary Targets:
  - Raspberry Pi 4B (BCM2711): Full support with GPU acceleration
  - Raspberry Pi 5 (BCM2712): Primary target platform

Secondary Targets:
  - Pine64 Plus (Allwinner A64): Community platform support
  - Pinebook Pro (RK3399): Mobile platform support
  - Generic ARM64: Server and development board support
```

## Testing Approaches

### Hardware Validation Strategy
```bash
# Comprehensive hardware testing
test_hardware_platform() {
    local platform="$1"

    echo "Testing hardware platform: $platform"

    # Core functionality tests
    test_cpu_performance "$platform" || return 1
    test_memory_functionality "$platform" || return 1
    test_storage_systems "$platform" || return 1

    # Platform-specific tests
    case "$platform" in
        rpi4|rpi5)
            test_videocore_gpu "$platform" || return 1
            test_broadcom_audio "$platform" || return 1
            ;;
        pine64)
            test_mali_gpu "$platform" || return 1
            test_allwinner_audio "$platform" || return 1
            ;;
    esac

    echo "Hardware testing completed successfully for $platform"
}
```

### Integration Testing
- **Boot System Integration**: Hardware configurations work with ARM64 bootloaders
- **ISO Building Integration**: Hardware support packages integrate properly
- **Cross-Platform Testing**: Consistent behavior across different ARM64 platforms
- **Performance Validation**: Acceptable performance benchmarks for target use cases

## ARM64-Specific Considerations

### Platform Differences
```bash
# Platform-specific optimizations
configure_platform_optimization() {
    local platform="$1"

    case "$platform" in
        rpi4)
            # Raspberry Pi 4 optimizations
            echo "gpu_mem=128" >> /boot/config.txt
            echo "arm_freq=1800" >> /boot/config.txt
            ;;
        rpi5)
            # Raspberry Pi 5 optimizations
            echo "gpu_mem=256" >> /boot/config.txt
            echo "arm_boost=1" >> /boot/config.txt
            ;;
        pine64)
            # Pine64 optimizations
            configure_mali_gpu_governor
            set_cpu_frequency_scaling "ondemand"
            ;;
    esac
}
```

### Hardware Acceleration
- **GPU Support**: VideoCore (RPi), Mali (Pine64), Generic ARM64 GPU configurations
- **Media Acceleration**: Hardware video encoding/decoding where available
- **Compute Acceleration**: GPU compute capabilities for supported platforms
- **Performance Tuning**: Platform-specific performance optimization and thermal management

## Quality Standards

### Hardware Support Requirements
- **Functional Completeness**: All major hardware components working (CPU, GPU, audio, network)
- **Performance Standards**: Acceptable performance for desktop and development use
- **Reliability**: Stable operation under normal and stress conditions
- **Documentation**: Comprehensive setup and troubleshooting documentation

### Testing Standards
```bash
# Hardware acceptance testing
hardware_acceptance_test() {
    local platform="$1"
    local test_results=()

    # Core functionality requirements
    test_results+=("cpu:$(test_cpu_basic_functionality)")
    test_results+=("memory:$(test_memory_stability)")
    test_results+=("storage:$(test_storage_performance)")
    test_results+=("network:$(test_network_connectivity)")

    # Platform-specific requirements
    test_results+=("gpu:$(test_gpu_acceleration "$platform")")
    test_results+=("audio:$(test_audio_output "$platform")")

    # Evaluate results
    for result in "${test_results[@]}"; do
        if [[ "$result" =~ :FAIL ]]; then
            return 1
        fi
    done

    return 0
}
```

## Common Patterns and Anti-Patterns

### Recommended Patterns
```bash
# Good: Platform-agnostic with specific optimizations
setup_audio_system() {
    local platform="$1"

    # Common audio setup
    configure_alsa_base

    # Platform-specific optimizations
    case "$platform" in
        rpi*) configure_rpi_audio_optimization ;;
        pine64) configure_pine64_audio_optimization ;;
    esac
}

# Good: Comprehensive hardware validation
validate_platform_support() {
    local platform="$1"

    validate_required_hardware "$platform" || return 1
    validate_driver_support "$platform" || return 1
    validate_performance_requirements "$platform" || return 1

    echo "Platform $platform validation successful"
}
```

### Anti-Patterns to Avoid
```bash
# Bad: Hardcoded platform assumptions
echo "gpu_mem=128" >> /boot/config.txt  # Assumes Raspberry Pi

# Bad: No hardware validation
load_driver "some_driver"  # No check if hardware supports this driver

# Bad: Platform-specific code without abstraction
if [[ $(uname -m) == "aarch64" ]]; then
    # Assumes all ARM64 is the same
fi
```

## Project-Specific Context

### AcreetionOS ARM64 Integration
- **Hardware Target**: Primary focus on Raspberry Pi 5 for desktop use
- **Performance Requirements**: Suitable for daily desktop computing and development
- **Community Focus**: Popular single-board computers for accessible ARM64 computing
- **Educational Value**: Learning hardware support development for Linux distributions

### Multi-Repository Coordination
- **boot-systems**: Hardware-specific bootloader configurations and firmware
- **iso-builder**: Hardware drivers and configurations included in ISO
- **arm64-toolchain**: Cross-compilation of hardware-specific drivers
- **testing-infrastructure**: Hardware validation and quality assurance

---

*These instructions help GitHub Copilot provide contextually appropriate suggestions for hardware-support development within the AcreetionOS ARM64 project ecosystem.*