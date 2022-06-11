#Level15
Connect to game via ssh on port 2220 using credentials bandit15:BfMYroe26WYalil77FoDi9qh59eK5xNr
_(format username:password)_
```sh
$ ssh -p 2220 bandit15@bandit.labs.overthewire.org
```

#Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

**Commands you may need to solve this level**
ssh, telnet, nc, openssl, s_client, nmap

**Helpful Reading Material**
Secure Socket Layer/Transport Layer Security on Wikipedia
OpenSSL Cookbook - Testing with OpenSSL

#Walkthrough
Let's have a look at manpage of s_client
```
       -connect host:port
           This specifies the host and optional port to connect to. If not
           specified then an attempt is made to connect to the local host on
           port 4433.
```
Lets try connecting to localhost port 30001
```
bandit15@bandit:~$ man s_client
bandit15@bandit:~$ openssl s_client -connect localhost:30001
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
MIICBjCCAW+gAwIBAgIEXcVbPTANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAls
b2NhbGhvc3QwHhcNMjIwMzA5MTk0NzQyWhcNMjMwMzA5MTk0NzQyWjAUMRIwEAYD
VQQDDAlsb2NhbGhvc3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBALDCas6k
DHxTRoxVISHtXOeCwJ8Sax5BZN76Hle8AH6pYTAdv9/FRssWL1xppFAtiGnFvglu
95FJvHEQirY4F0oPBTbtGU2xhzZzkWRL5Yj2C3Q2c99cyh+uWQT7sXPtB8W1osPc
YIo83YkXiArpt28474ZYdl+ohbPtP1oQHBv3AgMBAAGjZTBjMBQGA1UdEQQNMAuC
CWxvY2FsaG9zdDBLBglghkgBhvhCAQ0EPhY8QXV0b21hdGljYWxseSBnZW5lcmF0
ZWQgYnkgTmNhdC4gU2VlIGh0dHBzOi8vbm1hcC5vcmcvbmNhdC8uMA0GCSqGSIb3
DQEBBQUAA4GBAC2693WiK/kXMCauf1fEg5DwuxIfm0saYKiLSceyZo1G4IggqOBO
9JCtvMIV/xRAmYEnPvJmf0JtYv+2fsicaPh9E1GRmU0vGoYDZzA7NTZOgRmHlRKe
ihh/XSGrY7tE1qU+EfizmhcB35iZ7W5INIKlu7oyBWcvk3rI4jtPQeZp
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
    Session-ID: ADE48F1003FE8AA886E47357CB6BE9499C81F45CC1A0C5D2B9C5FBFB9F008CEC
    Session-ID-ctx: 
    Master-Key: 5AF5B4D63CF28F42757609FB5FAA0CC87399293978302DBCEB946E28E347BDC758CBCBB3B262DED09FFCB5115D439679
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - d3 53 01 d9 4c 2e 82 50-ef 16 38 c5 d8 1b dd f2   .S..L..P..8.....
    0010 - ab 39 7d 1a af 9b 4d 77-97 11 bf da 2f ec b2 73   .9}...Mw..../..s
    0020 - bb de 6e 33 c1 11 63 d6-85 b6 d1 c8 07 65 b2 63   ..n3..c......e.c
    0030 - 90 6e f5 84 21 63 28 c8-86 2f 92 a3 48 ea e0 8f   .n..!c(../..H...
    0040 - 38 08 29 04 f7 37 0e 44-67 21 77 66 5b 4c a4 ba   8.)..7.Dg!wf[L..
    0050 - e0 20 a2 1b 21 42 61 53-58 80 19 e6 67 6a f7 10   . ..!BaSX...gj..
    0060 - a0 a8 6d 1c 98 f1 9d 1c-57 0e 95 94 08 a8 5e 55   ..m.....W.....^U
    0070 - 58 49 76 70 22 8a 90 b4-fb 41 85 de 16 31 37 03   XIvp"....A...17.
    0080 - 77 56 87 60 44 94 68 87-38 a3 99 71 1b 39 e8 c9   wV.`D.h.8..q.9..
    0090 - 4d d7 a2 10 6e 0b 06 3e-fe 7e 0c 12 6e 0c 5e 4b   M...n..>.~..n.^K

    Start Time: 1654350880
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
---
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

closed
```

**Note**
https://www.feistyduck.com/library/openssl-cookbook/online/