# Название выполняемого задания
Поработать с репликацией MySQL.

# Текст задания
1) Настроить GTID репликацию

# Описание

Запуск
```bash
vagrant up
```

Подключаемся к `slavemy` запрашиваем отчет, убеждаемся что репликация работает
```bash
sergey@fedora:~/Otus/homework/28/ALP28$ vagrant ssh slavemy 
Welcome to Ubuntu 24.04.2 LTS (GNU/Linux 6.8.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Jun 25 12:53:39 PM UTC 2025

  System load:  0.11               Processes:             158
  Usage of /:   16.6% of 30.34GB   Users logged in:       0
  Memory usage: 16%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

Use of this system is acceptance of the OS vendor EULA and License Agreements.
Last login: Wed Jun 25 12:49:54 2025 from 10.0.2.2
vagrant@slavemy:~$ sudo mysql -uroot -p111 -D bet
mysql: [Warning] Using a password on the command line interface can be insecure.
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 22
Server version: 8.0.42-0ubuntu0.24.04.1 (Ubuntu)

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for source to send event
                  Master_Host: 192.168.12.150
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: binlog.000011
          Read_Master_Log_Pos: 157
               Relay_Log_File: slavemy-relay-bin.000003
                Relay_Log_Pos: 367
        Relay_Master_Log_File: binlog.000011
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: bet.events_on_demand,bet.v_same_event
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 157
              Relay_Log_Space: 759
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 1
                  Master_UUID: c43c6f04-51c0-11f0-9a7f-08002764e1ff
             Master_Info_File: mysql.slave_master_info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Replica has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind: 
      Last_IO_Error_Timestamp: 
     Last_SQL_Error_Timestamp: 
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
           Retrieved_Gtid_Set: 
            Executed_Gtid_Set: 
                Auto_Position: 1
         Replicate_Rewrite_DB: 
                 Channel_Name: 
           Master_TLS_Version: 
       Master_public_key_path: 
        Get_master_public_key: 1
            Network_Namespace: 
1 row in set, 1 warning (0.00 sec)
```

Подключаемся к `mastermy`, просматриваем содержимое таблицы `bookmaker`, добавляем в эту таблицу запись, сного запрашиваем записи таблицы, убеждаемся что запись добавилась.
```bash
sergey@fedora:~/Otus/homework/28/ALP28$ vagrant ssh mastermy
Welcome to Ubuntu 24.04.2 LTS (GNU/Linux 6.8.0-31-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Jun 25 12:55:25 PM UTC 2025

  System load:  0.0                Processes:             144
  Usage of /:   16.6% of 30.34GB   Users logged in:       0
  Memory usage: 18%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

Use of this system is acceptance of the OS vendor EULA and License Agreements.
Last login: Wed Jun 25 12:42:34 2025 from 10.0.2.2
vagrant@mastermy:~$ sudo mysql -uroot -p111 -D bet
mysql: [Warning] Using a password on the command line interface can be insecure.
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.0.42-0ubuntu0.24.04.1 (Ubuntu)

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
4 rows in set (0.00 sec)

mysql> INSERT INTO bookmaker (id,bookmaker_name) VALUES(1,'1xbet');
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 sec)
```

Подключаемся  `slavemy`, запрашиваем значение таблицы 'bookmaker'. 
```bash
vagrant@slavemy:~$ sudo mysql -uroot -p111 -D bet
mysql: [Warning] Using a password on the command line interface can be insecure.
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 23
Server version: 8.0.42-0ubuntu0.24.04.1 (Ubuntu)

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 s
```

Видим что репликация сработала запись появилась.

## Проверка фильтрации по именам

Проверяем что репликация работает не для всех объектов.

В `mastermy`, запрашиваем список таблиц в базе `bet`, потом добавляем две таблицы, проверяем что они появились.
```bash
mysql> SHOW TABLES;
+---------------+
| Tables_in_bet |
+---------------+
| bookmaker     |
| competition   |
| market        |
| odds          |
| outcome       |
+---------------+
5 rows in set (0.00 sec)

mysql> CREATE TABLE otus_forever ( id INT PRIMARY KEY);
Query OK, 0 rows affected (0.01 sec)

mysql> CREATE TABLE events_on_demand ( id INT PRIMARY KEY);
Query OK, 0 rows affected (0.03 sec)

mysql> SHOW TABLES;
+------------------+
| Tables_in_bet    |
+------------------+
| bookmaker        |
| competition      |
| events_on_demand |
| market           |
| odds             |
| otus_forever     |
| outcome          |
+------------------+
7 rows in set (0.00 sec)

```

Запрашиваем списое таблиц в `slavemy`
```bash
mysql> SHOW TABLES;
+---------------+
| Tables_in_bet |
+---------------+
| bookmaker     |
| competition   |
| market        |
| odds          |
| otus_forever  |
| outcome       |
+---------------+
6 rows in set (0.01 sec)
```

Убеждаемся что перенеслась только одна таблица. Все работает!
