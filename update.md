## Оновлення драйверів Windows
### Запустіть TWRP за допомогою fastboot використовуючи комп'ютер

```cmd
fastboot boot <twrp.img>
````
> Якщо у вас вже встановлено TWRP, просто затисніть кнопки живлення та гучності + під час запуску телефону

### Скопіюйте сценарій msc.sh у телефон
```cmd
adb push msc.sh /sbin
````

### Виконання сценарію msc.sh
```cmd
adb shell sh /sbin/msc.sh
````

### Призначення літери диску
> Коли ваш телефон визначився як диск запустіть diskpart
```diskpart
Використовуйте list vol для того, щоб знайти розділи Windows і ESP, звичайно вони останні
# select volume <win-partition-number>
# assign letter=x
# select volume <esp-partition-number>
# assign letter=y
# exit
```

### Встановлення драйверів

> Замініть `<vayudriversfolder>` на вашу назву теки з драйверами.
> Відкрийте cmd від імені адміністратора
```cmd
.\driverupdater.exe -d <vayudriversfolder>\definitions\Desktop\ARM64\Internal\vayu.txt -r <vayudriversfolder> -p X:
````

### Завантаження образу UEFI
````
fastboot flash boot <uefi.img>
