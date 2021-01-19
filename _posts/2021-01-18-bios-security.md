---
layout: post
title:  BIOS Security to Protect your Files
image:  bios/bootloader.jpg
tags:   Security
---
Almost everyone is taught that if they use strong passwords on their computer, that it will keep attackers out. However, with physical access to an unhardened computer, anyone could easily circumvent the login and directly access your data, including online accounts you're logged into and sensitive documents. Many people are not taught to protect against physical attacks or they dismiss them as improbable, but if an attacker can physically reach a machine, these attacks can be very reliable and easy even for an amature. In this post you will be shown how to do one such attack and how to prevent it. At the end you will have a safer system and even be able to recover files when you get locked out of your system!

NOTE: It is slowly becoming more common for computers to ship with the necessary protections or for organizations to enable them on company laptops, so although many many computers will remain vulnerable, this tutorial may not work on yours if it's protected already (great!).

## The Attack
*Disclaimer: This is for educational purposes only. Do not bypass security on systems you neither own nor are explicitly authorized to attack by the owner.*

We will use an approach similar to what a computer repair person may use to recover your files. When booting, normally your computer's BIOS finds a valid bootable OS on your hard drive and attempts to run it. We will prepare a second bootable system on a thumb drive and tell the BIOS to boot into it instead. Once booted, you can mount your main drive and access any files on the other drive without using your login credentials from the main system.

### Step 1: Prepare Thumb Drive
You will need...
- A live OS. As an example, I'll use [SystemRescueCD](https://www.system-rescue.org/Download/).
- A thumb drive at least 1GB (or as large as the ISO).
- A program to flash the OS to the thumb drive. I recommend [Balena Etcher](https://www.balena.io/etcher/).

When picking your thumb drive, please ensure you are okay LOSING ALL DATA on it. Copy the data somewhere before proceeding!

Now open Etcher, copy and paste the download link for the 64 bit .iso file, and select the thumb drive where you want to flash the OS. Once it's done, congratulations! You now have a handy data recovery tool to serve you now and anytime you need to recover data from an unusable system.

### Step 2: Boot into SystemRescueCD
Restart the computer and as soon as the lights turn on, quickly repeatedly press whatever key on your computer opens the BIOS' boot menu until it appears. This is often F2 or F12. It may take a couple tries for you to get the timing and the correct keys. You may be able to check online how to reach the boot menu for your model of computer.

In the boot menu, you should see a list of drives. Use your arrow keys to select your thumb drive then press enter. Has it booted the other OS? Great work! You just completed the step we will prevent attackers from later.

### Step 3: Access the Data
Because you booted into a different OS that doesn't need your login but has access to your hardware, none of your files are protected by your password.

1. When the teal bootloader comes up, press enter on the first option.

   ![bootloader](/images/bios/bootloader.jpg)

2. When the black command line appears, type `startx`.

   ![command line](/images/bios/terminal.jpg)

3. Click on the hard drive logo in the taskbar to open GParted.
4. Recognize which partition your main OS is on based on the size and the name in the list.
5. Open the terminal with the black square in the taskbar.
6. Type `mount /dev/sda1 /mnt/`, substituting `/dev/sda1` for your partition path from step 4.
7. Double click on the "File System" button in the corner of the desktop, and go to mnt -> Users -> YourUsername

   ![desktop](/images/bios/desktop.jpg)

There you see all of your files! The "AppData" folder is where your applications store most of your offline data, including passwords you've saved in your browser and cookies for sites you haven't logged out of.

This may have been many steps for us, but an attacker can prepare a thumb drive to automatically do all the steps in this section in half a jiffy (pun intended).


## The Defense
You will set a BIOS password to ensure no one else can modify how the computer boots. We need to restart again while pressing a special key to get to the BIOS admin menu. This is often F12 or F10 but (like the boot menu) varies. In this menu you probably can only navigate with the up, down, left, right arrow keys. You want want to find where you set 1-3 passwords.

These may protect:
- Reaching this admin menu
- Reaching the boot menu you used earlier
- Booting the computer at all

Be very certain not to forget this password or you could permanently lock yourself out!

Remember the password? Save the settings and restart. Try repeating step 2 to check that your protection works. Did it work? Great job! You are now protected from one of the easiest exploits in the book!

However, this isn't the end of the story. Security isn't a destination; it's a process. See below for a brief summary of other physical attacks and links for further learning.

## Remaining Attacks and Mitigations
Attackers with time to spare could still bypass login by physically removing the drive and connecting it to another machine. To protect your data, you want [full disk encryption](https://en.wikipedia.org/wiki/Disk_encryption#Full_disk_encryption). You will then enter a password or use a [hardware token](https://en.wikipedia.org/wiki/Security_token) to decrypt the drive before loading the rest of the system. We did not cover this because the process varies drastically depending on your operating system, but here is Windows' [help page](https://support.microsoft.com/en-us/windows/device-encryption-in-windows-10-ad5dcf4b-dbe0-2331-228f-7925c2a3012d) on it.

Full disk encryption combined with a strong password is a great measure that can protect you from even very tech savvy thieves. However, other users sharing the machine also need to decrypt the drive to see their files, so they can use step 3 combined with the encryption key to see your files without your login. If you have very determined adversaries, you should know about [evil maid attacks](https://en.wikipedia.org/wiki/Evil_Maid_attack), which could be partially mitigated with [secure boot](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface#Secure_boot).
