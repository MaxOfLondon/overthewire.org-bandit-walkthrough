#Level 33
Connect to game via ssh on port 2220 using credentials bandit33:c9c3199ddf4121b10cf581a98d51caee
_(format username:password)_
```sh
$ ssh -p 2220 bandit33@bandit.labs.overthewire.org
```
#Level Goal
At this moment, level 34 does not exist yet.

#Walkthrough
This suggests that level 33 still exists. Let's look around
```sh
bandit33@bandit:~$ ls -al
total 24
drwxr-xr-x  2 root     root     4096 May  7  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rw-------  1 bandit33 bandit33  430 May  7  2020 README.txt
bandit33@bandit:~$ cat README.txt 
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```