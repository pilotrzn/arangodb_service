# ArangoDB Starter

Программа, предназначенная для простого развертывания БД на сервере или ВМ.
Может создавать от простого экземпляра  до кластера с репликацией между ЦОДами.

Варианты развертывания:

1. Single mode
2. Active Failover
3. Cluster

Режим выбирается опцией --starter.mode. 

```bash
# arangodb --starter.mode=(cluster|single|activefailover) (default "cluster")
```

В cluster режиме один Starter запускает не более одного агента, одного DB-сервера и одного координатора.

