# Linux OS and networks

## **Contents**
* [Installing and working with a boot manager (GRUB 2)](#installing-and-working-with-a-boot-manager-grub-2)
* [Shared libraries managment](#shared-libraries-managment)

## **Installing and working with a boot manager (GRUB 2)**

The instructions on this section are to be used with GRUB 2 boot manager. Usually modern linux installations come by default with a GRUB 2 manager installed, in case that is not installed on your system can be installed using the package manager of your current system like: apt, yum, etc.

### Folder/files olders of insterest
<hr/>

This file and directories locations could vary from OS to OS. The ones presented here were checked using Ubuntu OS

#### **1- Directory /boot/grub/:**

Heart of the GRUB 2 manager, that allow the system to boot with grub. On this dir is located a file called *grub.cfg* that store a script with the auto-generated configuration of GRUB. **Important!!** this configuration file must not manually edited cause will be overwrite on a update of the system 

#### **2- Directory /etc/grub.d/:**

This folder scripts that generate the the configuration of the GRUB loader in a order given by the number of the begining of the script file name. If you need to customize the GRUB 2 menu, you add your changes to a file in /etc/grub.d/ such as *40_custom*, or add your own file.

#### **3- File /etc/default/grub:**
Add or changes variables on this file to change the configuration/style of GRUB boot menu. To change to effect is need to run command `update-grub` or on others OS could be called `update-grub2`

**Important configuration variables**
* **GRUB_TIMEOUT:** Timeout on seconds to init system by defualt option
* **GRUB_DISTRIBUTOR:** String that shows system destribution name
* **GRUB_MENU_PICTURE:** Background picture to be showed on menu

### Commands of interest
<hr/>

**1- grub-install** build a new core image file on a device
```
[root@emiguel-ubuntu ~]# # Build a core.img file on Fedora 22
[root@emiguel-ubuntu ~]# grub2-install --recheck --grub-setup=/bin/true /dev/sda
Installing for i386-pc platform.
Installation finished. No error reported.
[root@emiguel-ubuntu ~]# ls -l /boot/grub2/i386-pc/core.img
-rw-r--r--. 1 root root 25887 Jul 12 22:56 /boot/grub2/i386-pc/core.img

root@emiguel-ubuntu:~$ # Build a core.img file on Ubuntu 14
root@emiguel-ubuntu:~$ sudo grub-install --non-sectarian /dev/sda
[sudo] password for root:
Installing for i386-pc platform.
Installation finished. No error reported.
root@emiguel-ubuntu:~$ ls -l /boot/grub/i386-pc/core.img
-rw-r--r-- 1 root root 25363 Jul 12 23:15 /boot/grub/i386-pc/core.img
```

**Booting with GRUB 2 rescue CD from existing GRUB installation**

Use the next example to repair a old grub installation that got hidden cause of a new OS installation.

From GRUB boot terminal use `ls` command to list the devices and search for the system that have a GRUB installation. Next follow the next steps:

* `set prefix=(hd1,gpt1)/boot/grub` *--to point grub root installation that you want to boot--*
* `set root=(hd1,gpt1)/` *-- partition root where GRUB  is installed --*
* `lsmod normal`
* `normal`

**2- grub-mkrescue or grub2-mkrescue** command to help you create a rescue CD image

```
[root@emiguel-ubuntu ~]# # Build a core.img file on Fedora 22
[root@emiguel-ubuntu ~]# grub2-install --recheck --grub-setup=/bin/true /dev/sda
Installing for i386-pc platform.
Installation finished. No error reported.
[root@emiguel-ubuntu ~]# ls -l /boot/grub2/i386-pc/core.img
-rw-r--r--. 1 root root 25887 Jul 12 22:56 /boot/grub2/i386-pc/core.img

root@emiguel-ubuntu:~$ # Build a core.img file on Ubuntu 14
root@emiguel-ubuntu:~$ sudo grub-install --non-sectarian /dev/sda
[sudo] password for root:
Installing for i386-pc platform.
Installation finished. No error reported.
root@emiguel-ubuntu:~$ ls -l /boot/grub/i386-pc/core.img
-rw-r--r-- 1 root root 25363 Jul 12 23:15 /boot/grub/i386-pc/core.img
```
and use command `dd` to write the rescue image to a USB flash drive

```
root@emiguel-ubuntu:~$ # Burn .iso image to USB stick /dev/sde
root@emiguel-ubuntu:~$ sudo dd if=rescue.iso of=/dev/sde
9864+0 records in
9864+0 records out
5050368 bytes (5.1 MB) copied, 3.95946 s, 1.3 MB/s
```

**3- grub-update** command to generate a new /boot/grub/grub.cfg file
```
root@emiguel-ubuntu:~$ sudo grub-update
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.16.0-43-generic
Found initrd image: /boot/initrd.img-3.16.0-43-generic
Found linux image: /boot/vmlinuz-3.16.0-30-generic
Found initrd image: /boot/initrd.img-3.16.0-30-generic
Found memtest86+ image: /boot/memtest86+.elf
Found memtest86+ image: /boot/memtest86+.bin
Found Fedora release 20 (Heisenbug) on /dev/sda10
Found CentOS release 6.6 (Final) on /dev/sda11
Found Fedora release 22 (Twenty Two) on /dev/sda5
Found Slackware Linux (Slackware 13.37.0) on /dev/sda6
Found Fedora release 18 (Spherical Cow) on /dev/sda7
Found openSUSE 11.4 (x86_64) on /dev/sda8
Found Ubuntu 12.04 LTS (12.04) on /dev/sda9
done
```

## **Shared libraries managment**

Linux shared library can have three names. Which are:

* **Linker name** (eg: libexample.so)
* **Soname** (eg : libexample.so.1.2) Represent the major version of the library
* **Real Name** (eg : libexample.so.1.2.3) Minor version of the library. The actual name

Linker name points to the soname, and soname points to real name

### Commands of interest
<hr/>

* **ldconfig:** `ldconfig -n pathof_dir_where_shared_library_is_located`. This command is to install a custom shared library. *ldconfig* creates a symbolic with soname which points to the real name and update the cache which is used by programs to speed up the loading of shared libraries. ldconfig doesn’t setup the linker name. Therefore, you have to manually create a symbolic link with the linker name which points to the soname. The command you would have to execute for this purpose is `ln -s libexample.so.1.2 libexample.so`.
* **ldd:** `ldd` command print shared object dependencies
```
# example to check ls command shared libraries dependencies
emiguel@emiguel-ubuntu:~$ ldd /usr/bin/ls
linux-vdso.so.1 (0x00007fff5b38d000)
libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007f45902a0000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f45900ae000)
libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007f459001e000)
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f4590018000)
/lib64/ld-linux-x86-64.so.2 (0x00007f459030d000)
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f458fff5000)
```

### Shared libraries directories
<hr/>

Use environment variable **LD_LIBRARY_PATH** to add new custom directories with dynamic libraries. Also you can modified file /etc/ld.so.conf to add new libraries instead the adding it to save dirs like */usr/lib*, */lib* or */usr/local/lib*



