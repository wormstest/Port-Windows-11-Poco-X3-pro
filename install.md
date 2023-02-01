# Install Windows on POCO X3 Pro

## Create partitions for Windows

### Notes:
> **Warning** If you delete any partitions via diskpart later on or now windows will send a UFS command that gets misinterpreted which erase all your UFS.
- These commands have been tested.
- All your data in Android will be erased! Backup now if needed.
- Ignore `udevadm` warnings.
- Do not run the same command twice.
- DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/winonvayu)

### Need files
- [Windows 10/11 ARM image(Windows 11 is recommended)](https://uupdump.net/)
- [platform-tools(ADB & Fastboot)](https://developer.android.com/studio/releases/platform-tools)
- [DriverUpdater for install and update drivers](https://github.com/WOA-Project/DriverUpdater/releases/)
- [UEFI image](https://github.com/degdag/edk2-msm/releases/tag/V2.1.0)
- [Modified TWRP or OrangeFox](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases/tag/Recoveries)

### Boot TWRP or OrangeFox recovery through the PC with the command
```cmd
fastboot boot recovery.img
```
### Unmount all partitions
Go to TWRP "Mount" menu and unmount all partitions.

### Start the ADB shell
```cmd
adb shell
```

⚠️ Do not run all commands at once, execute them in order!

⚠️ DO NOT MAKE ANY MISTAKE!!! YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

### Resize the partition table
> So that the Windows partitions would fit
```sh
sgdisk --resize-table 64 /dev/block/sda
```

### Start parted
```sh
parted /dev/block/sda
```

### Delete the `userdata` partition
> You can make sure that 32 is the userdata partition number by running
>  `print all`
```sh
rm 32
```
### Create `win, esp and userdata` partitions
> If you get any warning message telling you to ignore or cancel, just type i and press enter.
<details> 
<summary><strong>For 6/128 models</strong></summary>

- Create the ESP partition (stores Windows bootloader data and EFI files)
```sh
mkpart esp fat32 11.8GB 12.2GB
```
- Create the main partition where Windows will be installed to
```sh
mkpart win ntfs 12.2GB 70.2GB
```
- Create Android's data partition
```sh
mkpart userdata ext4 70.2GB 127GB
```
</details>
<details>
<summary><strong>For 8/256 models</strong></summary>
  
- Create the ESP partition (stores Windows bootloader data and EFI files)
```sh
mkpart esp fat32 11.8GB 12.2GB
```
- Create the main partition where Windows will be installed to
```sh
mkpart win ntfs 12.2GB 132.2GB
```
- Create Android's data partition
```sh
mkpart userdata ext4 132.2GB 255GB
```
</details>

### Make ESP partiton bootable so the EFI image can detect it
```sh
set 32 esp on
```

### Quit from parted
```sh
quit
```

- Reboot to TWRP

### Start the adb shell again
```cmd
adb shell
```

### Format partitions for future use
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPVAYU
mkfs.ntfs -f /dev/block/by-name/win -L WINVAYU
```

- Format data for use Android: go to "Wipe" menu and press Format Data, then type `yes` and click "✓".

### Check if Android still starts
Restart the phone, and see if Android still works.

## Install Windows
> You need to have MTP disabled in "Mount" menu
### Execute the msc script
```cmd
adb shell msc.sh
```

### Assign letters to disks
> Once the X3 Pro is detected as a disk launch diskpart
```diskpart
Use list volume to find it, it's the ones named "WINVAYU" and "ESPVAYU"
# select volume <win-partition-number>
# assign letter=x
# select volume <esp-partition-number>
# assign letter=y
# exit
```

### Deploying a Windows image
> Replace `<path/to/install.wim>` with the actual install.wim path, located in sources folder inside your ISO
> You can get it either by mounting or extracting it
```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

### Create Windows bootloader files for the EFI
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

### Allow unsigned drivers
> If don't do this you'll get a BSOD
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set {default} testsigning on
```

### Install drivers
#### Check what type of panel you have
> Go to platform-tools folder
```cmd
adb shell cat /proc/cmdline
```
> Look for `msm_drm.dsi_display0` almost at the bottom
> If your device is `Tianma` `msm_drm.dsi_display0` will be `dsi_j20s_36_02_0a_video_display`
> If your device is `Huaxing` `msm_drm.dsi_display0` will be `dsi_j20s_42_02_0b_video_display`, if it is, go to the drivers folder `Vayu-Drivers/components/QC8150/Device/DEVICE.SOC_QC8150.VAYU/Drivers/Touch/` and delete j20s_novatek_ts_fw01.bin, finally rename j20s_novatek_ts_fw02.bin to j20s_novatek_ts_fw01.bin

> Replace `<vayudriversfolder>` with the location of the drivers folder
```cmd
driverupdater.exe -d <vayudriversfolder>\definitions\Desktop\ARM64\Internal\vayu.txt -r <vayudriversfolder> -p X:
```

### Boot into Windows

<details> 
<summary><strong>Dualboot between Android and Windows</strong></summary>

- [You should follow this guide](/dualboot.md)
</details>

<details> 
<summary><strong>Boot Windows manually every time</strong></summary>
 
Reboot the phone into fastboot then boot into UEFI:
```fastboot
fastboot boot <uefi.img>
```
Android will boot on reboot, you need to boot into UEFI again to boot into Windows.
</details>  
  
- If you have a BSOD with error code BOUND_IMAGE_UNSUPPORTED when you first boot Windows, you need to use [old UEFI](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases)(touchscreen not working in the old version of UEFI)



