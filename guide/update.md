<img align="right" src="https://github.com/n00b69/woa-raphael/blob/main/raphael.png" width="350" alt="Windows 11 running on a Redmi K20 Pro">

# Running Windows on the Xiaomi Mi 9T Pro / Redmi K20 Pro

## Updating drivers

### Prerequisites
- [Modified TWRP](https://github.com/n00b69/woa-raphael/releases/tag/Recovery) (should already be installed)

- [Drivers](https://github.com/n00b69/woa-raphael/releases/tag/Drivers)

- [UEFI image](https://github.com/n00b69/woa-raphael/releases/tag/UEFI)

### Boot into TWRP
> If your recovery has been replaced by the stock recovery, flash it again using
```cmd
fastboot flash recovery path\to\modded-twrp-raphael.img reboot recovery
```

#### Execute the msc script
> If it asks you to run it once again, do so
```cmd
adb shell msc
```

### Diskpart
```cmd
diskpart
```

#### Select the Windows volume of the phone
> Use `list volume` to find it, replace "$" with the actual number of **WINRAPHAEL**
```diskpart
select volume $
```

#### Assign the letter X
```diskpart
assign letter x
```

#### Exit diskpart
```diskpart
exit
```

### Installing drivers
> [!Note]
> This process will take +- 20 minutes. Do not worry, this is normal.

- Unpack the driver archive, then open the `OfflineUpdater.cmd` file (if an error shows up, run `OfflineUpdaterFix.cmd` instead)

> If it asks you to enter a letter, enter the drive letter of **WINRAPHAEL** (which should be **X**), then press enter

### Reboot your device
> Make sure to also change the UEFI image in Android, otherwise you may face a "blue screen of death" (BSoD) when booting Windows later.
```cmd
adb reboot
```

## Finished!













