## Troubleshooting Issues

<details> 
<summary><strong>Device can boot into Android but not bootloader</strong></summary>

> Need files: [platform-tools](https://developer.android.com/studio/releases/platform-tools)

  This is caused by partitions with volume names the bootloader cannot handle, to fix this:

1. Boot to recovery
2. Connect phone to PC
3. Open cmd on PC
4. Run ```adb shell```
5. Run ```parted```
6. Run ```print``` to list all partitions
7. Look for partitions that have spaces in the names e.g "Basic Data Partition" and note their volume number
8. Now run ```rm <vol number>``` e.g ```rm 36```
</details>   
  
<details>   
<summary><strong>Touchscreen touches are inaccurate/upside down</strong></summary>

You have incorrectly configured the touch driver, to fix this:
- [Follow this part of the guide](/install.md#check-what-type-of-panel-you-have)
</details>    
