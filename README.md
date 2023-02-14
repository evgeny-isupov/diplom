# Дипломный проект


###  Создание  инфраструктуры
С помощью Terraform создаётся [инфраструктура](./main.tf) .  
[Сеть](./network.tf) с четырмя подсетями в трёх зонах доступности. Реализуется [файервол](https://cloud.yandex.ru/docs/vpc/concepts/security-groups) разрешающий  входящий трафик только к определённым портам.  
![s](/img/subnet.png)  

Балансировщик
![balancer1](/img/balancer1.png) 
![balancer](/img/balancer.png)   

Security Grups
![security](/img/security.png) 

Далее с помощью ansible проверяем доступность узлов `ansible all -m ping` 
![ping](/img/ping.png)  

Все узлы доступны, последний это наш балансировщик.     
![out](/img/output.png)   

Далее с помощью плейбуков устанавливается сервисы на соответсвующие ВМ - `ansible-playbook prometheus.yml grafana.yml web-notls.yml node-exporter.yml nginxlog-exporter.yml elastic.yml kibana.yml filebeat.yml`  
![вм](/img/вм.png) 


### Cайт
Две ВМ vm-nginx-1 и vm-nginx-2 для web-серверов во внутренней сети. Установлен nginx с нашим сайтом. Заходим через bastion по ssh. Отображается запущенные nginx и filebeat 
![ssh](/img/ssh.png)  

Публичный ip [балансировщика](https://cloud.yandex.ru/docs/application-load-balancer/) открывает сайт. Конфигурация плейбука устанавливающий nginx и сайт взята из книги "Запускаем ansible" с небольшими доработками.
![сайт](/img/сайт.png)   

### Мониторинг  
Проверяем работу Prometheus, сбор метрик  
![prometheus](/img/prometius.png)  
Проверяем работу сервиса node-exporter  
![node](/img/node.png)  
Переходим к Grafana. Основные метрики с обоих серверов nginx1, nginx2 -  CPU, RAM, диски, сеть, http_response_count_total, http_response_size_bytes
![g1](/img/g1.png)   
![g2](/img/g2.png)  
![g3](/img/g3.png)   

Добавление tresholds  
![tresholds](/img/tresholds1.png)   
![tresholds](/img/tresholds2.png)   
![tresholds](/img/tresholds3.png)   

### Логи  
Проверка ВМ с установленным elasticsearch  
![elastic](/img/elastic.png)  

Добавляем в Kibana индексы kibana1.png
![kibana1](/img/kibana1.png) 
![kibana1](/img/kibana2.png) 
![kibana1](/img/kibana3.png)   

### Резервное копирование  
Создайте snapshot дисков всех ВМ. Ограничьте время жизни snaphot в неделю. Сами snaphot настройте на ежедневное копирование.    
Расписание  
![sn](/img/sn1.png) 

Создание по расписанию в процессе
![sn1](/img/sn2.png) 






