## Двойная загрузка между Android и Windows

<details>
<summary><strong>С Android на Windows</strong></summary>

<details>
<summary><strong>Использование UEFI вместо recovery</strong></summary>

Мы можем вместо recovery использовать образ UEFI, для этого нужно прошить UEFI в раздел recovery:
```fastboot
fastboot flash recovery uefi.img
```
- Для того что бы восстановить recovery нужно скачать TWRP или OrangeFox и прошить командой ``fastboot flash recovery recovery.img```
</details>

</details>

<details>
<summary><strong>С Windows на Android</strong></summary>

<details>
<summary><strong>Использование резервной копии раздела Boot</strong></summary>

Мы можем использовать резервную копию раздела Boot для загрузки Android после использования Windows.

Чтобы создать резервную копию раздела Boot в TWRP, нужно перед прошивкой UEFI в главном меню нажать «Резервное копирование», выбрать раздел Boot, место, куда будет сохранен бэкап, и свайпом вниз подтвердить выбор и запустить резервное копирование.

- Вам не нужно делать резервную копию каждый раз перед загрузкой Windows.

После использования Windows, чтобы вернуться на Android, нужно восстановить Boot раздел из резервной копии. Для этого в главном меню TWRP нажмите «Восстановление», выберите резервную копию Boot раздела и свайпните пальцем внизу, чтобы подтвердить выбор. После восстановления необходимо перезагрузить телефон в систему и Android должен загрузиться.
</details>
