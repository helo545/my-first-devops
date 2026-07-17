# my-first-devops

Здесь описана:
• Создание виртуальной машины (серверная версия Ubuntu), подклчение к ней по SSH;
•	Установка Git и создание аккаунта GitHub;
•	Создание репозитория и файла README.md;
•	Установка Docker и запуск контейнеров.


# My First DevOps Project


ЭТАП 1: СОЗДАНИЕ ВИРТУАЛЬНОЙ МАШИНЫ И ПОДКЛЮЧЕНИЕ ПО SSH

Первый этап - установка программы Oracle VirtualBox для создания виртуальной машины, затем необходимо скачать образ операционной системы Ubuntu Server 22.04 LTS с официального сайта https://ubuntu.com/download/server. 

В VirtualBox нажать кнопку "Создать" и заполнить параметры:
- Имя: Ubuntu
- Тип: Linux
- Версия: Ubuntu (64-bit)
- Оперативная память: 2048 МБ
- Жесткий диск: 35 ГБ, динамический

В настройках виртуальной машины:
- Система -> Процессор: выделил 2 CPU
- Сеть: тип подключения "Сетевой мост" (Bridge Adapter)
- Носители: выбрал скачанный ISO-образ Ubuntu Server

Запуск виртуальной машины и установка Ubuntu Server:
1. Выбор языка  
2. Выбор раскладки клавиатуры
3. Тип установки: Ubuntu Server
4. Настройка сети (DHCP)
5. Создание пользователя:
   - Имя: administrator
   - Имя сервера: ubuntu
   - Установка пароля
7. ВАЖНО: поставить галочку "Install OpenSSH server"
8. Дождаться установки и нажать "Reboot Now"

После перезагрузки войти в систему и узнать IP-адрес командой:

ip a

В выводе найти строку вида "inet 192.168.1.50/24" - это IP-адрес виртуальной машины.

На основном компьютере (Windows 10) необходимо открыть MobaXterm и подключиться по SSH (введя ip-адрес):

При первом подключении система спросит о доверии к хосту:
"The authenticity of host '192.168.1.50' can't be established.
Are you sure you want to continue connecting (yes/no)?"

Ввод "yes" и нажать Enter, затем ввод пароля. После успешного входа произошло приглашение командной строки:

devops@ubuntu-devops:~$

ЭТАП 2: УСТАНОВКА GIT И СОЗДАНИЕ АККАУНТА GITHUB

На своем компьютере с Windows 10 перейти на официальный сайт Git https://git-scm.com/download/win и скачать установщик Git-2.43.0-64-bit.exe.
 
Ввести команду в Powershell:

winget install --id Git.Git -e --source winget

Проверить версию Git:

git --version

Получение ответа:
git version 2.43.0.windows.1

Настройка своих данных для коммитов:

git config --global user.name "Ваше Имя"
git config --global user.email "ваш_email@example.com"

Далее создание аккаунта на GitHub:
1. Переход на https://github.com/signup
2. Ввод email, пароля и username
3. Подтвердил email через письмо
4. Выбор Free plan

Для безопасной работы с GitHub были настроены SSH-ключи. В PowerShell ввести команду:

ssh-keygen -t ed25519 -C "ваш_email@example.com"

На все вопросы нажать Enter (оставить путь по умолчанию и пустой пароль).

Запуск SSH-агента:

Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent

Добавление ключа в агент:

ssh-add $env:USERPROFILE\.ssh\id_ed25519

Получение ответа:
Identity added: C:\Users\Acer Swift 3\.ssh\id_ed25519

Ввод публичного ключа:

cat $env:USERPROFILE\.ssh\id_ed25519.pub

Копирование всей строки, которая начинается с "ssh-ed25519".

Переход на GitHub в настройки, нажать "New SSH key", вставка скопированного ключа и нажать "Add SSH key".

Проверил подключение (через Powershell):

ssh -T git@github.com

