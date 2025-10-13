# Мини-гайд по использованию SNMP Агента

## Запуск
Запустите автозапуск агента командами:
```
chmod +x install_snmpagent.sh
./install_snmpagent.sh
```
Логи записываются в `snmpagent.log`. Для остановки найдите PID (`ps aux | grep SnmpAgent`) и завершите (`sudo kill PID`).

## Конфигурация
Файл `config.properties` (создаётся автоматически после первого запуска):
```
trap.receiver.address=udp:192.168.0.112/162  # Адрес куда отпралвять TRAP (сообщения об авариях) (udp:IP/PORT)
trap.community=public                   # Community string
username=admin                          # Логин от веб-интерфейса
password=password                       # Пароль от веб-интерфейса
```
Измените `trap.receiver.address` и `trap.community` для отправки уведомлений. Перезапустите агент после правок.
- Для безопасности измените `public` на сложный community string.

## Custom MIB
Используйте `KLIMATIKUM-MIB` для удобного просмотра данных:
- В iReasoning MIB Browser: File > Load MIBs > выберите `KLIMATIKUM-MIB`.
- В net-snmp: Скопируйте в `~/.snmp/mibs/` и добавьте в `snmp.conf`: `mibs +KLIMATIKUM-MIB`.

Базовый OID: `.1.3.6.1.4.1.99999`.

## Использование
Агент работает по SNMP v2c, порт 161, community: `public` (или из config).

### Через iReasoning MIB Browser (можно использовать любую программу которая поддерживает SNMP)
1. Укажите IP: 192.168.0.250 (ip машины, где крутится java приложение), порт: 161, community: public. (или из config)
2. Загрузите MIB.
3. Выполните SNMP Walk по `.1.3.6.1.4.1.99999`.
4. Для TRAP: Настройте прослушивание на порту 162

### Через командную строку (net-snmp)
- Температура в помещении: `snmpget -v 2c -c public 127.0.0.1 .1.3.6.1.4.1.99999.2.1.0`
- Таблица кондиционеров: `snmptable -v 2c -c public 127.0.0.1 .1.3.6.1.4.1.99999.1.1`
- Установить период ротации: `snmpset -v 2c -c public 127.0.0.1 .1.3.6.1.4.1.99999.3.1.0 s "01d00h00m"`
- Сброс аварий: `snmpset -v 2c -c public 127.0.0.1 .1.3.6.1.4.1.99999.3.5.0 i 1`
- TRAP: Запустите `snmptrapd -f -Lo` для прослушки.

## Примеры получения данных 
- о текущей температуре в помещении: <img width="306" height="213" alt="image" src="https://github.com/user-attachments/assets/51acf38f-5dcc-424e-b89c-865bc5436045" />
-----
- получение данных об авариях (в виде таблицы) <img width="310" height="312" alt="image" src="https://github.com/user-attachments/assets/496879a9-b53f-4774-b5ca-ed4310413f64" />
-----
- установка нового значения <img width="346" height="369" alt="image" src="https://github.com/user-attachments/assets/64763ab9-d3a9-4af3-8abd-96a7720cfdb1" />
- вводим значение в поле value и нажимаем ок <img width="587" height="393" alt="image" src="https://github.com/user-attachments/assets/9d52e800-8f56-4505-9b6f-24c61ef4c175" />
-----
- получение сообщений об авариях Tools --> Trap Receiver <img width="320" height="60" alt="image" src="https://github.com/user-attachments/assets/aa8379cd-5bdf-4386-8433-ffc7593caee6" />




## Примеры ввода данных для изменяемых объектов

- **acName** (имя кондиционера)  
  Пример: `K2 Mitshu`

- **rotationPeriod** (Период ротации)  
  Пример: `01d12h30m`

- **temperatureThreshold** (Порог температуры)  
  Пример: `25`

- **resetAlarms** (Очистка всего журнала)  
  Пример: `1`

- **temperatureRange** (Рабочий диапазон температур на выходе кондиционера)  
  Пример: `0 - 30`

- **failureDelay** (Время срабатывания Аварии кондиционера по датчику температуры в минутах)  
  Пример: `1`

- **pageRefreshInterval** (Время обновления страницы в минутах)  
  Пример: `10`

- **ipAddress**  
  Пример: `192.168.0.251`

- **netmask**  
  Пример: `24`

- **routerAddress**  
  Пример: `192.168.0.1`

- **dnsAddress**  
  Пример: `8.8.8.8`

- **smtpServer**  
  Пример: `127.0.0.1`

- **alarmEmail**  
  Пример: `alerts@klimatikum.ru`

- **c1ThermometerId**  
  Пример: `28-0000002b0901`

- **c2ThermometerId**  
  Пример: `28-0000003edc44`

- **c3ThermometerId**  
  Пример: `28-020c91777771`

- **c4ThermometerId**  
  Пример: `28-020c91777772`

- **indoorThermometerId**  
  Пример: `28-0000003edc44`

- **outdoorThermometerId**  
  Пример: `28-020c91777771`

- **c3Active**  
  Пример: `1` или `0`

- **c4Active**  
  Пример: `1` или `0`

- **ventilationActive**  
  Пример: `1` или `0`
