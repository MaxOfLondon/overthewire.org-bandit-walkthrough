#Level28
Connect to game via ssh on port 2220 using credentials bandit28:0ef186ac70e04ea33b4c1853d2526fa2
_(format username:password)_
```sh
$ ssh -p 2220 bandit28@bandit.labs.overthewire.org
```
#Level Goal
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level**
git

#Walkthrough
Lets create temporary directory, clone git then look around

```sh
bandit28@bandit:~$ mkdir /tmp/bandit28 && cd $_
bandit28@bandit:/tmp/bandit28$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit28/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit28-git@localhost's password: 
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit28@bandit:/tmp/bandit28$ cd repo/
bandit28@bandit:/tmp/bandit28/repo$ ls -al
total 16
drwxr-sr-x 3 bandit28 root 4096 Jun  5 18:15 .
drwxr-sr-x 3 bandit28 root 4096 Jun  5 18:15 ..
drwxr-sr-x 8 bandit28 root 4096 Jun  5 18:15 .git
-rw-r--r-- 1 bandit28 root  111 Jun  5 18:15 README.md
bandit28@bandit:/tmp/bandit28/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
This suggests that password could have been comminted to git previously. Lets look at commit history
```sh
bandit28@bandit:/tmp/bandit28/repo$ git log
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    initial commit of README.md
```
'fix info leak' draws our attention. Lets checkout that commit and explore the file
```sh
bandit28@bandit:/tmp/bandit28/repo$ git checkout c086d11a00c0648d095d04c089786efef5e01264
Note: checking out 'c086d11a00c0648d095d04c089786efef5e01264'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at c086d11... add missing data
bandit28@bandit:/tmp/bandit28/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: bbc96594b4e001778eee9975372716b2

```