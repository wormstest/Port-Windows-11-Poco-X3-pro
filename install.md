# Установка Windows на POCO X3 Pro
## Создание разделов для Windows
### Заметки:
> **Внимание!** Если вы удалили любой раздел используя diskpart, Windows рано или поздно отправит команду памяти, которая будет неверно распознана, отчего память будет стёрта.
- Все команды были протестированы.
- Все ваши данные в Android будут удалены.
- Игнорируйте предупреждения `udevadm`.
- Не запускайте одну и ту же команду дважды.
- НЕ ПЕРЕЗАГРУЖАЙТЕ ТЕЛЕФОН если думаете, что совершли ошибку - обратитесь в [Telegram-чат проекта](https://t.me/winonvayu).

### Нужные файлы
- [Образ Windows 10/11 ARM (Windows 11 рекомендуется)](https://uupdump.net/)
- [platform-tools(ADB & Fastboot)](https://developer.android.com/studio/releases/platform-tools)
- [DriverUpdater для установки и обновления драйверов](https://github.com/WOA-Project/DriverUpdater/releases/)
- [UEFI образ](https://github.com/degdag/edk2-msm/releases/tag/V2.1.0)
- [Модифицированный TWRP или OrangeFox](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases/tag/Recoveries)

### Запустите TWRP через комьютер с помощью fastboot
```cmd
fastboot boot <twrp.img>
```
⚠️ Не запускайте все команды сразу, выполняйте их по порядку!

⚠️ НЕ ДЕЛАЙТЕ ОШИБОК!!! ВЫ МОЖЕТЕ СЛОМАТЬ ВАШЕ УСТРОЙСТВО КОМАНДАМИ НИЖЕ, ЕСЛИ ВЫ СДЕЛАЕТЕ ИХ НЕПРАВИЛЬНО!!!

### Размонтирование всех разделов
Перейдите в раздел "Монтирование" и размонтируйте все разделы.

## Запустите adb shell:
```cmd
adb shell
```

### Измените размер таблицы разделов
> Чтобы разделы Windows поместились в таблице разделов
```sh
sgdisk --resize-table 64 /dev/block/sda
```

### Запустите parted
```sh
parted /dev/block/sda
```

### Удалите раздел `userdata`
> Вы должны убедится что 32 это номер раздела `userdata` командой
>  `print all`
```sh
rm 32
```

### Создайте разделы
> Если вы получите какое либо сообщение, говоря вам либо игнорировать либо отменить, просто введите i и нажмите enter.

<details>
<summary><strong>Для моделей 6/128</strong></summary>

- Создайте ESP раздел (будет содержать загрузчик Windows)
```sh
mkpart esp fat32 11.8GB 12.2GB
```

- Создайте раздел, в который будет установлена Windows
```sh
mkpart win ntfs 12.2GB 70.2GB
```

- Создайте раздел `userdata` для использования Android вместе с Windows
```sh
mkpart userdata ext4 70.2GB 127GB
```
</details>

<details>
<summary><strong>Для моделей 8/256</strong></summary>

- Создайте ESP раздел (будет содержать загрузчик Windows)
```sh
mkpart esp fat32 11.8GB 12.2GB
```

- Создайте раздел на который будет установлена Windows
```sh
mkpart win ntfs 12.2GB 132.2GB
```

- Создайте раздел `userdata` для использования Android вместе с Windows
```sh
mkpart userdata ext4 132.2GB 255GB
```
</details>


### Сделайте раздел ESP загрузочным, чтобы образ EFI смог его обнаружить
```sh
set 32 esp on
```

### Закройте parted
```sh
quit
```

- Перезагрузитесь в TWRP

### Запустите adb shell снова
```cmd
adb shell
```

### Отформатируйте разделы
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPVAYU
mkfs.ntfs -f /dev/block/by-name/win -L WINVAYU
```

- Отформатируйте userdata: перейдите в меню "Очистка", выберите "Форматировать data", напишите `yes`, и нажмите "✓".

### Убедитесь что Android запускается
Перезагрузите телефон в систему и убедитесь, что Android запускается.

## Установка Windows
> Вам нужно будет отключить MTP в разделе "Монтирование"
### Запустите скрипт msc.sh
```cmd
adb shell msc.sh
```

### Назначение букв разделам

> Когда ваш телефон определился как диск запустите diskpart
```diskpart
Используйте `list volume` для того, чтобы найти разделы Windows и ESP, они называются "WINVAYU" и "ESPVAYU"
# select volume <win-partition-number>
# assign letter=x
# select volume <esp-partition-number>
# assign letter=y
# exit
```

## Развёртывание образа Windows
> Замените `<path/to/install.wim>` действительным путём к install.wim, `install.wim` находится в папке sources внутри вашего ISO. Вы можете получить этот файл распаковав или смонтировав его
```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

### Создайте файлы загрузчика Windows
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

### Разрешите неподписанные драйвера
> Если вы этого не сделаете, то получите BSOD.
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set {default} testsigning on
```
### Установка драйверов
#### Узнайте ваш тип панели
> Перейдите в папку platform-tools
```cmd
adb shell cat /proc/cmdline
```
> Ищите экран почти в самом внизу
> Если ваше устройство `Tianma`, то `msm_drm.dsi_display0` будет `dsi_j20s_36_02_0a_video_display`.
> Если ваше устройство `Huaxing`, то `msm_drm.dsi_display0` будет `dsi_j20s_42_02_0b_video_display`: перейдите в папку драйверов (Vayu-Drivers/components/QC8150/Device/DEVICE.SOC_QC8150.VAYU/Drivers/Touch/) и удалите j20s_novatek_ts_fw01.bin, наконец, переименуйте j20s_novatek_ts_fw02.bin в j20s_novatek_ts_fw01.bin

> Замените `<vayudriversfolder>` путём к папке с вашими драйверами
```cmd
driverupdater.exe -d <vayudriversfolder>\definitions\Desktop\ARM64\Internal\vayu.txt -r <vayudriversfolder> -p X:
```

### Загрузка в Windows

<details>
<summary><strong>Двойная загрузка между Android и Windows</strong></summary>

- [Вы должны следовать этой инструкции](/dualboot.md)
</details>

<details>
<summary><strong>Загрузка Windows вручную каждый раз</strong></summary>

Перезагрузите телефон в fastboot и загрузитесь с UEFI:
```fastboot
fastboot boot <uefi.img>
```

Android загрузится при следующей перезагрузке, для загрузки Windows вы должны вновь загрузится с UEFI.
</details>

- Если у вас BSOD с кодом ошибки BOUND_IMAGE_UNSUPPORTED при первой загрзке Windows, вы должны использовать [старое UEFI](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases) для первой загрузки (тачскрин не работает в старой версии UEFI).


