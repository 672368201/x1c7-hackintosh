# OpenCore for macOS on ThinkPad X1 Carbon 7th, 20R1-0005CD

An OpenCore bootloader to boot macOS on Lenovo ThinkPad X1 Carbon 7th Generation, Type 20R1-0005CD

**Status: Work In Progress | Stable | Daily driver**

[![macOS](https://img.shields.io/badge/macOS-Monterey-blueviolet.svg)](https://www.apple.com/macos/monterey/)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.8.0-blue.svg)](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.0)
[![Model](https://img.shields.io/badge/Model-20R1-red)](https://www.lenovo.com/us/en/p/laptops/thinkpad/thinkpadx1/x1-carbon-gen-7/22tp2txx17g)

This repository is forked from several X1C Hackintosh repositories (See **REFERENCES**).

This README file is modified from [HJebbour/ThinkPad-X1C8-Hackintosh/README.md](https://github.com/HJebbour/ThinkPad-X1C8-Hackintosh/blob/main/README.md).

**DISCLAIMER:**
As you embark on your Hackintosh journey you are encouraged to **READ** the entire README and [Dortania](https://dortania.github.io/getting-started/) guides before you start.

This X1C7 Hackintosh project aims to be an all-in-one maintained hub for Opencore-based hackintoshes on the ThinkPad X1 Carbon 7th. In short, this X1C7-Hackintosh is very stable and is currently my daily driver. I fully recommend this project to anyone looking for a MacBook alternative.

You can find a wealth of knowledge on [Reddit](https://www.reddit.com/r/hackintosh/), [TonyMacX86](https://www.tonymacx86.com) or [Google](https://www.google.com).

Should you find an error, or improve anything, be it in the config itself or in the my documentation, please consider opening an issue or a pull request to contribute.

**I am not responsible for any damages you may cause.**

## Summary

<details>  

<summary><strong>WORKING ✅</strong></summary>
<br>

> ### Multimedia
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Audio Output | ✅ | `AppleALC.kext` with `layout-id` = `71` | - |
| Audio Speakers | ✅ | `AppleALC.kext` with `layout-id` = `71` | You have to manually aggregate the two output using "Audio MIDI Setup" to have 4 speakers working |
| Audio Input | ✅ | `AppleALC.kext` with `layout-id` = `71` | Headset microphone is inconsistent and needs more testing |
| Automatic Headphone Output Switching | ✅ | `AppleALC.kext` with `layout-id` = `71` | - |
| Full Graphics Acceleration (QE/CI) | ✅ | `WhateverGreen.kext` with `AAPL,ig-platform-id` = `0500A63E` and `device-id` = `A63E0000` | To fake Intel Iris Plus Graphics 645, MacBookPro16,3's native iGPU |

> ### Power
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Battery | ✅ | `ECEnabler.kext` | - |
| CPU Power Management (SpeedShift) | ✅ | `CPUFriend.kext` with `CPUFriendDataProvider.kext` | - |
| iGPU Power Management | ✅ | `SSDT-PLUG.aml` | - |
| NVMe Drive Battery Management | ✅ | `NVMeFix.kext` | Improve NVMe drive power management |
| Hibernation | ✅ | `HibernateMode` = `Auto` in OpenCore and `hibernatemode` = `0` in macOS | - |

> ### Connectivity
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| WiFi | ✅ | `AirportIltwm.kext` | - |
| Bluetooth | ✅ | `BlueToolFixup.kext`, `IntelBluetoothFirmware.kext` and `UTBMap.kext` | Mouse and Keyboard not working via Bluetooth |
| Ethernet | ✅ | `IntelMausi.kext` | - |
| HDMI 1.4 | ✅ | BusID patching | Hotplug with 4K Resolution |
| USB 2.0 / USB 3.0 | ✅ | `UTBMap.kext` | Create your own UTBMap.kext using [USBToolBoxᵇᵉᵗᵃ](https://github.com/USBToolBox/tool) |
| USB 3.1 (Type-C) | ✅ | `UTBMap.kext` and enable Thunderbolt 3 in BIOS | Hotplug is working |
| USB Power Properties in macOS | ✅ | - | - |
| ThinkPad USB-C Docking Station | ✅ | - | Work smoothly |

> ### Peripherals
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Brightness Adjustments | ✅ | `SSDT-PNLF.aml`, `BrightnessKeys.kext`, `WhateverGreen.kext` with `enable-backlight-smoother` | `enable-backlight-smoother` property is optional for smoother birghtness adjustments |
| TrackPoint | ✅ | `VoodooPS2Controller.kext` | - |
| TrackPad | ✅ | `VoodooI2C.kext` and `VoodooI2CHID.kext` | - |
| Built-in Keyboard | ✅ | `VoodooPS2Controller.kext` | - |
| Webcam | ✅ | `UTBMap.kext` | - |

> ### macOS Continuity
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| iCloud, iMessage, FaceTime | ✅ | Valid SMBIOS, Whitelisted Apple ID | See [Fixing iMessage and other services with OpenCore](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html) |
| Handoff | ✅ | - | - |
| Universal Clipboard | ✅ | - | - |
| SMS & Phone Call via iPhone | ✅ | - | - |
| AirPlay to Mac | ✅ | - | - |

> ### Miscellaneous
| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Multiboot | ⚠️ | - | No sound in Windows booting from OpenCore (See [Multiboot with OpenCore](https://dortania.github.io/OpenCore-Multiboot/) to setup multiboot) |

</details>  

<details>  
<summary><strong>NOT WORKING ❌</strong></summary>
<br>

| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Fingerprint Reader | ❌ | - | Will never work |
| Wireless WAN | ❌ | `DISABLED` in BIOS to save power. | Unable to investigate as I have no need and my model did not come with WWAN |
| DRM | ❌ | iGPU | DRM is broken with iGPUs |
| Internal Microphone | ❌ | - | I hope it will work one day |
| Fan Control / Multimedia Keys | ❌ | `YogaSMC.kext` | YogaSMC.kext needs to be updated in order to work with X1C7 Hardware |
| Continuity Camera | ❌ | - | Not working with Intel cards |
| AirDrop | ❌ | - | Not working with Intel cards |
| Apple Watch Auto Unlock | ❌ | - | Not working with Intel cards |
| Instant Hotspot | ❌ | - | Not working with Intel cards |

</details>  

<details>  
<summary><strong>UNTESTED ⚠️</strong></summary>
<br>

| Feature | Status | Dependency | Remarks |
| :------ | ------ | ---------- | ------- |
| Thunderbolt 3 | ⚠️ | - | No device to test |
| Boot chime | ⚠️ | - | Not yet configured |
| FireVault 2 | ⚠️ | - | Not yet tested |
| Sidecar | ⚠️ | - | No device to test |
| Continuity Markup and Sketch | ⚠️ | - | No device to test |

</details> 

<details>  
<summary><strong>TO-DO ⏳</strong></summary>
<br>

| Feature | Status | Remarks |
| :------ | ------ | ------- |
| Battery Life | ⏳ | Between 3 and 4 hours but it still takes time to thoroughly test the battery life and compare it with Windows 11 |

</details>

## Introduction

<details> 
<summary><strong>THIS IS NOT A GUIDE!</strong></summary>
</br>

This is not a guide. It shoud only be used as a reference. I provide some tips and tricks I learned on my journey in building a hackintosh. The best way of using this is as a supplement to the OpenCore guide. If you have questions about how to setup your specific hardware, are unclear about what to do, or would like to see the settings I've used.

I understand that some may simply add the OC and Boot folders to their EFI folder. For clarity the EFI partition needs a folder called EFI that contains the Boot and OC folder.

```EFI
EFI / ESP (Drive or partition)
    ├──EFI
        ├── BOOT
        ├── OC
```

It should work and your X1C7 should boot and work fine. **You will at minimum need to generate SMBIOS values if you want Apple services to work.** Note that all error reporting/logging has been turned off in the config.plist. You will have a difficult time trouble shooting with the setup provided. You can easily turn on the error reporting and logging if you follow the Dortania guide. Best of luck.

> **NOTE** if you simply wish to copy my EFI please do the following:
>
>1. [Generate SMBIOS values](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake-plus.html#nvram) and add them in the config.plist (Use MacBookPro16,3)
>2. Ensure the value of `ShowPicker` is  `true` in the config.plist file to provide the opencore menu when booting. 
>3. Prepare your install [USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)
>4. Move the entire EFI folder (with your modifications) to the proper partition on your [USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#setting-up-opencore-s-efi-environment) (or [SSD](https://dortania.github.io/OpenCore-Post-Install/universal/oc2hdd.html) once the install is complete).
>5. [Install](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html#double-checking-your-work) - You'll need to select F12 to get the boot menu options and **boot from the USB each time the computer restarts** until you've copied the EFI folder onto the hard drive. You may also need to select the correct boot option during install.

</details>  

<details> 
<summary><strong>THIS IS A GUIDE!</strong></summary>
</br>

**The one and only guide to install macOS, provided by [Dortania](https://dortania.github.io/OpenCore-Install-Guide/)**

</details>  

<details>
<summary><strong>HARDWARE</strong></summary>

### Lenovo ThinkPad X1 Carbon 7th Generation, Type 20R1-0005CD

These are relevant components on my machine which may differ from yours, keep these in mind as you will need to adjust accordingly, depending on your machine's configuration.

| Category  | Component | Note |
| --------- | --------- | ---- |
| Processor | Intel® Core™ i7-10710U CPU @ 1.10 GHz | 6 Cores, 12 Threads, Base Frequency 1.10 GHz, Max Turbo Frequency 4.70 GHz, TDP 15W |
| Graphics | Intel® UHD Graphics 620 (Intel® Comet Lake-U v1 GT2) | Base Frequency 300 MHz, Max Dynamic Frequency 1.15 GHz, Video Max Memory 32GB, Max Resolution 4096 x 2304@24Hz, 3 Displays Supported |
| Memory | SK Hynix 8GB LPDDR3 2133MHz x2 | 16GB in total, soldered memory, not upgradable |
| Storage | Toshiba KXG6AZNV512G | BiCS FLASH™ TLC, M.2 2280-S2 Single-sided, PCIe® Gen3 x4, NVMe™ 1.3a |
| Audio Chip | Realtek® ALC3286 Codec (Realtek® ALC285) | High Definition (HD) Audio |
| Camera | AzureWave (UVC Camera, Vendor ID 5075, Product ID 22202) | 720p, with ThinkShutter, fixed focus |
| Battery | SMP 02DL005 (Integrated Li-Polymer 4c 51Wh) | Supports Rapid Charge (charge up to 80% in 1hr) with 65W AC adapter |
| Display | Lenovo LP140WF9-SPF1 (LEN40A9) | 14.0" (355mm) HDR HD (1920 x 1080) |
| Input | PS2 Keyboard & Synaptics I2C HID TrackPad | - |
| Ethernet | Intel® Ethernet Connection (10) I219-V (non-vPro models) | Gigabit Ethernet, RJ45 via optional ThinkPad Ethernet Extension Adapter Gen 2 |
| Wireless | Intel® Wireless-AC 9560 160MHz | 802.11ac Dual Band 2x2 Wi-Fi + Bluetooth 5.1 |
| Ports | 1x USB 3.1 Gen 1</br>1x USB 3.1 Gen 1 (Always On)</br>2x USB-C 3.1 Gen 2 / Thunderbolt 3 (support data transfer, Power Delivery and DisplayPort™ 1.2)</br>1x HDMI 1.4b</br>1x Ethernet extension connector</br>1x Headphone / Microphone combo jack (3.5mm)</br>1x Side docking connector | - |

Refer to [ThinkPad X1 Carbon (7th Gen) Specs](https://psref.lenovo.com/syspool/Sys/PDF/ThinkPad/ThinkPad_X1_Carbon_7th_Gen/ThinkPad_X1_Carbon_7th_Gen_Spec.PDF) for possible stock configurations.

</details>  

<details>

<summary><strong>SOFTWARE</strong></summary>
<br>

| Component | Version |
| --------- | ------- |
| [OpenCore](https://github.com/acidanthera/OpenCorePkg) | [0.8.0](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.0) [(Release)](https://github.com/acidanthera/OpenCorePkg/releases/download/0.8.0/OpenCore-0.8.0-RELEASE.zip) |
| [macOS Monterey](https://www.apple.com/macos/monterey/) | 12.2.1 (21D62) |

</details>

<details><summary><strong>UEFI DRIVERS</strong></summary>
<br>

| Component | Version |
| --------- | ------- |
| AudioDxe.efi | OpenCorePkg 0.8.0 |
| OpenCanopy.efi | OpenCorePkg 0.8.0 |
| OpenHfsPlus.efi | OpenCorePkg 0.8.0 |
| OpenRuntime.efi | OpenCorePkg 0.8.0 |

</details>

<details>
<summary><strong>ACPI</strong></summary>
<br>

| Component | Source |
| --------- | ------ |
| [SSDT-AWAC.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html) | [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-AWAC.aml), [SSDTTime](https://github.com/corpnewt/SSDTTime) |
| [SSDT-PLUG.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) | [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml), [SSDTTime](https://github.com/corpnewt/SSDTTime) |
| [SSDT-PNLF.aml](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) | [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml) |
| [SSDT-USBX.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html) | [Prebuilt](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-USBX.aml) |
| [SSDT-XOSI.aml](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/trackpad.html) | [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-XOSI.aml) |

</details>

<details>
<summary><strong>KEXT</strong></summary>
<br>

| Component | Version |
| --------- | ------- |
| [AirportItlwm.kext](https://github.com/OpenIntelWireless/itlwm) | [2.1.0](https://github.com/OpenIntelWireless/itlwm/releases/tag/v2.1.0) |
| [AppleALC.kext](https://github.com/acidanthera/AppleALC) | [1.7.1](https://github.com/acidanthera/AppleALC/releases/tag/1.7.1) |
| [BlueToolFixup.kext](https://github.com/acidanthera/BrcmPatchRAM) | [2.6.1](https://github.com/acidanthera/BrcmPatchRAM/releases/tag/2.6.1) |
| [BrightnessKeys.kext](https://github.com/acidanthera/BrightnessKeys) | [1.0.2](https://github.com/acidanthera/BrightnessKeys/releases/tag/1.0.2) |
| [CPUFriend.kext](https://github.com/acidanthera/CPUFriend) | [1.2.5](https://github.com/acidanthera/CPUFriend/releases/tag/1.2.5) |
| [CPUFriendDataProvider.kext](https://github.com/corpnewt/CPUFriendFriend) | 1.0.0 |
| [ECEnabler.kext](https://github.com/1Revenger1/ECEnabler) | [1.0.2](https://github.com/1Revenger1/ECEnabler/releases/tag/1.0.2) |
| [IntelBluetoothFirmware.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) | [2.1.0](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases/tag/v2.1.0) |
| [IntelBluetoothInjector.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) | [2.1.0](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases/tag/v2.1.0) |
| [IntelMausi.kext](https://github.com/acidanthera/IntelMausi) | [1.0.7](https://github.com/acidanthera/IntelMausi/releases/tag/1.0.7) |
| [Lilu.kext](https://github.com/acidanthera/Lilu) | [1.6.0](https://github.com/acidanthera/Lilu/releases/tag/1.6.0) |
| [NVMeFix.kext](https://github.com/acidanthera/NVMeFix) | [1.0.9](https://github.com/acidanthera/NVMeFix/releases/tag/1.0.9) |
| [SMCBatteryManager.kext](https://github.com/acidanthera/VirtualSMC) | [1.2.9](https://github.com/acidanthera/VirtualSMC/releases/tag/1.2.9) |
| [SMCProcessor.kext](https://github.com/acidanthera/VirtualSMC) | [1.2.9](https://github.com/acidanthera/VirtualSMC/releases/tag/1.2.9) |
| [SMCSuperIO.kext](https://github.com/acidanthera/VirtualSMC) | [1.2.9](https://github.com/acidanthera/VirtualSMC/releases/tag/1.2.9) |
| [USBToolBox.kext](https://github.com/USBToolBox/kext) | [1.1.1](https://github.com/USBToolBox/kext/releases/tag/1.1.1) |
| [UTBMap.kext](https://github.com/USBToolBox/tool) | 1.1 |
| [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC) | [1.2.9](https://github.com/acidanthera/VirtualSMC/releases/tag/1.2.9) |
| [VoodooI2C.kext](https://github.com/VoodooI2C/VoodooI2C) | [2.7](https://github.com/VoodooI2C/VoodooI2C/releases/tag/2.7) |
| [VoodooI2CHID.kext](https://github.com/VoodooI2C/VoodooI2C) | [2.7](https://github.com/VoodooI2C/VoodooI2C/releases/tag/2.7) |
| [VoodooPS2Controller.kext](https://github.com/acidanthera/VoodooPS2) | [2.2.8](https://github.com/acidanthera/VoodooPS2/releases/tag/2.2.8) |
| [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen) | [1.5.8](https://github.com/acidanthera/WhateverGreen/releases/tag/1.5.8) |

</details>

<details>
<summary><strong>REFERENCES</strong></summary>
<br>

- X1C8-Hackintosh repositories:
  - [HJebbour/ThinkPad-X1C8-Hackintosh](https://github.com/HJebbour/ThinkPad-X1C8-Hackintosh)
	
- X1C7-Hackintosh repositories:
  - [suhrmann/x1c7-hackintosh](https://github.com/suhrmann/x1c7-hackintosh)
  - [aidanchandra/x1c7-hackintosh](https://github.com/aidanchandra/x1c7-hackintosh)
  - [seven-of-eleven/Lenovo-ThinkPad-X1C7-OC-Hackintosh](https://github.com/seven-of-eleven/Lenovo-ThinkPad-X1C7-OC-Hackintosh)
  - [huyhoang8398/x1c7-hackintosh-20R1](https://github.com/huyhoang8398/x1c7-hackintosh-20R1)
  - [EequalsMCsquare/ThinkPad-X1C7-OpenCore](https://github.com/EequalsMCsquare/ThinkPad-X1C7-OpenCore)
	
- X1C6-Hackintosh repositories:
  - [tylernguyen/x1c6-hackintosh](https://github.com/tylernguyen/x1c6-hackintosh)
  - [benbender/x1c6-hackintosh](https://github.com/benbender/x1c6-hackintosh)
  - [zhtengw/EFI-for-X1C6-hackintosh](https://github.com/zhtengw/EFI-for-X1C6-hackintosh)

</details>  

<details> 
<summary><strong>CREDITS</strong></summary>

### Credit to all these great people whom I don't know but have made my hackintosh dreams a reality:

- [Apple](https://apple.com) for [macOS](https://www.apple.com/macos)
- The guys from [Acidanthera](https://github.com/acidanthera) that make this possible
- [ben9923](https://github.com/ben9923) for [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C)
- [CorpNewt](https://github.com/corpnewt) for [CPUFriendDataProvider](https://github.com/corpnewt/CPUFriendFriend)
- [headkaze](https://github.com/headkaze) for [Hackintool](https://github.com/headkaze/Hackintool)
- [Mieze](https://github.com/Mieze) for [IntelMausiEthernet](https://github.com/Mieze/IntelMausiEthernet)
- [OpenIntelWireless](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) for [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware)
- [USBToolBox](https://github.com/USBToolBox) for [tool](https://github.com/USBToolBox/tool) and [kext](https://github.com/USBToolBox/kext)
- People at [r/hackintosh](https://www.reddit.com/r/hackintosh/) for their advice and help
- And every other contributor

</details>  

## Before Installation

<details><summary><strong>UEFI SETTINGS</strong></summary>
<br>

**Security**

- **Secure Boot**
  - `Secure Boot` **Disabled**

</details>  

<details><summary><strong>SMBIOS</strong></summary>
<br>

Use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to create your own PlatformInfo based on your preferred model.

- MacBookPro16,3 -`What I used`
- MacBookPro16,2 -`Used by others`

**Note:** If you use a different SMBIOS model than the MacbookPro16,3 that I've used. The provided USB mapping will not work.  You will need to edit the `UTBMap.kext` file.  You can right click on the file and select **Show Package Contents**.  From there you can open the Info.plist file in ProperTree and change MacBookPro16,3 to whatever Model ID you've chosen. This should provide a working UTBMap.kext.

</details>

<details><summary><strong>KEYBOARD LAYOUT</strong></summary>
<br>

Either add as a `String` or as a `Data` (HEX Data [ProperTree](https://github.com/corpnewt/ProperTree))

Format is lang-COUNTRY:keyboard

🇺🇸 | [0] en_US - U.S --> en-US:0 --> (656e2d55 533a30 in HEX)

| Key | Type | Value |
| --- | ---- | ----- |
| prev-lang:kbd | String | en-US:0 |


Pick your keyboard layout here:

[AppleKeyboardLayouts.txt](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)

</details>

## Post-Install

<details><summary><strong>TRACKPAD</strong></summary>
<br>

To improve the Trackpad in macOS, you need to enable `Tap to click` in `System Preferences -> Trackpad`.

</details>  

<details><summary><strong>HiDPI</strong></summary>
<br>
	
Use [one-key-hidpi](https://github.com/xzhih/one-key-hidpi) to simulate macOS HiDPI on a non-retina display, and have a "Native" Scaled in `System Preferences -> Displays`.

</details>   

<details>  
<summary><strong>POWER MANAGEMENT</strong></summary>
<br>

Use [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend) to generate CPUFriendDataProvider.kext for your machine or use those I've provided. Highly recommended that you use power management.

</details>

<details>  
<summary><strong>AUDIO</strong></summary>
<br>

Using the Layout ID 71 will enable the 4 speakers (Top front & Bottom rear) in **System Preferences>Sound** allowing you to select either set of speakers (Two Output). To combine the two you'll need to open Audio MIDI Setup and create `Multi-Output Device` with both sets of speakers. Unfortunately you can't control natively the volume of an Aggregate Device with the volume keys. You'll need to install [AggregateVolumeMenu](https://github.com/adaskar/AggregateVolumeMenu).

</details>
