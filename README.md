# Hackintosh the ThinkPad X1 Carbon 4th Gen (OpenCore bootloader)

[![macOS](https://img.shields.io/badge/macOS-Big_Sur_11.7.7-blueviolet)](https://developer.apple.com/documentation/macos-release-notes)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.9.2-12AED6)](https://github.com/acidanthera/OpenCorePkg)
[![Model](https://img.shields.io/badge/Model-20FB*/20FC*-yellow)](https://psref.lenovo.com/Product/ThinkPad/ThinkPad_X1_Carbon_4th_Gen)
[![BIOS](https://img.shields.io/badge/BIOS-1.56-blue)](https://pcsupport.lenovo.com/hu/en/products/laptops-and-netbooks/thinkpad-x-series-laptops/thinkpad-x1-carbon-type-20fb-20fc/20fb/downloads/driver-list/component?name=BIOS%2FUEFI)
[![License](https://img.shields.io/badge/License-BSD_3-purple)](/LICENSE)

## Introduction

<details>  
<summary><strong>Getting started 📖</strong></summary>
</br>

**Meet the Bootloader:**

- [Why OpenCore](https://dortania.github.io/OpenCore-Install-Guide/why-oc.html)
- Dortania's [website](https://dortania.github.io)

**Recommended tools:**

- [OpenCore Configurator](https://mackie100projects.altervista.org/download-opencore-configurator/)
- Cross-platform GUI management tools for OpenCore [OCAT](https://github.com/ic005k/OCAuxiliaryTools)
- Py script that uses acidanthera's macserial to generate SMBIOS [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- The Swiss army knife of vanilla Hackintoshing [Hackintool](https://github.com/benbaker76/Hackintool)

**Resources**

- [OpenCore](https://github.com/acidanthera/OpenCorePkg)

</details>

<details>  
<summary><strong>My Hardware 💻</strong></summary>
</br>

| Model            | Thinkpad X1 Carbon Gen 4                                                                                   |
| :--------------- | :--------------------------------------------------------------------------------------------------------- |
| Processor        | Intel Core i5-6200U @ 2.30GHz                                                                              |
| Graphics         | Integrated Intel HD Graphics 520                                                                           |
| Memory           | 8GB Soldered 1866MHz DDR3, dual-channel                                                                    |
| Display          | 14" Full HD (1920x1080) IPS, non-touch                                                                     |
| Storage          | Lexar NM620 512GB M.2 PCIe NVMe SSD                                                                        |
| Ethernet         | Intel Ethernet Connection I219-LM (Jacksonville)                                                           |
| WLAN + Bluetooth | 11ac+BT, Intel® Dual Band Wireless-AC 8260NGW, 2x2 card                                                    |
| Camera           | HD720p resolution, low light sensitive, fixed focus                                                        |
| Audio support    | HD Audio, Conexant CX11852 codec, stereo speakers 1Wx2, dual array microphone, combo audio/microphone jack |
| Keyboard         | 6-row, JIS, spill-resistant, multimedia Fn keys, LED backlight                                             |
| Battery          | Integrated Lithium Polymer 4-cell (52Wh)                                                                   |

</details>

## Installation

<details>  
<summary><strong>How to install macOS</strong></summary>
</br>

1. [Create an installation media](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)
2. Download the [latest EFI folder](https://github.com/pichayakorn/thinkpad-x1c4-opencore/releases) and copy it into the EFI partition
3. Change your BIOS settings according to the table below
4. Boot from the USB installer (press `F12` to choose boot volume) and [start the installation process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html#booting-the-opencore-usb)

| Menu     |                   |                                 | Setting     |
| :------- | :---------------- | :------------------------------ | :---------- |
| Config   | USB               | UEFI BIOS Support               | `Enable `   |
|          | Power             | Intel SpeedStep Technology      | `Enable `   |
|          |                   | CPU Power Management            | `Enable `   |
|          | CPU               | Hyper-Threading Technology      | `Enable `   |
| Security | Security Chip     |                                 | `Disable `  |
|          | Memory Protection | Execution Prevention            | `Enable `   |
|          | Virtualization    | Intel Virtualization Technology | `Enable `   |
|          |                   | Intel VT-d Feature              | `Enable `   |
|          | Anti-Theft        | Computrace                      | `Disable `  |
|          | Secure Boot       |                                 | `Disable `  |
|          | Intel SGX         |                                 | `Disable `  |
|          | Device Guard      |                                 | `Disable `  |
| Startup  | UEFI/Legacy Boot  |                                 | `UEFI Only` |
|          | CSM Support       |                                 | `No`        |
|          | Boot Mode         |                                 | `Quick`     |

5. After fresh install, don't forget to replace downloaded `EFI` to fresh install `EFI` partition.

</details>

<details>  
<summary><strong>Enable Apple Services</strong></summary>
</br>

1. Run the following script in Terminal.

```sh
git clone https://github.com/corpnewt/GenSMBIOS && cd GenSMBIOS && chmod +x GenSMBIOS.command && ./GenSMBIOS.command
```

2. Type `1` for Install/Update MacSerial.
3. Type `2` ans select where the config.plist is located, `/EFI/OC/Config.plist`.
4. Type `3` to Generate SMBIOS, then press ENTER.
5. Type `MacbookPro13,1 5`, then press ENTER.

```diff
<key>PlatformInfo</key>
<dict>
   <key>Generic</key>
   <array>
      </dict>
         <key>AdviseFeatures</key>
         <true/>
         <key>MaxBIOSVersion</key>
         <false/>
         <key>SystemMemoryStatus</key>
         <string>Auto</string>
         <key>MLB</key>
+        <string>M0000000000000001</string>
         <key>ProcessorType</key>
         <integer>0</integer>
         <key>ROM</key>
         <data>ESIzRFVm</data>
         <key>SpoofVendor</key>
         <true/>
         <key>SystemProductName</key>
         <string>MacBookPro13,1</string>
         <key>SystemSerialNumber</key>
+        <string>W00000000001</string>
         <key>SystemUUID</key>
+        <string>00000000-0000-0000-0000-000000000000</string>
      </dict>
   </array>
</dict>
```

6. Save and reboot the system

</details>

## Status

<details>  
<summary><strong>What's working ✅</strong></summary>
</br>

- [x] CPU Power Management `~1W on IDLE`
- [x] Intel HD 520 Graphics `incuding graphics acceleration`
- [x] USB ports
- [x] Internal camera `working fine on FaceTime, Skype, Zoom and others`
- [x] Sleep / Wake / Shutdown / Reboot
- [x] Onelink+ Port with Intel Gigabit Ethernet support
- [x] Wifi, Bluetooth, Airdrop, Handoff, Continuity, Sidecar wireless `some functionalities may be buggy or broken on Intel WLAN cards`
- [x] iMessage, FaceTime, App Store, iTunes Store `Please generate your own SMBIOS`
- [x] Speakers and headphones combo jack
- [x] Battery management
- [x] Keyboard map and hotkeys with [YogaSMC](https://github.com/zhen-zen/YogaSMC)
- [x] Trackpad, Trackpoint and physical buttons `all macOS gestures working thanks to VoodooRMI`
- [x] SIP and FileVault 2 can be turned on
- [x] HDMI `with digital audio passthrough`
- [x] MiniDP
- [x] Micro SD Card Reader `slow r/w speed but works`

</details>

<details>  
<summary><strong>What's not working ⚠️</strong></summary>
</br>

- [ ] Safari DRM `Use Chromium engine to watch Apple TV+, Amazon Prime Video, Netflix and others`
- [ ] WWAN (needs to be implemented)
- [ ] Fingerprint Reader

</details>

<details>  
<summary><strong>Update tracker 🔄</strong></summary>
</br>

| Kext                                                                                           | Current |
| :--------------------------------------------------------------------------------------------- | :------ |
| [Lilu](https://github.com/acidanthera/Lilu/releases)                                           | 1.6.5   |
| [AppleALC](https://github.com/acidanthera/AppleALC/releases)                                   | 1.8.2   |
| [AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)                            | 2.1.0   |
| [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) | 2.2.0   |
| [IntelBluetoothInjector](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) | 2.2.0   |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)                               | 1.3.1   |
| [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)                   | 2.1.7   |
| [AirportE2200Ethernet](https://github.com/Mieze/AtherosE2200Ethernet/releases)                 | 2.2.2   |
| [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/releases)                       | 1.0.3   |
| [ECEnabler](https://github.com/1Revenger1/ECEnabler/releases)                                  | 1.0.3   |
| [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)                               | 1.0.7   |
| [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)                    | 2.4.2   |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents/releases)                       | 1.1.1   |
| [USBInjectAll](https://github.com/RehabMan/OS-X-USB-Inject-All)                                | 0.8.0   |
| [VoodooPS2Controller](https://github.com/acidanthera/VoodooPS2/releases)                       | 2.2.5   |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)                         | 1.6.4   |

</details>

## License

BSD-3-Clause license

Check out [LICENSE](/LICENSE) for more detail.
