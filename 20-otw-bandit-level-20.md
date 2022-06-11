#Level20
Connect to game via ssh on port 2220 using credentials bandit20:GbKksEFF4yrVs6il55v6gwY5aVje5f0j
_(format username:password)_
```sh
$ ssh -p 2220 bandit20@bandit.labs.overthewire.org
```

#Level Goal
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

**Commands you may need to solve this level**
ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

#Walkthrough
The setuid program expects current level password to be provided on port that we specify as an argument. This means we need to create a server that will respond with current password when the program connects to that port.
Using netcat we can serve current password string when a clinet connects to it.
```
cat /etc/bandit_pass/bandit20 | nc -nlp 4444 &
```
The options for netcat we use are -n (do not resolve host), -l (listen mode), -p (port to lisen on). The & is so the job is put on the background.
We then run setuid program with argument 4444.

```sh
bandit20@bandit:~$ ls -al
total 32
drwxr-xr-x  2 root     root      4096 May  7  2020 .
drwxr-xr-x 41 root     root      4096 May  7  2020 ..
-rw-r--r--  1 root     root       220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root      3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root       675 May 15  2017 .profile
-rwsr-x---  1 bandit21 bandit20 12088 May  7  2020 suconnect
bandit20@bandit:~$ file suconnect 
suconnect: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=74c0f6dc184e0412b6dc52e542782f43807268e1, not stripped
bandit20@bandit:~$ tmux
[exited]
bandit20@bandit:~$ cat /etc/bandit_pass/bandit20 | nc -nlp 4444 &
[1] 13139
bandit20@bandit:~$ ./suconnect 4444
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
[1]+  Done                    cat /etc/bandit_pass/bandit20 | nc -nlp 4444
```