## Задание 1. Создать и настроить репозиторий для дальнейшей работы на курсе

В рамках курса вы будете писать скрипты и создавать конфигурации для различных систем, которые необходимо сохранять для будущего использования. 
Сначала надо создать и настроить локальный репозиторий, после чего добавить удалённый репозиторий на GitHub.

### Создание репозитория и первого коммита

1. Зарегистрируйте аккаунт на [https://github.com/](https://github.com/). Если предпочитаете другое хранилище для репозитория, можно использовать его.  
   `Готово`
2. Создайте публичный репозиторий, который будете использовать дальше на протяжении всего курса, желательное с названием `devops-netology`.
   Обязательно поставьте галочку `Initialize this repository with a README`.  
   `Готово`
3. Создайте [авторизационный токен](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) для клонирования репозитория.  
   `Готово`
4. Склонируйте репозиторий, используя протокол HTTPS (`git clone ...`).  
   `git clone https://github.com/ap0ph2s/devops_netology.git`
5. Перейдите в каталог с клоном репозитория (`cd devops-netology`).  
`cd devops_netology`
6. Произведите первоначальную настройку Git, указав своё настоящее имя, чтобы нам было проще общаться, и email (`git config --global user.name` и `git config --global user.email johndoe@example.com`).  
`git config --global user.name "Tartyshev Alexander"`  
`git config --global user.email "ap0ph1s@yandex.ru"`  
7. Выполните команду `git status` и запомните результат.
```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

8. Отредактируйте файл `README.md` любым удобным способом, тем самым переведя файл в состояние `Modified`.  
` echo 'some text' >> README.md`
9. Ещё раз выполните `git status` и продолжайте проверять вывод этой команды после каждого следующего шага.  
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

10. Теперь посмотрите изменения в файле `README.md`, выполнив команды `git diff` и `git diff --staged`.  
```
diff --git a/README.md b/README.md
index 870ee8c..0450ec9 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-# devops_netology
\ No newline at end of file
+# devops_netologysome text
```

11. Переведите файл в состояние `staged` (или, как говорят, просто добавьте файл в коммит) командой `git add README.md`.  
12. И ещё раз выполните команды `git diff` и `git diff --staged`. Поиграйте с изменениями и этими командами, чтобы чётко понять, что и когда они отображают.  
```
diff --git a/README.md b/README.md
index 870ee8c..0450ec9 100644
--- a/README.md
+++ b/README.md
@@ -1 +1 @@
-# devops_netology
\ No newline at end of file
+# devops_netologysome text
```

13. Теперь можно сделать коммит `git commit -m 'First commit'`.

```
[main 57301c1] First commit
 1 file changed, 1 insertion(+), 1 deletion(-)
```

14. И ещё раз посмотреть выводы команд `git status`, `git diff` и `git diff --staged`.

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

### Создание файлов `.gitignore` и второго коммита

1. Создайте файл `.gitignore` (обратите внимание на точку в начале файла), проверьте его статус сразу после создания.  
`touch.gitignore`  
`git status`
```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

2. Добавьте файл `.gitignore` в следующий коммит (`git add...`).  
`git add .`  
`git status`
```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitignore
```

3. На одном из следующих блоков вы будете изучать `Terraform`, давайте сразу создадим соотвествующий каталог `terraform` и внутри этого каталога — файл `.gitignore` по примеру: https://github.com/github/gitignore/blob/master/Terraform.gitignore.  
 `cat Terraform/.gitignore`  
```
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log
crash.*.log

# Exclude all .tfvars files, which are likely to contain sensitive data, such as
# password, private keys, and other secrets. These should not be part of version
# control as they are data points which are potentially sensitive and subject
# to change depending on the environment.
*.tfvars
*.tfvars.json

# Ignore override files as they are usually used to override resources locally and so
# are not checked in
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Include override files you do wish to add to version control using negated pattern
# !example_override.tf

# Include tfplan files to ignore the plan output of command: terraform plan -out=tfplan
# example: *tfplan*

# Ignore CLI configuration files
.terraformrc
terraform.rc
```
4. В файле `README.md` опишите своими словами, какие файлы будут проигнорированы в будущем благодаря добавленному `.gitignore`.  
`Готово`  
5. Закоммитьте все новые и изменённые файлы. Комментарий к коммиту должен быть `Added gitignore`.  
`git status`
```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitignore

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Terraform/
```
`git add .`  
`git status`
```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitignore
        modified:   README.md
        new file:   Terraform/.gitignore
```
`git commit -m 'Added gitignore'`
```
[main cc58cf3] Added gitignore
 3 files changed, 45 insertions(+), 1 deletion(-)
 create mode 100644 .gitignore
 rewrite README.md (100%)
 create mode 100644 Terraform/.gitignore
```

### Эксперимент с удалением и перемещением файлов (третий и четвёртый коммит)

1. Создайте файлы `will_be_deleted.txt` (с текстом `will_be_deleted`) и `will_be_moved.txt` (с текстом `will_be_moved`) и закоммите их с комментарием `Prepare to delete and move`.  
`echo 'will_be_deleted' > will_be_deleted.txt`  
`echo 'will_be_moved' > will_be_moved.txt`  

`git commit -m 'Prepare to delete and move'`
```
[main 2fb8c0d] Prepare to delete and move
 2 files changed, 2 insertions(+)
 create mode 100644 will_be_deleted.txt
 create mode 100644 will_be_moved.txt
   ```
   
2. В случае необходимости обратитесь к [официальной документации](https://git-scm.com/book/ru/v2/Основы-Git-Запись-изменений-в-репозиторий) — здесь подробно описано, как выполнить следующие шаги. 
3. Удалите файл `will_be_deleted.txt` с диска и из репозитория.  
` git rm will_be_deleted.txt`
```
rm 'will_be_deleted.txt'
```

4. Переименуйте (переместите) файл `will_be_moved.txt` на диске и в репозитории, чтобы он стал называться `has_been_moved.txt`.
`git mv will_be_moved.txt has_been_moved.txt`  
5. Закоммитьте результат работы с комментарием `Moved and deleted`.  
`git add .`  
`git commit -m 'Moved and deleted'`
```
[main 2dd5f22] Moved and deleted
 2 files changed, 1 deletion(-)
 rename will_be_moved.txt => has_been_moved.txt (100%)
 delete mode 100644 will_be_deleted.txt
```

### Проверка изменения

1. В результате предыдущих шагов в репозитории должно быть как минимум пять коммитов (если вы сделали ещё промежуточные — нет проблем):
    * `Initial Commit` — созданный GitHub при инициализации репозитория. 
    * `First commit` — созданный после изменения файла `README.md`.
    * `Added gitignore` — после добавления `.gitignore`.
    * `Prepare to delete and move` — после добавления двух временных файлов.
    * `Moved and deleted` — после удаления и перемещения временных файлов.  

2. Проверьте это, используя комманду `git log`. Подробно о формате вывода этой команды мы поговорим на следующем занятии, но посмотреть, что она отображает, можно уже сейчас.  
`git log`
```
commit 2dd5f22a6bf7795885f87d2633cae944c5f14638 (HEAD -> main)
Author: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Wed Mar 1 17:49:42 2023 +0300

    Moved and deleted

commit 2fb8c0d5aa3d4c4c68d6ff0add10f05a91a77f27
Author: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Wed Mar 1 17:45:21 2023 +0300

    Prepare to delete and move

commit cc58cf322a8287a3d4ac63f6c32756d0d50e1cf1
Author: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Wed Mar 1 17:28:49 2023 +0300

    Added gitignore

commit 57301c1680899185250d2bae6a739317a406c3fc
Author: Tartyshev Alexander <ap0ph1s@yandex.ru>
Date:   Wed Mar 1 17:08:50 2023 +0300

    First commit

commit eefe05ba01c55bf501b7fac290fb8e708bf6c572 (origin/main, origin/HEAD)
Author: ap0ph2s <45497624+ap0ph2s@users.noreply.github.com>
Date:   Wed Mar 1 16:30:28 2023 +0300

    Initial commit
```

### Отправка изменений в репозиторий

Выполните команду `git push`, если Git запросит логин и пароль — введите ваши логин и пароль от GitHub. 

В качестве результата отправьте ссылку на репозиторий.  
https://github.com/ap0ph2s/devops_netology

## Задание 2. Знакомство с документаций

Один из основных навыков хорошего специалиста — уметь самостоятельно находить ответы на возникшие вопросы.  
Чтобы начать знакомиться с документацией, выполните в консоли команды `git --help`, `git add --help` и изучите их вывод.  

`git --help`
```
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone     Clone a repository into a new directory
   init      Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add       Add file contents to the index
   mv        Move or rename a file, a directory, or a symlink
   restore   Restore working tree files
   rm        Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect    Use binary search to find the commit that introduced a bug
   diff      Show changes between commits, commit and working tree, etc
   grep      Print lines matching a pattern
   log       Show commit logs
   show      Show various types of objects
   status    Show the working tree status

grow, mark and tweak your common history
   branch    List, create, or delete branches
   commit    Record changes to the repository
   merge     Join two or more development histories together
   rebase    Reapply commits on top of another base tip
   reset     Reset current HEAD to the specified state
   switch    Switch branches
   tag       Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch     Download objects and refs from another repository
   pull      Fetch from and integrate with another repository or a local branch
   push      Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.
```
 
----
