### 1 Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
  
![Screenshot 3](https://user-images.githubusercontent.com/45497624/218489940-810b2d4d-8aac-46ab-a9d1-37edd60cc0e6.png)

### 2 Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
  
![Screenshot 5](https://user-images.githubusercontent.com/45497624/218491949-adca0eac-5821-44e0-92ea-cdad5e200a2f.png)

### 3 Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
  
```sudo apt install apache2```
```sudo apt list --installed | grep 'apache2'```
```
apache2-bin/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed,automatic]
apache2-data/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 all [installed,automatic]
apache2-utils/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed,automatic]
apache2/jammy-updates,jammy-security,now 2.4.52-1ubuntu4.3 amd64 [installed]
```

### 4 Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).

### 5 Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

### 6 Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

### 7 Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
