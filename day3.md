Linux essentials Notes
================

General Info
------------

Host: MATTHEW_WILLIS  
IP: 10.50.15.85  

Task:
- 

Focus:
- 
- 





commands
-
hostname
uname -a
ip neigh / arp
ip route / route
ss / netstat
nft list tables
sudo -l
sudo -su
apropos


variables
-
a="100"
echo $a -> 100

command substition
-
directories=$(ls /)
echo $directories -> will run ls /

save errors:
ls bacon 2> errorfile



example of a for loop
  - 
  objects$(ls -d /etc/*)

  for item in $objects; do
    echo $item
  done

example of an if statement
  -
  target="/etc/passwd"

  if[ -f "$target" ];then
    echo "target is a file"
  else
    echo "target is not a file"
  fi

example of a while loop
  -
  start=$(date + "%s")
  end=$((start+10))

  while [ $(date + "%s") -lt $end ]; do
    echo "loop is running..."
    sleep 2
  done
