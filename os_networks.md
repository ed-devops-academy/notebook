# Linux OS and networks

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