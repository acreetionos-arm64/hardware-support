# CLAUDE.md - hardware-support AI Development Context

This file provides AI development context and guidance for working with the hardware-support submodule of the AcreetionOS ARM64 workspace.

## Repository Context

**Domain**: ARM64 Hardware Support and Platform Configuration
**Primary Technologies**: Device trees, Linux kernel modules, hardware drivers, firmware management
**Development Focus**: Comprehensive hardware support for ARM64 single-board computers and platforms

### Repository Purpose
The hardware-support repository manages all hardware-specific configurations, device trees, firmware files, and platform support required to run AcreetionOS on diverse ARM64 hardware platforms. This includes comprehensive support for popular single-board computers like Raspberry Pi and Pine64, as well as generic ARM64 hardware platforms, ensuring proper hardware acceleration, peripheral functionality, and platform-specific optimizations.

### Scope and Boundaries
- **In Scope**: Device trees, hardware drivers, firmware files, platform-specific configurations, hardware validation scripts
- **Out of Scope**: Bootloader implementations (boot-systems), cross-compilation toolchain (arm64-toolchain), application-level software
- **Key Focus**: Raspberry Pi 4/5 primary support, Pine64 secondary support, generic ARM64 compatibility

## Development Patterns

### Hardware Organization Structure
```
hardware-support/
├── platforms/                       # Platform-specific support
│   ├── raspberry-pi/
│   │   ├── rpi4/                    # RPi4-specific configs
│   │   ├── rpi5/                    # RPi5-specific configs
│   │   └── common/                  # Shared RPi configurations
│   ├── pine64/                      # Pine64 family support
│   └── generic-arm64/               # Generic ARM64 support
├── device-trees/                    # Device tree sources and overlays
├── firmware/                        # Platform-specific firmware
├── drivers/                         # Hardware drivers and configs
└── scripts/                         # Hardware management automation
```

### Configuration Management Patterns
- **Platform Hierarchy**: Common configurations inherited with platform-specific overrides
- **Modular Approach**: Separate configurations for GPU, audio, network, and peripheral components
- **Version Management**: Hardware configurations tagged and versioned with platform releases
- **Testing Integration**: Automated validation scripts for hardware functionality verification

### Hardware Abstraction Patterns
- **Device Tree Approach**: Hardware description through device tree sources and overlays
- **Driver Configuration**: Modular driver loading with platform-specific parameters
- **Firmware Management**: Automated firmware installation and version management
- **Performance Optimization**: Platform-specific performance tuning and thermal management

### Validation and Testing Patterns
```bash
# Hardware validation pattern
validate_platform() {
    local platform="$1"
    detect_hardware_platform || return 1
    validate_device_tree "$platform" || return 1
    test_hardware_functionality "$platform" || return 1
    verify_performance_requirements "$platform" || return 1
}
```

## Key Technologies

### Device Tree and Hardware Description
- **Device Tree Compiler (dtc)**: Compilation and decompilation of device tree binaries
- **Device Tree Overlays**: Runtime hardware configuration modification
- **Platform-specific DTS**: Hardware description sources for different ARM64 platforms
- **Overlay Management**: Dynamic hardware configuration based on detected platforms

### Hardware Driver Management
- **Kernel Modules**: ARM64-specific hardware driver compilation and loading
- **Driver Configuration**: udev rules, systemd services, and hardware-specific configurations
- **Firmware Loading**: Automated firmware installation and management for hardware components
- **Hardware Detection**: Runtime hardware identification and appropriate configuration loading

### Platform Support Technologies
- **Raspberry Pi Support**: BCM2711/BCM2712 SoC support with VideoCore GPU integration
- **Pine64 Support**: Allwinner A64 and Rockchip RK3399 platform configurations
- **Generic ARM64**: Standard ARM64 hardware support for servers and development boards
- **Cross-Platform Tools**: Hardware abstraction utilities for platform-independent functionality

