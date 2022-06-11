#Level30
Connect to game via ssh on port 2220 using credentials bandit30:5b90576bedb2cc04c86a9e924ce42faf
_(format username:password)_
```sh
$ ssh -p 2220 bandit30@bandit.labs.overthewire.org
```
#Level Goal
There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level**
git

#Walkthrough
Lets create temporary directory, clone git then look around
```sh
bandit30@bandit:~$ mkdir /tmp/bandit30 && cd $_
bandit30@bandit:/tmp/bandit30$ git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit30/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit30/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit30-git@localhost's password: 
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit30@bandit:/tmp/bandit30$ cd repo/
bandit30@bandit:/tmp/bandit30/repo$ ls -al
total 16
drwxr-sr-x 3 bandit30 root 4096 Jun  5 18:44 .
drwxr-sr-x 3 bandit30 root 4096 Jun  5 18:43 ..
drwxr-sr-x 8 bandit30 root 4096 Jun  5 18:44 .git
-rw-r--r-- 1 bandit30 root   30 Jun  5 18:44 README.md
bandit30@bandit:/tmp/bandit30/repo$ cat README.md 
just an epmty file... muahaha
bandit30@bandit:/tmp/bandit30/repo$ git log
commit 3aefa229469b7ba1cc08203e5d8fa299354c496b
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:54 2020 +0200

    initial commit of README.md
bandit30@bandit:/tmp/bandit30/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
bandit30@bandit:/tmp/bandit30/repo$ cd .git
bandit30@bandit:/tmp/bandit30/repo/.git$ ls -al
total 52
drwxr-sr-x 8 bandit30 root 4096 Jun  5 18:48 .
drwxr-sr-x 3 bandit30 root 4096 Jun  5 18:44 ..
drwxr-sr-x 2 bandit30 root 4096 Jun  5 18:43 branches
-rw-r--r-- 1 bandit30 root  276 Jun  5 18:44 config
-rw-r--r-- 1 bandit30 root   73 Jun  5 18:43 description
-rw-r--r-- 1 bandit30 root   23 Jun  5 18:48 HEAD
drwxr-sr-x 2 bandit30 root 4096 Jun  5 18:43 hooks
-rw-r--r-- 1 bandit30 root  137 Jun  5 18:48 index
drwxr-sr-x 2 bandit30 root 4096 Jun  5 18:43 info
drwxr-sr-x 3 bandit30 root 4096 Jun  5 18:44 logs
drwxr-sr-x 4 bandit30 root 4096 Jun  5 18:43 objects
-rw-r--r-- 1 bandit30 root  165 Jun  5 18:44 packed-refs
drwxr-sr-x 5 bandit30 root 4096 Jun  5 18:44 refs
bandit30@bandit:/tmp/bandit30/repo/.git$ cat packed-refs 
# pack-refs with: peeled fully-peeled 
3aefa229469b7ba1cc08203e5d8fa299354c496b refs/remotes/origin/master
f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea refs/tags/secret
bandit30@bandit:/tmp/bandit30/repo/.git$ git tag
secret
bandit30@bandit:/tmp/bandit30/repo/.git$ git show secret
47e603bb428404d265f59c42920d81e5
```
The password was hidden in tag