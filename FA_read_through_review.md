Exam 1 - 100 points max - 10 questions - 70% required to pass


## Windows PowerShell CTFd Challenges
  - Discuss Commands Used in PowerShell # common commands listed within the FA below
  - Identify Various Components of Windows Remoting # ssh / mobaxterm
  - Discuss PowerShell Profiles # all users, all hosts

  


```
get-variable # will find where variables are located
All Users, All Hosts	$PSHOME\Profile.ps1
All Users, Current Host	$PSHOME\Microsoft.PowerShell_profile.ps1
Current User, All Hosts	$HOME\Documents\WindowsPowerShell\Profile.ps1
Current User, Current Host	$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

```

``` example of switching from ps ver 5.1 to and from ps ver 7.3
what version am i using
  get-host | select-object Version

switch to PS Ver 7.3 by typing `pwsh` at the prompt

then run (again)
  get-host | select-object Version

to switch back type exit and get version again

```

```powershell help
```
Get-Content -Path "C:\\Test Files\\content.txt" #Displays the contents of the file 
```
Get-Variable #Displays current Variables 
Get-Verb #List the PowerShell verbs Get-Command #List the PowerShell cmdlets
Get-Command -Type Cmdlet | Sort-Object -Property Noun | Format-Table -GroupBy Noun #Get cmdlets and display them in order 
Get-Command -Module Microsoft.PowerShell.Security, Microsoft.PowerShell.Utility #Get commands in a module

Get-Help <cmdlet>                                                 #Displays help about a PowerShell cmdlet
Get-Help get-process                                              #Displays help for Get-Process cmdlet
Get-Help get-process -online                                      #Opens a web browser and displays help for the Get-Process cmdlet on the Microsoft website
Get-History <like Linux will return previous entered commands.>   #Displays history of commands in current window
Get-Location <similar to PWD on Linux, gl is the alias.>          #Displays present working directory

Get-Alias <alias> #Displays aliases for a given command name
Get-Alias dir #Returns Get-ChildItem

Get-Process | Get-Member #Gives the methods and properties of the object/cmdlet
(cmdlet).property #Command Structure 
(Get-Process).Name #Returns the single property of *name* of every process

Start-Process Notepad.exe #This cmdlet uses the Process.Start Method of the System.Diagnostics.Process class to open notepad.exe 
Stop-Process -name notepad #This cmdlet uses the Process.Kill Method of the System.Diagnostics.Process class to stop notepad.exe 
Get-Process | Select-Object Name, ID, path #Displays the Get-Process Properties of *Name, ID, Path* for every process

Get-Help Format-Table 
Get-Help Format-List

Get-Process | Get-Member | Where-Object {$_.Membertype -match "Method"}
# Displays all objects with Method in their name from the results from
Get-Member of the Get-Process cmdlet

Start-Process calc # Open an instance of calculator 
(Get-Process calculator *).kill() # Stops a named process using the kill() method directly 
Stop-Process -name calculator* # Uses a cmdlet to call the Process.Kill method

Get-Process | Select-Object Name, ID, path | Where-object {$_.ID -lt*1000*} # List all the processes with a PID lower than 1000
(Get-Process | Select-Object Name, ID, path | Where-object {$_.ID -lt*1000*}).count # List all the processes with a PID lower than 1000

Get-Cimclass * # Lists all CIM Classes 
Get-CimInstance –Namespace root\\securitycenter2 –ClassName antispywareproduct #Lists the antispywareproduct class from the root/security instance 
Get-CimInstance -ClassName Win32_LogicalDisk -Filter “DriveType=3” | gm # Shows properties and methods for this Instance 
Get-WmiObject -Class Win32_LogicalDisk -Filter “DriveType=3” # Using the Windows Management Instrumentation method

Get-CimInstance -class Win32_BIOS \# Queries Win32\_Bios 
Get-WmiObject -Class Win32_BIOS \# same output but deprecated command

Get-Variable                      # Names are displayed without the preceding <$>
Clear-Variable -Name MyVariable   # Delete the value of a Variable
Remove-Variable -Name MyVariable  # Delete the Variable

Get-ExecutionPolicy -list                                             # Lists all of the Scopes and ExecutionPolicies on the system
Get-ExecutionPolicy                                                   # Gets the current user's ExecutionPolicy
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser  # Sets the ExecutionPolicy for the CurrentUser to Unrestricted

get-help about_comparison_operators
Get-Service | Where-Object {$_.Status -eq "Stopped"}            # Takes the output from Get-Service and looks for Status property of Stopped and list those Services
Get-Service | where Status -eq "Stopped"                        # Same as above
Get-Process | Where-Object -Property Handles -GE -Value 1000    # Lists Processes that have Greater Than 1000 Handles
Get-Process | where Handles -GE 1000                            # Same as above

Get-PSSessionConfiguration # Displays permissions

Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles'   # Queries current network profiles.

6.3 Functional Usage of .NET APIs¶
Converts the text into a Unicode Array
([System.Text.Encoding]::Unicode.GetBytes("This Might be important")) #1



```

