University: ITMO University

Faculty: ICT

Course: Network programming

Year: 2022

Group: K34212

Автор: Сорокина Ирина Игоревна

Lab: 1

Цель работы: развертывание виртуальной машины с установленной системой контроля конфигураций Ansible, установка CHR в VirtualBox, установка VPN туннеля между машиной Ubuntu и RouterOS.

Ход работы:
1. Развернула ВМ «Ubuntu» на платформе «Яндекс Облако».
![image](https://user-images.githubusercontent.com/58992611/199180093-64bb5b80-cc43-4df3-9d1d-5521277f7c84.png)
Информация о ВМ

2. Установка системы контроля конфигураций «Ansible» на ВМ.
•	Обновила ВМ «Ubuntu» с помощью команд:

sudo apt update;

sudo apt upgrade.

•	Установила python3 и Ansible:

sudo apt install python3-pip;

sudo pip3 install ansible.

•	Подключилась к виртуальной машине в Яндекс.Облаке по ssh.
 ![image](https://user-images.githubusercontent.com/58992611/199180918-bb5adc5b-94d8-46a3-8f29-d12d67a8fbd8.png)



3. Установка Mikrotik RouterOS на VirtualBox

•	Загрузила Mikrotik

•	Установила CHR на VirtualBox с помощью загруженного файла (после загрузки в настройках VirtualBox отключила загрузочный диск вручную).

4. Создание Wireguard сервера на машине Ubuntu и настройка Wireguard клиента на RouterOS.

•	Устанавила Wireguard на удалённую машину (sudo apt install wireguard), прописала в нём в файл условий интерфейса. Создала пару ключей.

•	Затем начала работу с MikroTik. Поставила эмулятор графического интерфейса, умеющий запускать exe файлы. Подключилась к виртуальной машине MikroTik.
![image](https://user-images.githubusercontent.com/58992611/199181037-df59bd41-909d-4e61-9238-2d0073293932.png)
 
 
 
•	Поскольку в качестве VPN был выбран WireGuard, и установлен MikroTik последней версии, зашла в настройки Wireguard и создала там новый интерфейс. Заполнила строки: 

•	MTU оставляем 1420;

•	 ListenPort узнаём командой вывода файла /etc/wireguard/wg0.conf на сервере (13231);

•	ключи сгененрированы автоматически.
![image](https://user-images.githubusercontent.com/58992611/199181073-f5239110-d1fe-45bf-9f16-805c164cc28a.png)
 


•	Перешла во вторую вкладку, там создала Peer, указала там:

•	созданный интерфейс wireguard1;

•	публичный ключ сервера;

•	публичный IP сервера;

•	указанный ранее порт;

•	сеть адресов, по которым будет идти подключение.
![image](https://user-images.githubusercontent.com/58992611/199181130-0cd820e7-cf24-4880-9a7b-d35b5c027824.png)
 

•	В настройке IP - Addresses указала выбранную сеть и созданный интерфейс.
![image](https://user-images.githubusercontent.com/58992611/199181165-53ee9e17-d7fd-43ae-b6fb-28bb8b69b8a8.png)
 

•	В настройке IP - Firewall создала Masquerade NAT. Он позволяет преобразовывать несколько IP-адресов в другой одиночный IP-адрес. 
![image](https://user-images.githubusercontent.com/58992611/199181200-35764b85-4232-461a-94f2-b55f21f3f421.png)
 

•	Запустила ping к ВМ по IP адресу 192.168.6.1 для проверки работоспособности.
![image](https://user-images.githubusercontent.com/58992611/199181239-fee91390-b8dd-46b6-a549-e5f6805aeb9a.png)
 

•	В результате получаем связь по следующей схеме. 
![image](https://user-images.githubusercontent.com/58992611/199181271-135f1a09-0ae4-4a8e-8ba6-be157183129b.png)
 

Вывод: 
В ходе выполнения лабораторной работы мне удалось развернуть ВМ с установленной системой контроля конфигураций Ansible, установить CHR в VirtualBox, и пробросить VPN туннель между ВМ Ubuntu с ОС RouterOS  и удаленной виртуальной машиной при помощи WireGuard.
