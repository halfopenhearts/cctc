Exam 1 - 100 points max - 10 questions - 70% required to pass


Windows PowerShell CTFd Challenges
  Discuss Commands Used in PowerShell
  Identify Various Components of Windows Remoting
  Discuss PowerShell Profiles

  


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

Windows Boot CTFd Challenges
  Describe the Windows Boot Process
  Identify the Windows Logon Process
  Discuss Analyzing Boot Configurations with BCDEdit

Linux Boot CTFd Challenges
  Describe the Linux Boot Process
  Identify the Linux Logon Process

Windows Process Validity CTFd Challenges
  Describe Windows Processes
  Identify Valid Windows Processes
  Discuss Commands to Enumerate Processes
  Identify Processes Executed from Scheduled Tasks

Windows User Account Control CTFd Challenges
  Describe User Account Control Bypass
  Identify User Account Control Activity

Terminal Learning Objectives
  Identify SysInternals Tools to Enumerate Systems
  Identify SysInternals Tools to Analyze Processes

Linux Process CTFd Challenges
  Describe Linux Processes
  Identify Valid Linux Processes
  Discuss Commands to Enumerate Processes

Windows Auditing & Logging CTFd Challenges
  Identify Windows Artifacts
  Describe Windows Auditing & Logging

Linux Auditing & Logging CTFd Challenges
  Describe the Advantages and Disadvantages of Auditing & Logging
  Identify Auditing Activities
  Identify Actions that Contribute to Log Files
  Identify Linux Log Types


Memory Analysis CTFd Challenges
  Describe Memory Analysis
  Describe Order of Collecting Volatile Data


Active Directory CTFd Challenges
  Identify Active Directory Enumeration
  Describe Active Directory User Enumeration
  Describe Active Directory Group Enumeration






  
