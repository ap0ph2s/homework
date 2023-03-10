## Задание 1

Мы выгрузили JSON, который получили через API запрос к нашему сервису:

```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

### Ваш скрипт:
```json
{
  "info": "Sample JSON output from our service\\t",
  "elements": [
    {
      "name": "first",
      "type": "server",
      "ip": "7175"
    },
    {
      "name": "second",
      "type": "proxy",
      "ip": "71.78.22.43"
    }
  ]
}
```

---

## Задание 2

В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python

#!/usr/bin/env python3

import socket
import time
import json
import yaml

serv = {
    "drive.google.com": "ip1",
    "mail.google.com": "ip2",
    "google.com": "ip3"
    }

while True:
    for n in serv.keys():
        ip = socket.gethostbyname (n)
        if ip != serv[n]:
            print ('[ERROR] <'+n+'> IP mismatch: <'+serv[n]+'> <'+ip+'>')
        serv[n] = ip
    print (serv)
    with open ("py2json.json", "w") as j:
        json.dump(serv, j, sort_keys=True, indent=4)
    with open('py2yaml.yaml', 'w') as y:
        yaml.dump(serv, y)
    time.sleep(10)

```

### Вывод скрипта при запуске при тестировании:

`$ python3 JSON+YAML.py`  
```
[ERROR] <drive.google.com> IP mismatch: <ip1> <64.233.165.194>
[ERROR] <mail.google.com> IP mismatch: <ip2> <142.250.150.83>
[ERROR] <google.com> IP mismatch: <ip3> <142.251.1.138>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.83', 'google.com': '142.251.1.138'}
[ERROR] <google.com> IP mismatch: <142.251.1.138> <142.251.1.101>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.83', 'google.com': '142.251.1.101'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.83> <142.250.150.17>
[ERROR] <google.com> IP mismatch: <142.251.1.101> <142.251.1.102>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.17', 'google.com': '142.251.1.102'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.17> <142.250.150.19>
[ERROR] <google.com> IP mismatch: <142.251.1.102> <142.251.1.100>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.19', 'google.com': '142.251.1.100'}
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{
    "drive.google.com": "64.233.165.194",
    "google.com": "142.251.1.139",
    "mail.google.com": "142.250.150.83"
}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
drive.google.com: 64.233.165.194
google.com: 142.251.1.139
mail.google.com: 142.250.150.83
```
