### 1 **Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.**
  
![Screenshot 3](https://user-images.githubusercontent.com/45497624/218489940-810b2d4d-8aac-46ab-a9d1-37edd60cc0e6.png)

### 2 Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
  
![Screenshot 5](https://user-images.githubusercontent.com/45497624/218491949-adca0eac-5821-44e0-92ea-cdad5e200a2f.png)

### 3 Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
  
Установить ```apache2``` и включить ```mod_ssl``` для поддержка ssl-шифрования  
```sudo apt install apache2```  
```sudo a2enmod ssl```  
```sudo systemctl restart apache2```  
```sudo apt list --installed | grep 'apache2'```
```
apache2-bin/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed,automatic]
apache2-data/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 all [installed,automatic]
apache2-utils/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed,automatic]
apache2/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed]
```
Создать самоподписанный сертификат без защиты паролем, на 2,8 года, сгенерировать одновременно новый сертификат и новый ключ RSA длиной 2048 бит, указать где разместить создаваемый закрытый ключ и сам сертификат, указать информацию CSR в ключе
```sudo openssl req -x509 -nodes -days 1024 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt -subj "/C=RU/ST=KLG/L=Kaluga/O=KVT/OU=OIT/CN=UG-SRV-UbintuSRV1.lan.kvt.su"```  
Создать конфигурационный файл ```sudo nano /etc/apache2/sites-available/UG-SRV-UbintuSRV1.lan.kvt.su.conf``` с помощью ```moz://a SSL Configuration Generator```  
```
<VirtualHost *:443>
   ServerName UG-SRV-UbintuSRV1.lan.kvt.su
   DocumentRoot /var/www/UG-SRV-UbintuSRV1.lan.kvt.su

   SSLEngine on
   SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
   SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
    
   # enable HTTP/2, if available
   Protocols h2 http/1.1
</VirtualHost>

# intermediate configuration
SSLProtocol             all -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite          ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
SSLHonorCipherOrder     off
SSLSessionTickets       off

SSLUseStapling On
SSLStaplingCache "shmcb:logs/ssl_stapling(32768)"
```
Создать тестовую HTML страницу  
```sudo mkdir /var/www/UG-SRV-UbintuSRV1.lan.kvt.su```  
```sudo nano /var/www/UG-SRV-UbintuSRV1.lan.kvt.su/index.html```
```
<h1>it worked!</h1>
```
Активировать конфигурационный файл ```sudo a2ensite UG-SRV-UbintuSRV1.lan.kvt.su.conf```, ```sudo systemctl reload apache2``` и проверить на наличие ошибок ```sudo apache2ctl configtest```
```
Syntax OK
```
```sudo systemctl restart apache2```  
![Screenshot 6](https://user-images.githubusercontent.com/45497624/218987619-1df69fa0-e814-4a28-b114-0bb19c15b94a.png)


### 4 Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

Клонировать репозиторий git  
```git clone --depth 1 https://github.com/drwetter/testssl.sh.git```  
```cd testssl.sh```  
Запустить параллельное сканирование на уясвимости сайта https://ya.ru/ ```./testssl.sh -U --sneaky --parallel https://ya.ru/```
```
###########################################################
    testssl.sh       3.2rc2 from https://testssl.sh/dev/
    (e57527f 2023-02-08 17:07:42)

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-bad (1.0.2k-dev)" [~179 ciphers]
 on UG-SRV-UbintuSRV1:./bin/openssl.Linux.x86_64
 (built: "Sep  1 14:03:44 2022", platform: "linux-x86_64")


Testing all IPv4 addresses (port 443): 77.88.55.242 5.255.255.242
--------------------------------------------------------------------------------------------------------------------------------------------------------------
 Start 2023-02-15 13:48:22        -->> 77.88.55.242:443 (ya.ru) <<--

 Further IP addresses:   5.255.255.242 2a02:6b8::2:242
 rDNS (77.88.55.242):    ya.ru.
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK), reply empty
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    no gzip/deflate/compress/br HTTP compression (OK)  - only supplied "/" tested
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services, see
                                           https://search.censys.io/search?resource=hosts&virtual_hosts=INCLUDE&q=68C56160382AD96C62E4392A124209B291A40846890AB7832E23101BCA739A85
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-ECDSA-AES128-SHA ECDHE-RSA-AES128-SHA
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


 Done 2023-02-15 13:48:59 [  41s] -->> 77.88.55.242:443 (ya.ru) <<--

--------------------------------------------------------------------------------------------------------------------------------------------------------------
 Start 2023-02-15 13:49:00        -->> 5.255.255.242:443 (ya.ru) <<--

 Further IP addresses:   77.88.55.242 2a02:6b8::2:242
 rDNS (5.255.255.242):   ya.ru.
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    no gzip/deflate/compress/br HTTP compression (OK)  - only supplied "/" tested
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services, see
                                           https://search.censys.io/search?resource=hosts&virtual_hosts=INCLUDE&q=68C56160382AD96C62E4392A124209B291A40846890AB7832E23101BCA739A85
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-ECDSA-AES128-SHA ECDHE-RSA-AES128-SHA
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


 Done 2023-02-15 13:49:33 [  75s] -->> 5.255.255.242:443 (ya.ru) <<--

--------------------------------------------------------------------------------------------------------------------------------------------------------------
Done testing now all IP addresses (on port 443): 77.88.55.242 5.255.255.242

```
Посмотреть предпочтительный протоколы TLS (cipher suite) ```./testssl.sh -P https://ya.ru/```  
```
 Start 2023-02-15 13:44:39        -->> 5.255.255.242:443 (ya.ru) <<--

 Further IP addresses:   77.88.55.242 2a02:6b8::2:242
 rDNS (5.255.255.242):   ya.ru.
 Service detected:       HTTP


 Testing server's cipher preferences

Hexcode  Cipher Suite Name (OpenSSL)       KeyExch.   Encryption  Bits     Cipher Suite Name (IANA/RFC)
-----------------------------------------------------------------------------------------------------------------------------
SSLv2
 -
SSLv3
 -
TLSv1 (server order)
 xc009   ECDHE-ECDSA-AES128-SHA            ECDH 256   AES         128      TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
 xc013   ECDHE-RSA-AES128-SHA              ECDH 256   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
TLSv1.1 (server order)
 xc009   ECDHE-ECDSA-AES128-SHA            ECDH 256   AES         128      TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
 xc013   ECDHE-RSA-AES128-SHA              ECDH 256   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
TLSv1.2 (server order)
 xcca9   ECDHE-ECDSA-CHACHA20-POLY1305     ECDH 253   ChaCha20    256      TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256
 xcca8   ECDHE-RSA-CHACHA20-POLY1305       ECDH 253   ChaCha20    256      TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256
 xc02b   ECDHE-ECDSA-AES128-GCM-SHA256     ECDH 253   AESGCM      128      TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
 xc02f   ECDHE-RSA-AES128-GCM-SHA256       ECDH 253   AESGCM      128      TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
 xc0ae   ECDHE-ECDSA-AES128-CCM8           ECDH 253   AESCCM8     128      TLS_ECDHE_ECDSA_WITH_AES_128_CCM_8
 xc0ac   ECDHE-ECDSA-AES128-CCM            ECDH 253   AESCCM      128      TLS_ECDHE_ECDSA_WITH_AES_128_CCM
 xc023   ECDHE-ECDSA-AES128-SHA256         ECDH 253   AES         128      TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
 xc027   ECDHE-RSA-AES128-SHA256           ECDH 253   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
 xc009   ECDHE-ECDSA-AES128-SHA            ECDH 253   AES         128      TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
 xc013   ECDHE-RSA-AES128-SHA              ECDH 253   AES         128      TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
 xc02c   ECDHE-ECDSA-AES256-GCM-SHA384     ECDH 253   AESGCM      256      TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
 xc030   ECDHE-RSA-AES256-GCM-SHA384       ECDH 253   AESGCM      256      TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
 x9c     AES128-GCM-SHA256                 RSA        AESGCM      128      TLS_RSA_WITH_AES_128_GCM_SHA256
 xc0a0   AES128-CCM8                       RSA        AESCCM8     128      TLS_RSA_WITH_AES_128_CCM_8
 xc09c   AES128-CCM                        RSA        AESCCM      128      TLS_RSA_WITH_AES_128_CCM
 x3c     AES128-SHA256                     RSA        AES         128      TLS_RSA_WITH_AES_128_CBC_SHA256
TLSv1.3 (server order -- server prioritizes ChaCha ciphers when preferred by clients)
 x1302   TLS_AES_256_GCM_SHA384            ECDH 253   AESGCM      256      TLS_AES_256_GCM_SHA384
 x1303   TLS_CHACHA20_POLY1305_SHA256      ECDH 253   ChaCha20    256      TLS_CHACHA20_POLY1305_SHA256
 x1301   TLS_AES_128_GCM_SHA256            ECDH 253   AESGCM      128      TLS_AES_128_GCM_SHA256

 Has server cipher order?     yes (OK) -- TLS 1.3 and below



 Done 2023-02-15 13:45:01 [  45s] -->> 5.255.255.242:443 (ya.ru) <<--
```

### 5 Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

Генерация ключа SSH d ~>/.ssh/: id_rsa - приватный ключ, id_rsa.pub - публичный ключ
```ssh-keygen```
Скопировать публичный ключ на удаленный сервер и подключится по ключу ssh
```ssh-copy-id toor@192.168.20.9```
```ssh toor@192.168.20.9```
```
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-37-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Feb 15 03:33:21 PM MSK 2023

  System load:  0.0               Processes:             120
  Usage of /:   9.7% of 60.70GB   Users logged in:       1
  Memory usage: 54%               IPv4 address for eth0: 192.168.20.9
  Swap usage:   2%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

1 device has a firmware upgrade available.
Run `fwupdmgr get-upgrades` for more information.


 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro

6 updates can be applied immediately.
To see these additional updates run: apt list --upgradable


*** System restart required ***

1 device has a firmware upgrade available.
Run `fwupdmgr get-upgrades` for more information.

Last login: Wed Feb 15 15:19:30 2023 from 192.168.20.8
```
Можно отключить аутентификацию по паролю SSH ```sudo nano/etc/ssh/sshd_config```
```
PasswordAuthentication no
```


### 6 Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

Переименовать id_rsa в id_rsa_ ```mv ~/.ssh/id_rsa ~/.ssh/id_rsa_```
Создать файл конфигурации ```nano ~/.ssh/config```
```
Host test-server2
 HostName 192.168.20.9
 User tartyshev
 #Port 22
 IdentityFile ~/.ssh/id_rsa_
 Protocol 2
 ```
 Подключится на сервер по имени ``` ssh test-server2```
 ```
 Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-37-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Feb 15 04:03:56 PM MSK 2023
*     *     *
```

### 7 Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
