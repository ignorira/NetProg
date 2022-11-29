University: ITMO University

Faculty: ICT

Course: Network programming

Year: 2022/2023 

Group: K34212 

Author: Sorokina Irina

Lab2 

Цель работы
С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

Ход работы
Устанавливаем второй CHR и второго OpenVPN клиента на нем. Создаем вторую виртуальную машину Mikrotik по аналогии с первой лабораторной работой. И в результате имеем 2 ВМ, настроенные как OpenVPN client.
![image](https://user-images.githubusercontent.com/58992611/204485425-309e1246-fca5-40d2-8dde-6a9ba168f104.png)
![image](https://user-images.githubusercontent.com/58992611/204485486-c06e990e-9f4c-43a4-b553-fa07b72a9334.png)

Затем создаем файл hosts в /etc/ansible в котором размещаем информацию о локальных ip двух Mikrotik RouterOS.
![image](https://user-images.githubusercontent.com/58992611/204485525-d8ff1ad4-c1d2-4294-9406-c371e68f5a6d.png)

Настраиваем SSH соединения для виртуальных машин через winbox.
![image](https://user-images.githubusercontent.com/58992611/204485570-0468b0bf-00cc-4b2d-a745-91cfa7ec27bc.png)

В VirtualBox добавляем NAT соединение и прописываем SSH с портами. Проверяем подключение на сервере командой ssh -p 22 admin@172.27.244.8. После чего вводим установленный на CHR пароль.
![image](https://user-images.githubusercontent.com/58992611/204485666-8f695ba6-c7bc-4b7e-a8e9-53d85538a2f8.png)

 

Создаем первый playbook.yml файл в /etc/ansible. 
Для проверки соединения выводим информацию о роутерах с помощью "/system resource print". 
Также записываем команды, требующиеся для задания.
![image](https://user-images.githubusercontent.com/58992611/204485788-b8c97674-ff74-46bb-b194-9b048beb6c18.png)

Результат выполнения playbook, команда «ansible-playbook playbook.yml».
![image](https://user-images.githubusercontent.com/58992611/204485847-39c1c8d2-7a4d-4451-89be-e66fe73d8412.png)
 

Проверим, что новый пользователь был создан на обоих роутерах.
![image](https://user-images.githubusercontent.com/58992611/204485932-da1538d0-cea4-452a-b172-1e1e1736da8b.png)
![image](https://user-images.githubusercontent.com/58992611/204485964-0ff77e87-712b-4c14-bdab-0bae29e7ecd7.png)
 
 
Изменение времени также отобразилось в консоли на обоих роутерах.
![image](https://user-images.githubusercontent.com/58992611/204486025-b9e27ba0-9354-4d31-992d-dd8b42d3f11b.png)
![image](https://user-images.githubusercontent.com/58992611/204486049-f33ba9a1-7e9c-42dc-be14-c24880ae9d53.png)
 
 
Для проверки OSPF в winbox перейдем в /ip/neighbors и там должен появиться сосед.
![image](https://user-images.githubusercontent.com/58992611/204486100-d6730181-f7cd-4629-bd41-fb998a13e083.png)
 
Выполняем команду ping для роутеров в полученной локальной сети.
![image](https://user-images.githubusercontent.com/58992611/204486130-ab6dc08a-f737-435a-bcd9-051bebffa5ee.png)
![image](https://user-images.githubusercontent.com/58992611/204486153-61c85a61-cc06-4784-a57e-c6187518d5e7.png)
 
  
Сохраняем конфигурацию, после чего в /files можно будет найти нужный файл и скачать на основную машину.
![image](https://user-images.githubusercontent.com/58992611/204486202-891517fa-b1fd-4d25-aec7-391d452fdd0f.png)
 
Нарисуем схему связи.
![image](https://user-images.githubusercontent.com/58992611/204486261-76123c02-fbc9-4009-b7b5-9a7f4b94541f.png)

Вывод
В ходе выполнения работы было получено первичное представление об Ansible, как о мощном инструменте управления конфигурацией. Убедились в том, что его удобно использовать, когда в сети есть множество машин с незначительными отличиями.
