#Level25
Connect to game via ssh on port 2220 using credentials bandit25:uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
_(format username:password)_
```sh
$ ssh -p 2220 bandit25@bandit.labs.overthewire.org
```
#Level Goal
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

**Commands you may need to solve this level**
ssh, cat, more, vi, ls, id, pwd

#Walkthrough

Lets check what is the shell for bandit26
```sh
bandit25@bandit:~$ grep bandit26 /etc/passwd
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ file /usr/bin/showtext
/usr/bin/showtext: POSIX shell script, ASCII text executable
bandit25@bandit:~$ ls -l /usr/bin/showtext
-rwxr-xr-x 1 root root 53 May  7  2020 /usr/bin/showtext
```
The shell for bandit26 is a shell script and is world readable. Lets have a look at it
```sh
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```
Let's look around
```sh
bandit25@bandit:~$ ls -al
total 32
drwxr-xr-x  2 root     root     4096 May 14  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r-----  1 bandit25 bandit25   33 May 14  2020 .bandit24.password
-r--------  1 bandit25 bandit25 1679 May  7  2020 bandit26.sshkey
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit25 bandit25    4 May 14  2020 .pin
-rw-r--r--  1 root     root      675 May 15  2017 .profile
bandit25@bandit:~$ cat bandit26.sshkey
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uU1Afl2F8VyXQqbv/LTrIwdW
pTfaeRHXzr0Y0a5Oe3GB/+W2+PReif+bPZlzTY1XFwpk+DiHk1kmL0moEW8HJuT9
/5XbnpjSzn0eEAfFax2OcopjrzVqdBJQerkj0puv3UXY07AskgkyD5XepwGAlJOG
xZsMq1oZqQ0W29aBtfykuGie2bxroRjuAPrYM4o3MMmtlNE5fC4G9Ihq0eq73MDi
1ze6d2jIGce873qxn308BA2qhRPJNEbnPev5gI+5tU+UxebW8KLbk0EhoXB953Ix
3lgOIrT9Y6skRjsMSFmC6WN/O7ovu8QzGqxdywIDAQABAoIBAAaXoETtVT9GtpHW
qLaKHgYtLEO1tOFOhInWyolyZgL4inuRRva3CIvVEWK6TcnDyIlNL4MfcerehwGi
il4fQFvLR7E6UFcopvhJiSJHIcvPQ9FfNFR3dYcNOQ/IFvE73bEqMwSISPwiel6w
e1DjF3C7jHaS1s9PJfWFN982aublL/yLbJP+ou3ifdljS7QzjWZA8NRiMwmBGPIh
Yq8weR3jIVQl3ndEYxO7Cr/wXXebZwlP6CPZb67rBy0jg+366mxQbDZIwZYEaUME
zY5izFclr/kKj4s7NTRkC76Yx+rTNP5+BX+JT+rgz5aoQq8ghMw43NYwxjXym/MX
c8X8g0ECgYEA1crBUAR1gSkM+5mGjjoFLJKrFP+IhUHFh25qGI4Dcxxh1f3M53le
wF1rkp5SJnHRFm9IW3gM1JoF0PQxI5aXHRGHphwPeKnsQ/xQBRWCeYpqTme9amJV
tD3aDHkpIhYxkNxqol5gDCAt6tdFSxqPaNfdfsfaAOXiKGrQESUjIBcCgYEAxvmI
2ROJsBXaiM4Iyg9hUpjZIn8TW2UlH76pojFG6/KBd1NcnW3fu0ZUU790wAu7QbbU
i7pieeqCqSYcZsmkhnOvbdx54A6NNCR2btc+si6pDOe1jdsGdXISDRHFb9QxjZCj
6xzWMNvb5n1yUb9w9nfN1PZzATfUsOV+Fy8CbG0CgYEAifkTLwfhqZyLk2huTSWm
pzB0ltWfDpj22MNqVzR3h3d+sHLeJVjPzIe9396rF8KGdNsWsGlWpnJMZKDjgZsz
JQBmMc6UMYRARVP1dIKANN4eY0FSHfEebHcqXLho0mXOUTXe37DWfZza5V9Oify3
JquBd8uUptW1Ue41H4t/ErsCgYEArc5FYtF1QXIlfcDz3oUGz16itUZpgzlb71nd
1cbTm8EupCwWR5I1j+IEQU+JTUQyI1nwWcnKwZI+5kBbKNJUu/mLsRyY/UXYxEZh
ibrNklm94373kV1US/0DlZUDcQba7jz9Yp/C3dT/RlwoIw5mP3UxQCizFspNKOSe
euPeaxUCgYEAntklXwBbokgdDup/u/3ms5Lb/bm22zDOCg2HrlWQCqKEkWkAO6R5
/Wwyqhp/wTl8VXjxWo+W+DmewGdPHGQQ5fFdqgpuQpGUq24YZS8m66v5ANBwd76t
IZdtF5HXs2S5CADTwniUS5mX1HO9l5gUkk+h0cH5JnPtsMCnAUM+BRY=
-----END RSA PRIVATE KEY-----
```

We located bandit26.sshkey - too easy!

Lets try to login
```sh
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost
Could not create directory '/home/bandit25/.ssh'.
...
  Enjoy your stay!

  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to localhost closed.
bandit25@bandit:~$ 
```
A long MOTD was displayed then content of ~/text.txt and we were kicked out. It is not surprising because script had ```exit 0``` in last line.

I recall I seen solution some time back and I needed to get vim to spawn shell. I'll be honest I spent time trying and resorted to googling for hints. The idea was to force terminal to use ```more``` (pager) to display banner then to spawn ```vim``` (editor) and finally spawn the ```bash``` shell. Lets try that.

First make terminal window as small as possible then login. Once logged in ```more``` will hold page from completing display

![ssh](/images/level25-01.png "ssh")
![more](/images/level25-02.png "more")

Resize window back then press 'v' to launch vim.
![vim](/images/level25-03.png "vim")
In vim we need to set shell to bash then launch it.
Type ```:set shell=/bin/bash``` then press Enter. Type ```shell``` then press Enter again, we are now in bash shell and can retrieve bandit26's password.
![shell](/images/level25-04.png "shell")

```sh
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
```
That wasn't straight forward as I though but good trick though :)

