#Level 4
Connect to game via ssh on port 2220 using credentials bandit4:pIwrPrtPN36QITSp3EQaw936yaFoFgAB
_(format username:password)_
```sh
$ ssh -p 2220 bandit4@bandit.labs.overthewire.org
```

#Level Goal
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

**Commands you may need to solve this level**
ls, cd, cat, file, du, find

#Walkthrough
The ```file``` command returns type of the file in question. For the text files it will return 'ASCII text'.
If we list content of the inhere/ directory we can query each file for it's type using ```file <filepath>``` i.e.:
```sh
bandit4@bandit:~$ ls -al inhere/
total 48
drwxr-xr-x 2 root    root    4096 May  7  2020 .
drwxr-xr-x 3 root    root    4096 May  7  2020 ..
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file00
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file01
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file02
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file03
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file04
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file05
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file06
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file07
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file08
-rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file09
bandit4@bandit:~$ file inhere/-file00
inhere/-file00: data
```

It can be quite tedious to check each file by hand so the idea is to build a pipeline (workflow that passes output of previous command to the input of the next using pipe character |) to find the files in the inhere/ directory then execute ```file``` command on each file and finally filter output to those of  'ASCII text' type (partial match for 'text' string will be sufficient) i.e.:
```sh
bandit4@bandit:~$ find inhere/ -exec file {} + | grep text
inhere/-file07: ASCII text
bandit4@bandit:~$ cat inhere/-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
bandit4@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```
##Note
- Read manpage of find to learn more about handy -exec parameter (```man find```)
- If you need an absolute path from find command, concatenate \$PWD environmental variable with directiry name i.e. ```find $PWD/inhere/ -exec file {} + | grep text```