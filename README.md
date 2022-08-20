### Домашнее задание №3 (LVM)
#### Подготовительные мероприятия:
1. Установка пакета __xfsdump__, __nano__
2. Запуск утилиты __script__ 

#### 1. Подготовка временного тома для корневого каталога:
* Сделал physical volume на блочном устройстве __/dev/sdb__ => _Physical volume "/dev/sdb" successfully created_.
* Сделал volume group на __/dev/sdb__ с именем "vg_root" => _Volume group "vg_root" successfully created_.
* Сделал logical volume с именем "lv_root", занимающий 100% пространства => _Logical volume "lv_root" created_.
* Сделал файловую систему при помощи утилиты __mkfs.xfs__ и примонтировал volume в __/mnt__.
*  => _xfsrestore: Restore Status: SUCCESS_
* Переконфигурил root => _Generating grub configuration file ... Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64 Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img done_
* Обновил образ initrd => _*** Creating image file *** *** Creating image file done *** *** Creating initramfs image file '/boot/initramfs-3.10.0 862.2.3.el7.x86_64.img' done ***_
* Сделал правки в файле __/boot/grub2/grub2.cfg__ => Вывод команды __lsblk_:

[vagrant@localhost ~]$ lsblk

sdb                       8:16   0   10G  0 disk 
└─vg_root-lv_root       253:0    0   10G  0 lvm  /
 =>
#### 2. Уменьшение корневого каталога __/__ до 8Gb:
* Удалил старый logical volume __LogVol00__ командой __lvremove__ => _Logical volume "LogVol00" successfully removed_.
* Создал новый logical volume __LogVol00__ на __8Gb__ => _Logical volume "LogVol00" created_.
* Сделал файловую систему при помощи утилиты __mkfs.xfs__ и примонтировал volume в __/mnt__.
* Скопировал данные с __/dev/vg_root/lv_root__ в примонтированный volume в __/mnt__ при помощи утилиты __xfsdump__ => _xfsrestore: Restore Status: SUCCESS_
* Переконфигурировал __grub__ => _Generating grub configuration file ... Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64 Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img done_
* Обновил образ initrd => _*** Creating image file *** *** Creating image file done *** *** Creating initramfs image file '/boot/initramfs-3.10.0-862.2.3.el7.x86_64.img' done ***_

#### 3. Выделение тома под /var в зеркало:
* Сделал 2 __physical volume__ на __/dev/sdc, /dev/sdd__ =>  _Physical volume "/dev/sdc" successfully created. Physical volume "/dev/sdd" successfully created_.
* Сделал volume group на __/dev/sdc, /dev/sdd__ с именем "vg_var" => _Volume group "vg_var" successfully created_.
* Сделал logical volume с именем "vg_var" => _Logical volume "vg_var" created_.
* Создал файловую систему при помощи утилиты __mkfs.ext4__ на __lv_var__ => _Writing superblocks and filesystem accounting information: done_.
* Создал директорию __/tmp/oldvar__ и скопировал туда содержимое директории __/var__.
* Смониторовал __/dev/vg_var/lv_var__ в __/var__.
* Поправил конфигурационный файл __/etc/fstab__ для автоматического монтирования каталога __/var__.
* Удалил временную __volume group__ => _Logical volume "lv_root" successfully removed_.
* Удалил временную __volume group__ => _Volume group "vg_root" successfully removed_
* Удалил временную __physical volume__ на __/dev/sdb/__ => _Labels on physical volume "/dev/sdb" successfully wiped_.

#### 4. Выделение тома под /home:
* Создал новый __logical volume__ с именем "LogVol_Home" размером __2G__ =>  _Logical volume "LogVol_Home" created_.
* Создал файловую систему на __LogVol_Home__.
* Смониторовал __/dev/VolGroup00/LogVol_Home__ в __/mnt__.
* Скопировал __/home__ в __/mnt__.
* Рекурсивно удалил каталог __/home__.
* Смониторовал __/dev/VolGroup00/LogVol_Home__ в __/home__.
* Внес правки в __/etc/fstab__ для автоматического монтирования __/home__.

#### 5. Создание тома для снепшотов в /home:
* Сгенерировал файлы в каталоге __/home__.
* Сделал снепшот __"home_snap"__.
* Удалил часть файлов в каталоге __/home__.
* Восстановил информацию из снэпшота.


