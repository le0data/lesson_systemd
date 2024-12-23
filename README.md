# Работа с Systemd

Для работы создавалась ВМ с помощью Vagrant. Box = "generic/ubuntu2204"

## Первое задание - Service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова

1) Создан файл с конфигурацией в /etc/default

![Create_watchlog](https://github.com/user-attachments/assets/2d16ec83-1f11-4936-980a-da9e31ea443d)

2) Файл .log с ключевым словом 'ALERT'

![File watchlog_log](https://github.com/user-attachments/assets/37600bad-d039-468e-9338-bf4ec978556a)

3) Создание скрипта
   
![watchlog_script](https://github.com/user-attachments/assets/7fb6b10a-24cb-4532-bbdb-7fab0ea148df)

+ добавляем права на запуск файла `chmod +x /opt/watchlog.sh`

4) Создадим юнит для сервиса

![Unit_for_service](https://github.com/user-attachments/assets/5eeb76a4-cbc6-4ebc-a92d-b088e5e365f9)


5) Создание юнита для таймера

![Unit for timer](https://github.com/user-attachments/assets/d81f8194-be18-4519-a6cd-7c0fa87abc1c)

+ запускаем timer `systemctl start watchlog.timer`
+ запускаем сервис `systemctl status watchlog.service`

6) Итоговый результат:

![Result_word](https://github.com/user-attachments/assets/66ffc2c4-e73b-448c-ae05-ad164fe1df43)


## Второе задание - Установка spawn-fcgi и создание unit-файла (spawn-fcgi.service)

1) Устанавливаем spawn-fcgi и необходимые для него пакеты:

`apt install spawn-fcgi php php-cgi php-cli \ apache2 libapache2-mod-fcgid -y`

2) Создание файла fcgi.conf

![fcgi_conf](https://github.com/user-attachments/assets/958afb32-f730-483d-b5fa-aa7137337db4)

3) Юнит файл spawn-fcgi.service

![spawn-fcgi_service](https://github.com/user-attachments/assets/44fb7155-7ca3-44f0-9eef-8a56412b4991)

4) Проверяем, что все работает

 ![status_spawn-fcgi](https://github.com/user-attachments/assets/5a7b472e-ad5a-49d2-b8bc-4060fb388701)

## Третье задание - Доработка unit-файла Nginx (nginx.service) для запуска нескольких инстансов сервера с разными конфигурационными файлами одновременно

1) Устанавливаем Nginx

   `apt install nginx -y`

2) Создаем новый юнит для работы с шаблонами, для того, чтобы можно было запускать несколько экземпляров сервиса.

![unit_nginx_service](https://github.com/user-attachments/assets/138056f4-1af7-4521-9faf-e5230bf8ee6b)

3) Были созданы 2 файла конфигурации nginx-first и nginx-second
4) После модификации путей до PID-файлов и разделением портов получаем вот такой результат:
![nginx_ports](https://github.com/user-attachments/assets/4c852fd6-e4ff-4328-bb35-0504053ef005)

![ps afx_nginx](https://github.com/user-attachments/assets/87e8a82f-7ffa-4b49-817b-c0803296a944)