Windows Registry
  Explain the Purpose of Windows Registry
  Discuss Windows Registry Tools
  Identify Windows Registry for Suspicious Activity
  Identify Forensically Relevant Keys
  Identify Malicious Registry

```
Example Registry Layout


HKEY_Local_Machine (HIVE)
              ├──SOFTWARE (Key)
              ├──BCD00000 (Key)
              ├──HARDWARE (Key)
              └──SYSTEM   (Key)
                      └──RegisteredApplications (Subkey)
                                        ├── File Explorer : Data (value)
                                        ├── Paint : Data (value)
                                        └──Wordpad : Data (value)

```

```
There are five Registry Hives

HKEY_LOCAL_MACHINE (HKLM)
  - values are read every time the machine is started
  - contains configuration information for the entire computer

HKEY_USERS (HKU)
  - Contains all all user profiles on the system. Contains one key per user on the system
  - Each key is named after the SID(Security Identifier) of the user. (how to get sid of user and identify?)

HKEY_CURRENT_USERS (HKCU)
  - is the copy of the logged in user’s registry key based on thier SID from HKEY_USERS.
  -

HKEY_USERS (HIVE)
              └──SID (S-1-5-21-3939661428-3032410992-3449649886-XXXX) (Key)

HKEY_CURRENT_CONFIG (HKCC)
  - is a symbolic link (pointer or shortcut or alias) to the following registry key:
  -
HKEY_Local_Machine (HIVE)
              └──SYSTEM (Key)
                      └──CurrentControlSet (Subkey)
                                    └── Hardware Profiles (Subkey)
                                                └── Current (Subkey)


HKEY_CLASSES_ROOT (HKCR)
  - is a symbolic link (pointer or shortcut or alias) to the following registry key:
  -

HKEY_Local_Machine (HIVE)
              └──Software (Key)
                      └──Classes (Subkey)

```
```
Registry Structure and Data Types

Registry Path	Hive and Supporting Files
HKLM\SAM	SAM, SAM.LOG
HKLM\SECURITY	SECURITY, SECURITY.LOG
HKLM\SOFTWARE	software, software.LOG, software.sav
HKLM\SYSTEM	system, system.LOG, system.sav
HKLM\HARDWARE	(Dynamic/Volatile Hive)
HKU.DEFAULT	default, default.LOG, default.sav
HKU\SID	NTUSER.DAT
HKU\SID_CLASSES	UsrClass.dat, UsrClass.dat.LOG
```

```

reg /?                    #Displays help for all of the reg.exe commands
reg query /?              #Displays help for the `reg query`
reg add /?                #Displays help for `reg add`
reg delete /?             #Displays help for `reg delete`


Reads sub keys from the input value
Get-ChildItem HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run #1
Get-ChildItem HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\    #2

reads the value 
Get-item HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run




Get-PSDrive and net use * //
are documented here ?
talks alot about creating back doors when we're looking into persistence ?

Show all Environmental Variables in the Env: directory
Get-ChildItem Env:


Microsoft Edge Internet URL history and Browser Artifacts and Forensics
  -  referenced within a ctf and class?
    HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Storage\microsoft.microsoftedge_8wekyb3d8bbwe\Children\001\Internet Explorer\DOMStorage

USB history / USB Forensics

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USB
This registry key contains information about all USB devices that have been connected to the system at some point

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR
This registry key specifically deals with USB storage devices, such as USB flash drives, external hard drives, etc. It contains information about connected USB storage devices, including details like device instance paths, hardware IDs, and other configuration information.


HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePidlMRU
MRU is the abbreviation for most-recently-used.


Windows User Profiles User Account Forensics

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList
Saved Network Profiles and How to decode Network history

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
Windows Virtual Memory and why it is important

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management
This key maintains Windows virtual memory (paging file) configuration.
The paging file (usually C:\pagefile.sys) may contain evidence/important information that could be removed once the suspect computer is shutdown.


Recent search terms using Windows default search and Cortana - HKEY_CURRENT_USER\Software\Microsoft\Windows Search\ProcessedSearchRoots
```


