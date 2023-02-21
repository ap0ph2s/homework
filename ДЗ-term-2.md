### 1 Какого типа команда ```cd```? Попробуйте объяснить, почему она именно такого типа: опишите ход своих мыслей, если считаете, что она могла бы быть другого типа.
---
Встроенная в оболочку команда ```type -t cd```
```
builtin
```
---

### 2 Какая альтернатива без pipe команде ```grep <some_string> <some_file> | wc -l```?
---
Либо
```
grep <some_string> <some_file> > <temp_file>
wc -l <temp_file>
```
либо
```grep  <some_string> <some_file> -c```

---

### 3 Какой процесс с PID ```1``` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
---
Процесс systemd, ``` ps -p 1```
```
    PID TTY          TIME CMD
      1 ?        00:00:12 systemd
```
---

### 4 Как будет выглядеть команда, которая перенаправит вывод stderr ```ls``` на другую сессию терминала?
---
Отправить c 0 терминала на 1 терминал ```ls some 2> /dev/pts/1```

---

### 5 Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
---
```cat input```
```
one
two
three
```

```wc -l < input > output```  
```cat output```
```
3
```

---

### 6 Получится ли, находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
---
В консоли ВМ (эмулятор TTY) ```tty```
```
/dev/pts/0
```
В псевдотерминале SSH ```echo 'to TTY' > /dev/tty1```  
В консоли ВМ
```
to TTY
```
---

### 7 Выполните команду ```bash 5>&1```. К чему она приведет? Что будет, если вы выполните ```echo netology > /proc/$$/fd/5```? Почему так происходит?
---
```bash 5>&1``` создаёт для процесса bash новый fd 5 и перенаправит его на stdout  
```echo netology > /proc/$$/fd/5``` команда echo отправляет "netology" в fd 5 текущего шелла ($$), который перенаправлен на stdout предыдущей командой, поэтому на акране мы видим "netology"

---

### 8 Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty?
Напоминаем: по умолчанию через pipe передается только stdout команды слева от ```|``` на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

---
|Изначально|``` 5>&1```|```1>&2```|```2>&5```|
|:-|:-|:-|:-|
|0 --> /dev/tty0 <br /> 1 --> /dev/tty1 <br /> 2 --> /dev/tty2|0 --> /dev/tty0 <br /> 1 --> /dev/tty1 <br /> 2 --> /dev/tty2 <br /> 5 --> /dev/tty1|0 --> /dev/tty0 <br /> 1 --> /dev/tty2 <br /> 2 --> /dev/tty2 <br /> 5 --> /dev/tty1|0 --> /dev/tty0 <br /> 1 --> /dev/tty2 <br /> 2 --> /dev/tty1 <br /> 5 --> /dev/tty1  

```cat file1```
```
testn;fnf
test
```
```cat file2```
```
cat: file2: No such file or directory
```
```cat file1 file2 5>&1 1>&2 2>&5 | grep No```
```
testn;fnf
test
cat: file2: No such file or directory
```

---

### 9 Что выведет команда ```cat /proc/$$/environ```? Как еще можно получить аналогичный по содержанию вывод?
---
```cat /proc/$$/environ``` выводит все переменные окружения.
Похожий по сути вывод имеет ```env```

---

### 10 Используя ```man```, опишите что доступно по адресам ```/proc/<PID>/cmdline```, ```/proc/<PID>/exe```.
---
```
       /proc/[pid]/cmdline
              This read-only file holds the complete command line for the process, unless the process is a zombie.  In the latter case, there is nothing in this file: that is, a read on this file will return 0 characters.  The com‐
              mand-line arguments appear in this file as a set of strings separated by null bytes ('\0'), with a further null byte after the last string.

              If, after an execve(2), the process modifies its argv strings, those changes will show up here.  This is not the same thing as modifying the argv array.

              Furthermore, a process may change the memory location that this file refers via prctl(2) operations such as PR_SET_MM_ARG_START.

              Think of this file as the command line that the process wants you to see.
```
**cmdline** - содержит полную командную строку для процесса (с параметрами)  
```
/proc/[pid]/exe
              Under  Linux  2.2  and later, this file is a symbolic link containing the actual pathname of the executed command.  This symbolic link can be dereferenced normally; attempting to open it will open the executable.  You
              can even type /proc/[pid]/exe to run another copy of the same executable that is being run by process [pid].  If the pathname has been unlinked, the symbolic link will contain the string '(deleted)'  appended  to  the
              original pathname.  In a multithreaded process, the contents of this symbolic link are not available if the main thread has already terminated (typically by calling pthread_exit(3)).

              Permission to dereference or read (readlink(2)) this symbolic link is governed by a ptrace access mode PTRACE_MODE_READ_FSCREDS check; see ptrace(2).

              Under Linux 2.0 and earlier, /proc/[pid]/exe is a pointer to the binary which was executed, and appears as a symbolic link.  A readlink(2) call on this file under Linux 2.0 returns a string in the format:

                  [device]:inode

              For example, [0301]:1502 would be inode 1502 on device major 03 (IDE, MFM, etc. drives) minor 01 (first partition on the first drive).

              find(1) with the -inum option can be used to locate the file.
```
**exe** - символическая ссылка, содержащая фактическое имя пути выполняемой команды.

---

### 11 Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью ```/proc/cpuinfo```.
---
sse4_2

---

### 12 При открытии нового окна терминала и ```vagrant ssh``` создается новая сессия и выделяется pty.
Это можно подтвердить командой ```tty```, которая упоминалась в лекции 3.2.
Однако:
```
vagrant@netology1:~$ ssh localhost 'tty'
not a tty
```
Почитайте, почему так происходит, и как изменить поведение.

---

---

### 13 Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись ```reptyr```. Например, так можно перенести в ```screen``` процесс, который вы запустили по ошибке в обычной SSH-сессии.
---
```sudo apt-get install reptyr```  
```sudo nano /etc/sysctl.d/10-ptrace.conf```
```
kernel.yama.ptrace_scope = 0
```
``` top &```
```
[1] 1252
```
```jobs -l```
```
[1]+  1260 Stopped (signal)        top
```
```ps -a```
```
    PID TTY          TIME CMD
   1252 pts/1    00:00:00 top
   1253 pts/1    00:00:00 ps
```
```disown htop```
```
-bash: warning: deleting stopped job 1 with process group 1260
```
  
```reptyr 1252```


---

### 14 ```sudo echo string > /root/new_file``` не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию ```echo string | sudo tee /root/new_file```. Узнайте? что делает команда ```tee``` и почему в отличие от ```sudo echo``` команда с ```sudo tee``` будет работать.
---
---
