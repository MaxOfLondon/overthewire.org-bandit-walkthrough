#Level31
Connect to game via ssh on port 2220 using credentials bandit31:47e603bb428404d265f59c42920d81e5
_(format username:password)_
```sh
$ ssh -p 2220 bandit31@bandit.labs.overthewire.org
```
#Level Goal
There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level**
git

#Walkthrough
Lets create temporary directory, clone git then look around
```sh
bandit31@bandit:~$ mkdir /tmp/bandit31 && cd $_
bandit31@bandit:/tmp/bandit31$ git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password: 
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit31@bandit:/tmp/bandit31$ cd repo/
bandit31@bandit:/tmp/bandit31/repo$ ls -al
total 20
drwxr-sr-x 3 bandit31 root 4096 Jun  5 18:59 .
drwxr-sr-x 3 bandit31 root 4096 Jun  5 18:58 ..
drwxr-sr-x 8 bandit31 root 4096 Jun  5 18:59 .git
-rw-r--r-- 1 bandit31 root    6 Jun  5 18:59 .gitignore
-rw-r--r-- 1 bandit31 root  147 Jun  5 18:59 README.md
bandit31@bandit:/tmp/bandit31/repo$ cat README.md 
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master

```
It seems we have a task to push file key.txt into remote repository. Lets create file, add it, commit and push.

```sh
bandit31@bandit:/tmp/bandit31/repo$ echo May I come in? > key.txt
bandit31@bandit:/tmp/bandit31/repo$ git add key.txt
The following paths are ignored by one of your .gitignore files:
key.txt
Use -f if you really want to add them.
bandit31@bandit:/tmp/bandit31/repo$ git add -f key.txt
bandit31@bandit:/tmp/bandit31/repo$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   key.txt

bandit31@bandit:/tmp/bandit31/repo$ git commit -m "add key.txt"
[master 21b65cf] add key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
bandit31@bandit:/tmp/bandit31/repo$ git push
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password: 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 321 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```
The push was unsuccessful however password was revealed
56a9bf19c63d650ce78e6ec0354ee45e