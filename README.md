### Домашнее задание №3 (LVM)
#### Подготовительные мероприятия:
1. Установка пакета __xfsdump__, __nano__
2. Запуск утилиты __script__ 

#### 1. Подготовка временного тома для корневого каталога:
* Сделал physical volume на блочном устройстве __/dev/sdb__ => _Physical volume "/dev/sdb" successfully created_.
* Сделал volume group на __/dev/sdb__ с именем "vg_root" => _Volume group "vg_root" successfully created_.
* Сделал logical volume с именем "lv_root", занимающий 100% пространства => _Logical volume "lv_root" created_.
* Сделал файловую систему при помощи утилиты __mkfs.xfs__ и примонтировал volume в __/mnt__.
* Скопировал данные с корневого каталога __/__ в примонтированный volume в __/mnt__ при помощи утилиты __xfsdump__ => _xfsrestore: Restore Status: SUCCESS_
* Переконфигурил root => _Generating grub configuration file ... Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64 Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img done_
* Обновил образ initrd => _*** Creating image file *** *** Creating image file done *** *** Creating initramfs image file '/boot/initramfs-3.10.0 862.2.3.el7.x86_64.img' done ***_
* Сделал правки в файле __/boot/grub2/grub2.cfg__ => Вывод команды __lsblk_:

[vagrant@localhost ~]$ lsblk

sdb                       8:16   0   10G  0 disk 
└─vg_root-lv_root       253:0    0   10G  0 lvm  /

#### Уменьшение корневого каталога __/__ до 8Gb:
* Удалил старый logical volume __LogVol00__ командой __lvremove__ => Logical volume "LogVol00" successfully removed.


