#Level26
Connect to game via ssh on port 2220 using credentials bandit26:5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
_(format username:password)_
```sh
$ ssh -p 2220 bandit26@bandit.labs.overthewire.org
```
#Level Goal
Good job getting a shell! Now hurry and grab the password for bandit27!

**Commands you may need to solve this level**
ls

#Walkthrough
To login we need to use same trick as in [level25](25-otw-bandit-level-25.md "25-otw-bandit-level-25.md") and I won't show it here.
Once logged in lets have a look around.
```sh
bandit26@bandit:~$ echo $SHELL
/usr/bin/showtext
bandit26@bandit:~$ export SHELL=/bin/bash
bandit26@bandit:~$ ls -al
total 36
drwxr-xr-x  3 root     root     4096 May  7  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rwsr-x---  1 bandit27 bandit26 7296 May  7  2020 bandit27-do
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
drwxr-xr-x  2 root     root     4096 May  7  2020 .ssh
-rw-r-----  1 bandit26 bandit26  258 May  7  2020 text.txt
bandit26@bandit:~$ file bandit27-do 
bandit27-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=8e941f24b8c5cd0af67b22b724c57e1ab92a92a1, not stripped
```
We can see setuid bandit27-do file in home directory. Lets execute it.
```sh
bandit26@bandit:~$ ./bandit27-do /bin/cat /etc/bandit_pass/bandit27
3ba3118a22e93127a4ed485be72ef5ea
```
