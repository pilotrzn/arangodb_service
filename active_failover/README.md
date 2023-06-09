# Active failover

Активный переход(AF) представляет собой следующую конфигурацию:

- Один экземпляр автономного ArangoDB сервера, который доступен для чтения/записи клиентами, называется Leader.

- Один или несколько экземпляров автономных ArangoDB серверов, которые являются пассивными и недоступными для записи, называются последователями, которые асинхронно реплицируют данные из лидера.

- По крайней мере, одно Agency, действующее как “свидетель”, чтобы определить, какой сервер становится лидером в ситуации сбоя.

Преимущество настройки активного перехода на другой ресурс заключается в том, что существует активная третья сторона - Agency, которое наблюдает и контролирует все задействованные серверные процессы. Экземпляры-подписчики могут полагаться на Agency в определении правильного сервера-лидера. С операционной точки зрения одним из преимуществ является то, что переход на другой ресурс в случае отказа лидера происходит автоматически. Дополнительным операционным преимуществом является то, что нет необходимости запускать приложение репликации вручную.

В настоящее время не поддерживаются настройки активного перехода на другой ресурс в нескольких центрах обработки данных.

--- 

### Настройка активного перехода на другой ресурс в ArangoDB имеет несколько ограничений.

В отличие от кластера ArangoDB:

- Активный переход на другой ресурс имеет только асинхронную репликацию и следовательно, не гарантирует, сколько операций с базой данных могло быть потеряно во время перехода на другой ресурс.

- Активный переход на другой ресурс не имеет глобального состояния, и следовательно, переход на плохой подписчик(тот, у которого нет самых актуальных данных) переопределяет все другие подписчики с этим состоянием (включая предыдущего лидера, у которого могут быть более актуальные данные). При настройке кластера агентство предоставляет глобальное состояние, и, следовательно, ArangoDB знает о последнем состоянии.

- Если вы добавляете более одного подписчика, имейте в виду, что во время отработки отказа аварийный переход пытается выбрать самого современного подписчика в качестве нового лидера на основе максимальных усилий.

- Если вы используете ArangoDB Starter или Kubernetes Operator для управления развертыванием с активным переходом на другой ресурс, имейте в виду, что обновление может вызвать непреднамеренный переход на другой ресурс между компьютерами.

Инструменты запуска:

 - [Вручную](manual.md)
 - [ArangoDB Starter](starter.md)
 - [Docker](docker.md)

 [Назад](../README.md)