```
Persistence According to MITRE

Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder - MITRE

System-wide and per-user autoruns
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKU\ < SID > \Software\Microsoft\Windows\CurrentVersion\Run
HKU\ < SID > \Software\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\SYSTEM\CurrentControlSet\services
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
HKLM\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Winlogon

critical Registry Locations
These are keys that have value for red and blue teams to be taken advantage of.

HKLM\BCD00000000

Replacement of old boot.ini file
HKLM\SAM\SAM

Use "psexec -s -i regedit" from administrator cmd.exe to view the SAM

It opens a new regedit.exe window with system permissions
```

1. Alternate Data Streams - NO OBJECTIVES ?
  ADS was first introduced to NTFS in Windows NT 3.1 and was Microsoft’s attempt at implementing filesystem forks in order to maintain compatibility with other filesystems like Apple’s HFS+ and Novell’s NWFS and NSS.
  In NTFS – files consists of attributes, security settings, mainstreams and alternate streams. By default, only the mainstream is visible.
  ADS has been used to store metadata, like file attributes, icons, image thumbnails.
  Great way to hide data using NTFS.
  Can be scanned by antivirus (Windows Defender Smartscreen is ADS aware).
  Does not change the MD5 hash of the file.
  Deleted once copied to a fat32.
  Cannot be disabled.
  [filename.extension]:[alternate_stream_name]:$DATA


```
C:\windows\system32>more < reminder.txt:secret.info # 1
social security numbers

C:\windows\system32>notepad reminder.txt:secret.info # 2

C:\windows\system32>dir /R reminder.txt # 3
 Directory of C:\windows\system32
 02/27/2021 07:13 PM                   23 reminder.txt
                                       26 reminder.txt:secret.info:$DATA
                1 File(s)              23 bytes
                0 Dir(s)   20,060,557,312 bytes free

C:\windows\system32>type reminder.txt:secret.info # 4
The filename, directory name, or volume label syntax is incorrect.
```
```
C:\windows\system32>mkdir kids # 1

C:\windows\system32>echo top-secret information > kids:hidden.txt # 2

C:\windows\system32>dir /R kids # 3
 Directory of C:\windows\system32\kids
 02/27/2021 07:29 PM      <DIR>           .
                                       25 .:hidden.txt:$DATA
 02/27/2021 07:29 PM      <DIR>           ..
               0 File(s)                0 bytes
               2 Dir(s)    20,060,160,000 bytes free

C:\windows\system32>more < kids:hidden.txt # 4
top-secret information
```


linux essentials
  Explain the Purpose of Understanding the Linux Environment
  Identify Commands to Enumerate Processes
  Identify Methods of Automation and Logic
  Identify Critical Locations in the Linux File System
  Discuss String Manipulation Techniques to Identify Key Information

```
1.1.1 Situational Awareness
After first obtaining access to a system an operator must gather as much information about their environment as possible, this is referred to as situational awareness. pwd is just one command of many on Linux which can provide us some insight.

Other commands to help gain situational awareness:

hostname or uname -a displays the name of the host you are currently on.
whoami shows the user you are currently logged in as (useful after gaining access through service exploitation).
w or who shows who else is logged in.

ip addr or ifconfig displays network interfaces and configured IP addresses.

ip neigh or arp displays MAC addresses of devices observed on the network.

ip route or route shows where packets will be routed for a particular destination address.
ss or netstat will show network connections or listening ports
nft list tables or iptables -L to view firewall rules.
sudo -l displays commands the user may run with elevated permissions.
```



Windows Boot
  Describe the Windows Boot Process
  Identify the Windows Logon Process
  Discuss Analyzing Boot Configurations with BCDEdit



