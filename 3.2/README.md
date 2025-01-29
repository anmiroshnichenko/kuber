# Домашнее задание к занятию «Установка Kubernetes»

### Цель задания

Установить кластер K8s.

### Чеклист готовности к домашнему заданию

1. Развёрнутые ВМ с ОС Ubuntu 20.04-lts.


### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция по установке kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/).
2. [Документация kubespray](https://kubespray.io/).

-----

### Задание 1. Установить кластер k8s с 1 master node

1. Подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды.
```
sudo apt update
sudo apt install git python3 python3-pip python3-venv -y
git clone https://github.com/kubernetes-incubator/kubespray.git
cd kubespray
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
pip install -r requirements.txt 
cp -rfp inventory/sample inventory/mycluster
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -b -v 
```
  ![image](screenshots/1_1.jpg) 

```
[kube_control_plane]
 node1 ansible_host=192.168.10.10  # ip=10.3.0.1 etcd_member_name=etcd1

[etcd:children]
kube_control_plane

[kube_node]
 node4 ansible_host=192.168.10.40  # ip=10.3.0.4
 node5 ansible_host=192.168.10.50  # ip=10.3.0.5
 node6 ansible_host=192.168.10.60  # ip=10.3.0.6
 node7 ansible_host=192.168.10.70  # ip=10.3.0.6
```  
  ![image](screenshots/1_2.jpg) 

2. В качестве CRI — containerd.
  ![image](screenshots/1_2_1.jpg) 

3. Запуск etcd производить на мастере.
  ![image](screenshots/1_3.jpg) 
  
4. Способ установки выбрать самостоятельно.
  ![image](screenshots/1_4.jpg) 

## Дополнительные задания (со звёздочкой)

**Настоятельно рекомендуем выполнять все задания под звёздочкой.** Их выполнение поможет глубже разобраться в материале.   
Задания под звёздочкой необязательные к выполнению и не повлияют на получение зачёта по этому домашнему заданию. 

------
### Задание 2*. Установить HA кластер

1. Установить кластер в режиме HA.
2. Использовать нечётное количество Master-node.
3. Для cluster ip использовать keepalived или другой способ.

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl get nodes`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
