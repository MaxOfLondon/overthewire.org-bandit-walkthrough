#Level16
Connect to game via ssh on port 2220 using credentials bandit16:cluFn7wTiGryunymYOu4RcffSxQluehd
_(format username:password)_
```sh
$ ssh -p 2220 bandit16@bandit.labs.overthewire.org
```

#Level Goal
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

*Commands you may need to solve this level*
ssh, telnet, nc, openssl, s_client, nmap

*Helpful Reading Material*
Port scanner on Wikipedia

#Walkthrough
We want to use nmap to determine open ports in range 31000-32000.
```sh
bandit16@bandit:~$ nmap -p31000-32000 localhost

Starting Nmap 7.40 ( https://nmap.org ) at 2022-06-04 16:10 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00025s latency).
Not shown: 996 closed ports
PORT      STATE    SERVICE
31046/tcp open     unknown
31518/tcp filtered unknown
31691/tcp open     unknown
31790/tcp open     unknown
31960/tcp open     unknown

Nmap done: 1 IP address (1 host up) scanned in 1.26 seconds
```
Now that we have list of ports lets run default scripts and try to obtain version of services.
```sh
bandit16@bandit:~$ nmap -sC -sV -p31046,31518,31691,31790,31960 localhost

Starting Nmap 7.40 ( https://nmap.org ) at 2022-06-04 16:12 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00024s latency).
PORT      STATE    SERVICE     VERSION
31046/tcp open     echo
31518/tcp filtered unknown
31691/tcp open     echo
31790/tcp open     ssl/unknown
| fingerprint-strings: 
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq: 
|_    Wrong! Please enter the correct current password
| ssl-cert: Subject: commonName=localhost
| Subject Alternative Name: DNS:localhost
| Not valid before: 2022-04-26T10:22:02
|_Not valid after:  2023-04-26T10:22:02
|_ssl-date: TLS randomness does not represent time
31960/tcp open     echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.40%T=SSL%I=7%D=6/4%Time=629B6848%P=x86_64-pc-linux-gn
SF:u%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cur
SF:rent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the\
SF:x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Pleas
SF:e\x20enter\x20the\x20correct\x20current\x20password\n")%r(RTSPRequest,3
SF:1,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n
SF:")%r(Help,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x2
SF:0password\n")%r(SSLSessionReq,31,"Wrong!\x20Please\x20enter\x20the\x20c
SF:orrect\x20current\x20password\n")%r(TLSSessionReq,31,"Wrong!\x20Please\
SF:x20enter\x20the\x20correct\x20current\x20password\n")%r(Kerberos,31,"Wr
SF:ong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n")%r(
SF:FourOhFourRequest,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
SF:rrent\x20password\n")%r(LPDString,31,"Wrong!\x20Please\x20enter\x20the\
SF:x20correct\x20current\x20password\n")%r(LDAPSearchReq,31,"Wrong!\x20Ple
SF:ase\x20enter\x20the\x20correct\x20current\x20password\n")%r(SIPOptions,
SF:31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\
SF:n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 91.01 seconds
```
Port 31790 is interesting because it runs SSL server so it might be the target for this CTF.
Other ports 31046, 31691 and 31960 run echo service so they are not interesting and port 31518 is filtered and runs an unknown service.

Let's try connecting to port 31790 and submit our password.

