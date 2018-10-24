# Razer-Blade-Stealth-Hackintosh-Guide (Mojave changes)
Hackintosh for Kaby Lake RBS Guide

* Originally installed on 10.14.0
* Tested with a Razer Blade Stealth, i7 8550U / 16GB RAM

## **PLEASE READ THROUGH THE ENTIRE GUIDE FIRST**

This should be a relatively simple guide for setting up a hackintosh on a Razer Blade Stealth (Tested with a Razer Blade Stealth, i7 8550U / 16GB RAM) 

**Once everything is set up, you should generate a new serial number and stuff that goes along with it.**

### Geekbench

This laptop setup appears to be inline with a mid-2017 15" Macbook Pro with the i7 7920HQ:

![Geekbench](https://www.dropbox.com/s/v2hhia23azd11wu/Screen%20Shot%202018-10-25%20at%2010.22.17%20am.png?raw=1)


**Things to take note**

* Wifi will not work natively, it will require a USB dongle or a new card to be installed. I bought a `Broadcom BCM94352Z DW1560 802.11ac Wifi Bluetooth 4.0 M.2` from eBay, opened up the laptop and replaced the existing card with the Broadcom one.  After that change, both wifi and Bluetooth work fine, for a cost of about $30.

* The laptop will only boot reliably in verbose mode (`-v` in the boot flags).  I can't quite figure out why, and I'm sure it's something relatively easy to fix, but I haven't really been bothered by it.

* I haven't gotten Thunderbolt 3 to work.  I don't have a TB3 device to test with, but it does work as a USB3.1 port.  I'm just not sure if hotswap on the USB3.1 port is working.  I tested an ethernet dongle and it worked fine, but TB3-->HDMI didn't seem to work.

* I had a "crackle" on my headphone jack and volume was low.  Downloading the fix here and running the `install.command` file fixed that issue:  https://github.com/daliansky/XiaoMi-Pro/tree/master/ALCPlugFix

* Multi-touch works alright on the touchpad.  Not as good as a real Mac touchpad, but close enough.  A few caveats:
  * I haven't figured out how to get two-finger *click* for right mouse press to work.  A two-finger *tap* works fine.
  * Five-finger gestures will cause a kernel crash.  

**Things that should work fine**
	
* Intel HD 620 Graphics
* Audio, including port switching
* Backlight
* Trackpad (including multi-touch gestures)
* Battery status
* HDMI out, including audio switching
* Sleep

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


Time to add macOS Mojave to the drive (Downloaded from the app store)

In terminal
`sudo "/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction`


Open the EFI partition from the flash drive - copy the files in USB DRIVE STUFF/USB EFI Partition/EFI/CLOVER/ to /EFI/CLOVER on the flash drive

## **BIOS Configuration**

* Advanced>CPU Configuration> VMX => Disabled
* Chipset>SATA/RST Config>SATA Mode => AHCI
* Security>Secure Boot Menu>Secure Boot => Disabled
* Boot>Fastboot => Disabled
* Boot> CSM> Launch CSM => Disabled


## **Install**

Boot to USB drive

Use disk utility to wipe the drive and format it as APFS

Install

## **Post Install**

If trackpad doesn’t work, plug in a USB mouse temporarily.

In terminal
`diskutil list` find out where the EFI partition on the system is
`diskutil mountDisk /dev/disk#s#` to mount EFI from the system drive

Copy `Laptop EFI setup/EFI/*` to `/EFI/*` in the EFI partition just mounted

I know that the kexts should likely be placed in `/System/Library/Extensions`, but the above setup works fine for me.

### Audio fix

I had trouble with the headphone jack not working.  It would switch over appropriately when headphones were plugged in, but audio was distorted.  I downloaded this fix, ran the `install.command` file, and that appeared to fix the issue:  https://github.com/daliansky/XiaoMi-Pro/tree/master/ALCPlugFix.  Sometimes after reboot, if the headphones are already plugged in, I have to unplug / replug to get the crackle to go away.

Information/stuff: 

* Backlight - https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
* Battery - https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/
* Trackpad - https://voodooi2c.github.io/#Installation/Installation https://github.com/MacForceOne/VoodooI2C	
* This is the guide I followed to set mine up https://www.tonymacx86.com/threads/guide-razer-blade-2017.242627/ as well as a few other resources online
