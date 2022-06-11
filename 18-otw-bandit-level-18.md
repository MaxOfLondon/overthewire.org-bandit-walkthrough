#Level18
Connect to game via ssh on port 2220 using credentials bandit18:kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
_(format username:password)_
```sh
$ ssh -p 2220 bandit18@bandit.labs.overthewire.org
```
#Level Goal
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

**Commands you may need to solve this level**
ssh, ls, cat

#Walkthrough

Let's try connecting to the game using credentials
```sh
$ ssh -p 2220 bandit18@bandit.labs.overthewire.org
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
Linux bandit.otw.local 5.4.8 x86_64 GNU/Linux

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!
...
  Enjoy your stay!

Byebye !
Connection to bandit.labs.overthewire.org closed.
```

From the description of this game we learn that .bashrc forces the logout. Let's execure /bin/sh to login instead with addional option -t to force new pseudo-terminal allocation (so the prompt will show). 

```sh
$ ssh -p 2220 -t bandit18@bandit.labs.overthewire.org /bin/bash
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
Byebye !
Connection to bandit.labs.overthewire.org closed.
max@freyja:~/overthewire$ ssh -p 2220 -t bandit18@bandit.labs.overthewire.org /bin/sh
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
$ 
$ id  
uid=11018(bandit18) gid=11018(bandit18) groups=11018(bandit18)
$ ls -al
total 24
drwxr-xr-x  2 root     root     4096 May  7  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r-----  1 bandit19 bandit18 3549 May  7  2020 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rw-r-----  1 bandit19 bandit18   33 May  7  2020 readme
$ tail .bashrc	
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
echo 'Byebye !'
exit 
$ cat readme	
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```