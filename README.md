#### 30 October 2019 10.15.1 Update:
- Clover updated to r5096
- Lilu and WhateverGreen updated to the latest

#### 13 October 2019 Update:
- Replaced AppleBacklightFixup.kext with WhateverGreen.kext for future-proof
- SIP set to Enable to avoid future disasters like what google updater did recently. In cases of Clover doesn't install and give an error "Disks need to be read and write", Disable SIP before the installation and set to Enable afterwards. 

#### 09 October 2019 Catalina 10.15 Update:
- bluetooth kext changed, from BrcmFirmwareData.kext and BrcmPatchRAM2.kext to BrcmBluetoothInjector.kext, as the first two were not able to drive the stock bluetooth module running on Catalina. Don't know why
- clover EFI was broken during the first reboot of Catalina installer. Another USB EFI stick was used to boot and finish the installation
- post-installation Clover r5070 was not able to be installed (not compatible to install on macOS drive) - installed to a USB drive then copy the drivers back to the EFI of the macOS drive
- the wifi dongle (Edimax EW-7611ULB) driver is dead in Catalina (not compatible). Use [this](https://github.com/chris1111/Wireless-USB-Adapter) instead

#### 29 July 2019 Update:
- clear clutters
- '-v' is no longer necessary for successful boot up
- kexts updated to the latest
- Clover Bootloader updated to r5018, driver folder structure changed
- macOS updated to 10.14.6
- if you prefer to use trackpoint instead of trackpad, please refer to [this comment](https://github.com/littlegtplr/Hackintosh-X230-macOS/issues/1)

#### 26 Sep 2018 Update:
- kexts and drivers were updated to latest
- The repo is compatible with Mojave (the machine is currently running on)

#### 4 Nov 2018 Update:
- Remove backlight control (PNLF) from DSDT.aml and update the repo
- Include SSDT-PNLF.aml, lilu.kext and AppleBacklightFixup.kext for optimised backlight control. Now the backlight control works properly. Credit goes to RehabMan. [post 1](https://www.tonymacx86.com/threads/solved-applebacklightinjector-isnt-working-on-x230.257601), and [post 2](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightfixup-kext.218222/)

# Hackintosh-X230-macOS

https://imgur.com/a/pKfFYet

Relevant specs of my X230: 
- CPU: i5-3230m
- SSD: Plextor PX-256M2M (APFS is used without turning off trim, no speed decreases noticed)
- Bluetooth: Broadcom BCM20702A0 (come with my x230 but need extra kext to work properly. In my case, BrcmPatchRAM2.kext and BrcmFirmwareData.kext from RehabMan/OS-X-BrcmPatchRAM are necessary. https://bitbucket.org/RehabMan/os-x-brcmpatchram info credit goes to /u/stormy90 on reddit https://www.reddit.com/r/hackintosh/comments/3t5bly/bluetooth_not_working_on_el_capitan_broadcom/ )
- WiFi dongle: Edimax EW-7611ULB (its bluetooth is invisible to macOS, or choose EW-7811Un for a WiFi only solution)

The files used for this hackintosh are mainly from xxx10101xxx on github - https://github.com/xxx10101xxx/ThinkPad-X230-macOS

The installation of macOS to x230 can be done with the typical vanilla approach:
1. Prepare a macOS (in this case, it's High Sierra) installation USB stick - https://hackintosher.com/guides/make-macos-flash-drive-installer/
2. Install Clover Bootloader to the USB stick, make sure you choose the USB stick as the destination and tick install to UEFI only - https://medium.com/@salbito/installing-clover-on-a-macos-sierra-installer-130705b91bfa
3. Copy and paste the files of this repository to EFI/Clover/ , to mount the EFI partition, use either Clover Configurator or command line with diskutil - http://osxdaily.com/2013/05/13/mount-unmount-drives-from-the-command-line-in-mac-os-x/
4. Generate a serial number with Clover Configurator, go to SMBIOS (on the left sidebar), hit 'Generate New' next to 'Serial Number' for whatever times you like. 
5. Generate a uuid, go to terminal, type 'uuidgen', hit 'enter', copy paste the results to the 'smUUID' on Clover Configurator. 
6. Copy 'Board Serial Number' to 'Rt Variables' - 'MLB', save the plist and quit Clover Configurator. 
7. Set BIOS as indicated in xxx10101xxx's repository. 
8. Boot into the USB stick, install macOS - https://hackintosher.com/guides/macos-high-sierra-hackintosh-install-clover-walkthrough/
9. After the installation, install Clover Bootloader to the SSD/HDD of the macOS just installed, as what was did to the USB stick. 
10. Copy and Paste the EFI folder from the USB stick to the EFI paritition of the installed SSD/HDD. Now the macOS should be able to boot up without the USB stick. If not, don't panic, insert the USB stick and boot into the system installed see if there's anything wrong with the EFI partition and try again. 
11. Generate a SSDT for Power Management, follow the procedure suggested by Piker-Alpha - https://github.com/Piker-Alpha/ssdtPRGen.sh
12. Install WiFi dongle driver (in my case it's Edimax EW-7611ULB or EW-7811Un) - https://www.edimax.com/edimax/download/download/data/edimax/global/download/for_home/wireless_adapters/wireless_adapters_n150/ew-7611ulb
13. Done! 

Known issues:
1. d-sub output isn't working - not a news
2. trackpoint isn't working - both red dot and buttons, at chances they might work but not stable. Trackpad is working fine but nothing better than on windows (x230 users might have known what I'm talking about). For possible solutoin of getting trackpoint to work, see [this post](https://www.reddit.com/r/hackintosh/comments/92wbb7/lenovo_x230_high_sierra/?utm_content=full_comments&utm_medium=message&utm_source=reddit&utm_name=frontpage)
3. boot/awake animation glitch - minor stuff, but I wasn't able to fix it with the solution suggested by Bizzaro https://github.com/Bizzaro/x230-osx . If anyone knows how to fix it please share it with me. Thanks. 
4. ~~Backlight brightness is set to maximum at boot - In my case this is due to Clover Bootloader overwrite NVRAM's value at boot. It uses 'Backlight Level: 0xFFFF' at boot, this can be check by going to 'Options' - 'Graphics Injector' in Clover Bootloader in the middle of booting. The brightness stays at the previous value before rebooting if '0xFFFF' is deleted, but I seem can't find a way to set empty as default in Clover. If anyone knows how to do it, please share it with me. Thanks. Info credits goes to RehabMan https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/~~ 
~~1 Aug 2018 Update: the brightness level is preserved after rebooting by deleting EmuVariableUefi-64.efi from drivers64UEFI. But the minimum brightness is still brighter than it used to be on windows. Following procedures suggested in the post above did not fix the issue. If anyone knows how to do it, please feel free to share it. Thanks.~~

Hope the info can be helpful for anyone who'd like to install macOS on x230. 
