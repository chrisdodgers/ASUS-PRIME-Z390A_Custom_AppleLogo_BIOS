# Change your ASUS PRIME Z390-A BIOS Splash Screen from the ASUS logo to an Apple logo, or any logo of your choice.
[![Z390-A](https://img.shields.io/badge/ASUS_PRIME-Z390A-green)](https://www.asus.com/us/motherboards-components/motherboards/prime/prime-z390-a/)
[![BIOSVersion](https://img.shields.io/badge/BIOS-v2004-blue)](https://www.asus.com/us/motherboards-components/motherboards/prime/prime-z390-a/helpdesk_bios?model2Name=PRIME-Z390-A)

![AppleSplashScreen](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/a440020ba80634739f176f433f817318983f5615/Demo/SplashScreen.png)</br>

## About:
This is a guide on how to modify your BIOS so you can change your splash screen from the ASUS logo to an Apple logo, or any logo of your choice! This guide is intended, and only has been tested on an ASUS PRIME Z390-A motherboard running BIOS version 2004. 
> [!WARNING]
> **THIS PROCESS CAN BRICK YOUR SYSTEM, AND I TAKE NO RESPONSIBILITY IF YOU BRICK YOUR SYSTEM! ONLY CONTINUE WITH THIS GUIDE AT YOUR OWN RISK! I HAVE NOT TESTED NOR CONFIRMED IF THIS GUIDE WOULD WORK ON OTHER ASUS MOTHERBOARDS. THIS GUIDE WAS WRITTEN FOR SPECIFICALLY ASUS Z390-A MOTHERBOARDS. THIS GUIDE IS FOR EDUCATIONAL PURPOSES ONLY! IT IS ALSO 100% YOUR RISK IF YOU CHOOSE TO ATTEMPT THIS GUIDE ON OTHER MODEL ASUS MOTHERBOARDS.** You have been warned. Proceed very carefully if you chose to do so.
>


## Basics of this BIOS mod:
- We will be using UEFITool to find the Logo.bmp in our .CAP BIOS image.
- The raw data of where the existing Logo.bmp sits will be replaced with the raw data of our new Logo.bmp
- This will then be saved as a new .CAP BIOS image.
- Since flashing our custom .CAP image is not signed, it will result in an error when trying to flash using ASUS EZ Flash 3 Utility. So instead, we will export our custom .CAP image as a .rom image and flash using AMI Firmware Update Utility using a bootable FreeDOS USB drive.
- There is a known bug where sometimes BIOS flashing can erase the hardset MAC address on your built-in Intel NIC. This guide will also cover how to fix this.


## Pre-Requisites:
>[!WARNING]
>Take a backup of your current BIOS. You can boot into your BIOS and save your current configuration to a user profle. Save your user profile(s) to a USB drive as flashing the BIOS will erase your currrent BIOS configuration and profiles!
>

>[!WARNING]
>Write down your Intel Gigabit Ethernet NIC MAC address. This can be found in your network settings. You may need this later if the BIOS flash wipes your MAC address!

>[!WARNING]
>Make sure your PC is connected to a UPS (battery backup) as any BIOS flashing can brick your system if it loses power! Once again, only continue at your own risk and pay very close attention to details and information!
>



## Preparing Our .BMP Image:
- We will begin by creating a .bmp image as our logo. The image MUST meet ALL of the following requirements:
   - Resolution: 672x378
   - Format: Bitmap (.bmp)
   - Color Depth: 8 bit
   - Size: MAX SIZE 762 KB
- You can download the [pre-made Apple Splash Screen I edited](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/AppleLogoSplashscreen.bmp), or create your .bmp image own using your choice of image software.

>[!NOTE]   
>If the file size is larger than ~762 KB, it can cause you to have a black screen at during boot. Please ensure before you continue that your .bmp file meets the specifications above!


## Creating a Custom BIOS Image:
### [Download v2004 BIOS from ASUS's website for the PRIME Z390-A.](https://www.asus.com/us/motherboards-components/motherboards/prime/prime-z390-a/helpdesk_bios?model2Name=PRIME-Z390-A)
- Make sure to hit "Expand All" to see all available BIOS versions.

![BIOSDownload](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/BIOS-Download.png)</br>

>[!NOTE]
>I have not tested the latest v2101 version with this custom mod. I have only tested v2004. For this reason, I am going to suggest using v2004. You are at your own risk if you want to try this using the latest v2101 build.
>

- Download [UEFITool v0.28.0](https://github.com/LongSoft/UEFITool/releases?q=0.28&expanded=true). For this guide, I will be doing this in macOS. The software used in this guide also works in Windows and Linux. 
- Use UEFITool to open the BIOS image you downloaded. The file should be called `PRIME-Z390-A-ASUS-2004.CAP`
- Once opened, select `AMI Aptio capsule` and then press `⌘``F`. 
- A search window should pop up and click on the `GUID` tab. Paste in the following GUID: `7BB28B99-61BB-11D5-9A5D-0090273FC14D` and then press `Ok`.

![FindGUID](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/Find-GUID.png)</br>

- You should see in the "Messages" section of UEFITool a message saying `GUID pattern "7BB28B99-61BB-11D5-9A5D-0090273FC14D" found as....`. Double click on that message.
- UEFITool should have now highlighted `7BB28B99-61BB-11D5-9A5D-0090273FC14D` and you should now be able to click the drop downs to see whats inside of it. Eventually, you should see something that says `Raw section`. 

![ReplaceRawBody](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/Replace-Raw-Body.png)</br>

- To verify that this is indeed the raw data for the existing Logo.bmp, right click `Raw section` and select `Extract body...`. Save the file as `Logo.bmp`.
- Open the Logo.bmp you just saved. If this is indeed the correct `Raw section` we are working with, you should see the ASUS logo when you opened Logo.bmp.

>[!WARNING]
>If when you opened the file you saved and it is NOT the ASUS logo (or your current splash screen) then you need to STOP and go back to find the correct `Raw section`! It is very critical that you are ONLY working with the correct section!
>

- Now that you have verified you found the correct `Raw section`, right click `Raw section` and select `Replace body...`. This is going to replace the raw data of the Logo.bmp inside your BIOS image with the new raw data of the splash screen we want to use.
- Select the new .bmp that you created or downloaded from this guide.

>[!NOTE]
>Make sure to click `Show Options` so you can view `All files` instead of just `Raw files` as we want to select our new .bmp image we created.
> 

![SelectRawBody](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/Select-Raw-Body.png)</br>

- Now you should see a second `Raw section`. This is correct as you can see one says `Remove` as the action and the other shows `Replace` as the action.
- Press `⌘``S` to save our BIOS image as a `.CAP` file. Save it as `PZ390A.CAP`.
- Once saved, UEFITool will ask you if you want to load the new image. Select `Yes`.

![Save-BIOS-CAP](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/Save-BIOS-CAP.png)</br>

- Select and right click `APT Aptio capsule`.
- Select `Extract body...` and save the file as `bios.rom`. This will be the actual image that we will flash to our system.

![ExportBIOSRom](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/Export-BIOS-ROM.png)</br>

## Preparing a Bootable USB:

- Format your USB drive as FAT32.
- [Download UNetbootin](https://unetbootin.github.io) and select `FreeDOS` as the bootable distro to be installed on your formatted USB drive.
- Once you have a bootable FreeDOS USB made, open it so we can add a few files to it.
- Download the [BIOS Tools.zip](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/5c8a808a816514983b66d82b3ce3e25cc6c72601/Tools/BIOS%20Tools.zip) which contains the tools we need to flash our new BIOS image.
- Copy all the files inside of `"Copy Contents to USB"` to the root of your USB drive.
- Copy `bios.rom` which you previously made as well to the root of your USB drive.

## Preparing to Boot the USB:
- We need to be sure we can boot into FreeDOS. We need to ensure **CSM (Compatibility Support Module)** is `Enabled`, or else FreeDOS will not boot.
- Boot into your USB. Once booted you should be greeted with a prompt in FreeDOS.

>[!NOTE]
>If you find yourself still not able to boot your FreeDOS USB - Factory reset your BIOS to Default Settings. This will ensure no pieces of configuration is blocking legacy booting. 
>

## Flashing the Custom BIOS:


>[!WARNING]
>**THIS IS THE LAST CHANCE TO MAKE SURE YOU CREATED YOUR CUSTOM BIOS IMAGE CORRECTLY, USING THE CORRECT BIOS VERSION FOR YOUR MOTHERBOARD, ETC. THE NEXT COMMAND WILL WIPE YOUR EXISTING BIOS AND FLASH THE NEW BIOS IMAGE! MAKE SURE YOUR MACHINE DOES NOT LOSE POWER DURING THIS PROCESS!**
>

- Run the following command: `FLASH.BAT`
- Be patient as AMI Firmware Utility flashes your new BIOS image.

>[!WARNING]
>**DO NOT TURN OFF YOUR COMPUTER UNTIL THE UTILITY IS 100% COMPLETED FLASHING YOUR NEW BIOS! IF YOU TURN OFF YOUR MACHINE OR LOSE POWER FOR ANY REASON DURING THIS FLASH YOUR SYSTEM WILL BE A PAPERWEIGHT!**
>
![BIOSFlashing](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/BIOS-Flashing.png)</br>

## Testing the Custom BIOS:

- Once the utility is 100% done flashing, you can turn off your machine.
- Turn on your machine. After 1 or 2 cycles of your system attempting to POST, you should see your new splash screen!
- Once booted into your OS, check and make sure your MAC Address is still the same as it was when you wrote it down previously in this guide.
> [!NOTE]
> - If your MAC Address is the same as before. You can go ahead and load back your user profiles you backed up previously to restore your BIOS configuration. You do not need to follow the next steps in this guide!
> - If your MAC Address shows 88:88:88:87:88:88 - you should follow the next part of this guide to fix this.
> 

## Fix MAC Address after BIOS Flash:
This is a known issue in general with ASUS, Gigabyte, and other motherboard manufactures. Sometimes after a BIOS update/flash, the MAC address stored on your system is lost and not retained - hence why your MAC address is now 88:88:88:87:88:88. What we want to do is to flash our actual MAC address back on our NIC.

- Inside of the BIOS Tools folder you downloaded previously in this guide, open the file called `MAC.BAT`.
- Replace `PUTMACADDRESSHERE` with your actual MAC address and save the file. Example: `049226DEC1BA` (Do not include `:` and make sure it is in full caps like the example.) 

>[!NOTE]
> If your forgot to write down your real MAC address and don't feel like trying to remove the IO Cover to find it printed on your NIC. There is an easy way to generate a realisitc MAC address for your system. 
> 
> - Generate a MAC address using [this website](https://www.macvendorlookup.com/mac-address-generator). Set the prefix to `049226`.
> - Now you have a generated MAC address with an ASUS OUI that we can use.

- Boot your USB drive again. You should now be booted into FreeDOS.
- Run the following command `MAC.BAT`
- Once complete, you should now see your real MAC address instead of `88:88:88:88:87:88`.

![MACFlashing](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Photos/MAC-Flashing.png)</br>

- Once booted into your OS, you should now see your real MAC address (or the one you generated).
- You can go ahead and load back your user profiles you backed up previously to restore your BIOS configuration.

## Additional Photos:

![KubuntuBoot](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/blob/main/Demo/SplashScreen-Kubuntu.png)
