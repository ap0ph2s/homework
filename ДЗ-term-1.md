### 1 С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

* Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните ```vagrant init```. Замените содержимое Vagrantfile по умолчанию следующим:
```
 Vagrant.configure("2") do |config|
 	config.vm.box = "bento/ubuntu-20.04"
 end
 ```
* Выполнение в этой директории ```vagrant up``` установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

* ```vagrant suspend``` выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.
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
---
### 3 Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

### 4 Команда ```vagrant ssh``` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.

### 5 Ознакомьтесь с разделами ```man bash```, почитайте о настройках самого bash:

* какой переменной можно задать длину журнала ```history```, и на какой строчке manual это описывается?
* что делает директива ```ignoreboth``` в bash?
### 6 В каких сценариях использования применимы скобки ```{}``` и на какой строчке ```man bash``` это описано?

### 7 С учётом ответа на предыдущий вопрос, как создать однократным вызовом ```touch``` 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

### 8 В man bash поищите по ```/\[\[```. Что делает конструкция ```[[ -d /tmp ]]```

### 9 Сделайте так, чтобы в выводе команды ```type -a bash``` первым стояла запись с нестандартным путем, например bash is ... Используйте знания о просмотре существующих и создании новых переменных окружения, обратите внимание на переменную окружения PATH
```
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
```
(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

### 10 Чем отличается планирование команд с помощью ```batch``` и ```at```?

### 11 Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

