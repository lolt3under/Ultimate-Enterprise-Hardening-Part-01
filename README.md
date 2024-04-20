# Ultimate Enterprise Hardening Part-01

Created by [lolt3under](https://www.linkedin.com/in/achintya-vatsraj-7494111b4/) 

It is under MIT License But Please do mention [me](https://www.linkedin.com/in/achintya-vatsraj-7494111b4/) when using my work, if possible!

You can call this the self-proclaimed **UEH-01 Standard!**

### Summary

This document summarizes the information related to Hardening in a common enterprise environments, all the way from Network Devices to Threat Intel to Platform and endpoint security.

Something's missing? Create a Pull Request and add it. Get your name in the contributers section!!

### The Basics

- System Upgrades/Updates/patches are **NOT** always the best deal, excersise caution when updating/upgrading -- ie. Supply Chain Attacks.
- Use [AppLocker](https://technet.microsoft.com/en-us/library/dd759117(v=ws.11).aspx) to block exec content from running in user locations (home dir, profile path, temp, etc) -- ONLY FOR Windows 7 & Windows Server 2008 R2 Systems.
- Manage PowerShell execution via constrained language mode.
- Make sure PowerShell Script execution is blocked across the organization.
- Enable [PowerShell logging](https://www.fireeye.com/blog/threat-research/2016/02/greater_visibilityt.html) (v3+) & command process logging.
- [Block Office macros](https://blogs.technet.microsoft.com/mmpc/2016/03/22/new-feature-in-office-2016-can-block-macros-and-help-prevent-infection/) (Windows & Mac) on content downloaded from the Internet.
- Deploy security tooling that monitors for suspicious behavior. Consider using [WEF](https://blogs.technet.microsoft.com/jepayne/2015/11/23/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem/) to forward only interesting events to your SIEM or logging system (more on this later in part-02).
- Limit capability by blocking/restricting attachments via email/download:
	-  Executables extensions:
	-  (ade, adp, ani, bas, bat, chm, cmd, com, cpl,
crt, hlp, ht, hta, inf, ins, isp, job, js, jse, lnk, mda, mdb,
mde, mdz, msc, msi, msp, mst, pcd, pif, reg, scr, sct, shs,
url, vb, vbe, vbs, wsc, wsf, wsh, exe, pif, etc.)
	- Office files that support macros (docm, xlsm, pptm, etc.)
	-  Ensure [these file types](https://support.office.com/en-us/article/blocked-attachments-in-outlook-434752e1-02d3-4e90-9124-8b81e49a8519) are blocked.
	-  Block forgotten/unused [Excel file extensions](https://www.vmray.com/cyber-security-blog/forgotten-ms-office-features-used-deliver-malware/): IQY, SLK
-  Change default program for anything that opens with Windows scripting to notepad.exe, remember these are [all the common extentions](https://support.microsoft.com/en-us/windows/common-file-name-extensions-in-windows-da4a4430-8e76-89c5-59f7-1cdbbc75cb01).
	- bat, js, jse, vbe, vbs, wsf, wsh, hta, vbs, etc.
	- GPO: User Configuration -> Preferences -> Control Panel Settings -> Folder Options -> Open With
	-  Action: Replace
	-  File Extension: (extension)
	-  Associated Program: %windir%\system32\notepad.exe
	-  Set as default: Enabled.
- [Preventing activation of OLE packages and other eight hardening](https://learn.microsoft.com/en-us/compliance/essential-eight/e8-app-harden) in Office with the PackagerPrompt registry setting.
<<<<<<< HEAD
- CMD.EXE & POWERSHELL.EXE (all variants and the associated DLL) should not open/execute on Standard Employee Laptops (GPO)
- Block .LNK files entirely if not used by org users otherwise mix blacklisting & whitelisting to control. (GPO)
- Python3, Ruby, GOLang and Windows C++ SDKs should not be installed on Standard Employee Laptops.
- Prefer Windows 10 on laptops and Desktops when possible, even better is RHEL Desktop/Laptop edition when Windows 10/11/7 specific applications are not required.
- Avoid Windows 7 (any build) and Linux Kernel (any patch) < 4.4 like the plague.
- Prefer RHEL Servers over Windows, if Very High Risk Organization (GOV/MIL/INT etc.) then use Gentoo ([Compile your own kernel](https://www.odi.ch/prog/kernel-config.php)) with [Patches](https://grsecurity.net/features) as server. 
- Always use SELinux policies (On RHEL & Gentoo) and use whitelist methodology (BLOCK EVERYTHING BY DEFAULT).
- For RHEL servers prefer using Pre-CIS-Hardened Images or hire an expert. 
- Say it with me, GPO! GPO!! GPO!!!, always use GPO to restrict (be as strict as possible), unless approvals are there **DO NOT PROVIDE ACCESS.**
- Zero Trust Should be followed with People,Process & Products.

More to come soon...


