## writeup level09

When logging in as user level09, we can see 2 files: a token file, readable, and a `level09` file. \
When we execute `level09`, we realize that the string passed in as parameter is going through an algorithm \
which, for each character, adds its position as value. For instance:
```shell
$ ./level09 abcd
aceg
```
When we look inside the `token` file, we see it has garbage characters. It means there might be some non-printable \
ascii characters, so we pass it in inside the `hexdump` binary:
```shell
$ cat -e token
f4kmm6p|=M-^B^?pM-^BnM-^CM-^BDBM-^CDu{^?M-^LM-^I$
$ hexdump -C token
00000000  66 34 6b 6d 6d 36 70 7c  3d 82 7f 70 82 6e 83 82  |f4kmm6p|=..p.n..|
00000010  44 42 83 44 75 7b 7f 8c  89 0a                    |DB.Du{....|
0000001a
```
The only part of interest is the hexadecimal represention of ascii values (in the middle). \
We supposed the flag went through the incremental algorithm, that's why we had such weird values. \
So we wrote a little js script to decode this and print it as a string:
```shell
$ # from some place you have nodeJs installed
$ # i agree we could have done that in python or bash, but well..., who cares?
$ node ./decoder.js
f3iji1ju5yuevaus41q1afiuq

$ su flag09
f3iji1ju5yuevaus41q1afiuq

$ # we are logged in !
$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```