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

### Linux/*Nix/BSD Systems Hardening Checklist

1. **Host Information Documentation**:
   - Machine name
   - IP address (Internal & External)
   - Mac address (Virtual or Physical)
   - Name of the workforce who is doing the hardening
   - Date of Documentation
   - Asset Number

2. **BIOS/UEFI Protection**:
   - Protect the BIOS/UEFI with a password.
   - Prefer non-systemd init systems.
   - Use musl as the default C library.
   - Prefer LibreSSL over OpenSSL.
   - Configure boot order to prevent unauthorized booting.
   - Prefer BIOS over UEFI.
   - Adopt a 3-month audit cycle.
   - Prefer single boot systems.
   - Configure boot parameters.

3. **Operating System Preferences**:
   - Prefer Gentoo Linux for desktop use.
   - Prefer OpenBSD for server use.
   - Avoid using Processor Microcode.
   - Adopt hardened profiles for operating systems.

4. **Filesystem Hardening**:
   - Create separate partitions with specific options.
   - Bind mount directories.
   - Use LUKS Encryption for disk encryption.

5. **Kernel Hardening**:
   - Adopt self-compiled kernels.
   - Prefer LTS Kernels over Latest Stable.
   - Configure kernel settings with Sysctl functionality.

6. **Network Security and Firewall Configuration**:
   - Limit connections to authorized users.
   - Disable IP forwarding.
   - Enable bad error message protection.
   - Configure Remote Administration Via SSH Protocol.
   - Configure network time protocol (NTP).
   - Enable TCP/SYN

 cookies.
   - Use Hybrid-listing in firewall.
   - Prefer pf firewall system.

7. **PAM Configuration**:
   - Ensure secure PAM configuration.
   - Upgrade password hashing algorithm to SHA-512.
   - Set complex password creation requirements.
   - Restrict root login.

8. **Implement Linux Security Modules (LSM)**:
   - Enable SELinux, Smack, TOMOYO, Landlock during self-compilation.

9. **Optional Patches**:
   - Prefer optional patches from [grsecurity.net](https://grsecurity.net/).

### Annexure 1

Contents of /etc/sysctl.conf :
```
kernel.kptr_restrict=2 kernel.dmesg_restrict=1
kernel.printk=3 3 3 3 kernel.unprivileged_bpf_disabled=1 net.core.bpf_jit_harden=2 dev.tty.ldisc_autoload=0 vm.unprivileged_userfaultfd=0 kernel.kexec_load_disabled=1 kernel.sysrq=4 kernel.unprivileged_userns_clone=0 kernel.perf_event_paranoid=3 net.ipv4.tcp_syncookies=1 net.ipv4.tcp_rfc1337=1 net.ipv4.conf.all.rp_filter=1 net.ipv4.conf.default.rp_filter=1 net.ipv4.conf.all.accept_redirects=0 net.ipv4.conf.default.accept_redirects=0 net.ipv4.conf.all.secure_redirects=0 net.ipv4.conf.default.secure_redirects=0 net.ipv6.conf.all.accept_redirects=0 net.ipv6.conf.default.accept_redirects=0 net.ipv4.conf.all.send_redirects=0 net.ipv4.conf.default.send_redirects=0 net.ipv4.icmp_echo_ignore_all=1 net.ipv4.conf.all.accept_source_route=0 net.ipv4.conf.default.accept_source_route=0 net.ipv6.conf.all.accept_source_route=0 net.ipv6.conf.default.accept_source_route=0 net.ipv6.conf.all.accept_ra=0 net.ipv6.conf.default.accept_ra=0 net.ipv4.tcp_sack=0
net.ipv4.tcp_dsack=0 net.ipv4.tcp_fack=0 kernel.yama.ptrace_scope=2
vm.mmap_rnd_bits=32 vm.mmap_rnd_compat_bits=16 fs.protected_symlinks=1 fs.protected_hardlinks=1 fs.protected_fifos=2 fs.protected_regular=2
```