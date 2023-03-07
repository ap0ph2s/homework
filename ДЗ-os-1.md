### 1. Какой системный вызов делает команда `cd`? 

В прошлом ДЗ вы выяснили, что `cd` не является самостоятельной  программой. Это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае увидите полный список системных вызовов, которые делает сам `bash` при старте. 

Вам нужно найти тот единственный, который относится именно к `cd`. Обратите внимание, что `strace` выдаёт результат своей работы в поток stderr, а не в stdout.

----
`strace /bin/bash -c 'cd /tmp'`
```
chdir("/tmp")                           = 0
```
syscalls: chdir() изменяет текущий рабочий каталог вызвавшего процесса на каталог, указанный в path.

----

### 2. Попробуйте использовать команду `file` на объекты разных типов в файловой системе. Например:

```bash
vagrant@netology1:~$ file /dev/tty
/dev/tty: character special (5/0)
vagrant@netology1:~$ file /dev/sda
/dev/sda: block special (8/0)
vagrant@netology1:~$ file /bin/bash
/bin/bash: ELF 64-bit LSB shared object, x86-64
```
    
Используя `strace`, выясните, где находится база данных `file`, на основании которой она делает свои догадки.

----
```
newfstatat(AT_FDCWD, "/home/tartyshev/.magic.mgc", 0x7fff08c26db0, 0) = -1 ENOENT (No such file or directory)
newfstatat(AT_FDCWD, "/home/tartyshev/.magic", 0x7fff08c26db0, 0) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (No such file or directory)
newfstatat(AT_FDCWD, "/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}, 0) = 0
openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=111, ...}, AT_EMPTY_PATH) = 0
read(3, "# Magic local data for file(1) c"..., 4096) = 111
read(3, "", 4096)                       = 0
close(3)                                = 0
openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=7248904, ...}, AT_EMPTY_PATH) = 0
```
База данных читается из файла `/usr/share/misc/magic.mgc` с ключом readonly

----

### 3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удалён (deleted в lsof), но сказать сигналом приложению переоткрыть файлы или просто перезапустить приложение возможности нет. Так как приложение продолжает писать в удалённый файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков, предложите способ обнуления открытого удалённого файла, чтобы освободить место на файловой системе.

----
Знать куда продолжает писать программа `sudo lsof -c ProgName`
И с прпвами пользователя root очистить файл (дескриптор) `echo '' > /proc/PID/fd/1`.

----

### 4. Занимают ли зомби-процессы ресурсы в ОС (CPU, RAM, IO)?

----
Русурсов не занимает. Занимает только строку в списке процессов ядра.

----

### 5. В IO Visor BCC есть утилита `opensnoop`:

```bash
root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
```
   
На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные сведения по установке [по ссылке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).

----
```
PID    COMM               FD ERR PATH
1      systemd            36   0 /proc/686/cgroup
716    irqbalance          6   0 /proc/interrupts
716    irqbalance          6   0 /proc/stat
716    irqbalance          6   0 /proc/irq/8/smp_affinity
716    irqbalance          6   0 /proc/irq/9/smp_affinity
1      systemd            36   0 /proc/390/cgroup
1      systemd            36   0 /proc/445/cgroup
716    irqbalance          6   0 /proc/interrupts
716    irqbalance          6   0 /proc/stat
716    irqbalance          6   0 /proc/irq/8/smp_affinity
716    irqbalance          6   0 /proc/irq/9/smp_affinity
```
----

### 6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc` и где можно узнать версию ядра и релиз ОС.

----
`strace uname -a`
```
...
uname({sysname="Linux", nodename="UG-SRV-UbintuSRV1", ...}) = 0
...
```
```
       Part of the utsname information is also accessible  via  /proc/sys/ker‐
       nel/{ostype, hostname, osrelease, version, domainname}.
```
----

### 7. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:

```bash
root@netology1:~# test -d /tmp/some_dir; echo Hi
Hi
root@netology1:~# test -d /tmp/some_dir && echo Hi
root@netology1:~#
```
Есть ли смысл использовать в bash `&&`, если применить `set -e`?

----
`;` - вторая команда, идущая после `;`, выполнятеся вне зависимости от результата выполнения первой команды
`&&` - вторая команда выполнится только, в случае успешного выполнения первой команды (0).

```
 -e  Exit immediately if a simple command exits with a non-zero status, unless
       the command that fails is part of an until or  while loop, part of an
       if statement, part of a && or || list, or if the command's return status
       is being inverted using !.  -o errexit
```
Параметр -e указывает оболочке выйти, если команда дает ненулевой статус выхода (сбой), за исключением нескольких случаев, в том числе использования '&&'. Поэтому смысл использовать в `&&` есть.

----

### 8. Из каких опций состоит режим bash `set -euxo pipefail`, и почему его хорошо было бы использовать в сценариях?

----
`-e` - завершить работу, если простая команда завершается с ненулевым статусом
```
       Exit immediately if a simple command exits with a non-zero status, unless
       the command that fails is part of an until or  while loop, part of an
       if statement, part of a && or || list, or if the command's return status
       is being inverted using !
```
`-u` - считать незаданные переменные как ошибку, записывать сообщение об ошибке в stderr и завершить неинтерактивную оболочку
```
       Treat unset variables as an error when performing 
       parameter expansion. An error message will be written 
       to the standard error, and a non-interactive shell will exit.
```
`-x` - вывести трассировки простых команд после их расширения и до их выполнения
```
       Print a trace of simple commands and their arguments
       after they are expanded and before they are executed.
```
`-o pipefail` - выводить значения текущих опций, pipefail - возвращает значение пайп статус последней команды, завершившейся с ненулевым статусом, или ноль, если ни одна команда не завершилась с ненулевым статусом.
```
       If -o is supplied with no option-name, the values of the current options are printed.
       If +o is supplied with no option-name, a series of set commands to recreate  the current
       option settings is displayed on the standard output.
       
        -o option-name
          Set the option corresponding to 'option-name'
          The 'option-names' are listed above and below (in bold):

                 pipefail    If set, the return value of a pipeline is the value of the last (rightmost) command to 
                             exit with a non-zero status, or zero if all commands in the pipeline exit successfully.
                             By default, pipelines only return a failure if the last command errors.

                             When used in combination with set -e, pipefail will make a script exit if any command
                             in a pipeline errors.
       
       
```
Удобно отладкой и поиском ошибок.

----

### 9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` изучите (`/PROCESS STATE CODES`), что значат дополнительные к основной заглавной букве статуса процессов. Его можно не учитывать при расчёте (считать S, Ss или Ssl равнозначными).

----
`ps axo stat | sort | uniq -c | sort -h`
 ```
      1 R+
      1 Ss+
      1 S<sl
      1 STAT
      2 SN
      3 D+
      7 Ssl
      8 I
     16 Ss
     52 I<
    135 S
```
Наиболее часто встречающийся статус S — спящий процесс.
```
               <    high-priority (not nice to other users)
               N    low-priority (nice to other users)
               L    has pages locked into memory (for real-time and custom IO)
               s    is a session leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
               +    is in the foreground process group
```
< — высоко-приоритетный;  
N — низко-приоритетный;  
L — имеет страницы, заблокированные в памяти.
