# Ryzentosh nitro 5 an515-43 r59w
<h1>Device</h1>      


| *category*  | *hardware*  |
| ------------- | ------------- |
| PROCESSOR:    | AMD ryzen 5 3550h 3.7ghz 4 core 8 threads             |<br>
|MOTHERBOARD:   | Octavia_PKS                                           |<br>
|BIOS:          | v1.12                                                 |<br>
|IGPU:          | VEGA 8 1.5GB VRAM(can be changed on UMAF)             |<br>
|DGPU:          | NVIDIA Geforce GTX 1650(disabled)<br>|
|RAM:           | 32gb ram 2666mhz dual channel(UMAF dram overclock)<br>|
|ETHERNET:      | Realtek rtl8111                                       |<br>
|AUDIO:         | AMD 17h/19h family/ALC 255                                |<br>
|WIFI&BLUETOOTH:| qualcomm atheros qca61x4                              |<br>
|DRIVE:         | M.2 NVME KINGSPEC NE-1TB                              |<br>
|KEYBOARD:      | PS/2 keyboard                                         |<br>
|TOUCHPAD:      | elan 0504 i2c hid

<h1>What's Working...</h1>

| Features  |
| ------------- |
|Audio & headphone jack|
|AMD CPU Power management|
|iGPU|
|Battery Management|
|Ethernet|
|HDMI|
|WebCam|
|Usb 3.0 + Type C|
|trackpad with geatures|


<h1>What's not Working...</h1>

| Features  |
| ------------- |
|microphone & microphone headphone jack(only audio, for mic only over usb)|
|smbus for fan speed reading|
|Dgpu|
|sleep|
|wi-fi&bluetooth|
|some specific Intel apps|

# Kexts included

Kext|Description
:----|:----
[AMDRyzenCPUPowerManagement.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|For [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[SMCAMDProcessor.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|For [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[AppleMCEReporterDisabler.kext](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)|Useful starting with Catalina to disable the AppleMCEReporter kext which will cause kernel panics on AMD CPUs and dual-socket systems.
[Lilu.kext](https://github.com/acidanthera/Lilu/releases)|Patch many processes, required for AppleALC, WhateverGreen, VirtualSMC and many other kexts.
[VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)|Emulates the SMC chip found on real macs, without this macOS will not boot.<br>Alternative is FakeSMC which can have better or worse support, most commonly used on legacy hardware.
[AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)|Used for AppleHDA patching, allowing support for the majority of on-board sound controllers.<br>AMD 15h/16h may have issues with this and Ryzen/Threadripper systems rarely have mic support.
[RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)|For Realtek's Gigabit Ethernet.<br>Sometimes the latest version of the kext might not work properly with your Ethernet. If you see this issue, try older versions.
[NVMeFix](https://github.com/acidanthera/NVMeFix/releases)|Used for fixing power management and initialization on non-Apple NVMe.
[RestrictEvents](https://github.com/acidanthera/RestrictEvents/releases)|Better experience with unsupported processors like AMD, Disable MacPro7,1 memory warnings and provide upgrade to macOS Monterey via Software Updates when available.

Kext|Description
:----|:----


## Special notes - Mapping CORES for AMD CPU

**Note for Zen 4:** Zen 4 (Ryzen 7000) requires patching for IOPCIFamily.kext. <br>
This patch is enabled by default. Please ensure that you've added it to your current config for Zen 4 stability. <br>
This patch also allows MSI A520, B550, and X570 boards to boot macOS Monterey and newer. <br>

Core Count patch needs to be modified to boot your system.<br>
Find the four `algrey - Force cpuid_cores_per_package` patches and alter the `Replace` value only.

|   macOS Version      | Replace Value | New Value |
|----------------------|---------------|-----------|
| 10.13.x, 10.14.x     | B8000000 0000 | B8 < Core Count > 0000 0000 |
| 10.15.x, 11.x        | BA000000 0000 | BA < Core Count > 0000 0000 |
| 12.x, 13.0 to 13.2.1 | BA000000 0090 | BA < Core Count > 0000 0090 |
| 13.3                 |  BA000000 00  | BA < Core Count > 0000 00 |

From the table above substitue `< Core Count >` with the hexadecimal value matching your physical core count. <br>
Do not use your CPU's thread count. <br>
See the table below for the values matching your CPU core count.

| Core Count | Hexadecimal |
|------------|-------------|
|   4 Core   |     `04`    |
|   6 Core   |     `06`    |
|   8 Core   |     `08`    |
|   12 Core  |     `0C`    |
|   16 Core  |     `10`    |
|   24 Core  |     `18`    |
|   32 Core  |     `20`    |

So for example, a user with a 6-core processor should use these `Replace` values: `B8 06 0000 0000` / `BA 06 0000 0000` / `BA 06 0000 0090` / `BA 06 0000 00`

**Note:** *MacOS Monterey installation requires `Misc -> Security -> SecureBootModel` to be disabled in the config.<br />Also TPM needs to be disabled in the BIOS. Both can be enabled after install.*

## Special notes - Mapping USB

For usb mapping if usb toolbox make your hack because of 2 xhc controller, then you have to disable one of the controller on bios or via SMOKELESS UMAF, or use genericUSBXHCI from rehabman(using this you cant use usb drives, any complex usb thing than usb keyboard and mouse, but for that you can use a special fork from genericUSBXHCI (https://github.com/RattletraPM/GUX-RyzenXHCIFix/releases/tag/v1.3.0b1-ryzenxhcifix)

# BIOS Settings

### Disable
- Fast Boot
- Secure Boot

- # References
https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html

##CREDITS
    
**[Acidanthera](https://github.com/acidanthera)** <br> 
for making[OpenCore](https://github.com/acidanthera/OpenCorePkg) [VirtualSMC](https://github.com/acidanthera/VirtualSMC), [Lilu](https://github.com/acidanthera/Lilu), [WhateverGreen](https://github.com/acidanthera/WhateverGreen)

**[Gabriel Luchina](https://github.com/luchina-gabriel)**<br>
for making [BASE-EFI-AMD-RYZEN-THREADRIPPER](https://github.com/luchina-gabriel/BASE-EFI-AMD-RYZEN-THREADRIPPER)

**[Dortania](https://dortania.github.io/)** <br> 
for making [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) 
and [OpenCore Post-Install](https://dortania.github.io/OpenCore-Post-Install/)

**[RattletraPM](https://github.com/RattletraPM/GUX-RyzenXHCIFix)** <br>
for making possible to use any complex usb devices on ryzen laptops wtih xhc controller issues