```
smss.exe installs the Win32 subsystem kernel and user mode components (win32k.sys - kernel; winsrv.dll - user; and csrss.exe - user.)

csrss.exe - The Client/Server Runtime Subsystem supports process / thread creation and management.

wininit.exe marks itself as critical, initializes the Windows temp directory, loads the rest of the registry, and starts user mode scheduling. It also installs programs that require a reboot to finish the install process. It also starts:

lsm.exe - the Local Session Manager (LSM) handles all sessions of a system (both remote desktop sessions and local system sessions.)

lsass.exe - the Local Security Authority Subsystem (LSASS) provides user authentication services, manages the local security policy, and generates access tokens.

services.exe the Services Control Manager (SCM) loads AutoStart services, using LSASS to authenticate if they run as something other than System.
```
```
Mitre ATT&CK: Hijack Execution Flow: Services Registry

1. Showing the Spooler Service using SC

sc query spooler

SERVICE_NAME: Spooler
DISPLAY_NAME: Print Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
2. Showing the *Service Control Manager* registry key

reg query HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services | findstr Spooler

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Spooler
3. Showing the contents of the Spooler Service Registry Key

reg query HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Spooler

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Spooler
    DisplayName    REG_SZ    @%systemroot%\system32\spoolsv.exe,-1
    Group    REG_SZ    SpoolerGroup
    ImagePath    REG_EXPAND_SZ    %SystemRoot%\System32\spoolsv.exe #1
    Description    REG_SZ    @%systemroot%\system32\spoolsv.exe,-2
    ObjectName    REG_SZ    LocalSystem #2
1. The spooler service executable. What happens if someone changes that to a malicious binary?
The account who runs the Spooler Service!
1. Showing Services

C:\Windows> tasklist /svc

Image Name                     PID Session Name        Session#
========================= ======== ================ ===========
svchost.exe                   1040 EventSystem, fdPHost, FontCache, netprofm,
                                   nsi, WdiServiceHost
svchost.exe                   1076 AeLookupSvc, Appinfo, AppMgmt, BITS,
                                   CertPropSvc, EapHost, gpsvc, iphlpsvc,
                                   ProfSvc, Schedule, SCPolicySvc, SENS,
                                   ShellHWDetection, Themes, Winmgmt, wuauserv
CTAudSvc.exe                  1216 CTAudSvcService
igfxCUIService.exe            1328 igfxCUIService2.0.0.0
svchost.exe                   1388 CryptSvc, Dnscache, LanmanWorkstation,
                                   NlaSvc, WinRM
spoolsv.exe                   1568 Spooler
svchost.exe                   1604 FDResPub, QWAVE, SCardSvr, SSDPSRV
svchost.exe                   1644 BFE, DPS, MpsSvc
armsvc.exe                    1768 AdobeARMservice
```

```bcdedit```
  

Linux Boot
  Describe the Linux Boot Process
  Identify the Linux Logon Process

```
Looking at Grub configuration in Linux to find the Kernel

student@linux-opstation-kspt:/$ cat /boot/grub/grub.cfg #1
_truncated_
set linux_gfx_mode=auto
export linux_gfx_mode
menuentry 'Ubuntu' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-LABEL=cloudimg-rootfs' {
        recordfail
        load_video
        gfxmode $linux_gfx_mode
        insmod gzio
        if [ x$grub_platform = xxen ]; then insmod xzio; insmod lzopio; fi
        insmod part_msdos
        insmod ext2
        if [ x$feature_platform_search_hint = xy ]; then
            search --no-floppy --fs-uuid --set=root  6c0fba3b-b236-4b3a-b999-db7359c5d220
        else
            search --no-floppy --fs-uuid --set=root 6c0fba3b-b236-4b3a-b999-db7359c5d220
        fi
        linux   /boot/vmlinuz-4.15.0-76-generic root=LABEL=cloudimg-rootfs ro  console=tty1 console=ttyS0  #2
        initrd  /boot/initrd.img-4.15.0-76-generic
_truncated_
```
```
Modules in Linux

student@linux-opstation-kspt:/$ ltrace -S lsmod  # 1

Module                  Size  Used by
aesni_intel           188416  0
aes_x86_64             20480  1 aesni_intel # 2
crypto_simd            16384  1 aesni_intel
glue_helper            16384  1 aesni_intel
cryptd                 24576  3 crypto_simd,ghash_clmulni_intel,aesni_intel
psmouse               151552  0
ip_tables              28672  0
virtio_blk             20480  2 # 3
virtio_net             49152  0
virtio_rng             16384  0
virtio_gpu             53248  3
```


Windows Process Validity
  Describe Windows Processes
  Identify Valid Windows Processes
  Discuss Commands to Enumerate Processes
  Identify Processes Executed from Scheduled Tasks

Windows User Account Control
  Describe User Account Control Bypass
  Identify User Account Control Activity

Terminal Learning Objectives
  Identify SysInternals Tools to Enumerate Systems
  Identify SysInternals Tools to Analyze Processes

