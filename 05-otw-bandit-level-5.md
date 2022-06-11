#Level 5
Connect to game via ssh on port 2220 using credentials bandit5:koReBOKuIDDepwhWk7jZC0RTdopnAYKh
_(format username:password)_
```sh
$ ssh -p 2220 bandit5@bandit.labs.overthewire.org
```

#Level Goal
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

human-readable
1033 bytes in size
not executable

**Commands you may need to solve this level**
ls, cd, cat, file, du, find

#Walkthrough

The challenge is to find file with specific properties. Looking at man page of find command we can see following switches:
```
...
       -type c
              File is of type c:

              b      block (buffered) special

              c      character (unbuffered) special

              d      directory

              p      named pipe (FIFO)

              f      regular file

              l      symbolic link; this is never true if the -L option or the -follow option is in effect, unless the symbolic link is broken.  If you want to search for symbolic links
                     when -L is in effect, use -xtype.

              s      socket

              D      door (Solaris)

              To search for more than one type at once, you can supply the combined list of type letters separated by a comma `,' (GNU extension).
...
       -executable
              Matches  files  which  are  executable  and directories which are searchable (in a file name resolution sense) by the current user.  This takes into account access control
              lists and other permissions artefacts which the -perm test ignores.  This test makes use of the access(2) system call, and so can be fooled by NFS  servers  which  do  UID
              mapping  (or  root-squashing), since many systems implement access(2) in the client's kernel and so cannot make use of the UID mapping information held on the server.  Be‐
              cause this test is based only on the result of the access(2) system call, there is no guarantee that a file for which this test succeeds can actually be executed.
...
       -size n[cwbkMG]
              File uses n units of space, rounding up.  The following suffixes can be used:

              `b'    for 512-byte blocks (this is the default if no suffix is used)

              `c'    for bytes

              `w'    for two-byte words

              `k'    for kibibytes (KiB, units of 1024 bytes)

              `M'    for mebibytes (MiB, units of 1024 * 1024 = 1048576 bytes)

              `G'    for gibibytes (GiB, units of 1024 * 1024 * 1024 = 1073741824 bytes)

              The size is simply the st_size member of the struct stat populated by the lstat (or stat) system call, rounded up as shown above.  In other words, it's consistent with the
              result you get for ls -l.  Bear in mind that the `%k' and `%b' format specifiers of -printf handle sparse files differently.  The `b' suffix always denotes 512-byte blocks
              and never 1024-byte blocks, which is different to the behaviour of -ls.

              The  +  and  -  prefixes signify greater than and less than, as usual; i.e., an exact size of n units does not match.  Bear in mind that the size is rounded up to the next
              unit.  Therefore -size -1M is not equivalent to -size -1048576c.  The former only matches empty files, the latter matches files from 0 to 1,048,575 bytes.
...
   OPERATORS
       Listed in order of decreasing precedence:

       ( expr )
              Force precedence.  Since parentheses are special to the shell, you will normally need to quote them.  Many of the examples in this manual page  use  backslashes  for  this
              purpose: `\(...\)' instead of `(...)'.

       ! expr True if expr is false.  This character will also usually need protection from interpretation by the shell.

...
```
The requirement is to find human-readable files however such option is not available for find command. We can negate options using ```!``` operator and under vague assumption that human readable files are not executable we can use switch ```! -executable```.
```sh
bandit5@bandit:~$ find inhere/ -type f -size 1033c ! -executable -exec file {} + | grep text
inhere/maybehere07/.file2: ASCII text, with very long lines
bandit5@bandit:~$ cat inhere/maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
...
bandit5@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```