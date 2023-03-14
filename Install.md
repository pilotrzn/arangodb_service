# Установка БД ArangoDB

- [Установка на Ubuntu](#ubuntu-server)

- [Установка на Альт сервер 8 СП](#alt-linux-8-sp)

- [Установка на Альт сервер 9](#alt-linux-9)

 ## Ubuntu server

- server-name: ubuntu-arango--srv01
- version: 20.04


Устанавливается БД и пакеты символов отладки

```bash
~# curl -OL https://download.arangodb.com/arangodb310/DEBIAN/Release.key
~# sudo apt-key add - < Release.key
~# echo 'deb https://download.arangodb.com/arangodb310/DEBIAN/ /' | sudo tee /etc/apt/sources.list.d/arangodb.list
~# sudo apt-get install apt-transport-https
~# sudo apt-get update
~# sudo apt-get install arangodb3=3.10.2-1
~# sudo apt-get install arangodb3-dbg=3.10.2-1
```

Доступ к вебморде:

```bash
$ nano /etc/arangodb3/arangod.conf
 endpoint = tcp://localhost:8529
```

Заменить на ip сервера. выполнить перезапуск службы

Установка по умолчанию содержит одну базу данных _system и имя пользователя root.

Пакеты на основе Debian и установщик Windows запрашивают пароль во время процесса установки. Пакеты на основе Red Hat устанавливают случайный пароль. Для всех других пакетов установки необходимо выполнить следующее:


## Alt Linux 8 SP

На версии Альт Линукс 8 СП работает версия Arango 3.8.
Установка возможна из пакета .rpm


## Alt Linux 9

На этой версии можно установить последнюю версию Arango - 3.10.
Установка выполняется из пакета rpm.
Предварительно необходимо установить tzdata


[Назад](README.md)



