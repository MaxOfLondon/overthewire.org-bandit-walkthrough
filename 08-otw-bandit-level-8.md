#Level 8
Connect to game via ssh on port 2220 using credentials bandit8:cvX2JJa4CFALtqS87jk27qwqGhBM9plV
_(format username:password)_
```sh
$ ssh -p 2220 bandit8@bandit.labs.overthewire.org
```

#Level Goal
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

Helpful Reading Material
Piping and Redirection

#Walkthrough
We want to build a workflow that will automatically achieve the goal.
The data.txt file contains large list of md5 hashes as we notice by checking it with head and tail commands.

Our strategy will be:
2) Sort the file
3) Display only unique lines

The sorting is necessary for uniq to work properly.

Looking at manpage of uniq we can see:
```
       -u, --unique
              only print unique lines
```


```sh
$ ssh -p 2220 bandit8@bandit.labs.overthewire.org
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit8@bandit.labs.overthewire.org's password: 
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

If you find any problems, please report them to Steven or morla on
irc.overthewire.org.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ and /proc/ is disabled
  so that users can not snoop on eachother. Files and directories with
  easily guessable or short names will be periodically deleted!

  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few usefull tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /usr/local/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /usr/local/pwndbg/
    * peda (https://github.com/longld/peda.git) in /usr/local/peda/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /usr/local/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)
    * checksec.sh (http://www.trapkit.de/tools/checksec.html) in /usr/local/bin/checksec.sh

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us through IRC on
  irc.overthewire.org #wargames.

  Enjoy your stay!

bandit8@bandit:~$ ls -al
total 56
drwxr-xr-x  2 root    root     4096 May  7  2020 .
drwxr-xr-x 41 root    root     4096 May  7  2020 ..
-rw-r--r--  1 root    root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root    root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit9 bandit8 33033 May  7  2020 data.txt
-rw-r--r--  1 root    root      675 May 15  2017 .profile
bandit8@bandit:~$ head data.txt 
VkBAEWyIibVkeURZV5mowiGg6i3m7Be0
zdd2ctVveROGeiS2WE3TeLZMeL5jL7iM
sYSokIATVvFUKU4sAHTtMarfjlZWWj5i
ySvsTwlMgnUF0n86Fgmn2TNjkSOlrV72
NLWvtQvL7EaqBNx2x4eznRlQONULlCYZ
LfrBHfAh0pP9bgGAZP4QrVkut3pysAYC
U0NYdD3wHZKpfEg9qGQOLJimAJy6qxhS
flyKxCbHB8uLTaIB5LXqQNuJj3yj00eh
TThRArdF2ZEXMO47TIYkyPPLtvzzLcDf
cIPbot7oYveUPNxDMhv1hiri50CqpkTG
bandit8@bandit:~$ tail data.txt 
qaWWAOOquC3yHnfJI4zvPWzCBdfHQ8wa
0N65ZPpNGkUJePzFxctCRZRXVrCbUGfm
cR6riSWC0ST7ALZ2i1e47r3gc0QxShGo
TKUtQbeYnEzzYIne7BinoBx2bHFLBXzG
8NtHZnWzCA8HswoJSCU7Ojg8nP3eKpsA
SzwgS2ADSjP6ypOzp2bIvdqNyusRtrHj
5AdqWjoJOEdx5tJmZVBMo0K2e4arD3ZW
gqyF9CW3NNIiGW27AtWVNPqp3i1fxTMY
flyKxCbHB8uLTaIB5LXqQNuJj3yj00eh
w4zUWFGTUrAAh8lNkS8gH3WK2zowBEkA
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
bandit8@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```