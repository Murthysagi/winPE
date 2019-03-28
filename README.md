# Windows Privilege Escalation

## Windows PE using CMD (.bat)

If you want to search for files and registry that could contain passwords, set to *yes* the *long* variable at the beginning of the script.

The script will use acceschk.exe if it is available (with that name). But it is not necessary, it also uses wmic + icacls.

Some of the tests in this script were extracted from **[here](https://github.com/enjoiz/Privesc/blob/master/privesc.bat)** and from **[here](https://github.com/codingo/OSCP-2/blob/master/Windows/WinPrivCheck.bat)**


### Main checks

- [x] Systeminfo --SO version and patches-- (windows suggester)
- [x] Common Kernel exploits (2K, XP, 2K3, 2K8, Vista, 7)
- [x] UAC?? 
- [x] AV??
- [x] Mounted disks
- [x] WSUS vuln?? 
- [x] SCCM installed??
- [x] Interesting file permissions of binaries being executed 
- [x] Interesting file permissions of binaries run at startup
- [x] AlwaysInstallElevated??
- [x] Network info (see below)
- [x] Users info (see below)
- [x] Current user privileges 
- [x] Service binary permissions 
- [x] Check if permissions to modify any service registy
- [x] Unquoted Service paths  
- [x] Search for interesting writable files
- [x] Saved credentials  
- [x] Search for known files to have passwords inside
- [x] Search for known registry to have passwords inside
- [x] If *long*, search files with passwords inside 
- [x] If *long*, search registry with passwords inside 

### More enumeration

- [x] Date & Time
- [x] Env
- [x] Installed Software
- [x] Running Processes 
- [x] Current Shares 
- [x] Network Interfaces
- [x] Used Ports
- [x] Firewall
- [x] ARP
- [x] Routes
- [x] Hosts
- [x] Cached DNS
- [x] Info about current user (PRIVILEGES)
- [x] List groups (info about administrators)
- [x] Current logon users 

### Understanding icacls permissions

Icacls is the program used to check the rights that groups and users have in a file or folder.

Iclals is the main binary used here to check permissions.

Its output is not intuitive so if you are not familiar with the command, continue reading. Take into account that in XP you need administrators rights to use icacls (for this OS is very recommended to upload sysinternals accesschk.exe to enumerate rights).

**Interesting rights**

```
D - Delete access
F - Full access (Edit_Permissions+Create+Delete+Read+Write)
N - No access
M - Modify access (Create+Delete+Read+Write)
RX - Read and eXecute access
R - Read-only access
W - Write-only access
```

We will focus in **F** (full), **M** (Modify access) and **W** (write).

**Use of Icacls by wniPE**

When checking rights of a file or a folder the script search for the strings: *(F)* or *(M)* or *(W)* and the string ":\" (so the path of the file being checked will appear inside the output).

It also checks that the found right (F, M or W) can be exploited by the current user.

A typical output where you dont have any nice access is:
```
C:\Windows\Explorer.EXE NT SERVICE\TrustedInstaller:(F)
```

An output where you have some interesting privilege will be like:
```
C:\Users\john\Desktop\desktop.ini NT AUTHORITY\SYSTEM:(I)(F)
                                MYDOMAIN\john:(I)(F)
```

Here you can see that the privileges of user *NT AUTHORITY\SYSTEM* appears in the output because it is in the same line as the path of the binary. However, in the next line, you can see that our user (john) has full privileges in that file. 

This is the kind of outpuf that you have to look for when usnig the winPE.bat script.

[More info about icacls here](https://ss64.com/nt/icacls.html)

# Binaries

Some interesting precompiled binaries for privesc in Windows.

By Polop(TM)
