
### A few tools to assist in setting up a chroot jail (Linux)

When setting up a chroot jail knowing which shared object to copy
into the jail can be a problem.
The script `ls_shlibs` queries a list of executables and prints
a list of the required shared library files and runtime linkers.

### SYNOPSIS

    ls_shlibs [-D libdir1 -D libdir2...] [-d] program1 program2...

### EXAMPLES

    ls_shlibs /usr/bin/scp /usr/libexec/openssh/sftp-server

This lists the logical names of the needed libraries

    libc.so.6

rather than the actual library

    libc-2.12.so

The `-d` flag dereferences these links.  The actual files are usually
the ones to copy into the chroot jail. In order to set up the correct
links use `ldconfig(8)` to reestablish the links.
eg
	ldconfig -r $JAIL -n -N -v -l /lib64/libc-2.12.so

### EXAMPLE
```
ls_shlibs /bin/bash

/lib64/ld-linux-x86-64.so.2
/lib64/libc.so.6
/lib64/libdl.so.2
/lib64/libtinfo.so.5

ls_shlibs -d /bin/bash

/lib64/ld-2.12.so
/lib64/libc-2.12.so
/lib64/libdl-2.12.so
/lib64/libtinfo.so.5.7

cp -p /lib64/ld-2.12.so /lib64/libc-2.12.so \
      /lib64/libdl-2.12.so /lib64/libtinfo.so.5.7 \
      /tmp/jail/lib64


ldconfig -r /tmp/jail -n -N -v -l \
      /lib64/ld-2.12.so /lib64/libc-2.12.so \
      /lib64/libdl-2.12.so /lib64/libtinfo.so.5.7
	
ld-linux-x86-64.so.2 -> ld-2.12.so (changed)
libc.so.6 -> libc-2.12.so (changed)
libdl.so.2 -> libdl-2.12.so (changed)
libtinfo.so.5 -> libtinfo.so.5.7 (changed)

ls_shlibs -D /tmp/jail/lib64 /bin/bash

/lib64/ld-linux-x86-64.so.2
/tmp/jail/lib64/libc.so.6
/tmp/jail/lib64/libdl.so.2
/tmp/jail/lib64/libtinfo.so.5

```
	
### SEE ALSO

### LICENSE
Creative Commons CC0
[http://creativecommons.org/publicdomain/zero/1.0/legalcode]
(http://creativecommons.org/publicdomain/zero/1.0/legalcode)

### AUTHOR
[mailto:toves@sdf.lonestar.org](James Sainsbury)
