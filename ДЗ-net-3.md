###	1 Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

\route-views>show ip route 185.34.155.174
Routing entry for 185.34.154.0/23
  Known via "bgp 6447", distance 20, metric 0
  Tag 3303, type external
  Last update from 217.192.89.50 3w3d ago
  Routing Descriptor Blocks:
  * 217.192.89.50, from 217.192.89.50, 3w3d ago
      Route metric is 0, traffic share count is 1
      AS Hops 3
      Route tag 3303
      MPLS label: none

route-views>show bgp 185.34.155.174
BGP routing table entry for 185.34.154.0/23, version 2653864029
Paths: (20 available, best #19, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 20485 60172
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE1687C3638 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 3216 60172
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      Community: 3216:2001 3216:4440 65000:52254 65000:65049
      path 7FE13FB44448 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 1299 20485 60172
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8066 3257:30055 3257:50001 3257:53900 3257:53902 20912:650                                                                                                                                                             04
      path 7FE1782E89E8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 3216 60172
    64.71.137.241 from 64.71.137.241 (216.218.253.53)
      Origin IGP, localpref 100, valid, external
      path 7FE134E1AD28 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  8283 3216 60172
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4440 8283:1 8283:101 65000:52254 65000:65049
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x24
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 0000 205B 0000 0008
              0000 0040
      path 7FE18D350588 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 3356 3216 60172
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE0FC17CD50 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 3356 20485 60172
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin IGP, metric 10, localpref 100, valid, external
      Community: 3257:8794 3257:30043 3257:50001 3257:54900 3257:54901
      path 7FE108BD1D78 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 3216 60172
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:1
      path 7FE14394C010 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 3216 60172
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE0B9878178 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 174 3216 60172
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22010
      path 7FE020785350 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 3216 60172
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4440 3356:2 3356:22 3356:100 3356:123 3356:503 3356:903 3356:2067 3549:2581 3549:30840
      path 7FE11AF1D388 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 3356 20485 60172
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE0B9B34AD8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 3491 20485 20485 60172
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin IGP, localpref 100, valid, external
      Community: 101:20300 101:22100 3491:300 3491:311 3491:9001 3491:9080 3491:9081 3491:9087 3491:62210 3491:62220 20485:10040
      path 7FE1013F3410 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 20485 60172
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 20485:10040
      path 7FE0B711A450 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3356 3216 60172
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4440 3356:2 3356:22 3356:100 3356:123 3356:503 3356:903 3356:2067 57866:100 65100:3356 65103:1 65104:31
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x30
        value 0000 E20A 0000 0064 0000 0D1C 0000 E20A
              0000 0065 0000 0064 0000 E20A 0000 0067
              0000 0001 0000 E20A 0000 0068 0000 001F

      path 7FE0E5DA9DC8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  2497 3216 60172
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin IGP, localpref 100, valid, external
      path 7FE120ECE398 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 3216 60172
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE03D8527D0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3491 20485 20485 60172
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE15F3C7088 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3303 20485 60172
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external, best
      Community: 3303:1004 3303:1006 3303:1030 3303:1031 3303:3056 20485:10040 65101:1082 65102:1000 65103:276 65104:150
      path 7FE13011D6B0 RPKI State not found
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 1
  1351 6939 3216 60172
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE09B6C94A8 RPKI State not found
      rx pathid: 0, tx pathid: 0
	  
###	2 Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

Включить модуль ядра
$ sudo modprobe dummy
Создать новый виртуальный интерфейс
$ sudo ip link add eth0 type dummy

$ cat /etc/netplan/00-installer-config.yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    eth0:
      addresses:
      - 192.168.20.8/24
      routes:
        - to: default
          via: 192.168.20.1
        - to: 8.8.8.8
          via: 192.168.20.254
      nameservers:
        addresses:
        - 192.168.20.2
        search: []
    ethD:
      addresses:
      - 192.168.40.8/24
  version: 2

$ ip route
default via 192.168.20.1 dev eth0 proto static
8.8.8.8 via 192.168.20.254 dev eth0 proto static
192.168.20.0/24 dev eth0 proto kernel scope link src 192.168.20.8
192.168.40.0/24 dev ethD proto kernel scope link src 192.168.40.8


###	3 Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.
~$ ss -ltp
State                       Recv-Q                      Send-Q                                           Local Address:Port                                              Peer Address:Port                      Process
LISTEN                      0                           4096                                             127.0.0.53%lo:domain                                                 0.0.0.0:*
LISTEN                      0                           128                                                    0.0.0.0:ssh                                                    0.0.0.0:*
LISTEN                      0                           128                                                       [::]:ssh                                                       [::]:*
domain - 53 порт tcp (DNS)
ssh - 22 порт tcp


###	4 Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?
$ ss -lup
State                       Recv-Q                      Send-Q                                           Local Address:Port                                              Peer Address:Port                      Process
UNCONN                      0                           0                                                127.0.0.53%lo:domain                                                 0.0.0.0:*
domain - 53 порт udp (DNS)

###	5 Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.
![diag](https://user-images.githubusercontent.com/45497624/218222117-57d2af5e-b254-4f57-90f5-6a156e80080a.png)
