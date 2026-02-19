# Домашнее задание к занятию «Базовые объекты K8S»
## ` Дмитрий Климов `

## Задание 1. Создать Pod с именем hello-world
  1. Создать манифест (yaml-конфигурацию) Pod.
  2. Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
  3. Подключиться локально к Pod с помощью kubectl port-forward и вывести значение (curl или в браузере).

## Ответ:

### 1. Создание манифеста Pod (hello-pod.yaml)

### ` Манифест, использующий указанный образ и порт: `
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-world
  labels:
    app: hello-app
spec:
  containers:
  - name: echoserver-container
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```

### 2. Развертывание Pod’а

### ` Команда для создания ресурса на кластере MicroK8S: `

```bash
kubectl apply -f hello-pod.yaml
```

### ` Проверка статуса: `

```bash
kubectl get pods
```
### 3. Подключение локально через kubectl port-forward

### Для доступа к порту 8080 внутри Pod’а, мы перенаправляем его на свободный локальный порт (например, 8888).

### ` Команда запускается в отдельной сессии (блокирующая): `

```bash
kubectl port-forward hello-world 8888:8080
```

<img width="1920" height="1080" alt="Снимок экрана (2665)" src="https://github.com/user-attachments/assets/2f5c158a-1dc0-4a4e-ac77-4ac531c733ad" />


### 4. Получение значения (curl)

```bash
curl http://localhost:8888
```

<img width="1920" height="1080" alt="Снимок экрана (2666)" src="https://github.com/user-attachments/assets/e101e798-f2cf-4e5b-afc2-f022e1f218d5" />

<img width="1920" height="1080" alt="Снимок экрана (2667)" src="https://github.com/user-attachments/assets/cd13e0b4-0dac-4096-80bb-dafb0da64ed7" />

## Задание 2. Создать Service и подключить его к Pod
   1. Создать Pod с именем netology-web.
   2. Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.
   3. Создать Service с именем netology-svc и подключить к netology-web.
   4. Подключиться локально к Service с помощью kubectl port-forward и вывести значение (curl или в браузере).

## Ответ:

## Решение к Заданию 2: Создание Service и подключение к Pod

**Цель:** Развернуть Pod `netology-web`, создать Service `netology-svc`, связать их и проверить доступ через `port-forward`.

## 1. Создание манифеста Pod (`netology-pod.yaml`)

Для того чтобы Service мог найти Pod, мы добавляем метку (label) `app: netology-app`.

```yaml
# netology-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: netology-web
  labels:
    app: netology-app
spec:
  containers:
  - name: echoserver-container
    image: gcr.io/kubernetes-e2e-test-images/echoserver:2.2
    ports:
    - containerPort: 8080
```

### 2. Создание манифеста Service (netology-svc.yaml)

### ` Service будет перенаправлять трафик с порта 80 кластера на порт 8080 нашего Pod’а. `

```yaml
apiVersion: v1
kind: Service
metadata:
  name: netology-svc
spec:
  selector:
    app: netology-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```
### 3. Развертывание ресурсов

### ` Применяем созданные манифесты: `

```bash
kubectl apply -f netology-pod.yaml
kubectl apply -f netology-svc.yaml
```

### ` Проверка статуса объектов: `

```bash
kubectl get pods,svc
```

<img width="1920" height="1080" alt="Снимок экрана (2671)" src="https://github.com/user-attachments/assets/3d47019e-16bd-4ed2-974f-84127ad624f7" />

<img width="1920" height="1080" alt="Снимок экрана (2668)" src="https://github.com/user-attachments/assets/96853543-5248-4a4f-a3e6-aa60a179fc64" />


### 4. Подключение через Service (Port-Forward)

### ` Запускаем перенаправление портов для объекта Service. `

```bash
kubectl port-forward service/netology-svc 9090:80
```
### 5. Проверка доступности (curl)

### ` Во второй сессии терминала выполняем запрос к локальному порту 9090: `

```bash
curl http://localhost:9090
```
<img width="1920" height="1080" alt="Снимок экрана (2669)" src="https://github.com/user-attachments/assets/e042d66e-760c-459f-b009-e229fc19f7a9" />

<img width="1920" height="1080" alt="Снимок экрана (2670)" src="https://github.com/user-attachments/assets/1e54b20d-1ff1-4100-9d69-f7757e6abe0f" />


### Завершение работы

```bash
kubectl delete svc netology-svc
kubectl delete pod netology-web
```

<img width="1920" height="1080" alt="Снимок экрана (2671)" src="https://github.com/user-attachments/assets/4c8c7c7e-b0cc-4008-9195-ae57716f8181" />















