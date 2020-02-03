# SerialToMQTT
Шлюз для обмена данными с релейным блоком Shturman Light версии R9V11.

Работает на Node.JS v8.16.х и выше

Разрабатывался для передачи данных в Wiren Board 6, но так же будет нормально работать с любым другим MQTT брокером.

## Установка
`git clone https://github.com/haba1234/SerialToMQTT.git /opt/SerialToMQTT`
`sudo apt-get install build-essential`
`cd /opt/SerialToMQTT`
`npm install`

## Настройка
Выставить настройки в "setup.json".
Где:
- "timer_read_states" - период чтения состояний реле
- "timer_write_port" - частота записи команд из стека в порт (должна быть меньше чем предыдущий параметр, иначе возможно переполнение стека)
- "devices" - массив реле. Может быть от 1 до нескольких штук. Адреса должны быть все разные.

## Тестирование
Для проверки работы выполнить `npm start`.

## Автозапуск
Автозапуск через SystemD

В `/etc/systemd/system/` создать файл `serialtomqtt.service`

```
[Unit]
Description=SerialToMQTT
After=network-online.target

[Service]
Restart=always
WorkingDirectory=/opt/SerialToMQTT/
ExecStart=/usr/bin/node /opt/SerialToMQTT/index.js
User=root

[Install]
WantedBy=multi-user.target
```

Для включения сервиса в консоли:
`systemctl enable serialtomqtt.service`

После этого можно управлять командами:
`service serialtomqtt start`
`service serialtomqtt stop`
`service serialtomqtt restart`

## Настройка Wiren Board 6
```
defineVirtualDevice("R8V11_0", {
    title: "R8V11. addr = 0",
    cells: {
		CH1: {
			type: "switch",
			value: false
		},
		CH2: {
			type: "switch",
			value: false
		},
		CH3: {
			type: "switch",
			value: false
		},
		CH4: {
			type: "switch",
			value: false
		},
		CH5: {
			type: "switch",
			value: false
		},
		CH6: {
			type: "switch",
			value: false
		},
		CH7: {
			type: "switch",
			value: false
		},
		CH8: {
			type: "switch",
			value: false
		},
    }
});
```