При первом подключении ввести "yes". Получение успешного сообщения:
Hi ВАШ_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.

ЭТАП 3: СОЗДАНИЕ РЕПОЗИТОРИЯ И ФАЙЛА README.MD

На GitHub нажал "+" в правом верхнем углу и выбор "New repository". Заполнить параметры:
- Repository name: my-first-devops
- Public (публичный)
- Поставить галочку "Add a README file"
- Add .gitignore: None
- License: None

Нажать "Create repository".

На странице репозитория нажать на файл README.md, затем на иконку карандаша (Edit this file). Добавить начальный текст. Нажать "Commit changes".

Затем нажал зеленую кнопку "Code", выбор вкладки SSH и скопировать адрес репозитория:
git@github.com:ВАШ_USERNAME/my-first-devops.git
 
В PowerShell на рабочем столе выполнить клонирование:

cd $env:USERPROFILE\Desktop
git clone git@github.com:ВАШ_USERNAME/my-first-devops.git

Получение ответа:
Cloning into 'my-first-devops'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
Receiving objects: 100% (3/3), done.

Переход в папку репозитория:

cd my-first-devops

Проверка содержимого:

ls

Просмотр файла README.md. 

cat README.md

ЭТАП 4: УСТАНОВКА DOCKER И ЗАПУСК КОНТЕЙНЕРОВ

Перед установкой Docker нужно настроить WSL 2. Открыть PowerShell от имени администратора.

Включение подсистемы Linux:

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

Включение платформы виртуальной машины:

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

Перезагрузка компьютера:

shutdown /r /t 0

После перезагрузки скачать и установить пакет ядра WSL 2 с официального сайта Microsoft:
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

Запуск файла wsl_update_x64.msi и нажать Next -> Install.

Установка WSL 2 по умолчанию:

wsl --set-default-version 2

Проверка статуса:

wsl --status

Далее скачать Docker Desktop с официального сайта https://www.docker.com/products/docker-desktop/ и его запуск. При установке выбрать:
- Per-user installation (Beta)
- Add shortcut to desktop

Нажать OK и дождаться установки. Перезагрузка компьютера.

После перезагрузки запустить Docker Desktop из меню Пуск. Дождся статуса "Engine running" (зеленый индикатор внизу).

Проверка версии Docker:

docker --version

Запуск тестового контейнера:

docker run hello-world

Вывод:
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb59a4: Pull complete
Hello from Docker!
This message shows that your installation appears to be working correctly.

Создание собственного Docker-образ. В папке репозитория создать файл script.py:

Set-Content -Path "script.py" -Value 'print("Hello, DevOps World!")'

Проверка содержимого:

cat script.py

Вывод:
print("Hello, DevOps World!")

Создание Dockerfile:

@"
FROM python:3.11-slim

WORKDIR /app

COPY script.py .

CMD ["python", "script.py"]
"@ | Set-Content -Path "Dockerfile" -Encoding UTF8

Проверка структуры папки:

ls

Вывод три файла: Dockerfile, README.md, script.py

Собрать Docker-образ с помощью команды:

docker build -t my-devops-app .

Вывод:
[+] Building 20.5s (8/8) FINISHED
 => [1/3] FROM docker.io/library/python:3.11-slim
 => [2/3] WORKDIR /app
 => [3/3] COPY script.py .
 => naming to docker.io/library/my-devops-app

Запуск контейнера:

docker run my-devops-app

Вывод:
Hello, DevOps World!


## Скриншоты
СКРИНШОТЫ ВЫПОЛНЕННЫХ ЭТАПОВ

ssh-connection.png - Подключение к Ubuntu Server по SSH
git.png - Процесс установки Git
ssh-key.png - Генерация SSH-ключа
github-auth.png - Проверка подключения к GitHub
hello-world.png - Запуск тестового контейнера Docker
my-devops-app.png - Запуск собственного контейнера
wsl-status.png - Проверка статуса WSL 2
docker-install.png - Процесс установки Docker Desktop