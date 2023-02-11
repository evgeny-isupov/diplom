# Дипломный проект


### 1. Создание  инфраструктуры
С помощью Terraform создаётся [инфраструктура](./main.tf)








```shel
ansible all -m ping

ansible-playbook prometheus.yml

ansible-playbook grafana.yml

ansible-playbook nginx.yml

ansible-playbook node-exporter.yml

ansible-playbook nginxlog-exporter.yml

ansible-playbook prometheus.yml grafana.yml web-notls.yml node-exporter.yml nginxlog-exporter.yml 

