# Windows on the POCO X3 Pro
<details> 
<summary><strong>Another languages</strong></summary>

</details>

<img align="center" src="https://github.com/wormstest/src_vayu_windows/blob/main/2Poco X3 Pro Windows.png" alt="Windows 11 Running On A Poco X3 Pro">

- Status of the port
- Windows installation instructions
- Dualboot beetween Android and Windows
- Uninstall Windows
- Update drivers instruction
- Troubleshooting issues
<details>
<summary><b><strong>Contributors</strong></b></summary>

- [Morc](Https://GitHub.com/themorc) ```Made the vayu images```
- [Icesito68](https://github.com/Icesito68) ```Made Windows partitioning commands and made this repo```
- [Map220v](https://github.com/map220v) ```Provided help and vayu UEFI uses nabu UFS patches and ACPI and also ported mi pad 5 drivers```
- [Degdag](https://github.com/degdag) ```Improves UEFI and ported drivers```
- [halal-beef](https://github.com/halal-beef) ```Built EDK2 and modified it enough to boot Windows, also ported drivers```
- [Renegade Project](https://github.com/edk2-porting) ```Making the core of this project```
- [gus33000](https://github.com/gus33000) ```Providing help, also made base install guide, all of the original drivers and the msc script```
- [Renegade Project Discord members](https://discord.gg/XXBWfag) ```Provided Help```
- [ArturoGC06](https://github.com/ArturoGC06) ```Helped in the beginning of the project to the translations and gave Windows data```
- [SebastianZSXS](https://github.com/SebastianZSXS) ```Helped to patch Windows PE```
- [MollySophia](https://github.com/MollySophia) ```Helped to fix battery status```
- [haouarihk](https://github.com/haouarihk) ```Great suggestions on the command notes, also made the new guide```
- [bibarub](https://github.com/bibarub) ```Guide improvenents```
- [wormstest](https://github.com/wormstest) ```Russian and Ukrainian translation``` 
- [proganime1200](https://github.com/proganime1200) ```Tremendously helped to make this possible, heavily contirbuted to the old guide by finding bk01-04 partitions and had managed to nearly get winpe booting in the early stages```

</details>  



<details> 
<summary><strong>Old README</strong></summary>


#### Features

- [ ] Audio ```Only by USB or Bluetooth```
- [x] Battery status
- [x] Bluetooth
- [x] Brightness
- [ ] Camera
- [ ] Charging ```Only slow charging now```
- [x] Display
- [x] GPU
- [x] LTE ```Only SIM1; requires provisioning on every boot```
- [x] SD ```Need unplug and plug many times```
- [x] Touchscreen ```Need off on display after boot```
- [x] UFS
- [x] USB ```PD hub needed```
- [x] Wi-Fi

#### Sensors (Need to provision after install Windows)
- [x] Accelerometer
- [ ] Fingerprint
- [x] GPS
- [x] Gyroscope
- [x] Light sensor
- [x] Magnetometer
- [x] Proximity



## Guides and requirements

<details> 
<summary><strong>Required Tools/Files</strong></summary>

Human:

- Understand English, Spanish, Russian, Lithuanian or Ukrainian 

- Understand how to use TWRP

- Understand how to use CMD

- Functioning brain

PC:

- [Windows on ARM image](https://uupdump.net/) (Windows 11 is recommended)

- [platform-tools](https://developer.android.com/studio/releases/platform-tools).

- [DriverUpdater](https://github.com/WOA-Project/DriverUpdater/releases/) to install the [drivers](https://github.com/degdag/Vayu-Drivers/releases/tag/v2.1.0)

Phone:
- [UEFI image](https://github.com/degdag/edk2-msm/releases/tag/V2.1.0)

- [Modified TWRP](https://github.com/Icesito68/Port-Windows-11-Poco-X3-pro/releases/tag/Recoveries)

</details> 

<details> 

<summary><strong>For people who followed the old guide, broke their partition table or want to uninstall Windows</strong></summary>

- [English](guide/English/restore-stock-en.md)
- [Español](guide/Español/0-transicion-es.md)
- [Русский](/guide/Russian/restore-stock-ru.md) 
- [Українська](/guide/Ukrainian/restore-stock-uk.md) 
- [Lietuvių](guide/Lithuanian/restore-stock-lt.md)

</details> 

### Windows installation instructions

<details> 

<summary><strong>English</strong></summary>

1. [Create partitions](guide/English/1-partition-en.md)
2. [Install Windows](guide/English/2-install-en.md)
  
</details> 
  
<details> 

<summary><strong>Español</strong></summary>

1. [Crear particiones](guide/Español/1-particiones-es.md)
2. [Instalar Windows](guide/Español/2-instalacion-es.md)
  
</details> 

<details> 
  
<summary><strong>Русский</strong></summary>

1. [Создание разделов](/guide/Russian/1-partition-ru.md)
2. [Установка Windows](/guide/Russian/2-install-ru.md)
  
</details> 

<details> 

<summary><strong>Українська</strong></summary>

1. [Створення розділів](guide/Ukrainian/1-partition-uk.md)
2. [Встановлення Windows](guide/Ukrainian/2-install-uk.md)
  
</details> 

<details> 

<summary><strong>Lietuvių</strong></summary>

1. [Particijų sukūrimas](guide/Lithuanian/1-partition-lt.md)
2. [Įdiegti Windows](guide/Lithuanian/2-install-lt.md)
  
</details> 

### Other guides:

<details> 

<summary><strong>English</strong></summary>

- [If you just want to update the drivers follow these commands](guide/English/update-en.md)
- [Troubleshooting issues](guide/English/troubleshooting-en.md)

</details> 

<details> 

<summary><strong>Español</strong></summary>

- [Si solo quieres actualizar los drivers sigue estos comandos](guide/Español/Actualizar-es.md)
  
</details> 

<details> 

<summary><strong>Русский</strong></summary>

- [Если вы хотите обновить драйвера, следуйте этим командам](guide/Russian/update-ru.md)
- [Устранение проблем](guide/Russian/troubleshooting-ru.md)

</details> 

<details> 

<summary><strong>Українська</strong></summary>

- [Якщо ви хочете оновити драйвера, дотримуйтесь цих команд](guide/Ukrainian/update-uk.md)
- [Усуненя проблем](guide/Ukrainian/troubleshooting-uk.md)

</details> 

<details> 

<summary><strong>Lietuvių</strong></summary>

- [Jeigu norite atnaujinti tik draiverius, sekite šias komandas](guide/Lithuanian/update-lt.md)
- [Problemų šalinimas](guide/Lithuanian/troubleshooting-lt.md)

</details> 

## Contributors

<details> 

</details>

<summary><b><strong>Credits</strong></b></summary>

- [Morc](Https://GitHub.com/themorc) ```Made the vayu images```
- [Icesito68](https://github.com/Icesito68) ```Made Windows partitioning commands and made this repo```
- [Map220v](https://github.com/map220v) ```Provided help and vayu UEFI uses nabu UFS patches and ACPI and also ported mi pad 5 drivers```
- [Degdag](https://github.com/degdag) ```Improves UEFI and ported drivers```
- [halal-beef](https://github.com/halal-beef) ```Built EDK2 and modified it enough to boot Windows, also ported drivers```
- [Renegade Project](https://github.com/edk2-porting) ```Making the core of this project```
- [gus33000](https://github.com/gus33000) ```Providing help, also made base install guide, all of the original drivers and the msc script```
- [Renegade Project Discord members](https://discord.gg/XXBWfag) ```Provided Help```
- [ArturoGC06](https://github.com/ArturoGC06) ```Helped in the beginning of the project to the translations and gave Windows data```
- [SebastianZSXS](https://github.com/SebastianZSXS) ```Helped to patch Windows PE```
- [MollySophia](https://github.com/MollySophia) ```Helped to fix battery status```
- [haouarihk](https://github.com/haouarihk) ```Great suggestions on the command notes, also made the new guide```
- [bibarub](https://github.com/bibarub) ```Guide improvenents```
- [wormstest](https://github.com/wormstest) ```Russian and Ukrainian translation``` 
- [proganime1200](https://github.com/proganime1200) ```Tremendously helped to make this possible, heavily contirbuted to the old guide by finding bk01-04 partitions and had managed to nearly get winpe booting in the early stages```

</details>  

