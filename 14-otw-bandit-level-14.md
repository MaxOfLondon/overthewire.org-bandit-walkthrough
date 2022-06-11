#Level14
Connect to game via ssh on port 2220 using credentials bandit14:4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
_(format username:password)_
```sh
$ ssh -p 2220 bandit14@bandit.labs.overthewire.org
```
#Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

**Commands you may need to solve this level**
ssh, telnet, nc, openssl, s_client, nmap

**Helpful Reading Material**
How the Internet works in 5 minutes (YouTube) (Not completely accurate, but good enough for beginners)
IP Addresses
IP Address on Wikipedia
Localhost on Wikipedia
Ports
Port (computer networking) on Wikipedia

#Walkthrough
Let's check if port 30000 is open with nmap.
```sh
bandit14@bandit:~$ nmap -p 30000 localhost

Starting Nmap 7.40 ( https://nmap.org ) at 2022-06-04 15:31 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00016s latency).
PORT      STATE SERVICE
30000/tcp open  ndmps

Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds
```
Then connect with netcat to submit password
```sh
andit14@bandit:~$ nc localhost 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

bandit14@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```