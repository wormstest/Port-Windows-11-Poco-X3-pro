# Встановлення Windows на POCO X3 PRO

## Створення розділів для Windows

### Нотатки:
> **Увага!** Якщо ви видалили будь-який розділ використовуючи diskpart, Windows рано или пізно відправить команду пам'яти, яка буде неправильно розпізнана, і тому пам'ять буде стерта.
- Всі команди були перевірені.
- Всі ваші дані в Android будуть видалені.
- Ігноруйте попередження `udevadm`.
- Не запускайте ту саму команду двічі.
- НЕ ПЕРЕЗАВАНТАЖУЙТЕ ТЕЛЕФОН якщо думаєте, що зробили помилку - зверніться в [Telegram-чат проекту](https://t.me/winonvayu).

### Необхідні файли
- [Образ Windows 10/11 ARM(11 рекомендується)](https://uupdump.net/)
- [platform-tools(ADB & Fastboot)](https://developer.android.com/studio/releases/platform-tools)
- [DriverUpdater для встановлення та оновлення драйверів](https://github.com/WOA-Project/DriverUpdater/releases/)
- [UEFI образ](https://github.com/degdag/edk2-msm/releases/tag/V2.1.0)
- [Модифікований TWRP чи OrangeFox](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases/tag/Recoveries)

### Завантажте TWRP чи OrangeFox через комп'ютер за допомогою fastboot
```cmd
fastboot boot <recovery.img>
```

### Розмонтування всіх розділів
Перейдіть до розділу "Монтування" та розмонтуйте всі розділи.
### Запустіть adb shell:
```cmd
adb shell
```

⚠️ Не запускайте всі команди одночасно, виконуйте їх по порядку!                                                                                           

⚠️ НЕ РОБІТЬ ЖОДНОЇ ПОМИЛКИ!!! ВИ МОЖЕТЕ ЗЛАМАТИ ВАШ ПРИСТРОЙ ЗА ДОПОМОГОЮ КОМАНД НИЖЧЕ, ЯКЩО ВИКОНАЄТЕ ЇХ НЕПРАВИЛЬНО!!!

### Змініть розмір таблиці розділів
> Щоб розділи Windows помістились в таблиці розділів
```sh
sgdisk --resize-table 64 /dev/block/sda
```

### Завантажте parted
```sh
parted /dev/block/sda
```

### Видаліть розділ `userdata`
> Ви повинні впевнитися що 32 это номер розділа `userdata`. Перевірте це командою `print all`.
```sh
rm 32
```

### Створення розділів `win, esp і userdata`
> Якщо ви отримаете повідомлення "Ignore or cancel?" просто введіть i и натісніть enter.
<details> 
<summary><strong>Для моделей 6/128</strong></summary>

- Створіть ESP розділ (буде містити завантажувач Windows)
```sh
mkpart esp fat32 11.8GB 12.2GB
```
- Створіть розділ, до якого буде встановлена Windows
```sh
mkpart win ntfs 12.2GB 70.2GB
```
- Створіть розділ `userdata` для використання Android поряд з Windows
```sh
mkpart userdata ext4 70.2GB 127GB
```
</details>
<details> 
<summary><strong>Для моделей 8/256</strong></summary>

- Створіть ESP розділ (буде містити завантажувач Windows)
```sh
mkpart esp fat32 11.8GB 12.2GB
```
- Створіть розділ, до якого буде встановлена Windows
```sh
mkpart win ntfs 12.2GB 132.2GB
```
- Створіть розділ `userdata` для використання Android поряд з Windows
```sh
mkpart userdata ext4 132.2GB 255GB
```
</details> 

### Зробіть розділ ESP завантажувальним, щоб образ EFI міг його виявити
```sh
set 32 esp on
```

### Закрийте parted
```sh
quit
```
- Перезавантажтеся у TWRP

### Запустіть adb shell знову
```cmd
adb shell
```
### Відформатуйте розділи для їх подальшого використання
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPVAYU
mkfs.ntfs -f /dev/block/by-name/win -L WINVAYU
```
- Відформатуйте /userdata для використання Android: перейдіть в меню "Очищення", виберіть "Форматувати data", напишіть `yes` та натисніть "✓".

### Переконайтеся, що Android запускається
Перезавантажте телефон в систему и переконайтесь, що Android запускається.

## Встановлення Windows
> Вам потрібно вимкнути MTP в розділі "Монтування"

### Запустіть скрипт msc.sh
```cmd
adb shell msc.sh
```

### Призначення літер розділам
> Коли ваш телефон визначився як диск запустіть diskpart
```diskpart
Використовуйте list vol для того, щоб знайти розділи Windows і ESP, вони називаються "WINVAYU" і "ESPVAYU".
# select volume <win-partition-number>
# assign letter=x
# select volume <esp-partition-number>
# assign letter=y
# exit
```

### Розгортання образу Windows
> Замініть `<path\to\install.wim>` дійсним шляхом до `install.wim`, він знаходиться у теці `sources` всередині вашого ISO
> Ви можете отримати цей файл розпакувавши або смонтувавши йего
```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

### Створіть файли завантажувача Windows
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

### Дозвольте непідписані драйвера
> Якщо ви цього не зробите, то отримаєте BSOD
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set {default} testsigning on
```

### Встановлення драйверів
#### Дізнайтесь, який у вас тип панелі
> Перейдіть у теку `platform-tools`
```cmd
adb shell cat /proc/cmdline
```
> Шукайте екран майже в самом низу
> Якщо ваш пристрій `Tianma`, то `msm_drm.dsi_display0` буде `dsi_j20s_36_02_0a_video_display`
> Якщо ваш пристрій `Huaxing`, то `msm_drm.dsi_display0` буде `dsi_j20s_42_02_0b_video_display`. Вам треба перейти до папки драйверів `Vayu-Drivers/components/QC8150/Device/DEVICE.SOC_QC8150.VAYU/Drivers/Touch/`, видалити j20s_novatek_ts_fw01.bin, і перейменувати j20s_novatek_ts_fw02.bin в j20s_novatek_ts_fw01.bin

> Замініть `<vayudriversfolder>` шляхом к теці с вашими драйверами
```cmd
driverupdater.exe -d <vayudriversfolder>\definitions\Desktop\ARM64\Internal\vayu.txt -r <vayudriversfolder> -p X:
```

### Завантаження Windows

<details> 
<summary><strong>Подвійне завантаження між Android та Windows</strong></summary>

- [Ви маєте переглянути цей посібник](/dualboot.md)
  
</details>

<details> 
<summary><strong>Ручне завантаження Windows кожного разу</strong></summary>
 
Перезавантажте телефон у fastboot, потім завантажте UEFI:
  
```fastboot
fastboot boot <uefi.img>
```
При перезавантаженні буде завантажуватись Android, для завантаження у Windows вам потрібно знов завантажити UEFI.
  
</details>  
  
- Якщо ви маєте BSOD з кодом помилки BOUND_IMAGE_UNSUPPORTED під час першого завантаження Windows, вам потрібно використовувати [старий UEFI](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases)(тачскрін не працює у старій версії UEFI)