### Validation and Testing Tools
- **Hardware Testing Scripts**: Automated validation of hardware functionality
- **Performance Monitoring**: Platform-specific performance analysis and optimization
- **Integration Testing**: Compatibility testing with boot systems and ISO building
- **Regression Testing**: Automated testing across supported hardware platforms

## Integration Points

### Cross-Repository Hardware Integration
- **boot-systems Integration**: Hardware-specific bootloader configurations and firmware
- **iso-builder Integration**: Hardware support packages included in ISO images
- **arm64-toolchain Integration**: Cross-compilation of hardware-specific drivers and modules
- **testing-infrastructure Integration**: Hardware validation testing and quality assurance

### Hardware Platform Coordination
```
Hardware Detection → Platform Configuration → Driver Loading → Functionality Validation
       ↓                      ↓                    ↓                    ↓
   detect-hw.sh → platform-config/ → load-drivers.sh → validate-hw.sh
```

### System Integration Points
- **Kernel Integration**: Hardware-specific kernel configurations and module parameters
- **Boot Process Integration**: Hardware initialization during system boot sequence
- **User Space Integration**: Hardware access through standard Linux interfaces (sysfs, /dev)
- **Service Management**: systemd services for hardware-specific functionality

### Cross-Platform Compatibility
- **Standard Interfaces**: Common hardware abstraction across different ARM64 platforms
- **Configuration Inheritance**: Shared configurations with platform-specific overrides
- **Feature Detection**: Runtime capability detection and feature enablement
- **Fallback Support**: Graceful degradation for unsupported or missing hardware features

## Testing Strategy

### Hardware Validation Philosophy
Comprehensive testing across actual ARM64 hardware platforms to ensure reliable hardware support, performance optimization, and compatibility with the overall AcreetionOS ARM64 system.

### Platform Testing Approach
- **Primary Platform Testing**: Comprehensive testing on Raspberry Pi 4/5 hardware
- **Secondary Platform Testing**: Validation on Pine64 and other supported platforms
- **Emulation Testing**: Initial validation using QEMU ARM64 where hardware emulation is available
- **Generic Platform Testing**: Compatibility testing on various ARM64 development boards

### Hardware Functionality Testing
```bash
# Comprehensive hardware testing suite
test_hardware_comprehensive() {
    test_cpu_functionality || return 1
    test_gpu_acceleration || return 1
    test_audio_system || return 1
    test_network_interfaces || return 1
    test_storage_systems || return 1
    test_peripheral_interfaces || return 1
    test_thermal_management || return 1
}
```

### Performance and Optimization Testing
- **CPU Performance**: Validation of CPU frequency scaling and performance governors
- **GPU Acceleration**: Graphics performance testing and hardware acceleration validation
- **Thermal Management**: Temperature monitoring and thermal throttling behavior
- **Power Management**: Power consumption optimization and battery life (for mobile platforms)

## Hardware Platform Specifications

### Raspberry Pi Platform Support
```yaml
Raspberry Pi 4B (BCM2711):
  CPU: ARM Cortex-A72 quad-core @ 1.5GHz
  GPU: VideoCore VI (OpenGL ES 3.x, Vulkan 1.0)
  RAM: 2GB/4GB/8GB LPDDR4
  Network: Gigabit Ethernet, 802.11ac WiFi, Bluetooth 5.0
  GPIO: 40-pin header with I2C, SPI, UART
  Storage: microSD, USB 3.0, optional PoE
  Status: Full support priority

Raspberry Pi 5 (BCM2712):
  CPU: ARM Cortex-A76 quad-core @ 2.4GHz
  GPU: VideoCore VII (enhanced performance)
  RAM: 4GB/8GB LPDDR4X
  Network: Gigabit Ethernet, 802.11ac WiFi, Bluetooth 5.0
  GPIO: 40-pin header with enhanced I/O capabilities
  Storage: microSD, USB 3.0, PCIe connector
  Status: Primary target platform
```

