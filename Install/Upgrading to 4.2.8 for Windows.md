# Installing Bismuth_installer_nuitka.exe

If you have previously installed a fresh copy of Bismuth_installer_nuitka.exe (4.2.7) this need not apply to you but if you have previously used Bismuth_installer.exe (4.2.7 and below) and are upgrading to the new Bismuth_installer_nuitka.exe (4.2.8) or you have installed Bismuth_installer_nuitka.exe over previous Bismuth_installer.exe then there is a couple of steps for you to do before you install Bismuth_installer_nuitka.exe (4.2.8).

## Pre-Uninstall 
- before you download Bismuth_installer_nuitka.exe (4.2.8) make sure you have a backup of your wallet.der (C:\Program Files (x86)\Bismuth) and removed it from the Bismuth folder, at this point you could always remove your static folder to save you having to bootstrap again and save yourself sometime after installing the new version. 

## Uninstall 
before doing this please make sure you have your wallet.der backed up in a different location and or removed your wallet.der from the bismuth folder. Optional is to remove your static folder as well to save you time from bootstrapping. 
- In bismuth folder (C:\Program Files (x86)\Bismuth) run the unins000.exe and it will remove Bismuth from your computer but will leave the Bismuth folder behind. 
- Once uninstall has finished come out of the bismuth folder (C:\Program Files (x86)) and right click on Bismuth folder and "Delete" to completely remove the folder.

## Install
- download the latest version 4.2.8 
https://github.com/hclivess/Bismuth/releases/download/4.2.8/Bismuth_installer_nuitka.exe 
- install like you normally would. 

## Replacing wallet.der and static folder
when you look in the Bismuth folder now you will see there is a difference from the old Bismuth_installer.exe but don’t worry everything has only moved one folder forward into "files". 
- wallet.der- C:\Program Files (x86)\Bismuth\files
- static folder- C:\Program Files (x86)\Bismuth\files

once you have replaced them in to "files" folder you can run node.exe and or wallet.exe as normal. 
If you do have any issues or problems, please do not hesitate to contact us via our support channel in discord
