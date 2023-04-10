# 10.4 «Резервное копирование» Акиньшин С.Г.

## Задание 1

Полное резервное копирование (Full backup) - это процесс создания резервной копии всей информации, находящейся на компьютере или в системе хранения данных. При полном резервном копировании все данные копируются с нуля, и каждый раз, когда выполняется полное резервное копирование, создается полная копия всей информации, что может занимать много времени и места для хранения.

Дифференциальное резервное копирование (Differential backup) - это процесс создания резервной копии только тех файлов, которые изменились с момента последнего полного резервного копирования. При этом происходит копирование всех изменений, включая добавленные, измененные и удаленные файлы. Это позволяет сократить время и объем хранимой информации по сравнению с полным резервным копированием, но требует больше времени и ресурсов на восстановление данных.

Инкрементное резервное копирование (Incremental backup) - это процесс создания резервной копии только тех файлов, которые были изменены или созданы с момента последнего резервного копирования (включая полное или инкрементное). Это означает, что при первом резервном копировании копируются все данные, а затем при каждом последующем копировании копируются только измененные или новые файлы. Это позволяет значительно сократить время и объем хранимой информации по сравнению с полным или дифференциальным резервным копированием.


## Задание 2

1. `Установил программное обеспечении Bacula, настроил bacula-dir, bacula-sd, bacula-fd. Протестировал работу сервисов.`

2. `Кофиги прилагаются`

https://github.com/akinya1974/BACULA-RSYNC/tree/main/CONFIGS

![RUN](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/RUN-2.jpg)

![RUN](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/RUN.jpg)

![STATUS](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/DIR%20STATUS.jpg)

![STATUS](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/SD%20STATUS.jpg)


## Задание 3

### КОНФИГУРАЦИИ:

### backup-node1.sh

```
#!/bin/bash
date
syst_dir=/backup/
srv_name=node1 #из тестовой конфигурации
srv_ip=192.168.1.78
srv_user=akinya
srv_dir=data
echo "Start backup ${srv_name}"
mkdir -p ${syst_dir}${srv_name}/increment/

/usr/bin/rsync -avz --progress --delete
--password-file=/etc/rsyncd.scrt ${srv_user}@${srv_ip}::${srv_dir}
${syst_dir}${srv_name}/current/ --backup
--backup-dir=${syst_dir}${srv_name}/increment/`date +%Y-%m-%d`/
/usr/bin/find ${syst_dir}${srv_name}/increment/ -maxdepth 1 -type d
-mtime +30 -exec rm -rf {} \;
date
echo "Finish backup ${srv_name}"
```
### rsyncd.conf
```
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
transfer logging = true
munge symlinks = yes
[data]
path = /data
uid = root
read only = yes
list = yes
comment = Data backup Dir
auth users = akinya
secrets file = /etc/rsyncd.scrt
```


![LISTEN PORT](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/LISTEN%20PORT%20RCYNC.jpg)


![NETSTAT](https://github.com/akinya1974/BACULA-RSYNC/blob/main/JPG/RSYNC%20-netstat.jpg)
