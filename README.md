# Razer-Blade-Stealth-Hackintosh-Guide
Hackintosh for Kaby Lake RBS Guide

## **PLEASE READ THROUGH THE ENTIRE GUIDE FIRST**

This should be a relatively simple guide for setting up a hackintosh on a Razer Blade Stealth (mine is a 7200U i5 processor BIOS Version 8.02) 
If you fuck something up on your laptop, that’s on you…

Anyway

**Things to take note**

* wifi will not work natively, it will require a USB dongle or a new card to be installed

* trackpad works with tracking and right/left click buttons at the bottom and tap-to-click works all over on the trackpad, there are no multi gestures, just basic functionality 

**THINGS I HAVE NOT TESTED YET:**
* ANYTHING HDMI
* ANYTHING THUNDERBOLT 3 
* Sleep sorta works - display may require 2 openings to turn on from sleep

**Things that should work fine**
	
* Intel HD 620 Graphics (I recommend settings the resolution to 2048x1152 (by alt click on scaled when default is selected for display settings))
* Audio
* Backlight
* Trackpad
* Battery Status

Time to get into it

## **Setting up USB drive**

Instructions from: https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/

In terminal
`diskutil list` find out what /dev/disk# the flash drive is on

`diskutil partitionDisk /dev/disk# 2 MBR FAT32 "CLOVER EFI" 200Mi HFS+J "install_osx" R` change the # with the number the drive is

Download Clover from [here](https://sourceforge.net/projects/cloverefiboot/)

In the clover .pkg

* Select CLOVER EFI using change install location
* Select Customize
* Uncheck “Install for UEFI booting only”
* Uncheck “Install Clover in the ESP”
* Check “Install boot0af in MBR" in “Bootloader” Tab
* Make sure “CloverEFI is checked”
* Choose a theme (BGM has a Razer vibe to it)
* Press “Install” button


Time to add macOS High Sierra to the drive (Downloaded from the app store)

In terminal
`sudo "/Applications/Install macOS High Sierra.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction`


Open the EFI partition from the flash drive - copy the files in USB DRIVE STUFF/USB EFI Partition/EFI/CLOVER/ to /EFI/CLOVER on the flash drive

## **BIOS Configuration**

* Advanced>CPU Configuration> VMX => Disabled
* Chipset>SATA/RST Config>SATA Mode => AHCI
* Security>Secure Boot Menu>Secure Boot => Disabled
* Boot>Fastboot => Disabled
* Boot> CSM> Launch CSM => Disabled


## **Install**

Boot to USB drive

Use disk utility to wipe the drive and format it as OS X Extended Journaled

Install

## **Post Install**

If trackpad doesn’t work - delete the kexts
	AppleIntelLpssI2C.kext and AppleIntelLpssI2CController.kext
From /System/Library/Extentions

In terminal
`diskutil list` find out where the EFI partition on the system is
`diskutil mountDisk /dev/disk#s#` to mount EFI from the system drive

Copy Laptop EFI setup/EFI/CLOVER/ to /EFI/CLOVER in the EFI partition just mounted

Copy files in Laptop System Drive/Library/Extentions/ to /Library/Extentions on the system drive


If stuff doesn’t work for you, give an attempt to fix it

Information/stuff: 

* Backlight - https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
* Battery - https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/
* Trackpad - https://voodooi2c.github.io/#Installation/Installation https://github.com/MacForceOne/VoodooI2C	
* This is the guide I followed to set mine up https://www.tonymacx86.com/threads/guide-razer-blade-2017.242627/ as well as a few other resources online

Disclaimer:
I have not tried to install macOS following these steps above, what I did was take the parts from. the installation and add my files from a working machine to the - in hope that it will work and boot smoothly - best of luck to ya!
