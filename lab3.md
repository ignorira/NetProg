University: ITMO University
Faculty: ICT
Course: Network programming
Year: 2022/2023 
Group: K34212 
Author: Sorokina Irina
Lab3

Цель: Изучить и настроить взаимодействие междy Ansible и NetBox.
Ход работы
Для начала создаем новую виртуальную машину. Затем запускаем NetBox. Это требует установки большого количества зависимостей, поэтому устанавливаем PostgreSQL с помощью команды «sudo apt install postgresql». 
![image](https://user-images.githubusercontent.com/58992611/207255462-7da10683-6843-4f62-bcd8-33eed5ec4ab1.png)

В PostgreSQL создаем новую БД NetBox и пользователя с таким же именем, выдаем ему максимальные права на управление этой БД.
![image](https://user-images.githubusercontent.com/58992611/207255533-2e9979b8-83ff-4460-8dde-2f7355b37490.png)

Затем устанавливаем Redis с помощью команды “apt install redis-server”, устанавливаем все пакеты, необходимые для работы NetBox. В новую директорию устанавливаем и распаковываем репозиторий с последней версией NetBox. Создаем специального системный пользователя, которому предоставляем права на все папки с файлами NetBox.
![image](https://user-images.githubusercontent.com/58992611/207255647-26aababd-f6b6-48a9-8597-9ddd77e4c753.png)

Копируем пример файла конфигурации и редактируем его. Генерируем случайный ключ с помощью команды «python3 ../generate_secret_key.py». Вносим в конфигурационный файл секретный ключ, разрешенные хосты и информацию о БД. Создаем виртуальное окружение python, внутри создаем суперюзера с помощью команды «python3 manage.py createsuperuser».
Для запуска NetBox в режиме демона сконфигурируем Nginx и gunicorn. После этого редактируем файл, добавляем хост и удаляем https server.
![image](https://user-images.githubusercontent.com/58992611/207255713-271dd92c-b374-4f80-bb38-fd85a7637271.png)

Далее создаем символическую ссылку и перезапускаем службу. Теперь используя браузер по IP открываем NetBox, и заходим под заранее созданным суперпользователем.
![image](https://user-images.githubusercontent.com/58992611/207255791-3dae1d6b-fe2a-4ff9-aae6-e82228acc418.png)

Теперь в NetBox создаем два роутера «router1» и «router2». Для того, чтобы добавить роутерам IP-адреса, во вкладке «Devices» добавляем интерфейс для роутеров, затем интерфейсы IP адресам и наконец, роутерам IP.
![image](https://user-images.githubusercontent.com/58992611/207255851-517d0d6e-0585-42d7-8ec1-1f1208adee80.png)
![image](https://user-images.githubusercontent.com/58992611/207255885-e617f663-86f7-4e02-9625-2ef3648e0e4e.png)
![image](https://user-images.githubusercontent.com/58992611/207255913-75fe3974-2968-4b07-8718-b47a111ea9f7.png)

Теперь скачиваем «netbox_devices.csv» файл.
![image](https://user-images.githubusercontent.com/58992611/207255992-be7ca5cf-e963-4a83-85b6-05b647bfeed3.png)

Переносим скачанный файл на сервер с Ansible. Для начала создаем файл «playbook1.yml», реализуем для проверки парсинг csv файла. Сохраняем название роутера в файл «new_playbook1.yml» и указываем ID в качестве ключа.
![image](https://user-images.githubusercontent.com/58992611/207256065-b9f0ff71-3434-4b8d-9b6b-ab7a9e6963d8.png)

Запускаем playbook1. Было записано сообщение с названием роутера: «router: router1».
![image](https://user-images.githubusercontent.com/58992611/207256131-e82cb2be-ff74-432b-89bd-34fe2c9782ad.png)

Теперь редактируем «playbook1.yml» для изменения имени устройства. Новым именем для роутеров станет название из «netbox_devices.csv», которое спарсится в файл «new_playbook1.yml».
![image](https://user-images.githubusercontent.com/58992611/207256206-8bd6fa8c-287b-4863-93bb-2b21bfa03dd6.png)
![image](https://user-images.githubusercontent.com/58992611/207256233-cd382219-406f-40f3-9b71-ff9d1c677631.png)
![image](https://user-images.githubusercontent.com/58992611/207256277-719c82a1-ffa5-4e63-8a09-95a2adf96394.png)

Для того, чтобы изменить данные в NetBox через Ansible, генерируем API токен в интерфейсе NetBox. Создаем такой тестовый playbook. После выполнения этого playbook в NetBox видим, что появился новый девайс “Device3”.
![image](https://user-images.githubusercontent.com/58992611/207256360-89705d3b-bc0d-4591-9de4-d0678b20b675.png)
![image](https://user-images.githubusercontent.com/58992611/207256398-da41bfb6-a174-433e-9e66-f5c1eb60207a.png)

Дописываем «playbook2.yml» для сбора информации об имени устройства. Теперь видим, что после выполнения playbook на NetBox в логах имеется информация о новом устройстве с именем из «new_playbook.yml».
![image](https://user-images.githubusercontent.com/58992611/207256492-1acf8a5d-1897-4c2e-93ad-a15649fcaa00.png)
![image](https://user-images.githubusercontent.com/58992611/207256520-c21570a1-0497-432a-b84c-f5e019bb19e9.png)

Схема связи
![image](https://user-images.githubusercontent.com/58992611/207256605-13d34c10-e85f-49b8-8b15-b4daa745d07d.png)

Вывод
После выполнения работы можно сделать вывод, что NetBox это интересный и гибкий инструмент для документирования сетей, а Ansible крайне многофункционален и с его помощью можно легко решать сложные и долгие задачи, автоматизируя весь процесс. 
