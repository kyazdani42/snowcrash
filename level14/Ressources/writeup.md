## writeup level14

When logging in as user level14, we are kind of lost because we cannot find anything to break in the system. \
But as its the last level, we thought: *why not break getflag directly ?*. \
So the first step we took was opening in gdb, but hey, nowadays there are more powerful tools right ? \
So we used `radare2` to analyze the binary. We started seeing some patterns (like `cmp` and `jump` that would jump to series of `ft_des` and `fputs`). \
Those `cmp` are using numbers which look very close to the user ids. So we thought, hey, why not bypass all the checks and jump directly to the flag14 getflag call ? \
and so we did. \
Here is the radare2 output: \
![alt text](img2.png?raw=true "radare1")
![alt text](img1.png?raw=true "radare2")
Here is the gdb output:
```gdb
(gdb) disass main
...
   0x08048b0a <+452>:	cmp    eax,0xbbe
   0x08048b0f <+457>:	je     0x8048ccb <main+901>
   0x08048b15 <+463>:	cmp    eax,0xbbe
   0x08048b1a <+468>:	ja     0x8048b68 <main+546>
   0x08048b1c <+470>:	cmp    eax,0xbba
   0x08048b21 <+475>:	je     0x8048c3b <main+757>
   0x08048b27 <+481>:	cmp    eax,0xbba
   0x08048b2c <+486>:	ja     0x8048b4d <main+519>
   0x08048b2e <+488>:	cmp    eax,0xbb8
   0x08048b33 <+493>:	je     0x8048bf3 <main+685>
   0x08048b39 <+499>:	cmp    eax,0xbb8
   0x08048b3e <+504>:	ja     0x8048c17 <main+721>
   0x08048b44 <+510>:	test   eax,eax
   0x08048b46 <+512>:	je     0x8048bc6 <main+640>
   0x08048b48 <+514>:	jmp    0x8048e06 <main+1216>
   0x08048b4d <+519>:	cmp    eax,0xbbc
   0x08048b52 <+524>:	je     0x8048c83 <main+829>
   0x08048b58 <+530>:	cmp    eax,0xbbc
   0x08048b5d <+535>:	ja     0x8048ca7 <main+865>
   0x08048b63 <+541>:	jmp    0x8048c5f <main+793>
   0x08048b68 <+546>:	cmp    eax,0xbc2
   0x08048b6d <+551>:	je     0x8048d5b <main+1045>
   0x08048b73 <+557>:	cmp    eax,0xbc2
   0x08048b78 <+562>:	ja     0x8048b95 <main+591>
   0x08048b7a <+564>:	cmp    eax,0xbc0
   0x08048b7f <+569>:	je     0x8048d13 <main+973>
   0x08048b85 <+575>:	cmp    eax,0xbc0
   0x08048b8a <+580>:	ja     0x8048d37 <main+1009>
   0x08048b90 <+586>:	jmp    0x8048cef <main+937>
   0x08048b95 <+591>:	cmp    eax,0xbc4
   0x08048b9a <+596>:	je     0x8048da3 <main+1117>
   0x08048ba0 <+602>:	cmp    eax,0xbc4
   0x08048ba5 <+607>:	jb     0x8048d7f <main+1081>
   0x08048bab <+613>:	cmp    eax,0xbc5
   0x08048bb0 <+618>:	je     0x8048dc4 <main+1150>
   0x08048bb6 <+624>:	cmp    eax,0xbc6
   0x08048bbb <+629>:	je     0x8048de5 <main+1183>
   0x08048bc1 <+635>:	jmp    0x8048e06 <main+1216>
...
   0x08048d3e <+1016>:	mov    DWORD PTR [esp],0x804919c
   0x08048d45 <+1023>:	call   0x8048604 <ft_des>
   0x08048d4a <+1028>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048d4e <+1032>:	mov    DWORD PTR [esp],eax
   0x08048d51 <+1035>:	call   0x8048530 <fputs@plt>
   0x08048d56 <+1040>:	jmp    0x8048e2f <main+1257>
   0x08048d5b <+1045>:	mov    eax,ds:0x804b060
   0x08048d60 <+1050>:	mov    ebx,eax
   0x08048d62 <+1052>:	mov    DWORD PTR [esp],0x80491b6
   0x08048d69 <+1059>:	call   0x8048604 <ft_des>
   0x08048d6e <+1064>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048d72 <+1068>:	mov    DWORD PTR [esp],eax
   0x08048d75 <+1071>:	call   0x8048530 <fputs@plt>
   0x08048d7a <+1076>:	jmp    0x8048e2f <main+1257>
   0x08048d7f <+1081>:	mov    eax,ds:0x804b060
   0x08048d84 <+1086>:	mov    ebx,eax
   0x08048d86 <+1088>:	mov    DWORD PTR [esp],0x80491d0
   0x08048d8d <+1095>:	call   0x8048604 <ft_des>
   0x08048d92 <+1100>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048d96 <+1104>:	mov    DWORD PTR [esp],eax
   0x08048d99 <+1107>:	call   0x8048530 <fputs@plt>
   0x08048d9e <+1112>:	jmp    0x8048e2f <main+1257>
   0x08048da3 <+1117>:	mov    eax,ds:0x804b060
   0x08048da8 <+1122>:	mov    ebx,eax
   0x08048daa <+1124>:	mov    DWORD PTR [esp],0x80491ea
   0x08048db1 <+1131>:	call   0x8048604 <ft_des>
   0x08048db6 <+1136>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048dba <+1140>:	mov    DWORD PTR [esp],eax
   0x08048dbd <+1143>:	call   0x8048530 <fputs@plt>
   0x08048dc2 <+1148>:	jmp    0x8048e2f <main+1257>
   0x08048dc4 <+1150>:	mov    eax,ds:0x804b060
   0x08048dc9 <+1155>:	mov    ebx,eax
   0x08048dcb <+1157>:	mov    DWORD PTR [esp],0x8049204
   0x08048dd2 <+1164>:	call   0x8048604 <ft_des>
   0x08048dd7 <+1169>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048ddb <+1173>:	mov    DWORD PTR [esp],eax
   0x08048dde <+1176>:	call   0x8048530 <fputs@plt>
   0x08048de3 <+1181>:	jmp    0x8048e2f <main+1257>
   0x08048de5 <+1183>:	mov    eax,ds:0x804b060
   0x08048dea <+1188>:	mov    ebx,eax
   0x08048dec <+1190>:	mov    DWORD PTR [esp],0x8049220
   0x08048df3 <+1197>:	call   0x8048604 <ft_des>
   0x08048df8 <+1202>:	mov    DWORD PTR [esp+0x4],ebx
   0x08048dfc <+1206>:	mov    DWORD PTR [esp],eax
   0x08048dff <+1209>:	call   0x8048530 <fputs@plt>
   0x08048e04 <+1214>:	jmp    0x8048e2f <main+1257>
   0x08048e06 <+1216>:	mov    eax,ds:0x804b060
   0x08048e0b <+1221>:	mov    edx,eax
   0x08048e0d <+1223>:	mov    eax,0x8049248
   0x08048e12 <+1228>:	mov    DWORD PTR [esp+0xc],edx
   0x08048e16 <+1232>:	mov    DWORD PTR [esp+0x8],0x38
   0x08048e1e <+1240>:	mov    DWORD PTR [esp+0x4],0x1
   0x08048e26 <+1248>:	mov    DWORD PTR [esp],eax
   0x08048e29 <+1251>:	call   0x80484c0 <fwrite@plt>
...
(gdb) b *0x8048999
Breakpoint 1 at 0x8048999
(gdb) r
Breakpoint 1, 0x8048999 in main ()
=> 0x08048999 <main+83>:	e8 42 fb ff ff	call   0x80484e0 <puts@plt>
(gdb) jump *0x8048de5
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
[Inferior 1 (process 7096) exited normally]
(gdb) quit
```
```shell
$ su flag14
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
Congratulation. Type getflag to get the key and send it to me the owner of this livecd :)

$ getflag
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
```