Linux Process
  Describe Linux Processes
  Identify Valid Linux Processes
  Discuss Commands to Enumerate Processes

Windows Auditing & Logging
  Identify Windows Artifacts
  Describe Windows Auditing & Logging

Linux Auditing & Logging
  Describe the Advantages and Disadvantages of Auditing & Logging
  Identify Auditing Activities
  Identify Actions that Contribute to Log Files
  Identify Linux Log Types


Memory Analysis
  Describe Memory Analysis
  Describe Order of Collecting Volatile Data


Active Directory
  Identify Active Directory Enumeration
  Describe Active Directory User Enumeration
  Describe Active Directory Group Enumeration




collobarative effots


--------------------------------WINDOWS PERSISTENCE COMMANDS--------------------------------------------------------
1. Registry Run Keys (Most Common)
PowerShell
# System-wide persistence
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run"
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce"

# Per-user persistence
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce"

# All users (requires admin)
reg query "HKU" /s | findstr Run
------------------------------------------------------------------------------------------------------------------
2. Services (Persistence via Registry)
PowerShell
# List all services and their startup type
Get-Service | Format-Table -Property Name, DisplayName, StartType

# Detailed service info including binary path
Get-WmiObject win32_service | Select-Object Name, DisplayName, PathName, StartMode

# Services with suspicious paths (non-system directories)
Get-WmiObject win32_service | Where-Object {$_.PathName -notlike "C:\Windows*"} | Select-Object Name, PathName

# View service registry path and ImagePath
reg query "HKLM\SYSTEM\CurrentControlSet\Services" /s | findstr ImagePath
---------------------------------------------------------------------------------------------------------------------
3. Startup Folder
PowerShell
# All users startup
Get-ChildItem "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"

# Current user startup
Get-ChildItem "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup"
--------------------------------------------------------------------------------------------------------------------
4. Scheduled Tasks
PowerShell
# All scheduled tasks
Get-ScheduledTask

# Detailed info with triggers
Get-ScheduledTask | Get-ScheduledTaskInfo | Format-List *

# Tasks in specific folders
Get-ScheduledTask -TaskPath "\Microsoft\Windows\*" -ErrorAction SilentlyContinue

# Export all tasks to check execution paths
Get-ScheduledTask | Export-ScheduledTask
---------------------------------------------------------------------------------------------------------------------
5. WMI Event Subscriptions
PowerShell
# List all WMI event subscriptions (used for fileless persistence)
Get-WmiObject __EventFilter -Namespace root\subscription
Get-WmiObject __EventConsumer -Namespace root\subscription
Get-WmiObject __FilterToConsumerBinding -Namespace root\subscription
----------------------------------------------------------------------------------------------------------------------
6. AppInit DLLs (DLL Injection)
PowerShell
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v AppInit_DLLs
reg query "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v AppInit_DLLs
----------------------------------------------------------------------------------------------------------------------
7. Shell Extensions & Context Menu
PowerShell
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers"
reg query "HKLM\Software\Classes\*\shellex\ContextMenuHandlers"
----------------------------------------------------------------------------------------------------------------------
8. PowerShell Profiles
PowerShell
# PowerShell profiles (auto-executed on shell start)
$PROFILE  # Current user profile
$PSHOME\Profile.ps1  # All users, all hosts

# Check all profile locations
Get-Item "$PSHOME\Profile.ps1"
Get-Item "$PSHOME\Microsoft.PowerShell_profile.ps1"
Get-Item "$HOME\Documents\WindowsPowerShell\Profile.ps1"
Get-Item "$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
-------------------------------------------------------------------------------------------------------------------------
9. Browser Extensions & Helpers
PowerShell
# Chrome extensions
Get-ChildItem "C:\Users\*\AppData\Local\Google\Chrome\User Data\*\Extensions"

# Firefox extensions
Get-ChildItem "C:\Users\*\AppData\Roaming\Mozilla\Firefox\Profiles\*\extensions"
---------------------------------------------------------------------------------------------------------------------------
10. Winlogon
PowerShell
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon"
# Check for: Userinit, Shell, GinaDLL, Notify keys
--------------------------------------------LINUX PERSISTENCE COMMANDS-------------------------------------------------------
SYSTEMD Systems (Modern Linux)
1. Systemd Services
bash
# List all services
systemctl list-units --type=service

# Show all services (including disabled)
systemctl list-units --type=service --all

# List services in specific directory
ls -la /lib/systemd/system/
ls -la /etc/systemd/system/

