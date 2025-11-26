# In-depth-Windows-tweaking

This guide covers tweaks beyond "advanced" PC optimization focusing on Windows settings that are more on the experimental side. Basic BIOS and Windows settings are widely documented, so they’re not included here. Most advanced tweaks are in my `.bat` files.<br>

You can search for things you are interested in by searching for keywords like Privacy, Telemetry, CoreParking, Kernel settings, while undefined ones are in Miscellaneous.

 <mark>Always use at least 3200 DPI if you can.<br>
 <mark>If you used the scripts from the tweaks folder, do not set a new default task manager, it increases latency.

&nbsp;&nbsp;&nbsp;https://github.com/Hyyote/files-/tree/main/tweaks

> **Note:** If you require components for games like Valorant or FACEIT, or need Windows Update support, this guide is not for you.

---

## 1. The Operating System (ranting, feel free to skip)

   - Linux isn't suitable for competitive gaming because tweaks don't go as deep as the ones you can do in Windows. Distros are similar in latency and input feel, which I found to be subpar compared to any version of Windows.

   - I used to recommend Windows 7 and Windows 10 1803 specifically, but I have to admit that later versions are superior only because of perceived stability, higher performance and better software compatibility at the moment, but all of them have higher idle latency. I don't find much difference between Windows 10 22H2 and Windows 11 23H2/24H2.<br>

   - Windows 11 always used to feel smoother on the desktop but when I ran tests and played games, the two felt identical.<br>

   - Windows 10 is more resilient when it comes to tweaks and it can be stripped down to its bones without sacrificing any functionality (22 services running), while Windows 11 needs UWP app support, StateRepository, DWM and many other things running to ensure nothing breaks and can be taken down to around 31 services.

---

## 2. Windows Tweaks

   - First, install a version of Windows you like, it can be a stock install or a custom one, however I really dislike having a lot of things running in the background in the first place,
so I recommend one that is pre-tweaked such as my NTLite builds or Tiny11

   - Install Windows without internet connection until you ensure all the update services and automatic driver updates are disabled

   - Disable audio enhancements in the sound control panel
<br>

**Import a power plan**<br>

