# In-depth-Windows-tweaking

This guide is meant to cover things beyond "advanced" PC optimization as I want to share my observations regarding hardware and software. Most known tweaks like basic BIOS and Windows settings are not going to be mentioned here, for those I would recommend checking out https://github.com/dguvs/PC-Tuningnew first. I'm recommending this older version because it's easier to read than the updated one.
Most advanced tweaks can be found in my .bat files so I'm not going to explain them either, but leave everyone to test them on their own.


1. The operating system (recommendation: 24H2/23H2)

Linux isn't suitable for competitive gaming because tweaks don't go as deep as the ones you can do in Windows. Distros are similar in latency and input feel, which I found to be subpar compared to any version of Windows.

I used to recommend Windows 7 and Windows 10 1803 specifically, but I have to admit that Windows 11 is superior only because of its gaming performance and software compatibility at the moment. There are ongoing debates about 23H2 vs 24H2 which I cannot settle, however trying the newer version first would make more sense.



2. Experimental settings

Custom Resolution Utility: delete everything, uncheck everything, add Detailed Resolution -> Exact Reduced with your preferred resolution and refresh rate
I tried most combinations and still find this to be the best one.

Interrupt affinities: set these to separate cores
-USB XHCI
-Generic PnP Monitor
-GPU and every related PCI-to-PCI bridge to the same core
-Storage controller


The next ones aren't really experimental, but more sensible. Running things in the background, even if they are not fully operational takes away performance and possibly makes latency worse.

Disable Task Manager and use Process Explorer in portable mode. SystemInformer (ProcessHacker) causes insane amounts of input lag.
REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v "DisableTaskMgr" /t Reg_DWORD /d "1" /f >NUL 2>&1