# Check service details
systemctl status servicename
systemctl cat servicename  # Shows service file content

# Find suspicious services (non-standard locations)
find /lib/systemd/system /etc/systemd/system -type f -newermt "30 days ago"
----------------------------------------------------------------------------------------------------------------------------------
2. Systemd Timers
bash
# List all timers (systemd equivalent of cron)
systemctl list-timers --all

# Show timer details
systemctl cat timer-name.timer

# Check timer execution
journalctl -u timer-name.service
-----------------------------------------------------------------------------------------------------------------------------------
3. Systemd User Services
bash
# User-level services (run as specific user)
systemctl --user list-units --type=service

# User service directory
ls -la ~/.config/systemd/user/
-----------------------------------------------------------------------------------------------------------------------------------
4. Default Target & Unit Dependencies
bash
# Check default target (similar to runlevel)
ls -la /lib/systemd/system/default.target
cat /lib/systemd/system/default.target

# Check unit dependencies
systemctl list-dependencies

# Multi-user target services
systemctl list-dependencies multi-user.target
----------------------------------------SYSV INIT Systems (Older Linux)---------------------------------------------------------------
1. Init Scripts
bash
# List init scripts
ls -la /etc/rc*.d/

# Check specific runlevel (commonly 3=multiuser, 5=GUI)
ls -la /etc/rc3.d/

# View init script content (S=start, K=kill)
cat /etc/rc3.d/S99mystartup
---------------------------------------------------------------------------------------------------------------------------------------
2. Inittab Configuration
bash
# Check default runlevel and init configuration
cat /etc/inittab

# Respawn processes defined in inittab
grep respawn /etc/inittab
---------------------------------------------------------------------------------------------------------------------------------------
3. Init Process
bash
# Check what init system is being used
ls -l /proc/1/exe
# If it points to /usr/lib/systemd/systemd = systemd
# If it points to /sbin/init = SysV init

------------------------------------------CROSS-PLATFORM LINUX PERSISTENCE-------------------------------------------------------------
Cron Jobs (Works on Both SystemD & SysV)
bash
# System cron jobs
cat /etc/crontab
ls -la /etc/cron.d/

# Per-user cron jobs
ls -la /var/spool/cron/crontabs/
crontab -l  # Current user
sudo crontab -l -u username  # Specific user

# Cron job directories
/etc/cron.daily/
/etc/cron.hourly/
/etc/cron.weekly/
/etc/cron.monthly/
------------------------------------------------Shell Profiles (Executed on Login)---------------------------------------------------------
bash
# System-wide profiles
cat /etc/profile
cat /etc/bash.bashrc
ls -la /etc/profile.d/

# User profiles
cat ~/.profile
cat ~/.bashrc
cat ~/.bash_profile
cat ~/.bash_login
--------------------------------------------------SSH Keys & Authorized Access--------------------------------------------------------------
bash
# SSH keys for persistence
ls -la ~/.ssh/authorized_keys
ls -la /root/.ssh/authorized_keys

# Check all user SSH keys
find /home -name authorized_keys 2>/dev/null
find /root -name authorized_keys 2>/dev/null
----------------------------------------------Kernel Modules (Rootkit Persistence)------------------------------------------------------------
bash
# List loaded kernel modules
lsmod

# Kernel module directory
ls -la /lib/modules/$(uname -r)/kernel/

# Check for suspicious modules
cat /proc/modules
--------------------------------------------------GRUB Boot Configuration------------------------------------------------------------------------
bash
# Boot configuration
cat /boot/grub/grub.cfg

# Check for boot hooks
ls -la /etc/grub.d/

# MBR verification
sudo xxd -l 512 -g 1 /dev/sda
--------------------------------------------DETERMINE INIT SYSTEM (CRITICAL FOR LINUX)-------------------------------------------------------------
bash
# Method 1: Check /proc/1/exe
ls -l /proc/1/exe

# Method 2: Check systemctl availability
systemctl --version

# Method 3: Check common files
[ -f /etc/systemd/system-generators ] && echo "systemd" || echo "SysV"

# Method 4: ps command
ps -p 1 -o comm=
-----------------------------------------------------------------------------------------------------------------------------------------------------
Output Interpretation:
/usr/lib/systemd/systemd or /lib/systemd/systemd = SYSTEMD
/sbin/init or /bin/init = SYSV INIT
User Login|PowerShell profiles|shell profiles|
System Hooks|WMI, Winlogon|kernel modules

  
