### 1 Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

```~$ ip link```
```1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 00:15:5d:14:19:0c brd ff:ff:ff:ff:ff:ff
```
```~$ ls /sys/class/net/```  
```eth0  lo```

```>ipconfig /all```  
```Настройка протокола IP для Windows

   Имя компьютера  . . . . . . . . . : THN-41390
   Основной DNS-суффикс  . . . . . . : lan.kvt.su
   Тип узла. . . . . . . . . . . . . : Гибридный
   IP-маршрутизация включена . . . . : Нет
   WINS-прокси включен . . . . . . . : Нет
   Порядок просмотра суффиксов DNS . : lan.kvt.su

Адаптер Ethernet local:

   DNS-суффикс подключения . . . . . :
   Описание. . . . . . . . . . . . . : Intel(R) Ethernet Connection (5) I219-LM - VLAN : VLAN20
   Физический адрес. . . . . . . . . : 40-B0-34-F7-1D-AC
   DHCP включен. . . . . . . . . . . : Нет
   Автонастройка включена. . . . . . : Да
   IPv4-адрес. . . . . . . . . . . . : 192.168.20.13(Основной)
   Маска подсети . . . . . . . . . . : 255.255.255.0
   Основной шлюз. . . . . . . . . : 192.168.20.1
   DNS-серверы. . . . . . . . . . . : 192.168.20.2
                                       192.168.30.2
   NetBios через TCP/IP. . . . . . . . : Включен

Неизвестный адаптер OpenVPN TAP-Windows6:

   Состояние среды. . . . . . . . : Среда передачи недоступна.
   DNS-суффикс подключения . . . . . :
   Описание. . . . . . . . . . . . . : TAP-Windows Adapter V9
   Физический адрес. . . . . . . . . : 00-FF-E0-8F-85-6F
   DHCP включен. . . . . . . . . . . : Да
   Автонастройка включена. . . . . . : Да
```

### 2 Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

Протокол LLDP, пакет lldpd  
```$ lldpctl```
```-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
Interface:    eno1, via: LLDP, RID: 1, Time: 9 days, 16:56:28
  Chassis:
    ChassisID:    mac e8:ed:f3:02:96:00
    SysName:      sw_tlsk.SR1.lan.kvt.su
    SysDescr:     Cisco IOS Software, C2960S Software (C2960S-UNIVERSALK9-M), Version 15.2(2)E9, RELEASE SOFTWARE (fc4)
                  Technical Support: http://www.cisco.com/techsupport
                  Copyright (c) 1986-2018 by Cisco Systems, Inc.
                  Compiled Sat 08-Sep-18 14:56 by prod_rel_team
    MgmtIP:       192.168.20.250
    Capability:   Bridge, on
    Capability:   Router, off
  Port:
    PortID:       ifname Gi1/0/11
    PortDescr:    GigabitEthernet1/0/11
    TTL:          120
    PMD autoneg:  supported: yes, enabled: yes
      Adv:          10Base-T, HD: yes, FD: yes
      Adv:          100Base-TX, HD: yes, FD: yes
      Adv:          1000Base-T, HD: no, FD: yes
      MAU oper type: 1000BaseTFD - Four-pair Category 5 UTP, full duplex mode
-------------------------------------------------------------------------------
Interface:    eno2, via: LLDP, RID: 1, Time: 9 days, 16:59:09
  Chassis:
    ChassisID:    mac e8:ed:f3:02:96:00
    SysName:      sw_tlsk.SR1.lan.kvt.su
    SysDescr:     Cisco IOS Software, C2960S Software (C2960S-UNIVERSALK9-M), Version 15.2(2)E9, RELEASE SOFTWARE (fc4)
                  Technical Support: http://www.cisco.com/techsupport
                  Copyright (c) 1986-2018 by Cisco Systems, Inc.
                  Compiled Sat 08-Sep-18 14:56 by prod_rel_team
    MgmtIP:       192.168.20.250
    Capability:   Bridge, on
    Capability:   Router, off
  Port:
    PortID:       ifname Gi1/0/12
    PortDescr:    GigabitEthernet1/0/12
    TTL:          120
    PMD autoneg:  supported: yes, enabled: yes
      Adv:          10Base-T, HD: yes, FD: yes
      Adv:          100Base-TX, HD: yes, FD: yes
      Adv:          1000Base-T, HD: no, FD: yes
      MAU oper type: 1000BaseTFD - Four-pair Category 5 UTP, full duplex mode
```

### 3 Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

Технология VLAN, в Linux пакет vlan.
Пример конфига:  
```cat /etc/netplan/01-network-manager-all.yaml```
```
network:
    version: 2
    renderer: networkd
    ethernets:
        enx28ee5205f51c:
            dhcp4: true
    vlans: 
        vlan22:
            id: 22
            link: enx28ee5205f51c
            dhcp4: no
            addresses: [192.168.22.208/24]
```

### 4 Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.
Есть технологии teaming и bonding. Параметры (опции) модуля bonding для балансировки нагрузки: balance-tlb, balance-alb, balance-rr, balance-xor и 802.3ad  
```cat /etc/netplan/01-network-manager-all.yaml```
```
network:
    version: 2
    renderer: networkd
    ethernets:
        ens2f0: {}
        ens2f1: {}
    bonds:
        bond0:
            dhcp4: no
            interfaces:
            - ens2f0
            - ens2f1
            parameters:
                mode: active-backup
            addresses:
                - 192.168.122.195/24
            gateway4: 192.168.122.1
            mtu: 1500
            nameservers:
                addresses:
                    - 8.8.8.8
                    - 77.88.8.8
```

### 5 Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.
В сетях с маской /29 8 адресов, 6 из которыых для устройств. Из сети класса 1С можно получить 32 подсети /29. 10.10.10.48/29, 10.10.10.168/29

### 6 Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.
Взять диапозон из частной подсети 100.64.0.0/10 Carrier-Grade NAT, например 100.64.0.1/26: 100.64.0.1-100.64.0.62

### 7 Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?
/#/ ARP таблица  
```ip neighbour```  
```arp -a```  
/# очистить всю ARP таблицу  
```ip neigh flush all```  
```arp -d *```  
/# очистить один адрес в ARP таблице  
```ip neigh delete 192.168.25.2 dev dev wlp1s0```  
```arp /d 192.168.20.208```

