PowerShell Notes
================

General Info
------------

Host: MATTHEW_WILLIS  
IP: 10.50.15.85  

Task:
- Download OmniSA and access it via RDP for homework

Focus:
- PowerShell profiles
- Persistence


Day 2 — Registry
----------------

Registry Structure
- Hives
  - Keys
    - Subkeys
      - Values


Physically Stored Hive Files
----------------------------

Only two hives are physically stored as files:

HKLM (HKEY_LOCAL_MACHINE)
- System-wide configuration

HKU (HKEY_USERS)
- All user profiles on the system


Logical / Virtual Hives
----------------------

HKCU (HKEY_CURRENT_USER)
- Active user content

HKCC (HKEY_CURRENT_CONFIG)
- Current hardware profile

HKCR (HKEY_CLASSES_ROOT)
- File associations and COM objects


Registry Tooling
----------------

reg.exe:
- Command-line based
- Fast, repeatable changes
- Commonly used for persistence and cleanup


PowerShell & the Registry
-------------------------

- HKLM and HKCU are mounted automatically
- Registry is accessed via PSDrives
- Registry keys are treated as items
- Registry values are treated as properties

Example Commands:
Get-ChildItem HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
Get-Item HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run


Persistence Locations
---------------------

Run / RunOnce:
- Executed at user login

Services:
- Executed at system boot


Registry Artifacts
------------------

Examples:
- Leftover CAC certificates



- USB device history

Example Path:
HKLM\SYSTEM\CurrentControlSet\Enum\USB\...





ADS in NTFS
- compatible with HFS+,NWFS

NTFS
- attributes
- security
- main streams
- alternate streams
- only mainstream ($DATA) is visible by default

they exist' to store icons, thumbnails, file attributes
 - allow multiple data streams per file without altering visible content



ADS example :: [filename]:[stream_name]:$DATA
  - removed when copied over to FAT32 , does not change hash
  - to put hidden data in : echo secret data > [filename]:[stream_name]
  - to read the hidden data use 'more < [filename]:[stream_name]







