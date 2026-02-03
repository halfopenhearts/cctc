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


Day 1 — Lecture Takeaways
------------------------

PowerShell Basics
- Cmdlet naming pattern: Verb-Noun (noun is singular)
- Parameters usually start with '-'
- Key terms:
  - Cmdlet
  - Parameter
  - Argument

Pipeline Object
- $_
  - Represents the current object flowing through the pipeline (|)


SIM Concepts
- Class
  - Blueprint or form describing what data exists
- Instance
  - A filled-out form containing real data from the system
- Namespace
  - Like a drawer that groups related information


Loops in PowerShell

Foreach Loop:
foreach ($item in $collection) {
    Get-Help
}

While Loop:
- Used for repeated execution while a condition remains true


PowerShell Remoting & Networking
--------------------------------

HTTP: 5985  
HTTPS (TLS): 5986  

WinRM (WS-Man):
- Enabled on Windows Server 2012 R2 and newer
- Uses Kerberos when available (localized authentication)
- Falls back to NTLM when Kerberos is unavailable
- Traffic is encrypted by default

Trusted Hosts:
- Used when Kerberos is not available
- Reduces overall security


.NET API Usage
--------------

- Used for direct .NET access and specialized tasks
- Common in cross-platform or advanced scenarios

Example:
[System.Text.Encoding]::Unicode.GetBytes(
    "This might be important"
)


Key Takeaways (Day 1)
--------------------

- PowerShell is object-based
- Cmdlets + pipelines enable scalable automation
- Security defaults matter
