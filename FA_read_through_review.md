Exam 1 - 100 points max - 10 questions - 70% required to pass


Windows PowerShell CTFd Challenges
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

Get-Content -Path "C:\\Test Files\\content.txt" #Displays the contents of the file 
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


Windows Boot
  Describe the Windows Boot Process
  Identify the Windows Logon Process
  Discuss Analyzing Boot Configurations with BCDEdit

Linux Boot
  Describe the Linux Boot Process
  Identify the Linux Logon Process

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






  