### Pine64 Platform Support
```yaml
Pine64 Plus (Allwinner A64):
  CPU: ARM Cortex-A53 quad-core @ 1.2GHz
  GPU: Mali-400 MP2
  RAM: 2GB LPDDR3
  Network: Gigabit Ethernet, optional WiFi
  GPIO: Euler connector with I2C, SPI, UART
  Status: Secondary support platform

Pinebook Pro (Rockchip RK3399):
  CPU: ARM Cortex-A72 dual + A53 quad
  GPU: Mali-T860 MP4
  RAM: 4GB LPDDR4
  Network: 802.11ac WiFi, Bluetooth 4.2
  Storage: eMMC, microSD, NVMe support
  Status: Mobile platform support
```

## Quality Standards

### Hardware Configuration Standards
- **Device Tree Accuracy**: Device tree sources must accurately describe hardware capabilities
- **Driver Compatibility**: All drivers tested and validated on target hardware platforms
- **Performance Optimization**: Platform-specific optimizations implemented for acceptable performance
- **Standard Compliance**: Hardware support follows Linux kernel and ARM64 standards

### Documentation Requirements
- **Platform Documentation**: Comprehensive documentation for each supported hardware platform
- **Hardware Setup Guides**: Step-by-step hardware setup and configuration instructions
- **Troubleshooting Guides**: Common hardware issues and resolution procedures
- **Performance Guidelines**: Platform-specific performance expectations and optimization tips

### Testing and Validation Standards
- **Hardware Testing**: All configurations tested on actual hardware before release
- **Regression Testing**: Automated testing prevents hardware support regressions
- **Integration Testing**: Hardware support validated with boot systems and ISO building
- **Performance Benchmarking**: Consistent performance testing across platform updates

### Security Considerations
- **Firmware Security**: Secure firmware loading and verification procedures
- **Hardware Access Control**: Proper permissions and security for hardware device access
- **Update Security**: Secure hardware configuration and firmware update processes
- **Vulnerability Management**: Regular security updates for hardware drivers and firmware

## Troubleshooting and Debug Patterns

### Hardware Detection and Validation
```bash
# Hardware platform detection
detect_arm64_platform() {
    local platform=""

    # Check device tree model
    if [[ -f /proc/device-tree/model ]]; then
        local model=$(tr -d '\0' < /proc/device-tree/model)
        case "$model" in
            "Raspberry Pi 4"*) platform="rpi4" ;;
            "Raspberry Pi 5"*) platform="rpi5" ;;
            *"Pine64"*) platform="pine64" ;;
            *) platform="generic-arm64" ;;
        esac
    fi

    echo "$platform"
}

# Hardware validation
validate_hardware_support() {
    local platform="$1"
    local validation_failed=0

    # CPU validation
    if ! validate_cpu_support "$platform"; then
        echo "CPU support validation failed for $platform" >&2
        validation_failed=1
    fi

    # GPU validation
    if ! validate_gpu_support "$platform"; then
        echo "GPU support validation failed for $platform" >&2
        validation_failed=1
    fi

    return $validation_failed
}
```

### Performance Analysis and Optimization
- **CPU Performance**: Frequency scaling analysis and governor optimization
- **GPU Performance**: Graphics acceleration benchmarking and optimization
- **Memory Performance**: RAM and storage performance analysis
- **Thermal Analysis**: Temperature monitoring and thermal throttling evaluation

### Common Hardware Issues Resolution
- **Device Tree Problems**: Device tree compilation errors and hardware description issues
- **Driver Loading Issues**: Kernel module loading failures and dependency problems
- **Firmware Problems**: Missing or incompatible firmware files
- **Performance Issues**: Suboptimal performance due to misconfiguration or thermal throttling

## Milestone Alignment

