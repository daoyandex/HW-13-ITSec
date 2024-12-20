# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Алексеев Александр`
---

### Задание 1
Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.  
Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.  

Просканируйте эту виртуальную машину, используя nmap.  

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.  
Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.  
Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.  

Ответьте на следующие вопросы:  
Какие сетевые службы в ней разрешены?  
Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)  
Приведите ответ в свободной форме.  

### Ответ:   
**Какие сетевые службы в ней разрешены?**  
``` bash
user@debian:~$ nmap -sV 172.16.111.128
Starting Nmap 7.93 ( https://nmap.org ) at 2024-12-07 22:06 MSK
Nmap scan report for 172.16.111.128
Host is up (0.0017s latency).
Not shown: 977 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.48 seconds
user@debian:~$ 
```
**Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)**

| PORT     | SERVICE     | VERSION                      | VULNURABILITY                                                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 21/tcp   | ftp         | vsftpd 2.3.4                 |   vsftpd 2.3.4 - Backdoor Command Execution                           |
|          |             |                              |     https://www.exploit-db.com/exploits/49757                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 2121/tcp | ftp         | ProFTPD 1.3.1                |   ProFTPd 1.x - 'mod_tls' Remote Buffer Overflow                      |
|          |             |                              |     https://www.exploit-db.com/exploits/4312                          |
|          |             |                              |   ProFTPd IAC 1.3.x - Remote Command Execution                        |
|          |             |                              |     https://www.exploit-db.com/exploits/15449                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 3306/tcp | mysql       | MySQL 5.0.51a-3ubuntu5       | Свыше 40 уязвимостей, например                                        | 
|          |             |                              |   MySQL yaSSL (Linux) - SSL Hello Message Buffer Overflow (Metasploit)| 
|          |             |                              |     https://www.exploit-db.com/exploits/16849                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 5432/tcp | postgresql  | PostgreSQL DB 8.3.0 - 8.3.7  |   PostgreSQL 8.3.6 - Conversion Encoding Remote Denial of Service     |
|          |             |                              |     https://www.exploit-db.com/exploits/32849                         |
|          |             |                              |   PostgreSQL 8.3.6 - Low Cost Function Information Disclosure	        |
|          |             |                              |     https://www.exploit-db.com/exploits/32847                         |
|          |             |                              |   PostgreSQL 8.2/8.3/8.4 - UDF for Command Execution                  |
|          |             |                              |     https://www.exploit-db.com/exploits/7855                          |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 5900/tcp | vnc         | VNC (protocol 3.3)           |   VNC Keyboard - Remote Code Execution (Metasploit)                   |
|          |             |                              |     https://www.exploit-db.com/exploits/37598                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 6667/tcp | irc         | UnrealIRCd                   |   UnrealIRCd 3.2.8.1 - Backdoor Command Execution (Metasploit)        |
|          |             |                              |     https://www.exploit-db.com/exploits/16922                         |
|----------|-------------|------------------------------|-----------------------------------------------------------------------|
| 8180/tcp | http        | Apache Tomcat/Coyote         |   Apache Tomcat - AJP 'Ghostcat' File Read/Inclusion (Metasploit)     |
|          |             | JSP engine 1.1               |     https://www.exploit-db.com/exploits/49039                         |



### Задание 2  
Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.  
Запишите сеансы сканирования в Wireshark.  
  
Ответьте на следующие вопросы:  
Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?  
Как отвечает сервер?  
Приведите ответ в свободной форме.  

### Ответ:  

1. SYN сканирование (-sS )- отправляет SYN пакеты (как при начале TCP соединения) на разные порты целевой машины. Если сервер отвечает SYN-ACK (знак того, что порт открыт), сканер отправляет RST (сброс), чтобы избежать полного установления соединения. Это минимизирует логирование на целевой системе. Однако, чувствительные системы обнаружения вторжений и даже персональные брандмауэры вполне способны обнаруживать сканирования SYN по умолчанию.  
  
2. FIN сканирование  (-sF ) отправляет FIN пакеты, которые обычно отправляются для закрытия соединения. По стандарту TCP, если порт закрыт, то в ответ на FIN пакет должен прийти RST от целевой системы. Открытые порты, в теории, не отвечают на FIN пакеты в большинстве операционных систем, что может использоваться для обнаружения открытых портов без установления полного соединения.
  
3. Xmas сканирование  ( -sX ) отправляются пакеты с установленными флагами FIN, PSH и URG. Принцип аналогичен вышеописанным: открытые порты не отвечают на такие пакеты, а закрытые отправляют RST.
  
4. UDP сканирование ( -sU ) отличается от предыдущих тем, что оно направлено на UDP порты, а не TCP. UDP — это протокол без установления соединения, так что сканирование отправляет UDP пакеты на различные порты. Если порт закрыт, то система должна ответить сообщением о недостижимости порта (ICMP port unreachable error).  
Если ответа нет, сканер может предположить, что порт открыт или фильтруется. Для уточнения состояния порта и службы необходимо более точное формирование пакета.  
Чтобы отправить правильный пакет для каждого популярного сервиса UDP, Nmap понадобится большая база данных, определяющая их форматы зондов.  
Nmap имеет это в виде nmap-service-probes, которая является частью подсистемы обнаружения служб и версий.  
UDP сканирование в общем случае медленнее и сложнее TCP, однако, существуют UDP службы, которые используются атакующими. Тремя наиболее популярными сервисами, использующими протокол UDP являются DNS, SNMP и DHCP (используют порты 53, 161/162 и 67/68). Nmap позволяет инвентаризировать UDP порты.

Все эти методы сканирования создают различные типы сетевых пакетов и ориентируются на разные ответы, что позволяет обнаруживать открытые, закрытые или фильтруемые порты без полного установления соединения.