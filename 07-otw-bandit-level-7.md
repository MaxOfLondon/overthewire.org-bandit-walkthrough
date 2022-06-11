#Level 7
Connect to game via ssh on port 2220 using credentials bandit7:HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
_(format username:password)_
```sh
$ ssh -p 2220 bandit7@bandit.labs.overthewire.org
```

#Level Goal
The password for the next level is stored in the file data.txt next to the word millionth

**Commands you may need to solve this level**
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

#Walkthrough
There are several ways we can achieve this goal. File is of large size and opening it in editor and searching for word 'millionth' is one of the options but seems cumbersome. We can build a better workflow to filter for sepcific condition.
We will grep file for word 'millionth' then filter result again for strings with length of 32 alphanumeric characters since from previous exercises we knwow the flag is of that length and format (md5 hash).

Looking at manpage of grep we can see:
```
       -o, --only-matching
              Print only the matched (non-empty) parts of a matching line, with each such part on a separate output line.
...
   Repetition
       A regular expression may be followed by one of several repetition operators:
       ?      The preceding item is optional and matched at most once.
       *      The preceding item will be matched zero or more times.
       +      The preceding item will be matched one or more times.
       {n}    The preceding item is matched exactly n times.
       {n,}   The preceding item is matched n or more times.
       {,m}   The preceding item is matched at most m times.  This is a GNU extension.
       {n,m}  The preceding item is matched at least n times, but not more than m times.
...
   Pattern Syntax
       -E, --extended-regexp
              Interpret PATTERNS as extended regular expressions (EREs, see below).
...
   Basic vs Extended Regular Expressions
       In basic regular expressions the meta-characters ?, +, {, |, (, and ) lose their special meaning; instead use the backslashed versions \?, \+, \{, \|, \(, and \).
...
   Character Classes and Bracket Expressions
       A bracket expression is a list of characters enclosed by [ and ].  It matches any single character in that list.  If the first character of the  list  is  the  caret  ^  then  it
       matches any character not in the list; it is unspecified whether it matches an encoding error.  For example, the regular expression [0123456789] matches any single digit.

       Within  a  bracket  expression,  a  range  expression  consists  of  two characters separated by a hyphen.  It matches any single character that sorts between the two characters,
       inclusive, using the locale's collating sequence and character set.  For example, in the default C locale, [a-d] is  equivalent  to  [abcd].   Many  locales  sort  characters  in
       dictionary order, and in these locales [a-d] is typically not equivalent to [abcd]; it might be equivalent to [aBbCcDd], for example.  To obtain the traditional interpretation of
       bracket expressions, you can use the C locale by setting the LC_ALL environment variable to the value C.

       Finally, certain named classes of characters are predefined within bracket expressions, as follows.  Their  names  are  self  explanatory,  and  they  are  [:alnum:],  [:alpha:],
       [:blank:],  [:cntrl:], [:digit:], [:graph:], [:lower:], [:print:], [:punct:], [:space:], [:upper:], and [:xdigit:].  For example, [[:alnum:]] means the character class of numbers
       and letters in the current locale.  In the C locale and ASCII character set encoding, this is the same as [0-9A-Za-z].  (Note that the brackets in these class names are  part  of
       the  symbolic  names,  and  must  be  included  in  addition  to  the brackets delimiting the bracket expression.)  Most meta-characters lose their special meaning inside bracket
       expressions.  To include a literal ] place it first in the list.  Similarly, to include a literal ^ place it anywhere but first.  Finally, to include a literal - place it last.

```

```
$ ssh -p 2220 bandit7@bandit.labs.overthewire.org
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit7@bandit.labs.overthewire.org's password: 
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

bandit7@bandit:~$ ls -al
total 4108
drwxr-xr-x  2 root    root       4096 May  7  2020 .
drwxr-xr-x 41 root    root       4096 May  7  2020 ..
-rw-r--r--  1 root    root        220 May 15  2017 .bash_logout
-rw-r--r--  1 root    root       3526 May 15  2017 .bashrc
-rw-r-----  1 bandit8 bandit7 4184396 May  7  2020 data.txt
-rw-r--r--  1 root    root        675 May 15  2017 .profile
bandit7@bandit:~$ grep millionth data.txt | grep -oE '[[:alnum:]]{32}'
cvX2JJa4CFALtqS87jk27qwqGhBM9plV
bandit7@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```