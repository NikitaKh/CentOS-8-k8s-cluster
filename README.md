# CentOS 8 K8s Cluster

В ходе выполнения инсрукции будут созданы:
```
    - K8s кластер
    - K8s Dashboard - подробнее тут https://github.com/kubernetes/dashboard
    - Настроен сбор метрик с кластера
```

#### Подготовка inventory
В файле inventory укажите адреса мастера и воркер нод и данные для подключения по ssh. У пользователя должны быть root права.

#### Обновление нод
Для обновления нод необходим плейбук node_prepare.yml
Команда для запуска: ansible-playbook -i hosts -vbc paramiko node_prepare.yml

#### Подготовка мастер ноды
Для инициализации мастера необходим плейбук master_init.yml
Команда для запуска: ansible-playbook -i hosts -vbc paramiko master_init.yml

По итогу выполнения плейбука будет выведена информация типа kubeadm join ....
Необходимо скопировать эту команду, она понадобится для следующего шага.

#### Подготовка воркер нод
1. Команду kubeadm join ... из предыдущего пункта копируем в CentOS-8-k8s-cluster/roles/worker_init/defaults/main.yml в переменную TOKEN
2. Команда для запуска: ansible-playbook -i hosts -vbc paramiko worker_join.yml

#### Установка K8s Dashboard
Необходим плейбук k8s_dashboard.yml
Команда для запуска: ansible-playbook -i hosts -vbc paramiko k8s_dashboard.yml
Для получения токена для доступа в ui и адреса по которому будет доступен ui нужно вызвать скрипт: CentOS-8-k8s-cluster/roles/k8s_dashboard_install/files/get-dashboard-info.sh

#### Установка Prometheus Monitoring
Необходим плейбук prometheus_monitoring.yml
Команда для запуска: ansible-playbook -i hosts -vbc paramiko prometheus_monitoring.yml
