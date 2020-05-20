Платформа: Windows
### Вопросы на 3
1. Создайте репозиторий на GitHub / GitLab по имени GitTraining
2. Создайте локальную версию репозитория в некоторой папке через git init
3. Создайте файл readme.md, с содержимым: “My first Repo”, добавьте его в git
    ```
    notepad readme.md
    git add readme.md
    git commit -m "Initializing repo with readme"
    ```
4. Присоедините remote с именем origin к адресу репозитория
    ```
    git remote add origin https://github.com/soft1q/GitTraining.git
    ```
5. Выполните команду “git push -u origin master” (-u необходимо, чтобы задать оригинальный - upstream repo)
6. Создайте папку GitTraining.git вне папки с проектом
7. Создайте удаленную (remote) версию репозитория через “git init --bare”
8. Сделайте “git remote add local <path to GitTraining.git>
9. Сделайте git push local master
10. Теперь мы умеем поднимать свой инстанс git

### Вопросы на 4
1. Создайте новую ветку my-new-branch
    ```
    git branch checkout -b my-new-branch 
    # Одновременно переключаемся на созданную ветку
    ```
2. Сделайте изменения в новой ветке - напишите hello world.
    ```
    notepad hello_world.py
    ```
3. Закоммитьте изменения и залейте на удаленный сервер (GitHub, GitLab).
    ```
    git add hello_world.py
    git commit -m "Add hello_world.py"
    git push -u origin my-new-branch
    ```
4. После этого создайте новую ветку из ветки master на удаленном сервере (GitHub, GitLab) - my-remote-branch, создайте в нем файл readmes/readme.md.
5. После этого выкачайте ветку my-remote-branch.
    ```
    git pull
    git checkout my-remote-branch
    # Просто git checkout в это пункте не сработал
    ```
6. Слейте ветку my-remote-branch в ветку my-new-branch.
    ```
    git checkout my-new-branch
    git merge my-remote-branch
    ```
7. Какой граф изменений получился?
    ```
    git log --graph --decorate --oneline
    *   dbeb696 (HEAD -> my-new-branch) Merge branch 'my-remote-branch' into my-new-branch
    |\
    | * 064ab1a (origin/my-remote-branch, my-remote-branch) Create readme.md
    * | 5ebfdad (origin/my-new-branch) Add hello_world.py
    |/
    * 8396427 (origin/master, local/master, master) Initialize repo with readme
    ```
    
### Вопросы на 5
1. Начните изменять свой код в репозитории (создайте в master A.cpp, закоммитьте его, переключитесь в ветку my-new-branch, создайте A.cpp, не коммитьте)
    ```
    git checkout master
    notepad A.cpp
    git add A.cpp
    git commit -m "Add A.cpp"
    git checkout my-new-branch
    notepad A.cpp
    ```
2. Попытайтесь переключиться на другую ветку. Какое сообщение вы получите?
    ```
    git checkout master
    error: The following untracked working tree files would be overwritten by checkout:
        A.cpp
    Please move or remove them before you switch branches.
    Aborting
    ```
3. Как можно решать эту проблему? Найдите хотя бы два решения этой проблемы
- Первое и самое очевидное - закоммитить A.cpp в ветку my-new-branch, чтобы изменения спокойно отслеживались
    ```
    git add A.cpp
    git commit -m "Add A.cpp"
    ```
- Если мы всё-таки не хотим, чтобы файл A.cpp в этой ветке отслеживался можно применить команду git stash, которая закинет его в хранилище
    ```
    git add A.cpp
    git stash
    ```
    Главное не забыть потом применить git stash apply чтобы достать этот файл из хранилища.
- Ещё два альтернативных способа, найденных на просторах интернета - удалить или переименовать A.cpp совсем, чтобы не было совпадений с файлами в ветке master, либо добавить его в .gitignore, что вызовет определённые трудности, так как в ветке master файл также перестанет отслеживаться.
4. Откройте папку в среде разработки (Pycharm, IDEA). Какие файлы появились при этом?
При открытии в PyCharm появилась папка .idea со следующим содержимым:
    ```
    inspectionProfiles
    .gitignore.txt
    GitTraining.iml
    misc.xml
    modules.xml
    vcs.xml
    ```
5. Добавьте эти файлы в git при помощи git add.
    ```
    git add .idea
    ```
6. Как можно удалить эти файлы из git, но не из файловой системы?
- git reset уберёт файлы из отслеживаемых git
- Другой способ: git rm --cached -r .idea/
7. Как сделать так, чтобы эти файлы не добавлялись в git автоматически при команде git add --all?
Добавить в текстовый файл .gitignore в репозитории строчку ".idea"

### Вопросы на 6
1. Сделайте в ветке my-new-branch следующие пять файлов: a.py, b.py, c.py, d.py, e.py - лучше через веб-интерфейс
2. Скачайте изменения локально
3. Выполните команду git rebase -i HEAD~5
4. Изменим названия коммитов “Create b.py и d.py”
Вместо pick пишем reword
5. Выполняем git rebase --continue.
6. Повторяем действие со вторым коммитом.
7. Выполнится ли git push origin my-new-branch?
Нет, не выполнится:
    ```
     ! [rejected]        my-new-branch -> my-new-branch (non-fast-forward)
    error: failed to push some refs to 'https://github.com/soft1q/GitTraining.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.
    ```
8. Разберитесь с опцией squash - как слить все коммиты в один?
Снова сделать git rebase -i HEAD~5 и указать всем коммитам кроме первого squash - тогда указанные коммиты сольются в один.

### Вопрос на 7
Создайте коммит с созданным python-файлом, закоммитьте в ветку master. Как запушить только этот коммит в local remote? Продемонстрируйте 
    
    git push local <commit_hash>:<branch_name>
    # В конце достаточно просто дать ссылку на конкретный коммит
    # Пример
    git push local HEAD


