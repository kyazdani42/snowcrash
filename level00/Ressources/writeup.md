Looking around the system, we see that `/tmp` has `-wx-wx-wx` rights, which means \
we can write and execute but not read, which will help us to download files, \
compile programs, and execute them.

When running this [script](https://github.com/mzet-/linux-exploit-suggester), we found that dirty cow can help us escalate privileges. \
So we created `/tmp/dirtycow.c`, compiled it with gcc and executed it so we can have root access and move around the system more freely.
