# Домашнее задание
## Основы работы с Kubernetes

### Цель:
#### минимальный сервис.
#### Вариант 1 (С КОДОМ)

##### Шаг 1. Создать минимальный сервис, который
- отвечает на порту 8000
- имеет http-метод GET /health/
- RESPONSE: {"status": "OK"}

##### Шаг 2. Cобрать локально образ приложения в докер.
- Запушить образ в dockerhub

##### Шаг 3. Написать манифесты для деплоя в k8s для этого сервиса.

- Манифесты должны описывать сущности: Deployment, Service, Ingress.
- В Deployment могут быть указаны Liveness, Readiness пробы.
- Количество реплик должно быть не меньше 2. Image контейнера должен быть указан с Dockerhub.
- Хост в ингрессе должен быть arch.homework. В итоге после применения манифестов GET запрос на http://arch.homework/health должен отдавать {“status”: “OK”}.


##### Шаг 4. На выходе предоставить
- ссылку на github c манифестами (в виде pull request). Манифесты должны лежать в одной директории, так чтобы можно было их все применить одной командой kubectl apply -f .
- url, по которому можно будет получить ответ от сервиса (либо тест в postmanе).

###### Задание со звездой:
В Ingress-е должно быть правило, которое форвардит все запросы с /otusapp/{student name}/* на сервис с rewrite-ом пути. Где {student name} - это имя студента.
Например: curl arch.homework/otusapp/aeugene/health -> рерайт пути на arch.homework/health

### Решение
#### Шаги 1, 2 в рамках предыдущего ДЗ
#### В консоли
- два экз. powershell. три, если запускать dashboard

- если драйвер миникуба докер, то в hosts
```
127.0.0.1 arch.homework 
```

#### Скачиваем, ставим
```
winget kubernetes.minikube
```

#### minikube
```
minikube start
```

#### Install Nginx
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx/
helm repo update
kubectl create namespace ng
helm install nginx ingress-nginx/ingress-nginx --namespace ng -f nginx-ingress.yaml
```

##### Проверяем поды ingress, ждем running
```
kubectl get pods -n ng
```

#### Переходим в папку k8s-manifests
```
cd k8s-manifests
```

#### Применяем все манифесты
```
kubectl apply -f .
```

#### Смотрим ждем running
```
kubectl get all
```

#### Проверяем сервисы и порты ingress controller  
```
kubectl get svc -n ng
```
~external ip~ pending пока не запустим туннель

#### в другой консоли и не закрываем
```
minikube tunnel
```

#### обратно в старой консоли видим назначенный external ip
```
kubectl get svc -n ng
```

#### проверяем
```
curl http://arch.homework/health/
curl http://arch.homework/health

curl http://arch.homework/otusapp/aeugene/health/
curl http://arch.homework/otusapp/aeugene/health
```