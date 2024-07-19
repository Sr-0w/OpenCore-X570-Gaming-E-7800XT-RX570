![OpenCore Logo](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png)

# Ryzentosh EFI Guide 
_Asus ROG STRIX X570 Gaming-E | AMD Ryzen 7 3700X | RX 570 Active - RX 7800XT Disabled_

Step-by-step guide for setting up Ventura on an AMD Zen series CPU with the Asus ROG STRIX X570-E GAMING motherboard.

![Setup Image](https://raw.githubusercontent.com/Sr-0w/OpenCore-X570-Gaming-E-7800XT-RX570/main/IMAGES/screenshot.png)
> Screenshot of Ventura Hackintosh

## 🖥️ Hardware Specification

| Component      | Specification                                                 |
|----------------|---------------------------------------------------------------|
| **CPU**        | AMD Ryzen 7 3700X                                            |
| **Motherboard**| Asus ROG STRIX X570-E GAMING                                  |
| **RAM**        | 80GB G.Skill DDR4 (2x8GB + 2x 32GB) @ 3600MHz                           |
| **GPU**        | RX 570 (4GB) Active - RX 7800XT (16GB) Disabled                               |
| **SSD**        | Samsung SSD 970 EVO Plus (1TB NVMe)                           |
| **Power Supply**| Corsair 750W (Full Modular)                                  |
| **SMBIOS**     | MacPro7,1                                                    |
| **MacOS Version**| Sonoma 14.5                                               |
| **Opencore Version**| 1.0.0                                                      |

## ⚠️ Important Notice

Before attempting any modifications or configurations, it's imperative to familiarize yourself with the [OpenCore Documentation](https://dortania.github.io/OpenCore-Install-Guide/). Understanding the basics will help you troubleshoot issues and ensure a smoother setup process.

## 🛠️ Recommended Editing Tools

### ProperTree
For editing the core count hex values and other configurations in the plist files, we recommend using **ProperTree**. It's a versatile plist editor tailored for OpenCore configuration files.

**Download ProperTree**: [GitHub Repository](https://github.com/corpnewt/ProperTree)

### OpenCore Configurator
To generate and edit platform information, use the **OpenCore Configurator**.

**Download OpenCore Configurator**: [Download link](https://mackie100projects.altervista.org/download-opencore-configurator/)

**Note**: Always be cautious when using third-party tools. Ensure you have backups of your configuration files and verify any changes you make.

## 🔧 BIOS Configuration

Follow these steps to configure your BIOS:

- Start BIOS: Press Delete -> Choose [Enter Setup]
- Go to Exit -> Select [Load Optimised Defaults]
- Navigate to Ai Tweaker -> Set Ai Overclock Tuner to [D.O.C.P.]
- Under Advanced:
  - APM Configuration -> Set Power On By PCIe to [Disable]
  - PCI Subsystem Settings:
    - Set Above 4G Decoding to [Enabled]
    - Set Re-Size BAR Support to [Enabled]
  - USB Configuration -> Set Legacy USB Support to [Auto]

> **Key Points**:
> - If you're using a Skylake CPU from 0 to 1537, adjust the `FakeCPUID` to 0x0306A0. This ensures correct CPU information in "About This Mac".
> - `SmallTreeIntel82576.kext` is operational as of Monterey 12.0 Beta 8.
> - For BIOS versions from 4010 onwards, disable Power On By PCIe to prevent shutdown issues.

For OpenCore settings, refer to the OpenCore Configurator:

![Opencore Config Image](https://i.imgur.com/sSquwww.png)
> Click on generate under Platformnfo with OpenCore Configurator

## 🛠️ CPU Core Patching

Use the OpenCore kernel quirk `ProvideCurrentCpuInfo` to set the correct core count. It's compatible with 15h to 19h CPUs. Ensure you're on OpenCore 0.7.1 or later.

**Zen 4 Users**: Apply patching for `IOPCIFamily.kext`. This is crucial for Zen 4 stability and for MSI A520, B550, and X570 motherboards to boot macOS Monterey.

**Core Count Patch Guide**:
Find the `algrey - Force cpuid_cores_per_package` patches. Adjust only the Replace value according to your CPU's core count:

| Cores | Hex Value |
|-------|-----------|
| 4     | [04]      |
| 6     | [06]      |
| 8     | [08]      |
| 12    | [0C]      |
| 16    | [10]      |
| 24    | [18]      |
| 32    | [20]      |

- `B8000000 0000` => `B8 <core count> 0000 0000`
- `BA000000 0000` => `BA <core count> 0000 0000`
- `BA000000 0090` => `BA <core count> 0000 0090`
- `BA000000 00`   => `BA <core count> 0000 00`

For a 6-core example:

![6-Core Processor Example](https://i.imgur.com/BbGgsap.png)
> Modify using ProperTree

---

🚀 **Last Step**: Before using the new EFI, always reset your NVRAM.
