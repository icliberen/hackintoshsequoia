# MSI B850M-G Hackintosh (macOS Sequoia)

This repository contains the OpenCore EFI folder for installing and running macOS Sequoia on an MSI B850M-G motherboard with an AMD Ryzen 5 9600X processor and an AMD Radeon RX 570 graphics card.

## Hardware Specifications

| Component | Model | Status |
| :--- | :--- | :--- |
| **CPU** | AMD Ryzen 5 9600X (6-Core) | Working (AMD Vanilla Patches) |
| **Motherboard** | MSI B850M-G | Working |
| **GPU** | Sapphire AMD Radeon RX 570 4GB | Working (Native Support) |
| **Ethernet** | Realtek 8126 (Dragon 5GbE) | **Unsupported** in macOS |
| **Storage 1 (Internal)** | Toshiba NVMe SSD | Disabled via `Kernel -> Block` (Incompatible) |
| **Storage 2 (External)** | WD HDD (USB) | Working (Used for macOS Install) |

## BIOS Settings
Before booting the USB, ensure the following BIOS settings are configured correctly:

- **Disable:**
  - Fast Boot
  - Secure Boot
  - CSM / Legacy Boot
  - IOMMU
  - Re-Size BAR Support
- **Enable:**
  - Above 4G Decoding
  - XHCI Hand-off
  - OS Type: UEFI (Windows UEFI Mode)

## Important Notes & Fixes Applied in this EFI
1. **6-Core CPU Patch:** The AMD kernel patches in `config.plist` (Specifically Patch 4 for Sequoia) have been manually adjusted with `ugYAAAA=` to match the exact 6 cores of the Ryzen 5 9600X to prevent Kernel Panics.
2. **SetupVirtualMap:** Kept as `True` specifically for the B850 chipset architecture.
3. **MCE Reporter Disabler:** Injected `AppleMCEReporterDisabler.kext` to prevent early Kernel Panics caused by the `MacPro7,1` SMBIOS probing AMD processors for memory errors.
4. **AMFI Fix for Sequoia:** Injected `AMFIPass.kext` and `-amfipassbeta` boot argument to pass the `Loading FileSet KC(s)` phase.
5. **GPU Acceleration:** `agdpmod=pikera` is included. `-radvesa` was removed to allow the RX 570 to initialize hardware acceleration natively during the installer phase.
6. **Network Limitations:** The onboard Dragon 5G LAN (Realtek 8126) is entirely unsupported by macOS at this time. Network access during installation requires USB Tethering, and an USB-to-Ethernet adapter (e.g., RTL8153).

## Credits
* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [AMD OS X](https://forum.amd-osx.com/)
* [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/issues/901)
