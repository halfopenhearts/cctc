MATTHEW_WILLIS,10.50.15.85
download "omnisa" to rdp for homework
powershell profiles - persistency 

day 1 lecture take away notes:
  cmdlet pattern: verb-noun (singular noun)
  usually starts with '-'
  cmdlet , param, arg


  $_ -- current object flowing through a pipeline '|'

  SIM -
    CLASS - blue print or form . what kind of data exists
    INSTANCE - a filled out form - real data from your machine
    NAMESPACES = drawer - groups related info


    foreach loops in pwsh
    foreach ($item in $collection) {
      Get-Help
    }

    while loops in pwsh




  pwsh HTTP (still encrypted) 5985 HTTPS 5986 (TLS)
      winRM (WS-Man) - allowed on windows server 2012 r2 and newer
      kerberos is localized
      non-krberos - WinRM uses NTLM - WinRM encrypts traffic by default

  trusted host - used when no kerberos - reduces security

  .NET api for direct.net access (specialized tasks) - unix
      [System.Text.Encoding]::Unicode.GetBytes(
      "This might be important"
      )





      takeaways:
  pwsh is object-baed
    cmdlets _ pipeelines = scalable automation
    security defaults matter



day 2

- registry -
  hives
    keys
      subkeys
        values

only two phyically stored as hive files::
HKLM (local machine) - system wide
HKU - all user profiles on system

KHCU (current user) - active user content
HKCC (current config) - current hardware profile
HKCR (classes root) - file associations & COM objects



reg.exe - command line , repeatable fast changes , persistance and cleanup often happen

powershell & the registry::
HKLM AND HKCU: MOUNTED AUTOMATICALLY
powershell registry accesed through PSDrives , cmdlets treat keys as items, values treated as properties, 

example command::
    Get-ChildItem HKLM:\SOFTWARE\Microcosoft\Windows\CurrentVersion\Run
    Get-Item ..... ^


run / RunOnce keys run @login , services run on boot


registry artifacts ; left over cac cert
HKLM\SYSTEM\CurentControlSet\Enum\... USB ...  ; device history example






