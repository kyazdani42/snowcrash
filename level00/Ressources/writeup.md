## level 00 writeup

We looked around the system for a while, but we know we were looking for the flag00 user password, so we did:
```
$ ls -lR / 2>/dev/null | grep flag00
...
----r--r--  1 flag00   flag00      15 Mar  5  2016 john
``` 
We found this `/usr/sbin/john` file containing a weird looking string.
```
$ cat /usr/sbin/john
cdiiddwpgswtgt
```
We had to try multiple stuff on this and finally we got it:
```
## rot 15 ##
$ cat /usr/sbin/john | tr ‘p-za-oP-ZA-O’ ‘a-zA-Z’
nottoohardhere
```
We then execute `su flag00` with password `nottoohardhere` and we are logged as flag00, we can then get the flag for user level01 with the `getflag` command.
```
flag00@SnowCrash:~$ getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```
