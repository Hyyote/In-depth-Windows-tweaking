# In-depth-Windows-tweaking

This guide is meant to cover things beyond "advanced" PC optimization as I want to share my observations regarding hardware and software. Most known tweaks like basic BIOS and Windows settings are [...]  
Most advanced tweaks can be found in my .bat files so I'm not going to explain them either, but leave everyone to test them on their own.

> [If you need components for games like Valorant or FACEIT and Windows Update support, this guide is not for you.]



## 1. The operating system

Linux isn't suitable for competitive gaming because tweaks don't go as deep as the ones you can do in Windows. Distros are similar in latency and input feel, which I found to be subpar compared to[...]

I used to recommend Windows 7 and Windows 10 1803 specifically, but I have to admit that later versions are superior only because of perceived stability, higher performance and better software com[...]
Windows 11 always used to feel smoother on the desktop but when I ran tests and played games, the two felt identical.
Windows 10 is more resilient when it comes to tweaks and it can be stripped down to its bones without sacrificing any functionality (16 services running), while Windows 11 needs UWP app support, S[...]


## 2. Windows tweaks

First, install a version of Windows you like, it can be a stock install or a custom one, however I really dislike having a lot of things running in the background in the first place,  
so I recommend one that is pre-tweaked such as my NTLite builds or Tiny11.

Install Windows without internet connection until you ensure all the update services and automatic driver updates are disabled.

Disable audio enhancements in the sound control panel.

Import a power plan of your liking, my recommendations are:
- my own one which has idle disabled by default (https://github.com/Hyyote/files-/blob/main/Hyote.pow)
- Bitsum Highest Performance
- Zoyata's power plan (https://github.com/IDIVASM/POWERPLAN-WINDOWS-10-)

Use AppxPackagesManager to clean up apps you don't need.

**Autoruns:** unhide Windows services  
**services:** disable AppXSvc, ApxSvc, BITS, Bluetooth related, FontCache, UDK related, WMI, Wpn services  
**drivers:** disable AppleSSD, Bluetooth related, HidBatt, HidBth, Intel Serial IO related, Microsoft_Bluetooth, WacomPen

**BCDEdits:** there are ongoing debates about which is the right configuration for each version of Windows, but in my experience, the legacy settings provided the best results.
