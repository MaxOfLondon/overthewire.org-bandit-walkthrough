#Level29
Connect to game via ssh on port 2220 using credentials bandit29:bbc96594b4e001778eee9975372716b2
_(format username:password)_
```sh
$ ssh -p 2220 bandit29@bandit.labs.overthewire.org
```
#Level Goal
There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level**
git

#Walkthrough
Lets create temporary directory, clone git then look around
```sh
bandit29@bandit:~$ mkdir /tmp/bandit29 && cd $_
bandit29@bandit:/tmp/bandit29$ git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit29/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit29-git@localhost's password: 
remote: Counting objects: 16, done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/bandit29$ cd repo/
bandit29@bandit:/tmp/bandit29/repo$ ls -al
total 16
drwxr-sr-x 3 bandit29 root 4096 Jun  5 18:31 .
drwxr-sr-x 3 bandit29 root 4096 Jun  5 18:31 ..
drwxr-sr-x 8 bandit29 root 4096 Jun  5 18:31 .git
-rw-r--r-- 1 bandit29 root  131 Jun  5 18:31 README.md
bandit29@bandit:/tmp/bandit29/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>

```
This indicates that there are other brancheck. Lets list all branches
```sh
bandit29@bandit:/tmp/bandit29/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```
There are other remote branches, lets checkout dev
```sh
bandit29@bandit:/tmp/bandit29/repo$ git checkout remotes/origin/dev
Note: checking out 'remotes/origin/dev'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at bc83328... add data needed for development
bandit29@bandit:/tmp/bandit29/repo$ ls -al
total 20
drwxr-sr-x 4 bandit29 root 4096 Jun  5 18:39 .
drwxr-sr-x 3 bandit29 root 4096 Jun  5 18:31 ..
drwxr-sr-x 2 bandit29 root 4096 Jun  5 18:39 code
drwxr-sr-x 8 bandit29 root 4096 Jun  5 18:39 .git
-rw-r--r-- 1 bandit29 root  134 Jun  5 18:39 README.md
bandit29@bandit:/tmp/bandit29/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf

```