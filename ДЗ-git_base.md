## Задание 1. Знакомимся с GitLab и Bitbucket 

Из-за сложности доступа к Bitbucket в работе достаточно использовать два репозитория: GitHub и GitLab.

Иногда при работе с Git-репозиториями надо настроить свой локальный репозиторий так, чтобы можно было 
отправлять и принимать изменения из нескольких удалённых репозиториев. 

Это может понадобиться при работе над проектом с открытым исходным кодом, если автор проекта не даёт права на запись в основной репозиторий.

Также некоторые распределённые команды используют такой принцип работы, когда каждый разработчик имеет свой репозиторий, а в основной репозиторий пушатся только конечные результаты 
работы над задачами. 

### GitLab

Создадим аккаунт в GitLab, если у вас его ещё нет:

1. GitLab. Для [регистрации](https://gitlab.com/users/sign_up)  можно использовать аккаунт Google, GitHub и другие. 
1. После регистрации или авторизации в GitLab создайте новый проект, нажав на ссылку `Create a projet`. 
Желательно назвать также, как и в GitHub — `devops-netology` и `visibility level`, выбрать `Public`.
1. Галочку `Initialize repository with a README` лучше не ставить, чтобы не пришлось разрешать конфликты.
1. Если вы зарегистрировались при помощи аккаунта в другой системе и не указали пароль, то увидите сообщение:
`You won't be able to pull or push project code via HTTPS until you set a password on your account`. 
Тогда перейдите [по ссылке](https://gitlab.com/profile/password/edit) из этого сообщения и задайте пароль. 
Если вы уже умеете пользоваться SSH-ключами, то воспользуйтесь этой возможностью (подробнее про SSH мы поговорим в следующем учебном блоке).
1. Перейдите на страницу созданного вами репозитория, URL будет примерно такой:
https://gitlab.com/YOUR_LOGIN/devops-netology. Изучите предлагаемые варианты для начала работы в репозитории в секции
`Command line instructions`. 
![image](https://user-images.githubusercontent.com/45497624/222344608-f52515ae-1cfa-4b46-af1e-862201267101.png)

1. Запомните вывод команды `git remote -v`.  
```
origin  https://github.com/ap0ph2s/homework (fetch)
origin  https://github.com/ap0ph2s/homework (push)
```
7. Из-за того, что это будет наш дополнительный репозиторий, ни один вариант из перечисленных в инструкции (на странице 
вновь созданного репозитория) нам не подходит. Поэтому добавляем этот репозиторий, как дополнительный `remote`, к созданному
репозиторию в рамках предыдущего домашнего задания: `git remote add gitlab https://gitlab.com/YOUR_LOGIN/devops-netology.git`.
1. Отправьте изменения в новый удалённый репозиторий `git push -u gitlab main`.
1. Обратите внимание, как изменился результат работы команды `git remote -v`.
```
gitlab  https://gitlab.com/ap0ph2s/devops_netology.git (fetch)
gitlab  https://gitlab.com/ap0ph2s/devops_netology.git (push)
origin  https://github.com/ap0ph2s/devops_netology.git (fetch)
origin  https://github.com/ap0ph2s/devops_netology.git (push)
```

#### Как изменить видимость репозитория в  GitLab — сделать его публичным 

* На верхней панели выберите «Меню» -> «Проекты» и найдите свой проект.
* На левой боковой панели выберите «Настройки» -> «Основные».
* Разверните раздел «Видимость» -> «Функции проекта» -> «Разрешения».
* Измените видимость проекта на Public.
* Нажмите «Сохранить изменения».

### Bitbucket* (задание со звёздочкой) 

Это самостоятельное задание, его выполнение необязательно.
____

Теперь необходимо проделать всё то же самое с [Bitbucket](https://bitbucket.org/). 

1. Обратите внимание, что репозиторий должен быть публичным — отключите галочку `private repository` при создании репозитория.
1. На вопрос `Include a README?` отвечайте отказом. 
1. В отличии от GitHub и GitLab в Bitbucket репозиторий должен принадлежать проекту, поэтому во время создания репозитория 
надо создать и проект, который можно назвать, например, `netology`.
1. Аналогично GitLab на странице вновь созданного проекта выберите `https`, чтобы получить ссылку, и добавьте этот репозиторий, как 
`git remote add bitbucket ...`.
1. Обратите внимание, как изменился результат работы команды `git remote -v`.

Если всё проделано правильно, то результат команды `git remote -v` должен быть следующий:

```bash
$ git remote -v
bitbucket https://andreyborue@bitbucket.org/andreyborue/devops-netology.git (fetch)
bitbucket https://andreyborue@bitbucket.org/andreyborue/devops-netology.git (push)
gitlab	  https://gitlab.com/andrey.borue/devops-netology.git (fetch)
gitlab	  https://gitlab.com/andrey.borue/devops-netology.git (push)
origin	  https://github.com/andrey-borue/devops-netology.git (fetch)
origin	  https://github.com/andrey-borue/devops-netology.git (push)
```

Дополнительно можете добавить удалённые репозитории по `ssh`, тогда результат будет примерно такой:

```bash
git remote -v
bitbucket	git@bitbucket.org:andreyborue/devops-netology.git (fetch)
bitbucket	git@bitbucket.org:andreyborue/devops-netology.git (push)
bitbucket-https	https://andreyborue@bitbucket.org/andreyborue/devops-netology.git (fetch)
bitbucket-https	https://andreyborue@bitbucket.org/andreyborue/devops-netology.git (push)
gitlab	git@gitlab.com:andrey.borue/devops-netology.git (fetch)
gitlab	git@gitlab.com:andrey.borue/devops-netology.git (push)
gitlab-https	https://gitlab.com/andrey.borue/devops-netology.git (fetch)
gitlab-https	https://gitlab.com/andrey.borue/devops-netology.git (push)
origin	git@github.com:andrey-borue/devops-netology.git (fetch)
origin	git@github.com:andrey-borue/devops-netology.git (push)
origin-https	https://github.com/andrey-borue/devops-netology.git (fetch)
origin-https	https://github.com/andrey-borue/devops-netology.git (push)
```

Выполните push локальной ветки `main` в новые репозитории. 

Подсказка: `git push -u gitlab main`. На этом этапе история коммитов во всех трёх репозиториях должна совпадать. 

## Задание 2. Теги

Представьте ситуацию, когда в коде была обнаружена ошибка — надо вернуться на предыдущую версию кода,
исправить её и выложить исправленный код в продакшн. Мы никуда не будем выкладывать код, но пометим некоторые коммиты тегами и создадим от них ветки. 

1. Создайте легковестный тег `v0.0` на HEAD-коммите и запуште его во все три добавленных на предыдущем этапе `upstream`.
`git lo`
```
2dd5f22 (HEAD -> main, origin/main, origin/HEAD, gitlab/main) Moved and deleted
2fb8c0d Prepare to delete and move
cc58cf3 Added gitignore
57301c1 First commit
eefe05b Initial commit
```
`git tag v0.0`  
`git lo`
```
2dd5f22 (HEAD -> main, tag: v0.0, origin/main, origin/HEAD, gitlab/main) Moved and deleted
2fb8c0d Prepare to delete and move
cc58cf3 Added gitignore
57301c1 First commit
eefe05b Initial commit
```
`git push gitlab --tags`
```
Username for 'https://gitlab.com': ap0ph2s
Password for 'https://ap0ph2s@gitlab.com':
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://gitlab.com/ap0ph2s/devops_netology.git
 * [new tag]         v0.0 -> v0.0
```

2. Аналогично создайте аннотированный тег `v0.1`.
`git tag -a v0.1 -m "test a.tag"`  
`git show v0.1`
```git
tag v0.1
Tagger: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Fri Mar 3 17:53:04 2023 +0300

test a.tag

commit 2dd5f22a6bf7795885f87d2633cae944c5f14638 (HEAD -> main, tag: v0.1, tag: v0.0, origin/main, origin/HEAD, gitlab/main)
Author: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Wed Mar 1 17:49:42 2023 +0300

    Moved and deleted

diff --git a/will_be_moved.txt b/has_been_moved.txt
similarity index 100%
rename from will_be_moved.txt
rename to has_been_moved.txt
diff --git a/will_be_deleted.txt b/will_be_deleted.txt
deleted file mode 100644
index ee10159..0000000
--- a/will_be_deleted.txt
+++ /dev/null
@@ -1 +0,0 @@
-will_be_deleted
```
`git push gitlab v0.1`  
`git push origin --tags`

3. Перейдите на страницу просмотра тегов в GitHab (и в других репозиториях) и посмотрите, чем отличаются созданные теги. 
    * в GitHub — https://github.com/YOUR_ACCOUNT/devops-netology/releases;
    * в GitLab — https://gitlab.com/YOUR_ACCOUNT/devops-netology/-/tags;
    * в Bitbucket — список тегов расположен в выпадающем меню веток на отдельной вкладке. 
![image](https://user-images.githubusercontent.com/45497624/222754461-fa7c1f17-fcb6-48e0-af8a-bc7c84d1bb92.png)
![image](https://user-images.githubusercontent.com/45497624/222754678-81dfb8b6-236f-4475-b35a-ca01abad1c25.png)

![image](https://user-images.githubusercontent.com/45497624/222755032-f09b7b58-1759-4d14-ba67-4f645388ac76.png)
![image](https://user-images.githubusercontent.com/45497624/222755162-03392a0a-d002-400b-bef1-b96e1c6f442c.png)

## Задание 3. Ветки 

Давайте посмотрим, как будет выглядеть история коммитов при создании веток. 

1. Переключитесь обратно на ветку `main`, которая должна быть связана с веткой `main` репозитория на `github`.
1. Посмотрите лог коммитов и найдите хеш коммита с названием `Prepare to delete and move`, который был создан в пределах предыдущего домашнего задания.  
`git log --oneline --decorate --graph --all`
```
* 2dd5f22 (HEAD -> main, tag: v0.1, tag: v0.0, origin/main, origin/HEAD, gitlab/main) Moved and deleted
* 2fb8c0d Prepare to delete and move
* cc58cf3 Added gitignore
* 57301c1 First commit
* eefe05b Initial commit
```

3. Выполните `git checkout` по хешу найденного коммита.  
`git checkout 2fb8c0d`  
`git log --oneline --decorate --graph --all`
```
* 2dd5f22 (tag: v0.1, tag: v0.0, origin/main, origin/HEAD, gitlab/main, main) Moved and deleted
* 2fb8c0d (HEAD) Prepare to delete and move
* cc58cf3 Added gitignore
* 57301c1 First commit
* eefe05b Initial commit
```

4. Создайте новую ветку `fix`, базируясь на этом коммите `git switch -c fix`.
```
Switched to a new branch 'fix'
```
5. Отправьте новую ветку в репозиторий на GitHub `git push -u origin fix`.
```
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'fix' on GitHub by visiting:
remote:      https://github.com/ap0ph2s/devops_netology/pull/new/fix
remote:
To https://github.com/ap0ph2s/devops_netology.git
 * [new branch]      fix -> fix
Branch 'fix' set up to track remote branch 'fix' from 'origin'.
```
6. Посмотрите, как визуально выглядит ваша схема коммитов: https://github.com/YOUR_ACCOUNT/devops-netology/network. 
![image](https://user-images.githubusercontent.com/45497624/222988864-f33d8bf5-3f0a-43fb-9681-5e237e15613a.png)

1. Теперь измените содержание файла `README.md`, добавив новую строчку.
`echo 'add new line' >> README.md`
1. Отправьте изменения в репозиторий и посмотрите, как изменится схема на странице https://github.com/YOUR_ACCOUNT/devops-netology/network 
и как изменится вывод команды `git log`.  
`git commit -a -m "add fix branch"`
```
[fix c9d40b3] add fix branch
 1 file changed, 1 insertion(+)
```
`git push`  
`git push gitlab`  
![image](https://user-images.githubusercontent.com/45497624/222989645-6cf59cfe-9101-46bb-8d9a-1d5af5758a00.png)  
![image](https://user-images.githubusercontent.com/45497624/222989870-5c6d8ad6-772f-4c15-b62d-62261c8bcf6c.png)  
`git log --oneline --decorate --graph --all`
```
* c9d40b3 (HEAD -> fix, origin/fix, gitlab/fix) add fix branch
| * 2dd5f22 (tag: v0.1, tag: v0.0, origin/main, origin/HEAD, gitlab/main, main) Moved and deleted
|/
* 2fb8c0d Prepare to delete and move
* cc58cf3 Added gitignore
* 57301c1 First commit
* eefe05b Initial commit
```

## Задание 4. Упрощаем себе жизнь

Попробуем поработь с Git при помощи визуального редактора. 

1. В используемой IDE PyCharm откройте визуальный редактор работы с Git, находящийся в меню View -> Tool Windows -> Git.
1. Измените какой-нибудь файл, и он сразу появится на вкладке `Local Changes`, отсюда можно выполнить коммит, нажав на кнопку внизу этого диалога. 
1. Элементы управления для работы с Git будут выглядеть примерно так:

   ![Работа с гитом](https://github.com/netology-code/sysadm-homeworks/blob/devsys10/02-git-02-base/img/ide-git-01.jpg)
   
1. Попробуйте выполнить пару коммитов, используя IDE. 

[По ссылке](https://www.jetbrains.com/help/pycharm/commit-and-push-changes.html) можно найти справочную информацию по визуальному интерфейсу. 

Если вверху экрана выбрать свою операционную систему, можно посмотреть горячие клавиши для работы с Git. 
Подробней о визуальном интерфейсе мы расскажем на одной из следующих лекций.

*В качестве результата работы по всем заданиям приложите ссылки на ваши репозитории в GitHub, GitLab и Bitbucket*.  
 
----
