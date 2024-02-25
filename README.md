<p align="center">
  <img 
src="https://www.lenovo.com/medias/?context=bWFzdGVyfHJvb3R8MTA0NDcwfGltYWdlL3BuZ3xoNzAvaGI5LzExNTI2NjE0MzE5MTM0LnBuZ3xiMDFjNmZkZDg4YzBjNGU4NWM2YzU2Yzk1MjNhNGMxNzBjNjI4NTRmMDJkMWYyNTY4ZTQxYTU5MThkMTUwNDY0" 
width="600"/>
</p>

# Lenovo Legion 5 15ACH6H OpenCore Configuration  
## Supports macOS Ventura 13.4  
### OpenCore Version: 0.9.3 (DEV)  

## Table of Contents

*   [Specifications](#specifications)
*   [BIOS Setup](#bios-setup)
*   [What's Working](#whats-working)
*   [What's not Working](#whats-not-working)
*   [SSDTs Used](#ssdts-used)
*   [Credits](#credits)
*   [Screenshots](#screenshots)

## Specifications

Type | Spec | Status
:---------|:---------|:----------
Model Name      | Lenovo Legion 5 15ACH6H | ✅
CPU              | AMD Ryzen 5 5600H CPU @ 3.30GHz | ✅
RAM           | 16 GB 3200 MHz DDR4 | ✅
Internal Graphics Card | AMD Radeon Graphics (iGPU) (Renoir/Cezanne) | ✅
External Graphics Card | NVIDIA GeForce RTX 3060 | ❌
Wi-Fi             | Intel AX200 Wi-Fi 6 (802.11ax) | ✅
Ethernet          | Realtek RTL8111 | ✅
Audio       | Realtek ALC 257 | ✅

# Prerequisites

# BIOS Setup  

## Unlocking the advanced BIOS menu
* Step 1. Format a USB as FAT32
* Step 2. Download [SmokelessRuntimeEFIPatcher](https://github.com/SmokelessCPUv2/SmokelessRuntimeEFIPatcher/releases/tag/0.1.4c)
* Step 3. Place all of the files in SREP.zip in the root of the flash drive
* Step 4. Download the [Legion 5 <2022 SREP Config](https://github.com/SmokelessCPUv2/SREP-Community-Patches/blob/main/Configs/Legion_Insyde_BiosUnlock.cfg)
* Step 5. Place the SREP config in the root of the flash drive named **SREP_Config.cfg**
* Step 6. Boot from the newly made USB drive and wait for the laptop to reboot into the patched BIOS

## Disabling XHCI1
With the patched BIOS loaded follow these steps to disable XHCI1:
* Step 1. Head to the AMD CBS menu
* Step 2. Open FCH Common Options
* Step 3. Enter the USB Configuration Options submenu
* Step 4. Find XHCI1 Controller Enable
* Step 5. Set to Disabled
* Step 6. Hit F10 to save your changes and reboot

## Increasing iGPU VRAM
* Step 1. Enter BIOS and enable "Switchable Graphics"
* Step 2. Go to the configuration menu
* Step 3. Find iGPU VRAM size (Default is 512MB)
* Step 4. Set to 1GB or above (recommended by NootedRed)

## Disabling Secure Boot
Disbale Secure Boot from your BIOS if you want to install MacOS on an external USB drive.

## Making the EFI partition with OpenCore
* Step 1. Format a partition as FAT32 with a name like `EFI_MACOS` (you can use a USB drive but it will be needed to start MacOS each time you boot)
* Step 2. Download the "EFI" folder from this repo and place it in the root of your EFI partition
* Step 3. Add the MacOS installer in `the com.apple.recovery.boot` folder following the instructions from [Dortania's guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer).
* Step 4. Before booting, be sure to have a formatted partition (as FAT32 next to the EFI partition for example) to install MacOS on it (the MacOS installer doesn't found the free space on my drive). You will format this partition after in APFS from the MacOS installer.
* Step 4. Boot from your EFI partition, format your partition where you want MacOS installed, and proceed on installation.

## Postinstallation
* Step 1. Edit your `EFi\OC\config.plist` file and add this entry at the comment `<!-- Add your NootedRed entry here -->`
```
                <dict>
                    <key>Arch</key>
                    <string>Any</string>
                    <key>BundlePath</key>
                    <string>NootedRed.kext</string>
                    <key>Comment</key>
                    <string>V1.0.0</string>
                    <key>Enabled</key>
                    <true/>
                    <key>ExecutablePath</key>
                    <string>Contents/MacOS/NootedRed</string>
                    <key>MaxKernel</key>
                    <string></string>
                    <key>MinKernel</key>
                    <string></string>
                    <key>PlistPath</key>
                    <string>Contents/Info.plist</string>
                </dict>
```
* Step 2. Save and reboot

## What's working  
Type | Status
:---------|:----------
Turbo boost and CPU frequency stage |  ✅  
AMD Radeon Graphics (1GB)              |  ✅  
Audio Realtek ALC 257            |  ✅  
Realtek Ethernet RTL8111            |  ✅  
Intel AX200 Wi-Fi, iMessage...         |  ✅  
USB 3.0 and Type-C (with Port Map)        |  ✅  
Touchpad (14 gestures are working)   |  ✅  
Battery status   |  ✅  
Camera   |  ✅  
Shutdown / Reboot   |  ✅  
Fn shortcut keys   |  ✅  
Brightness control | ✅

## What's not working  
Type | Info | Status
:---------|:---------|:----------
HDMI | Broken in NootedRed | ❌
Airdrop, Sidecar | Intel Wi-Fi Doesn't Support | ❌
Sleep | Broken in NootedRed | ❌
CPU Power Management | Causes crashes on Ventura 13.4 | ❌
Graphics glitches under load | Issue with NootedRed | ❌

## SSDTs Used
### Note: Most SSDTs here were generated by [SSDTTime](https://github.com/corpnewt/SSDTTime)
  
SSDT | Info
:---------|:---------
SSDT-ALS0.aml | Creates a fake Ambient Light Sensor for keyboard backlight.
SSDT-EC.aml | Creates a fake EC for macOS.
SSDT-GPU-DISABLE.aml | Disables the GeForce RTX 3060.
SSDT-HPET.aml | Fixes IRQ conflicts in HPET.
SSDT-I2C.aml | Fixes trackpad nodes.
SSDT-PLUG-ALT.aml | Adds CPU ACPI entries for macOS.
SSDT-PLNF.aml | Fixes backlight control for NootedRed.
SSDT-USBX.aml | Injects USB power management.
SSDT-XOSI.aml | Spoofs macOS as Windows to enable ACPI features.

## Credits
[Kalkmann](https://github.com/kalkmann/Legion-5600H-Hackintosh) | Created a reference EFI for the AMD version of the Legion 5.  
[ExtremeXT](https://github.com/Xmingbai/Lenovo_Legion_5_Hackintosh) | Created the patches to make this possible.  
[Dortania](https://dortania.github.io/OpenCore-Install-Guide) | Created the awesome OpenCore guide.  
[Acidanthera](https://github.com/acidanthera) | Created most of the kexts.  
[NootInc](https://github.com/NootInc) | Created most of the AMD kexts (including NootedRed).  
[OpenIntelWireless](https://github.com/OpenIntelWireless) | Created Intel Wi-Fi + Bluetooth kexts.  
[Lunarixus](https://github.com/Lunarixus) | Created some of the SSDT 
patches.  

## Screenshots
![](https://raw.githubusercontent.com/Lunarixus/Legion5-15ACH6H-macOS/Ventura/Screenshots/system_info.png?token=GHSAT0AAAAAACDUIAG6CX2RJXRRLC5OX67EZEGWL6A)
