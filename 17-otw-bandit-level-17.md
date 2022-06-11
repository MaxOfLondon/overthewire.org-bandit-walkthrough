#Level 17
We logged in as level17 user using private key obtained in Level 17 game.

#Level Goal
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

**Commands you may need to solve this level
cat, grep, ls, diff**

#Walkthrough
```sh
bandit17@bandit:~$ ls -al
total 36
drwxr-xr-x  3 root     root     4096 Jul 11  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r-----  1 bandit17 bandit17   33 Jul 11  2020 .bandit16.password
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit18 bandit17 3300 May  7  2020 passwords.new
-rw-r-----  1 bandit18 bandit17 3300 May  7  2020 passwords.old
-rw-r--r--  1 root     root      675 May 15  2017 .profile
drwxr-xr-x  2 root     root     4096 Jul 11  2020 .ssh
bandit17@bandit:~$ diff passwords.new passwords.old 
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
```