# Лабораторная работа. Базовая настройка коммутатора
## Топология

![alt-текст](https://github.com/Kirill11119/LAB01/blob/main/pic1.png "Топология")

## Задачи
### Часть 1. Проверка конфигурации коммутатора по умолчанию
### Часть 2. Создание сети и настройка основных параметров устройства 
- Настройте базовые параметры коммутатора.
- Настройте IP-адрес для ПК.
### Часть 3. Проверка сетевых подключений
-	Отобразите конфигурацию устройства.
-	Протестируйте сквозное соединение, отправив эхо-запрос.
-	Протестируйте возможности удаленного управления с помощью Telnet.

## Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
Подсоединяем консольный кабель как показано в топологии (порты Console и RS232) и подключаемся к коммутатору.
Проверяем настройки, предварительно зайдя в привелегированный режим:
```
enable
show running-config
show startup-config
show version

```
коммутатоор имеет 24 порта fastEthernet и 2 порта GigabitEthernet,
диапазон значений, отображаемых в vty-линиях от 0 до 15, IP-адресс на интерфейс vlan1 не назначен, сам интрефейс выключен.

Подсоединяем кабель Ethernet компьютера PC-A к порту 6 на коммутаторе.
Видим следующее
```
%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to up
```
Даём следующие команды:
```
show interface f0/6
show flash
dir flash
```
Интерфейс f0/6 включен, flhtc 000a.4104.0c06, rfyfk Full-duplex, 100Mb/s, версия ОС lanbasek9-mz.150-2.SE4, имя образа *2960-lanbasek9-mz.150-2.SE4.bin*

## Часть 2. Настройка базовых параметров сетевых устройств
В режиме глобальной конфигурации выполняем следующие команды:
```
no ip domain-lookup
hostname S1
service password-encryption
enable secret cisco
banner motd !***S1 switch***!
```
Для терминалов с 0 по 4 настраиваем доступ по telnet с паролем cisco:
```
line vty 0 4
transport input telnet
password cisco
login
transport input telnet
```
Настраиваем IP-адрес ПК

Настраиваем IP-адрес интерфейса VLAN1:
```
ip address 192.168.1.2 255.255.255.0
```

## Часть 3. Проверка сетевых подключений
Отображаем конфигурацию коммутатора:
```
S1#show run
S1#show running-config 
Building configuration...

Current configuration : 1287 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
banner motd ^C***S1 switch***^C
!
!
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 login
 transport input telnet
line vty 5 15
 login
!
!
!
!
end
```
Отображаем конфигурацию интерфейса vlan 1:
```
S1#show interfaces vlan 1
Vlan1 is up, line protocol is up
  Hardware is CPU Interface, address is 0040.0bed.6753 (bia 0040.0bed.6753)
  Internet address is 192.168.1.2/24
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 21:40:21, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1682 packets input, 530955 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicast)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     563859 packets output, 0 bytes, 0 underruns
     0 output errors, 23 interface resets
     0 output buffer failures, 0 output buffers swapped out
```
как видим полоса пропускания 100 Мбит/с.

В командной строке ПК выполняем следующие команды:
```
C:\>ping 192.168.1.10

Pinging 192.168.1.10 with 32 bytes of data:

Reply from 192.168.1.10: bytes=32 time=86ms TTL=128
Reply from 192.168.1.10: bytes=32 time=1ms TTL=128
Reply from 192.168.1.10: bytes=32 time=3ms TTL=128
Reply from 192.168.1.10: bytes=32 time=1ms TTL=128

Ping statistics for 192.168.1.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 86ms, Average = 22ms

C:\>ping 192.168.1.2

Pinging 192.168.1.2 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255
Reply from 192.168.1.2: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

```
Подключаемся с ПК по telnet  к коммутатору:
```
C:\>telnet 192.168.1.2
Trying 192.168.1.2 ...Open***S1 switch***


User Access Verification

Password: 
S1>enable
Password: 
S1#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#exit

[Connection to 192.168.1.2 closed by foreign host]
```

