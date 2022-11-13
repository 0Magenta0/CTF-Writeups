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
