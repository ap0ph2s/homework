### 1 Работа c HTTP через телнет
* Подключитесь утилитой телнет к сайту stackoverflow.com ```telnet stackoverflow.com 80```
* Отправьте HTTP запрос
```GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
В ответе укажите полученный HTTP код, что он означает?  


```$ curl -I stackoverflow.com:80```
```
HTTP/1.1 301 Moved Permanently
Connection: keep-alive
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/
x-request-guid: f19e9de1-e81e-478e-9115-a41341f86174
set-cookie: prov_tgt=; expires=Wed, 01 Feb 2023 19:33:02 GMT; domain=.stackoverflow.com; path=/; secure; samesite=none; httponly
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Fri, 03 Feb 2023 19:33:02 GMT
Via: 1.1 varnish
X-Served-By: cache-bma1656-BMA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1675452782.491577,VS0,VE104
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=ae68e3a8-87ac-4fe3-4e0d-a2ae9e50bc63; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly
```
Перенапраление со страницы HTTP на HTTPS - код 301

### 2 Повторите задание 1 в браузере, используя консоль разработчика F12.
* откройте вкладку Network
* отправьте запрос http://stackoverflow.com
* найдите первый ответ HTTP сервера, откройте вкладку Headers
* укажите в ответе полученный HTTP код
* проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
* приложите скриншот консоли браузера в ответ.

Первый ответ от сервера при открытии сайти - код 307 (Temporary Redirect) «временное перенаправление»
![Screenshot 1](https://user-images.githubusercontent.com/45497624/218485637-30678c45-b128-4065-bb23-5107b0bde23e.png)  
Из всех процессов дольше всего было ожидание ответа сервера и начальная загрузка контента.
![Screenshot 2](https://user-images.githubusercontent.com/45497624/218485682-56aed66f-6809-4c03-ab25-6e8895f99e4b.png)  

### 3 Какой IP адрес у вас в интернете?
```dig +short myip.opendns.com @resolver1.opendns.com```  
```185.34.155.174```

### 4 Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой ```whois```
```$ whois 185.34.155.174 | grep -E 'mnt-by|origin'```  
```
mnt-by:         YARNET-MNT
origin:         AS60172
```

### 5 Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой ```traceroute```  

```$ traceroute -An 8.8.8.8```  
```
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.25.1 [*]  7.578 ms  7.513 ms  7.497 ms
 2  192.168.20.254 [*]  5.775 ms  7.448 ms  7.435 ms
 3  185.34.155.169 [AS60172]  7.435 ms  7.422 ms  7.409 ms
 4  188.43.29.182 [AS20485]  7.386 ms  7.372 ms  7.358 ms
 5  * * *
 6  188.43.30.42 [AS20485]  17.343 ms 188.43.29.2 [AS20485]  22.554 ms  21.552 ms
 7  188.43.245.41 [AS20485]  18.801 ms  22.419 ms  11.990 ms
 8  217.150.55.234 [AS20485]  23.300 ms *  18.506 ms
 9  188.43.10.141 [AS20485]  23.185 ms  8.003 ms  23.112 ms
10  108.170.250.146 [AS15169]  19.295 ms 108.170.250.113 [AS15169]  32.865 ms 108.170.250.34 [AS15169]  13.624 ms
11  142.251.238.82 [AS15169]  39.064 ms 172.253.66.116 [AS15169]  26.494 ms 142.251.237.154 [AS15169]  24.695 ms
12  142.251.237.142 [AS15169]  46.693 ms 142.251.238.68 [AS15169]  58.106 ms 142.251.238.66 [AS15169]  27.643 ms
13  74.125.253.147 [AS15169]  33.797 ms 216.239.56.101 [AS15169]  22.854 ms 172.253.51.243 [AS15169]  37.774 ms
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * 8.8.8.8 [AS15169/AS263411]  32.471 ms
```
AS60172, AS20485, AS15169

### 6 Повторите задание 5 в утилите ```mtr```. На каком участке наибольшая задержка - delay?  

```mtr 8.8.8.8 -znrc 1```
```
HOST: UG-SRV-UbintuSRV1           Loss%   Snt   Last   Avg  Best  Wrst StDev
  1. AS???    192.168.20.254       0.0%     1    0.8   0.8   0.8   0.8   0.0
  2. AS60172  185.34.155.169       0.0%     1    1.4   1.4   1.4   1.4   0.0
  3. AS20485  188.43.29.182        0.0%     1    2.2   2.2   2.2   2.2   0.0
  4. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
  5. AS20485  188.43.31.186        0.0%     1    5.2   5.2   5.2   5.2   0.0
  6. AS20485  188.43.30.129        0.0%     1    5.6   5.6   5.6   5.6   0.0
  7. AS20485  217.150.55.234       0.0%     1    5.5   5.5   5.5   5.5   0.0
  8. AS20485  188.43.10.141        0.0%     1    7.2   7.2   7.2   7.2   0.0
  9. AS15169  108.170.250.34       0.0%     1    6.8   6.8   6.8   6.8   0.0
 10. AS15169  172.253.66.116       0.0%     1   19.9  19.9  19.9  19.9   0.0
 11. AS15169  72.14.235.69         0.0%     1   21.2  21.2  21.2  21.2   0.0
 12. AS15169  172.253.51.247       0.0%     1   20.7  20.7  20.7  20.7   0.0
 13. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 14. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 15. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 16. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 17. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 18. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 19. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 20. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 21. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 22. AS15169  8.8.8.8              0.0%     1   26.5  26.5  26.5  26.5   0.0
```
Самая большая задержка на 11 хосте, АС AS15169 - 72.14.235.69 


### 7 Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? Воспользуйтесь утилитой ```dig```  
```$ dig +short NS dns.google```
```
ns3.zdns.google.
ns2.zdns.google.
ns4.zdns.google.
ns1.zdns.google.
```
```$ dig +short A dns.google```
```
8.8.8.8
8.8.4.4
```

### 8 Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой ```dig```  


```$ dig -x 8.8.4.4  +noall +answer```   
```4.4.8.8.in-addr.arpa.	86399	IN	PTR	dns.google.```  
```$ dig -x 8.8.8.8  +noall +answer```  
```8.8.8.8.in-addr.arpa.	36156	IN	PTR	dns.google.```