### Current Milestone Objectives
**Milestone 0 - Infrastructure Setup**: Establish hardware support infrastructure and identify target platforms for comprehensive ARM64 hardware compatibility

### Repository-Specific Goals
- Complete hardware platform analysis and support matrix development
- Implement comprehensive Raspberry Pi 4/5 hardware support with full functionality
- Establish Pine64 and generic ARM64 hardware support framework
- Create automated hardware detection, configuration, and validation systems

### Success Criteria
- Raspberry Pi 4/5 hardware fully supported with GPU acceleration, audio, and networking
- Hardware detection and configuration automation working reliably
- Integration with boot-systems and iso-builder completed and tested
- Documentation and troubleshooting guides complete for supported platforms

### Dependencies on Other Submodules
- **arm64-toolchain**: Cross-compilation environment for hardware driver compilation
- **boot-systems**: Hardware-specific bootloader integration and firmware loading
- **iso-builder**: Hardware support package inclusion in ISO images
- **testing-infrastructure**: Hardware validation testing and quality assurance frameworks

## Reference Materials

### Hardware Documentation
- [Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/): Official hardware and software documentation
- [Pine64 Wiki](https://wiki.pine64.org/): Community documentation for Pine64 platforms
- [ARM Architecture Reference Manual](https://developer.arm.com/documentation/): Official ARM architecture documentation
- [Linux Kernel Documentation](https://www.kernel.org/doc/Documentation/): Kernel driver and device tree documentation

### Device Tree Resources
- [Device Tree Documentation](https://www.devicetree.org/): Device tree specification and best practices
- [Linux Device Tree Bindings](https://www.kernel.org/doc/Documentation/devicetree/bindings/): Kernel device tree binding documentation
- [ARM64 Device Trees](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/arm64/boot/dts): Upstream device tree sources

### Hardware Community Resources
- ARM64 hardware support communities and forums
- Single-board computer development communities
- Linux kernel hardware support development resources
- ARM64 performance optimization guides and best practices

## AI Assistance Guidelines

### Hardware Configuration Development
- **Platform Research**: Assistance with hardware specification research and analysis
- **Device Tree Development**: Support for device tree source creation and optimization
- **Driver Configuration**: Help with hardware driver configuration and integration
- **Performance Optimization**: Guidance on platform-specific performance tuning

### Testing and Validation Support
- **Test Development**: Assistance with hardware validation script development
- **Debugging Support**: Help with hardware issue diagnosis and resolution
- **Integration Testing**: Support for cross-repository hardware integration testing
- **Documentation**: Help with hardware setup and troubleshooting documentation

### Cross-Platform Development
- **Compatibility Analysis**: Assistance with cross-platform compatibility assessment
- **Configuration Management**: Support for hierarchical configuration management
- **Automation Development**: Help with hardware management automation scripts
- **Standards Compliance**: Guidance on Linux and ARM64 hardware support standards

## Project Context

This repository operates within the AcreetionOS ARM64 multi-repository workspace:

- **Main Workspace**: [acreetionos-arm64/workspace](https://github.com/acreetionos-arm64/workspace)
- **Architecture**: Specialized hardware support repository serving entire ARM64 ecosystem
- **Coordination**: Hardware configurations coordinate with boot systems and ISO building
- **Timeline**: Comprehensive hardware support developed throughout 18-36 month project cycle

## Upstream Relationships

### Hardware Vendor Integration
Coordinate with hardware vendor communities (Raspberry Pi Foundation, Pine64) for optimal hardware support while maintaining compatibility with upstream hardware configurations.

### Linux Kernel Community
Contribute hardware support improvements and device tree enhancements back to Linux kernel community for broader ARM64 ecosystem benefit.

### ARM64 Hardware Community
Active participation in ARM64 hardware support communities, sharing configurations and optimizations for community benefit and collaborative development.

---

*This AI context file is maintained to provide effective hardware support development assistance specific to the ARM64 hardware domain within the AcreetionOS ARM64 project.*