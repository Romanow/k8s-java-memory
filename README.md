# Java memory in k8s cluster

```shell
# create cluster and upload image
$ kind create cluster --config kind.yml
$ kind load docker-image romanowalex/k8s-java-memory:v1.0
# install helm chart
$ helm repo add romanow https://romanow.github.io/helm-charts/
$ helm repo update
# install service
$ helm install k8s-java-memory k8s/java
$ kubectl logs -f -l app=k8s-java-memory
```

## Результаты запуска

Используем образ `amazoncorretto:17`, запускаем с `request` == `limit` без дополнительных параметров JVM.

Запускаем команду `free -m` в pod:

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