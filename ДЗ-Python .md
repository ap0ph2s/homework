## Задание 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:

| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Интерпретатор вернет ошибку - нельзя использоваьть переменные разных типов  |
| Как получить для переменной `c` значение 12?  | `c = str(a) + b ` |
| Как получить для переменной `c` значение 3?  | `c = a + int(b)` |

------

## Задание 2

Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. 

Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os

path = "~/git/test"
bash_command = [f"cd {path}", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()

for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print("Modified file(s): " + path + ":\t" + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
python3 test1.py
Modified file(s): ~/git/test:   1
```

------

## Задание 3

Доработать скрипт выше так, чтобы он не только мог проверять локальный репозиторий в текущей директории, но и умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os

path = "/home/tartyshev/git/test"
path1 = input ("Enter directory (default "+ path + "): ")
if os.path.isdir(path1) == True:
    path = path1
print ("Current directory is \"" + path +"\"" )
bash_command = [f"cd {path}", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified:') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print("Modified file(s): " + path + ":\t" + prepare_result)

```

### Вывод скрипта при запуске при тестировании:

`$ python3 test2.py`  
```
Enter directory (default /home/tartyshev/git/test): sdfsd
Current directory is "/home/tartyshev/git/test"
Modified file(s): /home/tartyshev/git/test:     1
```
`$ python3 test2.py`  
```
Enter directory (default /home/tartyshev/git/test): /home
Current directory is "/home"
fatal: not a git repository (or any of the parent directories): .git
```
`$ python3 test2.py`
```
Enter directory (default /home/tartyshev/git/test): /home/tartyshev/git/test2
Current directory is "/home/tartyshev/git/test2"
Modified file(s): /home/tartyshev/git/test2:    2 (modified content)
```
------

## Задание 4

Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. 

Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. 

Мы хотим написать скрипт, который: 
- опрашивает веб-сервисы, 
- получает их IP, 
- выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. 

Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python

#!/usr/bin/env python3

import socket
import time

serv_o = {'drive.google.com': 'ip1', 'mail.google.com': 'ip2', 'google.com': 'ip3'}

while True:
    for n in serv_o.keys():
        ip = socket.gethostbyname (n)
        if ip != serv_o[n]:
            print ('[ERROR] <'+n+'> IP mismatch: <'+serv_o[n]+'> <'+ip+'>')
        serv_o[n] = ip
    time.sleep(10)
    print (serv_o)

```

### Вывод скрипта при запуске при тестировании:

`$ python3 test3.py`
```
[ERROR] <drive.google.com> IP mismatch: <ip1> <64.233.165.194>
[ERROR] <mail.google.com> IP mismatch: <ip2> <142.250.150.18>
[ERROR] <google.com> IP mismatch: <ip3> <142.251.1.138>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.18', 'google.com': '142.251.1.138'}
[ERROR] <google.com> IP mismatch: <142.251.1.138> <142.251.1.101>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.18', 'google.com': '142.251.1.101'}
[ERROR] <google.com> IP mismatch: <142.251.1.101> <142.251.1.138>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.18', 'google.com': '142.251.1.138'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.18> <142.250.150.19>
[ERROR] <google.com> IP mismatch: <142.251.1.138> <142.251.1.101>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.19', 'google.com': '142.251.1.101'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.19> <142.250.150.17>
[ERROR] <google.com> IP mismatch: <142.251.1.101> <142.251.1.139>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.17', 'google.com': '142.251.1.139'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.17> <142.250.150.83>
[ERROR] <google.com> IP mismatch: <142.251.1.139> <142.251.1.101>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.83', 'google.com': '142.251.1.101'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.83> <142.250.150.18>
[ERROR] <google.com> IP mismatch: <142.251.1.101> <142.251.1.100>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.18', 'google.com': '142.251.1.100'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.18> <142.250.150.17>
[ERROR] <google.com> IP mismatch: <142.251.1.100> <142.251.1.139>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.17', 'google.com': '142.251.1.139'}
[ERROR] <google.com> IP mismatch: <142.251.1.139> <142.251.1.101>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.17', 'google.com': '142.251.1.101'}
[ERROR] <google.com> IP mismatch: <142.251.1.101> <142.251.1.100>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.17', 'google.com': '142.251.1.100'}
[ERROR] <mail.google.com> IP mismatch: <142.250.150.17> <142.250.150.19>
[ERROR] <google.com> IP mismatch: <142.251.1.100> <142.251.1.113>
{'drive.google.com': '64.233.165.194', 'mail.google.com': '142.250.150.19', 'google.com': '142.251.1.113'}
```
