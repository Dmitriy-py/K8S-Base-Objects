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















