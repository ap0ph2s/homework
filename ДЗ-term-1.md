### 1 С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

* Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните ```vagrant init```. Замените содержимое Vagrantfile по умолчанию следующим:
```
 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
 end
 ```
* Выполнение в этой директории ```vagrant up``` установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

* ```vagrant suspend``` выключит виртуальную машину с сохранением ее состояния (т.е., при следующем ```vagrant up``` будут запущены все процессы внутри, которые работали на момент вызова suspend), ```vagrant halt``` выключит виртуальную машину штатным образом.
---
```sudo apt update```  
```sudo apt install virtualbox```  
```sudo apt install /home/dashat/vagrant_2.3.4-1_amd64.deb```  
```sudo mkdir -p /etc/vagrant```  
```cd /etc/vagrant```  
```vagrant init```  
```cp Vagrantfile Vagrantfile_```  
```sudo nano Vagrantfile```  
```
 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
 end
```
```vagrant up```
```
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'bento/ubuntu-20.04'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'bento/ubuntu-20.04' version '202112.19.0' is up to date...
==> default: A newer version of the box 'bento/ubuntu-20.04' for provider 'virtualbox' is
==> default: available! You currently have version '202112.19.0'. The latest is version
==> default: '202212.11.0'. Run `vagrant box update` to update.
==> default: Setting the name of the VM: 1_default_1676647663715_42227
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Mounting shared folders...
    default: /vagrant => /etc/vagrant/
```
---
### 2 Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
---
RAM: 1024Mb, CPU: 2, vHDD: 64Gb video: 4Мb
---
### 3 Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
---
```vagrant halt```
```
==> default: Attempting graceful shutdown of VM...
```
```sudo nano Vagrantfile```  
```
 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
	config.vm.provider "virtualbox" do |v|
		v.memory=1025
		v.cpus=3
	end
 end
```
```vagrant up```  

---
### 4 Команда ```vagrant ssh``` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
---
Попрактиковался
---
### 5 Ознакомьтесь с разделами ```man bash```, почитайте о настройках самого bash:
* какой переменной можно задать длину журнала ```history```, и на какой строчке manual это описывается?
* что делает директива ```ignoreboth``` в bash?
---
* HISTFILE и HISTFILESIZE
```
    600        HISTFILESIZE
    601               The maximum number of lines contained in the history file.  When this variable is assigned a value, the history file is truncated, if necessary, to contain no more than that number of lines by removing the oldest  e
    601 n‐
    602               tries.   The  history  file is also truncated to this size after writing it when a shell exits.  If the value is 0, the history file is truncated to zero size.  Non-numeric values and numeric values less than zero i
    602 n‐
    603               hibit truncation.  The shell sets the default value to the value of HISTSIZE after reading any startup files.

    609        HISTSIZE
    610               The  number of commands to remember in the command history (see HISTORY below).  If the value is 0, commands are not saved in the history list.  Numeric values less than zero result in every command being saved on t
    610 he
    611               history list (there is no limit).  The shell sets the default value to 500 after reading any startup files.

```
* Значение ```ignoreboth``` является сокращением для ```ignorespace``` (строки, начинающиеся с символа пробела, не сохраняются в истории) и ```ignoredups``` (строки, совпадающие с предыдущей записью истории, не сохраняются)
```
 HISTCONTROL
              A colon-separated list of values controlling how commands are saved on the history list.  If the list of values includes ignorespace, lines which begin with a space char acter  are not saved in the history list.  A value of ignoredups causes lines matching the previous history entry to not be saved.  A value of ignoreboth is shorthand for ignorespace and ignoredups.  A value of erasedups causes all previous lines matching the current line to be removed from the history list before that line is saved. Any value  not  in  the  above list is ignored.  If HISTCONTROL is unset, or does not include a valid value, all lines read by the shell parser are saved on the history list, subject to the value of HISTIGNORE.  The second and subsequent lines of a multi-line compound command are not tested, and are added to the history regardless of the value of HISTCONTROL.
```
---
### 6 В каких сценариях использования применимы скобки ```{}``` и на какой строчке ```man bash``` это описано?
---
```{}``` использовать, когда в аргумент функции удобно описать массивом, например ```mkdir dir_{1..10}```
```
764:   Brace Expansion
765-       Brace expansion is a mechanism by which arbitrary strings may be generated.  This mechanism is similar to pathname expansion, but the filenames generated need not exist.  Patterns to be brace expanded take the form of an op‐
766-       tional  preamble,  followed  by  either a series of comma-separated strings or a sequence expression between a pair of braces, followed by an optional postscript.  The preamble is prefixed to each string contained within the
767-       braces, and the postscript is then appended to each resulting string, expanding left to right.
768-
769-       Brace expansions may be nested.  The results of each expanded string are not sorted; left to right order is preserved.  For example, a{d,c,b}e expands into `ade ace abe'.
770-
771-       A sequence expression takes the form {x..y[..incr]}, where x and y are either integers or single characters, and incr, an optional increment, is an integer.  When integers are supplied, the expression expands to each  number
772-       between  x  and  y,  inclusive.  Supplied integers may be prefixed with 0 to force each term to have the same width.  When either x or y begins with a zero, the shell attempts to force all generated terms to contain the same
773-       number of digits, zero-padding where necessary.  When characters are supplied, the expression expands to each character lexicographically between x and y, inclusive, using the default C locale.  Note that both x and  y  must
774-       be of the same type.  When the increment is supplied, it is used as the difference between each term.  The default increment is 1 or -1 as appropriate.
--
2059:              Perform filename completion and insert the list of possible completions enclosed within braces so the list is available to the shell (see Brace Expansion above).
```

---
### 7 С учётом ответа на предыдущий вопрос, как создать однократным вызовом ```touch``` 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
---
100000 файлок одной командой создать можно, ```touch file{1..100000}```
Верхний предел количества аргумента в команде ```getconf ARG_MAX``` - 2097152
---
### 8 В man bash поищите по ```/\[\[```. Что делает конструкция ```[[ -d /tmp ]]```
---

---
### 9 Сделайте так, чтобы в выводе команды ```type -a bash``` первым стояла запись с нестандартным путем, например bash is ... Используйте знания о просмотре существующих и создании новых переменных окружения, обратите внимание на переменную окружения PATH
```
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
```
(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

### 10 Чем отличается планирование команд с помощью ```batch``` и ```at```?

### 11 Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

