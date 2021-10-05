# Домашнее задание к занятию "3.5. Файловые системы"

1. Узнайте о [sparse](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных) файлах.
    Ознакомился со статьей
    
1. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему
    - Жесткая ссылка и файл, для которой она создавалась имеют одинаковые inode. Поэтому жесткая ссылка имеет те же права доступа, владельца и время последней модификации, что и целевой файл. Различаются только имена файлов. Фактически жесткая ссылка это еще одно имя для файла.

1. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

    ```bash
    Vagrant.configure("2") do |config|
      config.vm.box = "bento/ubuntu-20.04"
      config.vm.provider :virtualbox do |vb|
        lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
        lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
        vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
        vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
      end
    end
    ```

    Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб. 
   - Создал
1. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
 ```bash
 - создаем первый раздел на 2 ГБ
sudo fdisk /dev/sdb

Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-5242879, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): 2000M
Value out of range.
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2000M

Created a new partition 1 of type 'Linux' and of size 2 GiB.

 - и следом второй на оставшиеся 500
 Partition number (2-4, default 2): 2
First sector (4098048-5242879, default 4098048): 4098048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4098048-5242879, default 5242879): +500M

Created a new partition 2 of type 'Linux' and of size 500 MiB.

sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
└─sdb2                 8:18   0  500M  0 part
sdc                    8:32   0  2.5G  0 disk
```
5. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.
```buildoutcfg
sudo sfdisk -d /dev/sdb > part_table_sdb
sudo sfdisk /dev/sdc < part_table_sdb
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 500 MiB.
/dev/sdc3: Done.
lsblk
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
└─sdb2                 8:18   0  500M  0 part
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
└─sdc2                 8:34   0  500M  0 part
```
6. Соберите `mdadm` RAID1 на паре разделов 2 Гб.
```buildoutcfg
sudo mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b1,c1}
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdb2                 8:18   0  500M  0 part
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdc2                 8:34   0  500M  0 part
```

7. Соберите `mdadm` RAID0 на второй паре маленьких разделов.
```buildoutcfg
sudo mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b2,c2}
```

8. Создайте 2 независимых PV на получившихся md-устройствах.
```buildoutcfg
sudo pvcreate /dev/md0
sudo pvcreate /dev/md1
```
9. Создайте общую volume-group на этих двух PV.
```buildoutcfg
sudo vgcreate vg-01 /dev/md0 /dev/md1
```
10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

```buildoutcfg
sudo lvcreate -n vol_100 -L 100M /dev/vg-01 /dev/md0
```
11. Создайте `mkfs.ext4` ФС на получившемся LV.
```
sudo mkfs.ext4 /dev/vg-01/vol_100
```
12. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`. 
    ```bash
         sudo mount /dev/vg-01/vol_100 /tmp/new
    ```
1. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.
    ```commandline
   Поместил
    ```
1. Прикрепите вывод `lsblk`.
```commandline
   sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
│   └─vg--01-vol_100 253:2    0  100M  0 lvm   /tmp/new
└─sdb2                 8:18   0  500M  0 part
  └─md1                9:1    0  996M  0 raid0
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
│   └─vg--01-vol_100 253:2    0  100M  0 lvm   /tmp/new
└─sdc2                 8:34   0  500M  0 part
  └─md1                9:1    0  996M  0 raid0
```

15. Протестируйте целостность файла:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
    ```commandline
    vagrant@vagrant:~$ gzip -t /tmp/new/test.gz
    vagrant@vagrant:~$ echo $?
    0

    ```

1. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
    - sudo pvmove /dev/md0 /dev/md1
1. Сделайте `--fail` на устройство в вашем RAID1 md.
    ```commandline
   sudo mdadm /dev/md0 --fail /dev/sdc1
    ```

1. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.
    ```commandline
    [ 9642.343745] md/raid1:md0: Disk failure on sdc1, disabling device.
               md/raid1:md0: Operation continuing on 1 devices.
    ```
1. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
    ```commandline
        Всё верно
    ``` 
1. Погасите тестовый хост, `vagrant destroy`.
