linux process validity
================

General Info
------------

  Host:
  -
  MATTHEW_WILLIS
  -
  IP:
  -
  10.50.15.85  
  -

Task:
- 
-

Focus:
- 
- 


parent process id of 2
ps -ppid 2 =lf | head


For user-space processes /sbin/init ( PID = 1 )

For kernel-space processes [kthreadd] ( PID = 2 )

 Process Ownership, Effective User ID (EUID), Real User ID (RUID), User ID (UID)

check like this:
ls -l /usr/bin/passwd

fork - creates a new process by duplicating the calling process. The new process is referred to as the child process. The calling process is referred to as the parent process.

The fork “processes” can be explained as the recreation of a process from system space and duplicated into user space in an attempt restrict user access to system processes/space.
exec - When a process calls exec, the kernel starts program, replacing the current process.


The /proc/ directory — also called the proc file system — contains a hierarchy of special files which represent the current state of the kernel, allowing applications and users to peer into the kernel’s view of the system.






