Mac OS X Unlocker for VMware V2.0
=================================

+-----------------------------------------------------------------------------+
| IMPORTANT:                                                                  |
| ==========                                                                  |
|                                                                             |
| Always uninstall the previous version of the Unlocker before using a new    |
| version. Failure to do this could render VMware unusable especially ESXi.   |
|                                                                             |
+-----------------------------------------------------------------------------+

1. Introduction
---------------

Unlocker 2 is designed for Workstation 11/12, Player 7/12, ESXi 6 and Fusion 7/8.

If you are using an earlier product please continue using Unlocker 1 

Version 2 has been tested against:

* Workstation 11/12 on Windows and Linux
* Player 7 & Workstation Player 12 on Windows and Linux
* Fusion 7/8 on El Capitan and Sierra
* ESXi 6.0/6.5

The patch code carries out the following modifications dependent on the product
being patched:

* Fix vmware-vmx and derivatives to allow Mac OS X to boot
* Fix vmwarebase .dll or .so to allow Apple to be selected during VM creation
* Fix libvmkctl.so on ESXi 6 to allow use with vCenter
* Download a copy of the latest VMware Tools for OS X

Note that not all products recognise the darwin.iso via install tools menu item.
You will have to manually mount the darwin.iso for example on Workstation 11 and Player 7.

The vmwarebase code does not need to be patched on OS X or ESXi so you will see a
message on those systems telling you that it will not be patched.

In all cases make sure VMware is not running, and any background guests have
been shutdown.

The code written in Python as it makes the Unlocker easier to run and maintain on ESXi.

2. Prerequisites
----------------

The code requires Python 2.7 to work. Most Linux distros, ESXi and OS X ship with a compatible
Python interpreter and should work without requiring any additional software.

Windows Unlocker has a packaged version of the Python script using PyInstaller, and so does not
require Python to be installed.

3. Limitations
--------------

If you are using VMware Player or Workstation on Windows you may get a core dump.

Latest Linux and ESXi products are OK and do not show this problem.

+-----------------------------------------------------------------------------+
| IMPORTANT:                                                                  |
| ==========                                                                  |
|                                                                             |
| If you create a new VM using version 11, 12 or 13 hardware VMware may stop  |
| and create a core dump. There are two options to work around this issue:    |
|                                                                             |
| 1. Change the VM to be HW 10 - this does not affect performance.            |
| 2. Edit the VMX file and add:                                               |
|    smc.version = "0"                                                        |
|                                                                             |
+-----------------------------------------------------------------------------+

4. Windows
----------
On Windows you will need to either run cmd.exe as Administrator or using
Explorer right click on the command file and select "Run as administrator".

win-install.cmd   - patches VMware
win-uninstall.cmd - restores VMware
win-update-tools.cmd - retrieves latest OS X guest tools

5. Linux
---------
On Linux you will need to be either root or use sudo to run the scripts.

You may need to ensure the Linux scripts have execute permissions
by running chmod +x against the 2 files.

lnx-install.sh   - patches VMware
lnx-uninstall.sh - restores VMware
lnx-update-tools.cmd - retrieves latest OS X guest tools

6. Mac OS X
-----------
On Mac OS X you will need to be either root or use sudo to run the scripts.
This is really only needed if you want to use client versions of Mac OS X.

You may need to ensure the OS X scripts have execute permissions
by running chmod +x against the 2 files.

osx-install.sh   - patches VMware
osx-uninstall.sh - restores VMware

7. ESXi
-------
You will need to transfer the zip file to the ESXi host either using vSphere client or SCP.

Once uploaded you will need to either use the ESXi support console or use SSH to
run the commands. Use the unzip command to extract the files. 

<<< WARNING: use a datastore volume to store and run the scripts >>>

Please note that you will need to reboot the host for the patches to become active.
The patcher is embbedded in a shell script local.sh which is run at boot from /etc/rc.local.d.

You may need to ensure the ESXi scripts have execute permissions
by running chmod +x against the 2 files.

esxi-install.sh   - patches VMware 
esxi-uninstall.sh - restores VMware 

There is a boot option for ESXi that disables the unlocker if there is a problem.

At the ESXi boot screen press shift + o to get the boot options and add nounlocker.

Note:
1. Any changes you have made to local.sh will be lost. If you have made changes to 
   that file, you will need to merge them into the supplied local.sh file.
2. The unlocker needs to be re-run after an upgrade or patch is installed on the ESXi host.
3. The macOS VMwwre tools are no longer shipped in the image from ESXi 6.5. They have to be
   downloaded and installed manually onto the ESXi host. For additional details see this web page:

   https://blogs.vmware.com/vsphere/2016/10/introducing-vmware-tools-10-1-10-0-12.html
   
8. Thanks
---------

Thanks to Zenith432 for originally building the C++ unlocker and Mac Son of Knife
(MSoK) for all the testing and support.

Thanks also to Sam B for finding the solution for ESXi 6 and helping me with
debugging expertise. Sam also wrote the code for patching ESXi ELF files and
modified the unlocker code to run on Python 3 in the ESXi 6.5 environment.


History
-------
12/12/14 2.0.0 - First release
13/13/14 2.0.1 - Removed need for Python for Windows
13/13/14 2.0.2 - darwin.iso was missing from zip file
02/01/15 2.0.3 - Added EFI firmware files to remove Server check
               - Refactored Python code
07/01/15 2.0.4 - Added View USB Service to Windows batch files
               - Fixed broken GOS Table patching on Linux
18/06/15 2.0.5 - ESXi 6 working
               - Latest tools from Fusion 7.1.2
20/06/15 2.0.6 - ESXi 6 patch for smcPresent vCenter compatibility
16/09/15 2.0.7 - Workstation 12 on Linux fixes
14/11/15 2.0.8 - Player 12 on Linux fixes
               - Get latest VMware tools command
               - Removed firmware files
               - Moved to PyInstaller 3.0
29/12/16 2.0.9 - New version to support ESXi 6.5
               - Disable new hostd VMX sandbox
               - Fix ESXI 6.5 libvmkctl.so patching for 32 and 64-bit versions
               - Added ESXi boot option to disable unlocker (nounlocker)

(c) 2011-2016 Dave Parsons