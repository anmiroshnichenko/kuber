# Домашнее задание к занятию «Запуск приложений в K8S»

### Цель задания

В тестовой среде для работы с Kubernetes, установленной в предыдущем ДЗ, необходимо развернуть Deployment с приложением, состоящим из нескольких контейнеров, и масштабировать его.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов.
2. [Описание](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) Init-контейнеров.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod

1. Создать Deployment приложения, состоящего из двух контейнеров — nginx и multitool. Решить возникшую ошибку.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-multitool-deployment
  labels:
    app: nginx-multitool
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-multitool
  template:
    metadata:
      labels:
        app: nginx-multitool
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        imagePullPolicy: IfNotPresent
      - name: multitool
        image: praqma/network-multitool:latest
        imagePullPolicy: IfNotPresent
        env:
        - name: HTTP_PORT
          value: "8080"
```
![image](screenshots/1_1.jpg)

2. После запуска увеличить количество реплик работающего приложения до 2.
![image](screenshots/1_2.jpg)
3. Продемонстрировать количество подов до и после масштабирования.
![image](screenshots/1_3.jpg)
![image](screenshots/1_3_1.jpg)
4. Создать Service, который обеспечит доступ до реплик приложений из п.1.
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-multitool-service
spec:
  selector:
    app: nginx-multitool 
  ports:
    - name: nginx  
      protocol: TCP
      port: 80
      targetPort: 80
      protocol: TCP
    - name: multitool   
      port: 81
      targetPort: 8080  
```
![image](screenshots/1_4.jpg)
5. Создать отдельный Pod с приложением multitool и убедиться с помощью `curl`, что из пода есть доступ до приложений из п.1.
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: multitool
  name: multitool-app  
spec:
  containers:
  - name: multitool
    image: praqma/network-multitool:latest
    imagePullPolicy: IfNotPresent
```
![image](screenshots/1_5.jpg)
![image](screenshots/1_5_1.jpg)
![image](screenshots/1_5_2.jpg)
------

### Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий

1. Создать Deployment приложения nginx и обеспечить старт контейнера только после того, как будет запущен сервис этого приложения.
![image](screenshots/2_1.jpg)
2. Убедиться, что nginx не стартует. В качестве Init-контейнера взять busybox.
![image](screenshots/2_2.jpg)
3. Создать и запустить Service. Убедиться, что Init запустился.
![image](screenshots/2_3.jpg)
4. Продемонстрировать состояние пода до и после запуска сервиса.
![image](screenshots/2_4.jpg)

------

### Правила приема работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl` и скриншоты результатов.
3. Репозиторий должен содержать файлы манифестов и ссылки на них в файле README.md.

------