Recommendations:
   - [BEYOND PERFORMANCE](https://github.com/Hyyote/files-/blob/main/Power%20plans/beyond.pow)
   - [BEYOND for AMD processors that need some auto settings for optimal performance](https://github.com/Hyyote/files-/blob/main/Power%20plans/beyondamd.pow)
<br>

Use AppxPackagesManager to clean up apps you don't need.


<br>

**Autoruns**: unhide Windows services (more can be disabled but these are the ones I find safe to disable, even though my NTLite preset and .bat scripts disable all of the unndeeded ones)

   - services: disable Appinfo, ApMgmt, AppReadiness AppXSvc, ApxSvc, BITS, Bluetooth related, CDP services, ClipSVC, CredentialEnrollment services, DeviceFlow services, KeyIso, PlugPlay, sppsvc StorSvc, UDK related, WMI, Wpn services

   - drivers: disable amd related (if not needed for AMD), AppleSSD, Bluetooth related, HidBatt, HidBth, i8042prt, Microsoft_Bluetooth, RFCOMM, swenum, WacomPen

<br>

**BCDEdits**:<br>
<br>
There are ongoing debates about which is the right configuration for each version of Windows, but I got the best results with these commands with HPET disabled in BIOS and Windows.<br>
<br>
*Note: "BCDEDIT /set xsavedisable Yes" disables AVX*
   ```batch
BCDEDIT /set nx AlwaysOff >NUL 2>&1
BCDEDIT /set ems No >NUL 2>&1
BCDEDIT /set bootems No >NUL 2>&1
BCDEDIT /set integrityservices disable >NUL 2>&1
BCDEDIT /set tpmbootentropy ForceDisable >NUL 2>&1
BCDEDIT /set bootmenupolicy Legacy >NUL 2>&1
BCDEDIT /set debug No >NUL 2>&1
BCDEDIT /set hypervisorlaunchtype Off >NUL 2>&1
BCDEDIT /set disableelamdrivers Yes >NUL 2>&1
BCDEDIT /set isolatedcontext No >NUL 2>&1
BCDEDIT /set allowedinmemorysettings 0x0 >NUL 2>&1
BCDEDIT /set vm No >NUL 2>&1
BCDEDIT /set vsmlaunchtype Off >NUL 2>&1
BCDEDIT /set x2apicpolicy Enable >NUL 2>&1
BCDEDIT /set configaccesspolicy Default >NUL 2>&1
BCDEDIT /set MSI Default >NUL 2>&1
BCDEDIT /set usephysicaldestination No >NUL 2>&1
BCDEDIT /set usefirmwarepcisettings No >NUL 2>&1
BCDEDIT /set tscsyncpolicy Enhanced >NUL 2>&1
BCDEDIT /set disabledynamictick Yes >NUL 2>&1
BCDEDIT /set useplatformtick Yes >NUL 2>&1
BCDEDIT /deletevalue useplatformclock >NUL 2>&1
BCDEDIT /set useplatformclock False >NUL 2>&1
BCDEDIT /set uselegacyapicmode No >NUL 2>&1
BCDEDIT /set sos No >NUL 2>&1
BCDEDIT /set pae ForceDisable >NUL 2>&1
BCDEDIT /set maxproc No >NUL 2>&1
BCDEDIT /set restrictapicluster 0 >NUL 2>&1
BCDEDIT /set pciexpress forcedisable >NUL 2>&1
BCDEDIT /set xsavedisable Yes >NUL 2>&1
```
   - Device Manager:
View -> Devices by type<br>
For your storage device under Disk drives, turn off write caching by going into Properties -> Policies<br>

   - Disable power saving on every device with a script or manually, since it takes around one minute to do
     (https://github.com/Hyyote/files-/tree/main/Disable%20power%20saving)

   - Disable unnecessary devices: (System Management BIOS, PCI Express Root Ports, ISA Bridge, PCI standard RAM Controller, generic software components, unused usb devices, generic pnp monitor)
Show hidden devices, disable Motherboard resources

A properly broken device manager after Windows and hidden BIOS tweaks can look like this:

<img src="Images/1.jpg" alt="Logo" width="400" height="500"/>

<img src="Images/2.jpg" alt="Logo" width="400" height="500"/>

<img src="Images/3.jpg" alt="Logo" width="400" height="500"/>

<br>

**Interrupt Affinities**

   - The devices worth assigning to separate cores are:<br>
   
USB host controller<br>
GPU<br>
<br>

   - Use benchmarks to determine which core gets you the highest performance for the GPU.<br>
   
   - In Device Manager, the PCI to PCI Bridge directly above your GPU entry also has to be bound to the same core.<br> Check its location by right clicking on the device -> Properties -> Location: PCI Bus 1-0-0 for example.

   - Affinities don't do anything for storage controllers
---

## 2.5. Poorly documented tweaks


<br>

**Delete ICC Color Profiles**

   - This can only be done with System Privileges, so it's best to use NSudo or something similar.<br>
colorcpl -> All Profiles -> delete everything until the page is blank

<br>

**Display scaling and custom resolutions**

   - It's generally recommended to use display scaling and native resolutions. This can only be achieved by creating custom resolutions in Custom Resolution Utility. Confirm display scaling being used by checking the active resolution in the monitor's menu.
   - Windows has settings for scaling but by creating a custom resolution, the result will always be display scaled, however it's ideal to set it anyway.

   - HKLM\SYSTEM\ControlSet001\Control\GraphicsDrivers\Configuration\<DisplayID>\00\00<br>

change Scaling to 2<br>

   - 1: Identity scaling
   - 2: No scaling
   - 3: Full-screen
   - 4: Aspect ratio

<br>

As for Custom Resolution Utility, I recommend deleting every value and adding resolutions to the Detailed Resolutions tab, using Exact Reduced timings with the monitor's highest supported refresh rate.
It's worth trying to lower the Vertical Total setting in the Manual tab before running restart64.exe.

<img src="Images/5.png" alt="Logo" width="650" height="500"/>

<br>

**Device Cleanup**

   - This one is hugely overlooked. There are only benefits to cleaning up unused entries if you don't use multiple devices that rely on Windows settings instead of onboard memory.

   - Device Manager -> View -> Devices by connection -> Show hidden devices

   - You can bulk remove them with: https://www.uwe-sieber.de/files/DeviceCleanup_x64.zip (use at your own risk, since the tool isn't open source)

Hidden devices should be checked on every startup.

**RWEverything**

Some of these settings might be already set in the BIOS, however I've seen a lot of strange behaviour in Windows regarding clock speeds and power saving features changing without a clear explanation.<br>

   - Disable PCIe power saving features (ASPM) on your device for lower latency. Replace BAR1 with your own BAR1 from RWEverything and replace the four digits at the end.<br>
W16 0xB0000004+0x2020 0x0  → W16 0xB0002024 0x0 (this is just an example)<br>

W16 BAR1+2020 0x0<br>
W16 BAR1+2040 0x0<br>
W16 BAR1+2060 0x0<br>
W16 BAR1+2080 0x0<br>
W16 BAR1+20A0 0x0<br>
W16 BAR1+20C0 0x0<br>
W16 BAR1+20E0 0x0<br>
W16 BAR1+2100 0x0<br>

   - Disable CPU power management features (SpeedStep, thermal throttling, prefetchers)<br>
WRMSR 0x1a0 0x00000000 0x00000000<br>

   - Remove CPU package power limits (allows unlimited power draw)<br>
WRMSR 0x610 0x00000000 0x00FFC000<br>

   - Clear additional power limit registers for CPU cores and iGPU<br>
WRMSR 0x638 0x00000000 0x00000000<br>
WRMSR 0x640 0x00000000 0x00000000<br>
WRMSR 0x618 0x00000000 0x00000000<br>

   - Disable Spectre/Meltdown security mitigations<br>
WRMSR 0x48 0x00000000 0x00000000<br>
WRMSR 0x49 0x00000000 0x00000000<br>

   - Disable CPU idle states (C1E)<br>
WRMSR 0x1FC 0x00000000 0x00000000<br>

   - Set turbo multiplier to a specific value. 32 (hex 50) stands for 5 GHz.<br>
WRMSR 0x1AD 0x00000000 0x32323232<br>

   - Copy your final settings in one line and separate them with semicolons (;) so they can be applied in one step on every startup.<br>
Eg. WRMSR 0x610 0x00000000 0x00FFC000; WRMSR 0x638 0x00000000 0x00000000; WRMSR 0x640 0x00000000 0x00000000<br>

More RWE settings: https://github.com/Hyyote/files-/blob/main/RWEverything%20settings/settings.txt

credits:

https://github.com/eskezje lots of undocumented registry values

https://twitter.com/BEYONDPERF_LLG process priority settings and removing clock interrupts
