<img align="right" src="https://raw.githubusercontent.com/graphiks/woa-raphael/main/raphael.png" width="350" alt="Xiaomi Mi 9T Pro">

# Running Windows on Redmi K20 Pro / Mi 9T Pro (raphael/raphaelin)

## WARNING

I am not responsible in any way for bricking your phone, YOU are responsible and YOU chose to make these modifications to your phone. Please read the guide carefully, do not just copy and paste the commands, and do not proceed if you do not understand something, or get an error, in this case try asking for help in the Project Renegade [discord server](https://discord.com/invite/XXBWfag) and [telegram](https://t.me/joinchat/MNjTmBqHIokjweeN0SpoyA).

Here is a support table to get you started:
| Feature                | Notes                                               | Status         |
|------------------------|-----------------------------------------------------|----------------|
| Bluetooth              |                                                     | ✅            |
| Wifi                   | Disabled  due to instabilities (see step X.X)       | ⚠️            |
| UFS                    |                                                     | ✅            |
| Touch                  |                                                     | ❌            |
| GPU                    | May not work on some devices with unofficial panel. | ✅            |
| Battery                |                                                     | ✅            |
| Buttons                |                                                     | ✅            |
| Light Sensor           |                                                     | ❌            |
| Thermal Sensor         |                                                     | ❌            |
| Haptic                 |                                                     | ❌            |
| Cellular Data          |                                                     | ❌            |
| Charge                 |                                                     | ❌            |


# 1. Pre-installation
## 1.1 Prerequisites
1. A Mi 9T Pro running latest MIUI firmware
2. A computer running Windows
3. An USB cable to connect your phone to your computer
4. A desire to learn (important)

## 1.1 Downloading files
### **Please avoid all stripped down versions of Windows 10 and 11** (ex. Tiny11). These builds of Windows usually break vital components or software. Raphael is powerful enough for vanilla Windows. Or you can proceed with caution, but don't ask for help with  broken stuff.

### 1.1.1 Downloading latest Windows arm64 ISO
[UUP Dump](http://uupdump.net/) is usuallly recommended for grabbing the latest arm64 builds.
You can use **"Latest Dev Channel build"** for the latest features in Windows. For more stability, choose **"Latest Release Preview build"**.

### 1.1.2 Downloading drivers, UEFI image and other tools
[Main driver pack](https://github.com/woa-msmnile/msmnile-Drivers) |
[Raphael specific drivers](https://github.com/woa-msmnile/Raphael/tree/d4fbba0c8c09c5915e672f53a64ec26f8271ed50) |
[Raphael UEFI image](https://github.com/graphiks/woa-raphael/releases/download/raphael/xiaomi-raphael.img) |
[DriverUpdater (for installing drivers)](https://github.com/WOA-Project/DriverUpdater/releases) |
[Platform-tools (adb and fastboot)](https://developer.android.com/tools/releases/platform-tools) |

### Make sure you also have installed fastboot drivers.

Extract both driver packs, and place the 3 folders inside the Raphael drivers folder into the main drivers folder, in the following path: **"\components\QC8150\Device\Raphael\"**

## 1.2 Partition the UFS
### Warning! this will erase **ALL** of your Android data!

To resize userdata partition, you need a third-party recovery, which allows you to use ADB. You will also need [parted].



# This guide is currently a WIP


