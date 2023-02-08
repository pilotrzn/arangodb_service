# Установка БД ArangoDB

 ## Ubuntu server

server-name: ubuntu-arango--srv01
version: 20.04
user: alex
pass: 123456

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

root
pass: 654321
alex
pass: 123

Доступ к вебморде:

nano /etc/arangodb3/arangod.conf
endpoint = tcp://localhost:8529
 заменить на ip сервера. выполнить перезапуск службы