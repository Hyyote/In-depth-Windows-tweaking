# In-depth-Windows-tweaking

This guide covers tweaks beyond "advanced" PC optimization, focusing on both hardware and software. Most standard tweaks—such as basic BIOS and Windows settings—are widely documented, so they’re not included here. Most advanced tweaks are in my `.bat` files; everyone is encouraged to test them independently.

> **Note:** If you require components for games like Valorant or FACEIT, or need Windows Update support, this guide is not for you.

---

## 1. The Operating System

Linux isn't suitable for competitive gaming because tweaks don't go as deep as the ones you can do in Windows. Distros are similar in latency and input feel, which I found to be subpar compared to any version of Windows.

I used to recommend Windows 7 and Windows 10 1803 specifically, but I have to admit that later versions are superior only because of perceived stability, higher performance and better software compatibility at the moment, but all of them have higher idle latency. I don't find much difference between Windows 10 22H2 and Windows 11 23H2/24H2.<br>

Windows 11 always used to feel smoother on the desktop but when I ran tests and played games, the two felt identical.<br>

Windows 10 is more resilient when it comes to tweaks and it can be stripped down to its bones without sacrificing any functionality (22 services running), while Windows 11 needs UWP app support, StateRepository, DWM and many other things running to ensure nothing breaks and can be taken down to around 31 services.

---

## 2. Windows Tweaks

First, install a version of Windows you like, it can be a stock install or a custom one, however I really dislike having a lot of things running in the background in the first place,
so I recommend one that is pre-tweaked such as my NTLite builds or Tiny11.

Install Windows without internet connection until you ensure all the update services and automatic driver updates are disabled.

Disable audio enhancements in the sound control panel.

**Import a power plan**:
   - [My custom plan (idle disabled by default)](https://github.com/Hyyote/files-/blob/main/Hyote.pow)
   - Bitsum Highest Performance
   - [Zoyata's power plan](https://github.com/IDIVASM/POWERPLAN-WINDOWS-10-)

Use AppxPackagesManager to clean up apps you don't need.

Autoruns: unhide Windows services

**services**: disable AppXSvc, ApxSvc, BITS, Bluetooth related, FontCache, UDK related, WMI, Wpn services

**drivers**: disable AppleSSD, Bluetooth related, HidBatt, HidBth, Intel Serial IO related, Microsoft_Bluetooth, WacomPen

BCDEdits: there are ongoing debates about which is the right configuration for each version of Windows, but in my experience, the legacy settings provided the best results.
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
Device Manager:
View -> Devices by type<br>
In the Disk drives category, uncheck write caching the Properties -> Policies section<br>

Disable power saving on every device with a script or manually, since it takes around one minute to do.

Disable unnecessary devices: (System Management BIOS, PCI Express Root Ports, ISA Bridge, PCI standard RAM Controller, generic software components, unused usb devices, generic pnp monitor)
Show hidden devices, disable Motherboard resources

A properly broken device manager after Windows and SCEWIN BIOS tweaks should look like this:
![Logo](Images/1.jpg)

![Logo](Images/2.jpg)

![Logo](Images/3.jpg)

Disable Task Manager and use Process Explorer in portable mode. SystemInformer (ProcessHacker) causes insane amounts of input lag.
REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v "DisableTaskMgr" /t Reg_DWORD /d "1" /f 
