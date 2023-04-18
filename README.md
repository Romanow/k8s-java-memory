# Java memory in k8s cluster

```shell
# build image
$ ./gradlew clean build
$ docker build . -t romanowalex/k8s-java-memory:v1.0
# create cluster and upload image
$ kind create cluster --config kind.yml
$ kind load docker-image romanowalex/k8s-java-memory:v1.0
# install service
$ helm install k8s-java-memory k8s/java
$ kubectl logs -f -l app=k8s-java-memory
```

## Результаты запуска

Запускаем локальный кластер kind, в java приложении используем образ `amazoncorretto:17`. Запускаем
с `request` == `limit` без дополнительных настроек памяти на стороне JVM.

На docker выдано 12Gb, 6CPU, 2Gb swap. Запускаем команду `free -m` в pod:

|        | total | used | free | shared | buff/cache | available |
|--------|:-----:|:----:|:----:|:------:|:----------:|:---------:|
| Memory | 9951  | 1203 | 833  |  330   |    7914    |   8211    |
| Swap   | 1535  |  1   | 1534 |        |            |           |

```
$ java -XX:+PrintFlagsFinal --version | grep -Ei 'HeapSize'

size_t MaxHeapSizЧe = 1073741824 (1024Mb)
size_t MinHeapSize = 8388608 (8Mb)
```

Это случается потому, что `MaxRAMPercentage` = 25%!

```shell
$ java -XX:+PrintFlagsFinal --version | grep -Ei 'RAMPercentage'
   double InitialRAMPercentage  = 1.562500
   double MaxRAMPercentage      = 25.000000
   double MinRAMPercentage      = 50.000000
```