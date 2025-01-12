<img align="right" src="https://github.com/n00b69/woa-raphael/blob/main/raphael.png" width="350" alt="Windows 11 running on a Redmi K20 Pro">

# Running Windows on the Xiaomi Mi 9T Pro / Redmi K20 Pro

## Uninstallation

### Why is this needed?
If you want to uninstall Windows, this is used instead of deleting partitions manually to avoid human error + writing a whole dedicated guide to just uninstalling.

If you want to relock your bootloader you'll need your partition table to be stock.

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Modified TWRP](https://github.com/n00b69/woa-raphael/releases/download/Files/modded-twrp-raphael.img) (new method)

- [gpt_both0.bin](https://github.com/n00b69/woa-raphael/releases/download/Files/gpt_both0.bin) (old method)

## New method

### Switch to Android
> Or your device will not boot into Android after uninstalling Windows
- Run the **Switch to Android** or **Android** shortcut on your desktop, or flash a **boot.img** backup in fastboot/recovery.

### Boot into the modified TWRP
> While in fastboot mode, replace `path\to\modded-twrp-raphael.img` with the actual path of the image
```cmd
fastboot boot path\to\modded-twrp-raphael.img
```

#### Execute the restore script
```cmd
adb shell restore
```

##### Finished!

## Old method 
> In case the new one didn't work

### Switch to Android
> Or your device will not boot into Android after uninstalling Windows
- Run the **Switch to Android** or **Android** shortcut on your desktop, or flash a **boot.img** backup in fastboot/recovery.

### Restore GPT
> Replace ```path\to\gpt_both0.bin``` with the path to the gpt_both0.bin file.
```cmd
fastboot flash partition:0 path\to\gpt_both0.bin
```

#### Erase userdata
> To avoid a bootloop and restore FS size
```cmd
fastboot -w
```

##### Finished!









