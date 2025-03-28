# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Ольга Антоненко`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`Установите Zabbix Server с веб-интерфейсом.`

   1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
   2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
   3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
   4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Интерфейс zabbix
![Скриншот-1](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/1-1.png)

#### Команды
```
sudo -s
apt install postgresq # устанавливаем БД

# ставим zabbix
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
dpkg -i zabbix-release_latest_7.0+debian12_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts

# настройка БД
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
sed -i 's/#DBPassword=/DBPassword=zabbix/g' /etc/zabbix/zabbix_server.conf

# старт сервера
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2
```
---

### Задание 2

`Установите Zabbix Agent на два хоста.`

   1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
   2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
   3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
   4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
   5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

Cкриншоты:
![Скриншот-1](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/2-1.png)

![Скриншот-2](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/2-2.png)

![Скриншот-3](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/2-3.png)

![Скриншот-4](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/2-4.png)

```
sudo -s
apt install zabbix-agent
sed -i 's/Server=127.0.0.1/Server=192.168.1.10/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/ServerActive=127.0.0.1/ServerActive=192.168.1.10/g' /etc/zabbix/zabbix_agentd.conf
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
---

### Задание 3

`Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.`

1. Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

Cкриншоты:
![Скриншот-1](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/3-1.png)

![Скриншот-2](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/3-2.png)

![Скриншот-3](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/3-3.png)

![Скриншот-4](https://github.com/Olejka22/sys40-mon-zbx-hw1/blob/main/img/3-4.png)
