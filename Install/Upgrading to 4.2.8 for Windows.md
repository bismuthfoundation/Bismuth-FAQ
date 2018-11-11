# Installing Bismuth_installer_nuitka.exe

If you installed a fresh copy of Bismuth_installer_nuitka.exe version 4.2.7 just install the latest version over the top of your existing installation after backing up your "wallet.der" file first

You should follow these instructions if: 

You have previously used Bismuth_installer.exe (4.2.7 and below) and are upgrading to the new Bismuth_installer_nuitka.exe (4.2.8.1) 

OR

You have installed Bismuth_installer_nuitka.exe over a previous Bismuth_installer.exe installation. 

## Instructions

### Pre-Uninstall

1. Backup your "wallet.der" file
2. Rename your "wallet.der" file to "wallet.der.old"

Hint: "wallet.der" can be found in the folder "C:\Program Files (x86)\Bismuth"

### Uninstall

CAUTION: DO NOT GO ANY FURTHER IF YOU DO NOT HAVE A COPY OF YOUR "wallet.der" FILE SAVED SOMEWHERE ELSE

1. Remove Bismuth from your computer using add / remove programs of running "unins000.exe" in the folder "C:\Program Files (x86)\Bismuth"
2. Rename the folder "C:\Program Files (x86)\Bismuth" to "C:\Program Files (x86)\Bismuth.old"

Hint: The old folders will be cleaned up later

### Install

1. Download the latest version 4.2.8.1 [Download](https://github.com/hclivess/Bismuth/releases/download/4.2.8.1/Bismuth_installer_nuitka.exe)
2. Install Bismuth by running "Bismuth_installer_nuitka.exe

### Replacing wallet.der and static folder

1. Copy your backed up "wallet.der" file into the folder "C:\Program Files (x86)\Bismuth\files"
2. Go to the folder "C:\Program Files (x86)\Bismuth.old" and right click on the folder "static" and click "copy" in the menu
3. Go to the folder "C:\Program Files (x86)\Bismuth\files", right click and click "paste" in the menu
4. Click "Yes" if you are asked to merge or overwrite existing files

### Run the progams

Run your node and / or wallet as normal and check that all is working as before.

### Clean Up

CAUTION: DO NOT GO ANY FURTHER IF YOUR NODE OR WALLET IS NOT WORKING OR IF YOU DO NOT HAVE A BACKUP OF YOUR "wallet.der" FILE

1. Go to the folder "C:\Program Files (x86)\"
2. Deleted the folder "Bismuth.old"

Hint: If you do have any issues or problems, please do not hesitate to contact us via our support channel in discord
