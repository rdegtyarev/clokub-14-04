# Домашнее задание к занятию "14.4 Сервис-аккаунты"

## Задача 1: Работа с сервис-аккаунтами через утилиту kubectl в установленном minikube

<details>

  <summary>Описание задачи</summary> 

Выполните приведённые команды в консоли. Получите вывод команд. Сохраните
задачу 1 как справочный материал.

### Как создать сервис-аккаунт?

```
kubectl create serviceaccount netology
```

### Как просмотреть список сервис-акаунтов?

```
kubectl get serviceaccounts
kubectl get serviceaccount
```

### Как получить информацию в формате YAML и/или JSON?

```
kubectl get serviceaccount netology -o yaml
kubectl get serviceaccount default -o json
```

### Как выгрузить сервис-акаунты и сохранить его в файл?

```
kubectl get serviceaccounts -o json > serviceaccounts.json
kubectl get serviceaccount netology -o yaml > netology.yml
```

### Как удалить сервис-акаунт?

```
kubectl delete serviceaccount netology
```

### Как загрузить сервис-акаунт из файла?

```
kubectl apply -f netology.yml
```

</details>

### Решение

1. Как создать сервис-аккаунт?

```
kubectl create serviceaccount netology
serviceaccount/netology created
```

2. Как просмотреть список сервис-акаунтов?

```
kubectl get serviceaccounts
NAME                                SECRETS   AGE
default                             1         7d20h
netology                            1         86s
nfs-server-nfs-server-provisioner   1         7d11h


kubectl get serviceaccount
kubectl get serviceaccount
NAME                                SECRETS   AGE
default                             1         7d20h
netology                            1         100s
nfs-server-nfs-server-provisioner   1         7d11h
```

3. Как получить информацию в формате YAML и/или JSON?

```
kubectl get serviceaccount netology -o yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: "2022-06-19T08:34:25Z"
  name: netology
  namespace: default
  resourceVersion: "2703499"
  uid: cb414c1f-657e-4095-bb06-7ee2f93800fb
secrets:
- name: netology-token-lqf69


kubectl get serviceaccount default -o json
{
    "apiVersion": "v1",
    "kind": "ServiceAccount",
    "metadata": {
        "creationTimestamp": "2022-06-11T11:58:50Z",
        "name": "default",
        "namespace": "default",
        "resourceVersion": "313",
        "uid": "761aa381-7b81-4410-943d-dae612ff7fc8"
    },
    "secrets": [
        {
            "name": "default-token-mr29n"
        }
    ]
}
```

4. Как выгрузить сервис-акаунты и сохранить его в файл?

```
kubectl get serviceaccounts -o json > serviceaccounts.json
kubectl get serviceaccount netology -o yaml > netology.yml
```

Созданы соотвествующие файлы.

5. Как удалить сервис-акаунт?

```
kubectl delete serviceaccount netology
serviceaccount "netology" deleted
```

6. Как загрузить сервис-акаунт из файла?

```
kubectl apply -f netology.yml
serviceaccount/netology created
```

---

## Задача 2 (*): Работа с сервис-акаунтами внутри модуля

<details>

  <summary>Описание задачи</summary> 

Выбрать любимый образ контейнера, подключить сервис-акаунты и проверить
доступность API Kubernetes

```
kubectl run -i --tty fedora --image=fedora --restart=Never -- sh
```

Просмотреть переменные среды

```
env | grep KUBE
```

Получить значения переменных

```
K8S=https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT
SADIR=/var/run/secrets/kubernetes.io/serviceaccount
TOKEN=$(cat $SADIR/token)
CACERT=$SADIR/ca.crt
NAMESPACE=$(cat $SADIR/namespace)
```

Подключаемся к API

```
curl -H "Authorization: Bearer $TOKEN" --cacert $CACERT $K8S/api/v1/
```

В случае с minikube может быть другой адрес и порт, который можно взять здесь

```
cat ~/.kube/config
```

или здесь

```
kubectl cluster-info
```

</details>

### Решение

Подключение к api kubernetes успешно, авторизация с использованием токена сервис-аккаунта  ($TOKEN) и корневого сертификата кластера $CACERT.

```
curl -H "Authorization: Bearer $TOKEN" --cacert $CACERT $K8S/api/v1/
```

---

