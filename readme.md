<img align="right" src="https://raw.githubusercontent.com/graphiks/woa-raphael/65c0ee06045c13d1ef0f5f88aa687c50274ef7f5/raphael.png" width="350" alt="Xiaomi Mi 9T Pro">

# Running Windows on Redmi K20 Pro / Mi 9T Pro (raphael/raphaelin)

## WARNING

DO NOT USE THIS GUIDE ON A REDMI K20 / MI 9T!! YOU RUN THE RISK OF BRICKING YOUR DEVICE! Altough they might seem the same, they have completely different SoC's and codenames!

I am not responsible in any way for bricking your phone, YOU are responsible and YOU chose to make these modifications to your phone. Please read the guide carefully, do not just copy and paste the commands, and do not proceed if you do not understand something, or get an error, in this case try asking for help in the Project Renegade [discord server](https://discord.com/invite/XXBWfag) and [telegram](https://t.me/joinchat/MNjTmBqHIokjweeN0SpoyA). (or you can ask me on [telegram](https://t.me/graphiks23) at my discord at graphik23, or at [Windows on Raphael](https://t.me/woaraphael) telegram)

Credits to sunflower2333 for the mass storage mode script and the command for resizing the partition limit. Huge thanks to them.

Here is a support table to get you started:
| Feature                | Notes                                               | Status         |
|------------------------|-----------------------------------------------------|----------------|
| Bluetooth              |                                                     | ✅            |
| Wifi                   |                                                     | ✅            |
| UFS                    |                                                     | ✅            |
| Touch                  |                                                     | ✅            |
| GPU                    | May not work on some devices with unofficial panel  | ⚠️            |
| Audio                  | Read step 4.5 to fix the dim screen issue           | ✅            |
| Battery                | i dont know                                         | ⚠️            |
| Buttons                |                                                     | ✅            |
| Light Sensor           |                                                     | ❌            |
| Thermal Sensor         |                                                     | ❌            |
| Haptic                 |                                                     | ❌            |
| Cellular Data          |                                                     | ❌            |
| Charge                 | Charges slowly if plugged in                        | ❌            |


# 1. Pre-installation
## 1.1 Prerequisites
1. A Mi 9T Pro running latest MIUI firmware
2. A computer running Windows
3. An USB cable to connect your phone to your computer
4. A desire to learn (important)
5. an actual brain

## 1.1 Downloading files
### **Please avoid all stripped down versions of Windows 10 and 11** (ex. Tiny11). These builds of Windows usually break vital components or software. Raphael is powerful enough for vanilla Windows. Or you can proceed with caution, but don't ask for help with  broken stuff.

### 1.1.1 Downloading latest Windows arm64 ISO
[UUP Dump](http://uupdump.net/) is usuallly recommended for grabbing the latest arm64 builds.
You can use **"Latest Dev Channel build"** for the latest features in Windows. For more stability, choose **"Latest Release Preview build"**.

### 1.1.2 Downloading drivers, UEFI image and other tools
[DriverUpdater (for installing drivers)](https://github.com/WOA-Project/DriverUpdater/releases) |
[Raphael UEFI image](https://github.com/graphiks/woa-raphael/releases/download/raphael/xiaomi-raphael.img) |
[Platform-tools (adb and fastboot)](https://developer.android.com/tools/releases/platform-tools) |
[SM8150 Drivers](https://github.com/woa-msmnile/msmnile-Drivers/archive/refs/heads/main.zip) and [raphael drivers](https://github.com/woa-msmnile/Raphael/archive/d4fbba0c8c09c5915e672f53a64ec26f8271ed50.zip)

Before continuing, we must place the raphael drivers in the correct path. Unzip both the SM8150 and raphael drivers. Go in the raphael drivers folder, copy all of the three folders, go to msmnile-drivers-main (sm8150 drivers folder), from here navigate to: **"components/QC8150/Device/Raphael"** paste the raphael drivers here. You may now continue with the guide.

### Make sure you also have installed fastboot drivers

## 1.2 Partition the UFS
### Warning! this will erase **ALL** of your Android data!

To resize userdata partition, you need a third-party recovery, which allows you to use ADB. You will also need [parted](https://github.com/graphiks/woa-raphael/raw/main/parted) to modify the partitions.
After downloading parted, open a terminal (in the platform-tools folder). Then reboot your device into fastboot mode. In the terminal type:
```
fastboot boot (recovery-image-name).img
```
### 1.2.1 Partitioning using parted
You should now boot into recovery.
Your Redmi K20 Pro / Mi 9T Pro may have different storage sizes. Check free space using "(parted) free" and set the end value accordingly. (In guide, example is set for 128gb models)
Before partitioning, we must remove the 32 partition number limit. Raphael has 31 partitions by default, which is not enough for installing Windows or dualbooting with Android.
~~~
adb shell
sgdisk --resize-table=128 /dev/block/sda
~~~
```
adb push parted /cache/
adb shell "chmod 755 /cache/parted"
adb shell
```
Run these commands seperately.

After entering ADB shell:
```
cd /cache
./parted /dev/block/sda
```
Again, run thes commands seperately.

Now, in parted print the current table partition:

```
(parted) print
```

Parted will print the list of partitions, userdata should be the last partition in the list.
Now we will resize the userdata partition. You can choose the size you want in this example we resize it to 30GB.
```
(parted) resizepart (userdata partition number)
End? [122GB]? 32GB
```
We will now create both ESP and Windows partitions.
Note: 32GB is the **End** of the **userdata** partition and 32.5GB is the end of the partition we will be creating, so it will be 500MB in size. Also replace 32GB to the end of userdata accordingly. Check free space by typing: "(parted) free"
```
(parted) mkpart esp fat32 32GB 32.5GB
# Here, 32GB is the end of userdata partition

(parted) set $ esp on
# Replace "$" with your ESP partition number, usually 30, or 31

(parted) mkpart win ntfs 32.5GB 122GB
# Here, 122GB is the end value of your phone's total storage. Replace with the end value you see when executing "p free"
# 32.5GB is the end of esp
(parted) quit
# exit the tool
```
Reboot to android to check if everything works correctly.

# 1.3 Backing up essential partitions
### Omitting to backup the modem partition will probably fatally break cellular on your phone! You can only fix it later by a modem backup!

To backup essential partitions, press the button "Backup" on the main menu (for TWRP); or press the second tab at the bottom (for OrangeFox). Then select Modem and swipe to backup. Then copy the backup either using ADB or MTP. (They should be at /sdcard/TWRP/ or /sdcard/Fox/) Save them somewhere safe.

# 2. Installation

## 2.1 Formatting partitions
### 2.1.1 Enabling Mass Storage Mode
Download [this Mass Storage Mode script](https://cdn.discordapp.com/attachments/1148093151744118816/1158416286703943840/surfaceduo1-msc.tar?ex=6548fdbd&is=653688bd&hm=e7657c8eda4bf6d33a14e380f4e4061dfffe1ae46117a8575680324ac7ba93e8&), then move it to the platform-tools folder. This script does not work on OrangeFox, you can temporarily boot into TWRP using fastboot. After TWRP is booted, open a terminal in the platform-tools folder and then execute the following commands:
```
adb push msc.sh /
adb shell
sh msc.sh
```
### 2.1.2 Formatting in diskpart
## WARNING!!!! DO NOT ERASE ANY PARTITION WHILE IN DISKPART!!!! THIS WILL ERASE ALL OF YOUR UFS!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for taking the device to xiaomi)
Your should hear a device connected sound on your PC. Then open diskpart, and type the following:
```
DISKPART> lis dis
# The above command will list out all of the disks on your computer. Find the appropiate sized one which should be your phone

DISKPART> sel dis $
# Replace "$" with the appropiate disk number

DISKPART>lis par
# This will print out all of the partitions in the selected disk. Check if they match up with your device.

DISKPART>sel par $
# Replace the "$" with the ESP partition, usually 30 or 31

DISKPART> format quick fs=fat32 label="System"
# format the ESP partition as fat32

DISKPART> assign letter="Z"
# Assign a drive letter to the ESP partition

DISKPART> sel par $
# Select the Windows Partition, usually 31 or 32

DISKPART> format quick fs=ntfs label=Windows
# Format Windows partition as NTFS

DISKPART> assign letter="X"
# Assign a drive letter to the Windows partition

DISKPART> exit
```
## 2.2 Windows image deployment

Before deploying a Windows image, we need a **install.wim** file, to do this:
> Mount the Windows ARM64 iso image you have downloaded on UUPDUMP
> Open the **sources** folder
> Find install.wim there
> Copy this file to your PC and mark the location


### 2.2.1 Applying Windows image to your device
After the previous command is done, in the same Powershell command prompt execute:
```
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
# <path/to/install.wim> should be the location to your install.wim
# X: is the windows disk letter, change it if you assigned a differrent letter
```
Wait until it finishes. After that, in the same command prompt execute:
### 2.2.3 Creating boot files
```
bcdboot X:\Windows /s Z: /f UEFI
# X: is the windows partition
# Z: is the esp partition
```
The above command will create a BCD Configuration in the EFI partition for Windows to boot.

### 2.2.2 Tweaking boot configuration

After the last command is finished we need to enable test signing and disable Automatic Repair, to do so, execute these commands:
```
cd Z:\EFI\Microsoft\Boot
# Z: is the ESP partition mounted
bcdedit /store BCD /set "{default}" testsigning on
bcdedit /store BCD /set "{default}" nointegritychecks on
bcdedit /store BCD /set "{default}" recoveryenabled no
```
### 2.2.3 Adding drivers
Open the driverupdater zip file you downloaded back in step 1.1.2, extract both files in the root directory of msmnile-drivers-main folder (sm8150 drivers folder). Open an admin terminal in the root directory of msmnile-drivers-main and execute the following command:
```
DriverUpdater.exe -p X: -d definitions\Desktop\ARM64\Internal\raphael.txt -r .
# X: is the drive letter of your phone's Windows partition
```

After Windows and the drivers are hopefully installed correctly, we will now reboot into fastboot mode. Place the raphael UEFI image in the platform-tools folder you downloaded (make sure platform tools is extracted). Then, open a terminal in the platform-tools directory and type this:
```
fastboot boot xiaomi-raphael.img
```
If fastboot hangs on "< waiting for device >", make sure your device is properly plugged in, otherwise install fastboot drivers.
You should now see a Windows 11 logo with a throbber.
To permanently flash the image execute the following command:
```
fastboot flash boot xiaomi-raphael.img
```

# 3. Post-installation

## 3.1 Bypassing the "Let's connect you to a network" or "Let's add your Microsoft account"

To bypass this, tap on the accessibility button in the bottom right, tap on "On-screen keyboard". An virtual keyboard should open. From there, tap on shift, and then F10. You should now see a command prompt open. In the command prompt type the following:
```
oobe\bypassnro
```
If that did not work try this:
```
cd oobe
bypassnro.cmd
```
Your device should now restart. At the same screen you should now see an option that says **I don't have internet**. Continue with setup as normal.

# 4. Troubleshooting

## 4.1 Restoring the WiFi drivers

The wifi drivers are disabled due to some instabilities, altough they aren't that frequent and does not bother me.
To reenable them, (assuming you have reached the Windows desktop) copy over the driver files to your windows partition (by using mass storage mode or some other way), then in the Windows desktop, go to the driver folder, then inside of it navigate to the following directory: **components/QC8150/Device/Raphael/DEVICE.SOC_QC8150.RAPHAEL/Extensions/Subsystems/**. From within that folder find a file called "raphael_subextscss.inf_". Rename that file to remove the "_" at the end. Open Device Manager. Click on the "Add drivers" button at the top (should be the 6th icon). Click on "Browse". Navigate to the previous folder and select the .inf file you have just renamed. Windows will install the wifi driver, then reboot. Wifi should now be working.

## 4.2 DISM Error:87 The add-driver option is unkown

This usually means that you have an unclean Windows image with some other drivers. You need to get an clean Windows image.

## 4.2 0xc000021a BSOD

This usually means that winlogon.exe has failed, and you may need to reapply Windows image.

## 4.3 The computer restarted unexpectedly or encountered an unexpected error

If you stumble upon this error, you may need to redeploy the Windows image. This error usually occurs when you get a BSOD while Windows is setting up. Altough this BSOD is very uncommon since the WiFi drivers are disabled by default, which may crash 

## 4.4 INACCESSIBLE_BOOT_DEVICE BSOD 
This Blue Screen of Death likely means some broken driver installation. To fix this, redo step 2.1 (but dont execute any format commands or you will have to redeploy Windows again), after that, repeat step x.x (replace drivers using driverupdater)

## 4.5 My screen is dimmer than before

A weird workaround for this... is to just press the power button to put the phone to sleep, and again to wake it. Just works for some reason

half assed guide lol
