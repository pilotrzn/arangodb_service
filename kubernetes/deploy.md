Инструкция с офф сайта кривая... Основное подсмотренно здесь https://github.com/arangodb/arangodb/issues/10951
Актуальная информация по helm arangoDB тут https://github.com/arangodb/kube-arangodb/releases/       
А тут инструкция может быть полезна https://github.com/arangodb/kube-arangodb

1. Ставим helm, пакетом или просто тащим бинарь, на мастер, далее все делаем с пользователя у которого есть возможность запускать kubectl.

helm install arangodb-crd --create-namespace --namespace=web https://github.com/arangodb/kube-arangodb/releases/download/1.2.24/kube-arangodb-crd-1.2.24.tgz
(--create-namespace --namespace=web в первый раз указываем ключ --create-namespace , называем как хотим)
helm install arangodb --namespace=web https://github.com/arangodb/kube-arangodb/releases/download/1.2.24/kube-arangodb-1.2.24.tgz

// To use ArangoLocalStorage, set field operator.features.storage to true
helm install arangodb-storage --namespace=web https://github.com/arangodb/kube-arangodb/releases/download/1.2.24/kube-arangodb-1.2.24.tgz --set "operator.features.storage=true"

helm list -A (просматриваем все установленные chart)

kubectl get all -n web
Имеет смысл посмотреть и запомнить изначальные ресурсы)
  
2. Далее делаем все по инструкции с офф сайта https://www.arangodb.com/docs/3.9/tutorials-kubernetes.html

2.1 Для соло сервера (вариант для тех у кого 1worker, мне пришлось на своем докинуть до 4х ядер):
создаем файл server.yaml:

apiVersion: "database.arangodb.com/v1alpha"
kind: "ArangoDeployment"
metadata:
  name: "single-server"
spec:
  mode: Single

kubectl apply -f server.yaml --namespace=web

ждем. периодически смотрим статус:
kubectl get all -n web
Должен появится под + сервис, в последнем прописан ExternalIP:8529 
Можем подключаться) Then login using username root and an empty password. 

2.2 Для кластера (минимум 3 worker`а сейчас установлен в sandbox)
создаем файл cluster.yaml:

apiVersion: "database.arangodb.com/v1alpha"
kind: "ArangoDeployment"
metadata:
  name: "cluster"
spec:
  mode: Cluster

kubectl apply -f cluster.yaml --namespace=web  
  
Далее тоже самое, что и в 2.1, только больше сущностей.

3. Примеры комманд, все из инструкции с оффа + наш ns.
kubectl get pods --selector=arango_deployment=cluster -n web
kubectl describe ArangoDeployment cluster -n web
kubectl delete ArangoDeployment cluster -n web
kubectl delete ArangoDeployment single-server -n web    (именно single-server)

4. На VM с anydesk прописал следующие тунели(с телека проверял, работают):
ssh -fN -L 7788:10.233.34.89:80 altlinux@192.168.0.20     7788 - longhorn without autorization
ssh -fN -L 7789:10.233.30.139:80 altlinux@192.168.0.20    7789 - longhorn with autorization
ssh -fN -L 7790:10.233.13.247:8529 altlinux@192.168.0.20  7790 - ArangoDB HTTPS!!!!  https://127.0.0.1:7790 Then login using username root and an empty password.
Принцип:
ssh -fN -L {порт_на_машине}:{service_ClusterIP}:{service_ClusterIP_port} altlinux@{worker_node1}
kill -9 `lsof -ti:7789`  (Пример как выключить)

Прописываем у себя в anydesk тунель 2230 localhost 7790
И идем с своего браузера на https://127.0.0.1:2230
Then login using username root and an empty password.

БОНУС. Немного полезного по Helm (hubs, plugin and more... ) 
https://github.com/cdwv/awesome-helm