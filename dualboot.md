## Dual boot between Android and Windows

<details>
<summary><strong>From Android to Windows</strong></summary>
 
<details>
<summary><strong>Using UEFI instead of recovery</strong></summary>
  
- We can use the UEFI image instead of recovery, for this we need to flash UEFI in the recovery section:
```fastboot
fastboot flash recovery uefi.img
```
- In order to restore the recovery, you need to download TWRP or OrangeFox and flash with the ```fastboot flash recovery recovery.img``` command
</details>

</details>
  
<details>
<summary><strong>From Windows to Android</strong></summary>
  
<details>
<summary><strong>Using Boot partition backup</strong></summary>

We can use a backup Boot partition to boot Android after using Windows.
  
To create a backup in TWRP, you need to press "Backup" in the main menu before the UEFi firmware, select the Boot section and the place where the backup will be saved, and swipe down to confirm the selection and start the backup.
  
- You don't need to make a backup every time before booting Windows.
  
After using Windows, in order to return to Android, you need to restore the Boot partition from a backup copy. To do this, in the main menu of TWRP, click on "Recovery", select the backup copy of the Boot partition and swipe at the bottom to confirm the selection. After recovery, you need to reboot your phone into the system and Android should boot.
</details>
