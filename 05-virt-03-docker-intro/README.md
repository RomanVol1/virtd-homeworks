
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose» - RomanG

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com;
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 . 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
```
[hub.docker.com](https://hub.docker.com/repository/docker/romangvol1/custom-nginx/general)
```

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080

![task2.1](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B52.1.jpg)

2. Переименуйте контейнер в "custom-nginx-t2"

![task2.2](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B52.2.jpg)

3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" && sleep 0.150 && docker ps && ss -tlpn | grep 127.0.0.1:8080  && docker logs custom-nginx-t2 -n1 && docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```

![task2.3](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B52.3.jpg)

4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

![task2.4](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B52.4.jpg)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.

```
sudo docker attach custom-nginx-t2
```
![task3.1](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B53.1-3.jpg)

4. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.

```
При подключении к контейнеру с помощью "docker attach" и последующем нажатии "Ctrl+C" - будет отправлен сигнал "SIGINT" в сам контейнер, это и вызывает остановку контейнера, т.к. сам сигнал "SIGINT" вызывает прерывание работы запущенной команды.
```

5. Перезапустите контейнер
6. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.

***
sudo docker exec -it custom-nginx-t2 /bin/bash
***

8. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
9. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
10. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 && curl http://127.0.0.1:81```.
11. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
12. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.

***
Связано с тем что контейнер запущен на “80” порту и не знает о том что внутри контейнера порт прослушивания “nginx” был изменен на “81” и продолжает слушать “80” порт, а т.к. на нём ничего более нет – соединение сбрасывается
***
  
14. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
15. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера.

![task4.1](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B54.1.jpg)

- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера.

![task4.2](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B54.2.jpg)

- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.

***
sudo docker exec -ti 3d3cb9396de4 /bin/bash
***

- Добавьте ещё один файл в каталог ```/data``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-file/03-compose-file/)

***
стандартным именем файла для “docker compose” является “ docker-compose.yaml ” или “docker-compose.yml”, поэтому compose.yaml не будет запущен
***

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

4. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
5. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
6. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

![task5.1](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B55.1.jpg)

8. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите тестовый стенд ОДНОЙ(обязательно!!) командой.

![task5.2](https://github.com/RomanVol1/virtd-homeworks/blob/shvirtd-1/jpg/%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B55.2.jpg)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