```sh
bandit16@bandit:~$ openssl s_client -connect localhost:31790
CONNECTED(00000003)
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
---
Certificate chain
 0 s:/CN=localhost
   i:/CN=localhost
---
Server certificate
-----BEGIN CERTIFICATE-----
MIICBjCCAW+gAwIBAgIEDhNLxTANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAls
b2NhbGhvc3QwHhcNMjIwNDI2MTAyMjAyWhcNMjMwNDI2MTAyMjAyWjAUMRIwEAYD
VQQDDAlsb2NhbGhvc3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBALf++FPu
liF2KOZ+fIQ1iXlR9yTwTJtKFDSO1SexA4UatL8eCCzmXZbIzX8fcuH6BIqXMI3t
/4toG38CqHb/tbEGeT0Wr9JygfYsTY/r8bg6KQkqPJ0X2ZX7eUTKqQuKwv6VZdar
m8VU4Qews5if+WQYX8VAdJZB29RSHX3Jz9+vAgMBAAGjZTBjMBQGA1UdEQQNMAuC
CWxvY2FsaG9zdDBLBglghkgBhvhCAQ0EPhY8QXV0b21hdGljYWxseSBnZW5lcmF0
ZWQgYnkgTmNhdC4gU2VlIGh0dHBzOi8vbm1hcC5vcmcvbmNhdC8uMA0GCSqGSIb3
DQEBBQUAA4GBADqQQPOKtUZJRlwMfHjJvO6Tu9+9Cg1pjBFlbJ/uV+kuPBMfPHCt
z1pVEqiDWq1LO1C8Se21UzYEF+rOrAOvSkXgL+vIk3lhzC6P6QiVv2MRbam/U2d0
+VjlDeVpQ8YcKMrdJyPjg5qwF3Yt0w+aa76W5ylP8Zi7yaw0LRSIEDJf
-----END CERTIFICATE-----
subject=/CN=localhost
issuer=/CN=localhost
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1019 bytes and written 269 bytes
Verification error: self signed certificate
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 1024 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 839BE10766F328976DBA55EAD753CF06F63B9F80B9CA0E26436263C6367F3DE0
    Session-ID-ctx: 
    Master-Key: 4DC66000C8C1BB434675DDE048D8A935059AAC4D4D86427A82EC2103C567C10DAA716FE86EE4A43A86B7AC51CEC1CAB8
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 42 51 6b 2c 02 07 d6 dc-3d 81 75 1e a3 28 92 98   BQk,....=.u..(..
    0010 - e4 36 97 3e 66 ef 42 31-74 c8 a9 06 b2 fc 3c 95   .6.>f.B1t.....<.
    0020 - 86 25 1b a3 04 da a0 53-79 9f 5b a6 88 19 4f db   .%.....Sy.[...O.
    0030 - 04 47 98 68 b7 c9 54 79-34 51 45 ef 80 6b 4b df   .G.h..Ty4QE..kK.
    0040 - 22 bb 53 6d 33 ed ec da-cd 49 63 fd 71 c3 59 65   ".Sm3....Ic.q.Ye
    0050 - 22 91 bd df 6b b0 25 1c-98 91 bf 38 b0 77 87 65   "...k.%....8.w.e
    0060 - bb 97 5e 5b d3 fb 97 9e-5c 5a c5 d9 dc 92 b9 49   ..^[....\Z.....I
    0070 - 96 65 3f e1 6f 06 00 54-4b c1 5a 95 24 b0 35 86   .e?.o..TK.Z.$.5.
    0080 - 19 e5 06 00 37 c5 89 51-71 ac db 59 85 b6 04 3a   ....7..Qq..Y...:
    0090 - 7f bb ce 9c d6 cc f9 09-a3 e0 be fa 8e ce 43 ae   ..............C.

    Start Time: 1654352377
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
---
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

closed
```

We obtined a private key that we can use to login as level17 user.

Let's create temporary folder then save private key.
```sh
bandit16@bandit:~$ mkdir /tmp/bandit16 && cd $_
bandit16@bandit:/tmp/bandit16$ vim private.key
```
Once vim opens, press i to enter insert mode then paste key, then type `:wq to save and exit vim.

Private keys need to have correct permissions to be used by ssh client.

```sh
bandit16@bandit:/tmp/bandit16$ ls -al
total 1996
drwxr-sr-x 2 bandit16 root    4096 Jun  4 16:27 .
drwxrws-wt 1 root     root 2031616 Jun  4 16:30 ..
-rw-r--r-- 1 bandit16 root    1676 Jun  4 16:27 private.key
bandit16@bandit:/tmp/bandit16$ chmod 600 private.key 
bandit16@bandit:/tmp/bandit16$ ls -al
total 1996
drwxr-sr-x 2 bandit16 root    4096 Jun  4 16:27 .
drwxrws-wt 1 root     root 2031616 Jun  4 16:30 ..
-rw------- 1 bandit16 root    1676 Jun  4 16:27 private.key
bandit16@bandit:/tmp/bandit16$ ssh -i private.key bandit17@localhost
Could not create directory '/home/bandit16/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit16/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

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

bandit17@bandit:~$ id
uid=11017(bandit17) gid=11017(bandit17) groups=11017(bandit17)
```