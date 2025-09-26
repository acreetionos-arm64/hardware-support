# hardware-support

> Device-specific configurations, firmware, and hardware support for ARM64 platforms

## Repository Purpose

This repository provides hardware-specific configurations, device trees, firmware files, and hardware support packages required for ARM64 single-board computers and other ARM64 hardware platforms. It enables AcreetionOS to run on diverse ARM64 hardware with proper hardware acceleration, peripheral support, and platform-specific optimizations.

This repository is part of the AcreetionOS ARM64 multi-repository workspace, providing hardware abstraction and device-specific support for the overall ARM64 port project.

## Architecture Context

hardware-support integrates with the AcreetionOS ARM64 workspace as follows:

- **Primary Role**: Hardware abstraction and device-specific configuration management
- **Dependencies**: arm64-toolchain (for driver compilation), upstream hardware support packages
- **Integration Points**: iso-builder (hardware configs in ISO), boot-systems (hardware-specific boot), custom-packages (hardware-specific packages)
- **Upstream Relationship**: Linux kernel, device tree sources, vendor firmware repositories

### Related Repositories
- **[boot-systems](https://github.com/acreetionos-arm64/boot-systems)** - Hardware-specific bootloader configurations
- **[iso-builder](https://github.com/acreetionos-arm64/iso-builder)** - Includes hardware support in ISO images
- **[arm64-toolchain](https://github.com/acreetionos-arm64/arm64-toolchain)** - Cross-compilation for hardware-specific drivers
- **[custom-packages](https://github.com/acreetionos-arm64/custom-packages)** - Hardware-specific custom packages
- **[testing-infrastructure](https://github.com/acreetionos-arm64/testing-infrastructure)** - Hardware validation testing

## Development Status

**Current Milestone**: Milestone 0 - Infrastructure Setup
**Completion Status**: 🔄 Hardware support infrastructure planning and setup

### Milestone Progress
- ✅ Repository structure established
- 🔄 Hardware platform identification and prioritization
- 📋 Planned: Raspberry Pi 4/5 comprehensive support
- 📋 Planned: Pine64 and other popular SBC support
- 📋 Planned: Generic ARM64 hardware support framework

## Quick Start Guide

### Prerequisites
- Understanding of ARM64 hardware platforms and architectures
- Knowledge of Linux device trees and kernel modules
- Familiarity with single-board computer hardware (Raspberry Pi, Pine64, etc.)
- ARM64 cross-compilation environment (via arm64-toolchain submodule)

### Setup Instructions
```bash
# Clone with workspace context
cd /path/to/acreetionos-arm64/workspace
git submodule update --init --recursive
cd hardware-support/

# Review supported hardware platforms
ls platforms/

# Check device-specific configurations
ls device-trees/

# Review firmware requirements
ls firmware/
```

### Basic Usage
```bash
# Install hardware support for specific platform
./scripts/install-hardware-support.sh --platform=rpi4

# Generate device tree for target hardware
./scripts/generate-device-tree.sh --hardware=rpi5 --config=standard

# Validate hardware configuration
./scripts/validate-hardware.sh --target=rpi4

# Test hardware functionality
./scripts/test-hardware.sh --platform=rpi5 --component=gpu,audio,network
```

## Directory Structure

```
hardware-support/
├── platforms/
│   ├── raspberry-pi/
│   │   ├── rpi4/                    # Raspberry Pi 4 support
│   │   ├── rpi5/                    # Raspberry Pi 5 support
│   │   └── common/                  # Common RPi configurations
│   ├── pine64/
│   │   ├── pine64-plus/             # Pine64+ support
│   │   ├── pinebook-pro/            # Pinebook Pro support
│   │   └── common/                  # Common Pine64 configurations
│   └── generic-arm64/               # Generic ARM64 hardware support
├── device-trees/
│   ├── raspberry-pi/                # RPi device tree sources
│   ├── pine64/                      # Pine64 device tree sources
│   └── overlays/                    # Device tree overlays
├── firmware/
│   ├── raspberry-pi/                # RPi firmware files
│   ├── pine64/                      # Pine64 firmware files
│   └── common/                      # Common ARM64 firmware
├── drivers/
│   ├── gpu/                         # Graphics drivers and configs
│   ├── audio/                       # Audio system configurations
│   ├── network/                     # Network interface support
│   └── peripherals/                 # GPIO, SPI, I2C configurations
├── scripts/
│   ├── install-hardware-support.sh  # Hardware support installation
│   ├── generate-device-tree.sh      # Device tree generation
│   ├── validate-hardware.sh         # Hardware validation
│   └── test-hardware.sh             # Hardware functionality testing
└── configs/
    ├── kernel/                      # Kernel configurations
    ├── udev/                        # udev rules for hardware
    └── systemd/                     # systemd hardware services
```

### Key Files and Directories
- **platforms/**: Hardware platform-specific configurations and support files
- **device-trees/**: Device tree sources and overlays for hardware description
- **firmware/**: Platform-specific firmware files and bootloader components
- **drivers/**: Hardware drivers, configurations, and support packages
- **scripts/**: Automation tools for hardware setup, validation, and testing
- **configs/**: System-level configurations for kernel, udev, and hardware services

## Development Workflow

### Making Changes
1. **Identify target hardware**: Determine which ARM64 platform requires support
2. **Research hardware specifications**: Understand CPU, GPU, peripherals, and interfaces
3. **Create platform configuration**: Develop device trees, kernel configs, and driver setup
4. **Test on actual hardware**: Validate functionality on physical hardware platforms
5. **Document hardware-specific requirements**: Update documentation with setup and limitations
6. **Integration testing**: Ensure compatibility with boot-systems and iso-builder

### Testing Changes
- **Hardware validation**: Test on actual ARM64 hardware platforms
- **Emulation testing**: Initial validation using QEMU where possible
- **Functionality testing**: Validate GPU, audio, network, and peripheral functionality
- **Performance testing**: Ensure acceptable performance on target hardware
- **Integration testing**: Verify compatibility with bootloader and ISO building process

### Contributing
1. Focus on popular ARM64 single-board computers and development platforms
2. Ensure comprehensive hardware support including GPU acceleration
3. Test configurations on actual hardware before submitting
4. Document hardware-specific setup requirements and limitations
5. Coordinate with boot-systems for bootloader compatibility

## Dependencies

### System Requirements
- ARM64 cross-compilation toolchain for driver compilation
- Device tree compiler (dtc) for device tree processing
- Hardware documentation and specifications for target platforms
- Physical ARM64 hardware for testing and validation

### External Dependencies
- **Linux Kernel**: ARM64 kernel sources and hardware drivers
- **Device Tree Sources**: Platform-specific device tree repositories
- **Firmware Packages**: Vendor-provided firmware for ARM64 platforms
- **Hardware Drivers**: GPU, audio, and peripheral driver packages

### Submodule Dependencies
- **arm64-toolchain**: Cross-compilation environment for driver building
- **boot-systems**: Hardware-specific bootloader integration
- **iso-builder**: Hardware support inclusion in ISO images
- **testing-infrastructure**: Hardware validation and testing frameworks

## Testing Instructions

### Hardware Validation
- **Platform Detection**: Verify correct hardware platform identification
- **Driver Loading**: Confirm proper hardware driver initialization
- **Peripheral Testing**: Validate GPIO, SPI, I2C, and other peripheral functionality
- **Performance Validation**: Ensure acceptable performance for target use cases

### Functionality Testing
- **Graphics Acceleration**: Test GPU functionality and hardware acceleration
- **Audio System**: Validate audio output and input functionality
- **Network Interfaces**: Test wired and wireless network connectivity
- **Storage Systems**: Verify SD card, eMMC, and USB storage support

### Integration Testing
- **Boot System Integration**: Verify compatibility with ARM64 bootloaders
- **ISO Building**: Ensure hardware support integrates with ISO creation
- **Cross-Platform**: Test configurations across different ARM64 hardware
- **Performance Impact**: Monitor resource usage and system performance

## Hardware Platform Support

### Raspberry Pi Family
```yaml
Raspberry Pi 4 (BCM2711):
  - CPU: Cortex-A72 quad-core 1.5GHz
  - GPU: VideoCore VI (Vulkan support)
  - Memory: 2GB/4GB/8GB LPDDR4
  - Status: Priority platform for initial support

Raspberry Pi 5 (BCM2712):
  - CPU: Cortex-A76 quad-core 2.4GHz
  - GPU: VideoCore VII (improved performance)
  - Memory: 4GB/8GB LPDDR4X
  - Status: Primary target platform
```

### Pine64 Family
```yaml
Pine64 Plus:
  - CPU: Allwinner A64 quad-core Cortex-A53
  - GPU: Mali-400 MP2
  - Memory: 2GB LPDDR3
  - Status: Secondary support platform

Pinebook Pro:
  - CPU: Rockchip RK3399 (Cortex-A72 + A53)
  - GPU: Mali-T860 MP4
  - Memory: 4GB LPDDR4
  - Status: Laptop/mobile platform support
```

### Generic ARM64 Support
- **Server Platforms**: ARM64 server and cloud instance support
- **Development Boards**: Generic ARM64 development board compatibility
- **Embedded Systems**: Support for ARM64 embedded and IoT platforms

## Upstream Coordination

### GitLab CE Mirroring
- **Status**: 📋 Planned coordination for hardware configuration sharing
- **Purpose**: Share hardware configurations with upstream AcreetionOS team
- **Approach**: Selective sharing of platform-specific optimizations

### Upstream Relationships
- **Linux Kernel**: ARM64 hardware support and device driver integration
- **Vendor Repositories**: Hardware vendor firmware and driver repositories
- **ARM64 Community**: Collaboration with broader ARM64 hardware support community

### Contribution Upstream
- Hardware configuration improvements contributed to upstream projects
- Device tree enhancements shared with Linux kernel community
- ARM64 hardware support documentation for community benefit

## Troubleshooting

### Common Issues
- **Hardware not detected**: Check device tree configuration and kernel driver support
- **Graphics acceleration not working**: Verify GPU drivers and firmware installation
- **Audio system problems**: Validate audio driver configuration and ALSA setup
- **Network connectivity issues**: Check network driver support and interface configuration
- **Performance problems**: Review CPU governor settings and thermal management

### Debug Information
```bash
# Hardware detection and validation
./scripts/debug-hardware.sh --platform=rpi5 --verbose

# Device tree analysis
dtc -I dtb -O dts /boot/bcm2712-rpi-5-b.dtb > current-dt.dts

# Driver and firmware status
dmesg | grep -E "(firmware|driver|hardware)"
lsmod | grep -E "(gpu|audio|network)"

# Performance monitoring
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
vcgencmd measure_temp  # Raspberry Pi temperature
```

### Getting Help
- **GitHub Issues**: Use hardware-specific issue templates for platform problems
- **Hardware Documentation**: Check platform vendor documentation and community resources
- **Community Support**: ARM64 and single-board computer community forums
- **Integration Support**: Coordinate with boot-systems and iso-builder teams

## Project Context

- **AcreetionOS ARM64 Workspace**: [Main Repository](https://github.com/acreetionos-arm64/workspace)
- **Project Documentation**: [Documentation Repository](https://github.com/acreetionos-arm64/documentation)
- **Issue Tracking**: [GitHub Issues](https://github.com/acreetionos-arm64/hardware-support/issues)
- **Development Coordination**: See [CLAUDE.md](./CLAUDE.md) for AI development context

## License

MIT License (organization repositories)

Hardware-specific firmware and drivers maintain their respective upstream licenses.

---

*This repository is part of the AcreetionOS ARM64 port project - a sustainable side project focused on learning Linux distribution development while creating a functional ARM64 port of AcreetionOS.*