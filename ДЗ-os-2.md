## Задание

1. На лекции вы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку;
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`);
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.
    
 ----
Загрузка  
`wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz`  
Распаковка  
`tar zxvf node_exporter-1.5.0.linux-amd64.tar.gz`  
Исполняемый файл перенести в bin:  
`cd node_exporter-1.5.0.linux-amd64/`  
`cp node_exporter /usr/local/bin/`  
Файл node_exporter.service в systemd  
`sudo nano /etc/systemd/system/node_exporter.service`
```
[Unit]
Description=Node Exporter Service
After=network.target

[Service]
Type=simple
EnvironmentFile=-/etc/default/node_exporter 
ExecStart=/usr/local/bin/node_exporter
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Перезагрузка всех юнитов
`systemctl daemon-reload`  
Помещаем в автозагрузку и запускаем службу:  
`systemctl enable node_exporter`  
`systemctl start node_exporter`  

Параметр `EnvironmentFile` в блок `[Service]` можно использовать как файл с дополнительными настройками. Добавим опцию, отключенную по умолчанию - `--collector.ntp`  
`echo 'EXTRA_OPTS=\"--collector.ntp\"' | sudo tee /etc/default/node_exporter`  
Процесс корректно стартует, завершается и автоматически поднимается после перезагрузки

----
 

2. Изучите опции node_exporter и вывод `/metrics` по умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

----
CPU
```
node_cpu_seconds_total{cpu="0",mode="idle"} 5001.36
node_cpu_seconds_total{cpu="0",mode="iowait"} 11.6
node_cpu_seconds_total{cpu="0",mode="system"} 10.86
```
mem
```
# TYPE node_memory_MemAvailable_bytes gauge
node_memory_MemAvailable_bytes 6.54852096e+08
# TYPE node_memory_MemFree_bytes gauge
node_memory_MemFree_bytes 4.69254144e+08
# TYPE node_memory_MemTotal_bytes gauge
node_memory_MemTotal_bytes 1.323798528e+09
# TYPE node_memory_SwapFree_bytes gauge
node_memory_SwapFree_bytes 2.147479552e+09
# TYPE node_memory_SwapTotal_bytes gauge
node_memory_SwapTotal_bytes 2.147479552e+09
```
disk
```
# TYPE node_filesystem_avail_bytes gauge
node_filesystem_avail_bytes{device="/dev/mapper/ubuntu--vg-ubuntu--lv",fstype="ext4",mountpoint="/"} 5.4363619328e+10
# TYPE node_disk_read_bytes_total counter
node_disk_read_bytes_total{device="sda"} 3.00301312e+08
# TYPE node_disk_written_bytes_total counter
node_disk_written_bytes_total{device="sda"} 2.3225344e+07
# TYPE node_disk_io_now gauge
node_disk_io_now{device="sda"} 0
```
net
```
# TYPE node_network_info gauge
node_network_info{address="00:15:5d:14:19:0c",broadcast="ff:ff:ff:ff:ff:ff",device="eth0",duplex="full",ifalias="",operstate="up"} 1
# TYPE node_network_receive_errs_total counter
node_network_receive_errs_total{device="eth0"} 0
# TYPE node_network_transmit_errs_total counter
node_network_transmit_errs_total{device="eth0"} 0
# TYPE node_network_receive_packets_total counter
node_network_receive_packets_total{device="eth0"} 33884
# TYPE node_network_transmit_packets_total counter
node_network_transmit_packets_total{device="eth0"} 6174
```
----

3. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). 
   
   После успешной установки:
   
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`;
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере на своём ПК (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata, и с комментариями, которые даны к этим метрикам.

----
![image](https://user-images.githubusercontent.com/45497624/226575990-2edbd8b3-cec3-46c0-a25d-c46bbd8c9448.png)
На самом первом дашборде показываются главные метрики системы. Далее идут графики использования ресурсов процессора, потребления памяти, операций ввода-вывода, сетевой активности и другое.

----

4. Можно ли по выводу `dmesg` понять, осознаёт ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

----
```
[    0.000000] Hypervisor detected: Microsoft Hyper-V
```
----

5. Как настроен sysctl `fs.nr_open` на системе по умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?

----
`cat /proc/sys/fs/nr_open` - лимит открытых файлов для каждого отдельного процесса
```
1048576
```
`Ulimit` — команда отображения и ограничения ресурсов, опция -n: мягкое ограничение количества файлов, которые пользователь может открыть одновременно
```
-n        the maximum number of open file descriptors
```
`ulimit -n`
```
1024
```

----

6. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в этом задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т. д.

----

`unshare -f --pid --mount-proc sleep 1h &`
```
[1] 140912
```
`ps aux | grep sleep`
```
root      140912  0.0  0.1   5768   996 pts/1    S    17:45   0:00 unshare -f --pid --mount-proc sleep 1h
root      140913  0.0  0.1   5768  1012 pts/1    S    17:45   0:00 sleep 1h
root      140915  0.0  0.2   6476  2348 pts/1    S+   17:45   0:00 grep --color=auto sleep
```
`nsenter --target 140913 --pid --mount`  
`ps auxf`
```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           2  0.0  0.6   8656  5360 pts/1    S    17:46   0:00 -bash
root          13  0.0  0.1  10040  1560 pts/1    R+   17:48   0:00  \_ ps auxf
root           1  0.0  0.1   5768  1012 pts/1    S    17:45   0:00 sleep 1h
```
----

7. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время всё будет плохо, после чего (спустя минуты) — ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации.  
Как настроен этот механизм по умолчанию, и как изменить число процессов, которое можно создать в сессии?

----
Конструкция `:(){ :|:& };:` рекурсивно выполняет системный вызов fork().  
Вызов `dmesg` подксказывает, что помог механизм `cgroup` (control groups) - механизм уровня ядра, позволяющий управлять использованием системных ресурсов (ограничить количество процессов).
```
[ 2321.630456] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-6.scope
```
Настройки ограничений для слайса user.slice, дочерний слайс user-1000 можно посмотреть в конфигурационном файле `cat /sys/fs/cgroup/pids/user.slice/user-1000.slice/pids.max`
```
2353
```
Это соответсвует счётчику использования ресурсов `systemd-cgtop`
![image](https://user-images.githubusercontent.com/45497624/226831258-7770f534-02c8-49f7-93d7-b730ea6dc895.png)

----
