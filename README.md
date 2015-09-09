
### Linux - enumerate the shared objects required by an ELF binary
When setting up a chroot jail knowing what shared object to copy
into the jail is a problem. This script query the binary and print
a list of commented out command which would copy the required files
and establish the correct logical links within the jail.

### SYNOPSIS

    ls_chroot_shlibs -r /Jail elf_binary

### EXAMPLES
    
````ls_chroot_shlibs -r /jail /usr/bin/scp
````
Output is a list commands (cp and ldconfig) to copy the underlying
shared libraries and runtime loaders to /Jail/lib /Jail/lib64.
The commands are prefixed with a shell comment (#X by default.)


````  
#! /bin/sh
## We work out what shared objects the binary needs
## (the actual files not symlinks) and copy those into
## the chroot (/jail) library directory (/lib64)
## The logical links are reestablished with ldconfig(8)
## 
## The actual commands are prefixed with a comment #X
## For the brave or foolhardy:
## 
## ls_chroot_shlibs -r /jail /usr/bin/scp | sed -e 's/^#X// | sh -s
## 
## The rest of us might redirect the output into a file
## to peruse the contents and edit as required before executing

#X cp -p /lib64/ld-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/ld-2.12.so 

#X cp -p /usr/lib64/libcrypto.so.1.0.1e /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /usr/lib64/libcrypto.so.1.0.1e 

#X cp -p /lib64/librt-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/librt-2.12.so 

#X cp -p /lib64/libdl-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libdl-2.12.so 

#X cp -p /lib64/libutil-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libutil-2.12.so 

#X cp -p /lib64/libz.so.1.2.3 /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libz.so.1.2.3 

#X cp -p /lib64/libnsl-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libnsl-2.12.so 

#X cp -p /lib64/libcrypt-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libcrypt-2.12.so 

#X cp -p /lib64/libresolv-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libresolv-2.12.so 

#X cp -p /lib64/libc-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libc-2.12.so 

#X cp -p /lib64/libpthread-2.12.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libpthread-2.12.so 

#X cp -p /lib64/libfreebl3.so /jail/lib64 
#X ldconfig -n -N -v -l -r /jail /lib64/libfreebl3.so 
````

### PREREQUISITES
ls_chroot_shlibs is a shell script (sh,bash) that uses the same
techniques as ldd(1) to enumerate the required shared libraries.
Uses readlink(1) and ldconfig(8).

### SEE ALSO

### LICENSE
Creative Commons CC0
[http://creativecommons.org/publicdomain/zero/1.0/legalcode]
(http://creativecommons.org/publicdomain/zero/1.0/legalcode)

### AUTHOR
[mailto:toves@sdf.lonestar.org](James Sainsbury)
