# In-depth-Windows-tweaking

This guide covers tweaks beyond "advanced" PC optimization focusing on Windows settings that are more on the experimental side. Basic BIOS and Windows settings are widely documented, so they’re not included here. Most advanced tweaks are in my `.bat` files:<br>

you can search for things you are interested in, like MouseDataQueueSize, CoreParking, Kernel settings and random ones in Miscellaneous

&nbsp;&nbsp;&nbsp;https://github.com/Hyyote/files-/tree/main/Windows%2010<br>
&nbsp;&nbsp;&nbsp;https://github.com/Hyyote/files-/tree/main/Windows%2011

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
so I recommend one that is pre-tweaked such as my NTLite builds or Tiny11.

   - Install Windows without internet connection until you ensure all the update services and automatic driver updates are disabled.

   - Disable audio enhancements in the sound control panel.
<br>

**Import a power plan**<br>

Recommendations:
   - [My custom plan](https://github.com/Hyyote/files-/blob/main/Power%20plans/Hyote.pow)
   - [Lawliet](https://github.com/Hyyote/files-/blob/main/Power%20plans/lawliet.pow)
   - [Sapphire](https://github.com/Hyyote/files-/blob/main/Power%20plans/sapphire.pow)
<br>

Use AppxPackagesManager to clean up apps you don't need.


<br>

**Autoruns**: unhide Windows services

   - services: disable AppXSvc, ApxSvc, BITS, Bluetooth related, FontCache, UDK related, WMI, Wpn services

   - drivers: disable AppleSSD, Bluetooth related, HidBatt, HidBth, Intel Serial IO related, Microsoft_Bluetooth, swenum, WacomPen

<br>

**BCDEdits**: there are ongoing debates about which is the right configuration for each version of Windows, but in my experience, the legacy settings provided the best results.
   ```batch
bcdedit /set nx AlwaysOff
bcdedit /set ems No
bcdedit /set bootems No
bcdedit /set integrityservices disable
bcdedit /set tpmbootentropy ForceDisable
bcdedit /set bootmenupolicy Legacy
bcdedit /set debug No
bcdedit /set disableelamdrivers Yes
bcdedit /set isolatedcontext No
bcdedit /set allowedinmemorysettings 0x0
bcdedit /set vm NO
bcdedit /set vsmlaunchtype Off
bcdedit /set configaccesspolicy Default
bcdedit /set MSI Default
bcdedit /set usephysicaldestination No
bcdedit /set usefirmwarepcisettings No
bcdedit /set sos no
bcdedit /set pae ForceDisable
bcdedit /set tscsyncpolicy legacy
bcdedit /set hypervisorlaunchtype off
bcdedit /set useplatformclock false
bcdedit /set useplatformtick no
bcdedit /set disabledynamictick yes
bcdedit /set x2apicpolicy disable
bcdedit /set uselegacyapicmode yes
```
   - Device Manager:
View -> Devices by type<br>
In the Disk drives category, uncheck write caching the Properties -> Policies section<br>

   - Disable power saving on every device with a script or manually, since it takes around one minute to do.

   - Disable unnecessary devices: (System Management BIOS, PCI Express Root Ports, ISA Bridge, PCI standard RAM Controller, generic software components, unused usb devices, generic pnp monitor)
Show hidden devices, disable Motherboard resources

A properly broken device manager after Windows and SCEWIN BIOS tweaks should look like this:

<img src="Images/1.jpg" alt="Logo" width="400" height="500"/>

<img src="Images/2.jpg" alt="Logo" width="400" height="500"/>

<img src="Images/3.jpg" alt="Logo" width="400" height="500"/>

<br>

**Interrupt Affinities**

   - Use benchmarks to determine which cores get you the highest performance and set your GPU to two cores.<br>
 In Device Manager, PCI to PCI Bridge devices above your GPU entry also have to be bound to the same cores.<br> Check their locations individually by right clicking on the device -> Properties -> Location: PCI Bus 1-0-0 for example.
   - The devices worth assigning to separate cores are: USB host controller, GPU, network card.
   - If you are using Receive Side Scaling, your network card should be set to IrqPolicySpreadMessageAcrossAllProcessors
---

## 2.5. Poorly documented tweaks


I won't claim these to make a world of a difference or even make directly positive changes to your system, but I find them very important.

<br>

**Disable Task Manager and Control Panel**

   - Not having something running in the background is always good. Your system could freeze for many possible reasons while you're playing games or doing something important, but realistically you almost never need to use these.

   - I recommend not replacing Task Manager with either ProcessExplorer or SystemInformer (ProcessHacker) because both are sources of input lag. Using them portably is completely fine.

   - These settings can be turned on and off, but you need to restart for the Control Panel to turn back on:

```cmd
REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v "DisableTaskMgr" /t Reg_DWORD /d "1" /f
REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v "NoControlPanel" /t Reg_DWORD /d "1" /f
```
<br>
<br>

**Delete ICC Color Profiles**

   - This can only be done with System Privileges, so it's best to use NSudo or something similar.<br>
colorcpl -> All Profiles -> delete everything under ICC Profiles

Use limited RGB mode with the lowest visibly acceptable color depth in your GPU's software.

<br>
<br>

**DWM**

https://github.com/LuSlower/dwm-basic

   - Disabling DWM has way too many unintended effects, but this program makes it possible to use the most minimal version of DWM without breaking anything.

<br>
<br>


**Display scaling and custom resolutions**

   - It's generally recommended to use display scaling and native resolutions.
However Windows complicates things with an additional setting that needs to be changed in order to really use display scaling:

   - HKLM\SYSTEM\ControlSet001\Control\GraphicsDrivers\Configuration\<DisplayID>\00\00<br>

change Scaling to 2<br>

   - 1: Identity scaling
   - 2: No scaling
   - 3: Full-screen
   - 4: Aspect ratio

<br>

As for Custom Resolution Utility, I recommend deleting every value and adding resolutions to the Detailed Resolutions tab, using Exact Reduced timings with the monitor's highest supported refresh rate.

<img src="Images/5.png" alt="Logo" width="650" height="500"/>

<br>

**Device Cleanup**

   - This one is hugely overlooked. There are only benefits to cleaning up unused entries if you don't use multiple devices that rely on Windows settings instead of onboard memory.

   - Device Manager -> View -> Devices by connection -> Show hidden devices

   - You can bulk remove them with: https://www.uwe-sieber.de/files/DeviceCleanup_x64.zip (use at your own risk, since the tool isn't open source)

Hidden devices should be checked on every startup.
