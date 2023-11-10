# Devops_lab

## ТЗ
Пользуясь терминалом на компьютере А перенести файл с компьютера Б на компьютер С, находящиеся в одной локальной сети.

## Выполнение работы

1) Распределяем роли между участниками команды: Mac OS (1) берет файл у Mac OS (2) и перекидывает его на Mac OS (3)

2) Настраиваем общий доступ, удаленный вход и удаленное управление на Mac OS через "Системные настройки" -> "Общий доступ"

<img width="589" alt="image" src="https://github.com/MrRetyNine/Devops_lab/assets/112976351/04231df3-def8-4dd4-8356-4bce3c4e1a03">

<img width="549" alt="image" src="https://github.com/MrRetyNine/Devops_lab/assets/112976351/3ab6da9d-c31a-4381-bba2-159948ad0490">

(скрин был сделан после проделанной работы, поэтому ip другой)

3) Узнаем ip каждого компьютера в том же разделе системных настроек, где уже указана ssh команда с именем пользователя и ip

4) Подключаем mac(1) к mac(2)
```
MacBook-Pro-Alisa:~ alisadinkel$ ssh PolyaLobova@172.28.25.66
The authenticity of host '172.28.25.66 (172.28.25.66)' can't be established.
ED25519 key fingerprint is SHA256:VUcNv/YRUbtRPr7HaR+y24CJpClzh2bZRcr+Oevzvkk.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: 172.28.28.89
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.28.25.66' (ED25519) to the list of known hosts.
(PolyaLobova@172.28.25.66) Password:
Last login: Thu Nov  9 13:48:02 2023
```

5) Переходим на рабочий стол (там лежит нужный файл)
```
PolyaLobova@MacBook-Air-Julia ~ % cd Desktop
```

6) Вызываем команду scp, указывая путь к файлу на mac(2), имя пользователя и ip mac(3) и путь к папке, в которую мы хотим переместить файл
```
PolyaLobova@MacBook-Air-Julia Desktop % scp ./picture_for_clouds.png gvdvddaa@172.28.17.25:/Users/gvdvddaa/Desktop
Password:
picture_for_clouds.png                        100%  109KB  25.3KB/s   00:04    
```

7) Ищем перемещенный по указанному пути файл на mac(3) и радуемся успешному результату

![picture delivered](https://github.com/MrRetyNine/Devops_lab/assets/112976351/2da9a3e8-3fe9-41dc-b39b-f0ad42b08815)

