# Zigbee + BLE шлюз

Этот продукт  предназначен для работы с распространенными устройствами ZigBee, BLE.  В основе шлюза лежит контроллер [ESP32 от Espressif ](https://www.espressif.com/sites/default/files/documentation/esp32-wrover_datasheet_en.pdf). В качестве связущего звена протокола Zigbee  выступает тандем чипов от Texas Instruments [ZIgbee CC2538](https://www.ti.com/product/CC2538?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2538&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB37x__0Gm1rR2TUfCBETyuqrLjOtof6TuYSD3ZHzINYdNAbrXqfDxrwRoCpToQAvD_BwE&gclsrc=aw.ds) и  усилителя  [сс2592](https://www.ti.com/product/CC2592?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2592&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB3776CVlMD1GHdk-unOn9R0YeMtlwAnjUv-CIPuWvjhNqZRbiq6zy-ExoCxjYQAvD_BwE&gclsrc=aw.ds), либо готовый чип от [NXP JN5168](https://www.nxp.com/products/wireless/zigbee/zigbee-and-ieee802.15.4-wireless-microcontroller-with-256-kb-flash-32-kb-ram:JN5168). Для связи с устройствами по протоколу BLE используются встроенные возможности ESP32.

# Общие сведения
Шлюз выполняет роль координатора Zigbee и позволяет:
1) Использовать большинство доступного Zigbee оборудования. Список поддерживаемого и протестированного обрудования указан по (ссылке)[]. Новое оборудование может быть добавлено после обсуждения с нами.
2) Отказаться от необходимости использования облаков производителей устройств. В качестве альтернативы, предлагается использовать облачный сервис [Smart Logic System](https://cloud.slsys.io), либо нативные приложения для Android и Apple iPhone (в разработке). 
3) Использовать распространенные  локальные системы автоматизации, такие как [MajorDomo](https://mjdm.ru/), [ioBroker Smarthome](https://www.iobroker.net), [HomeAssisiant](https://www.home-assistant.io), [Node-Red](https://nodered.org) и др. Для интеграции с этими системами используется протокол MQTT. Структура топиков протокола MQTT идентична  проекту  [zigbee2mqtt](https://www.zigbee2mqtt.io), это позволяет использовать шлюз без какого-либо написания кода.

# Дополнительные возможности шлюза через Web интерфейс
1) Управление и просмотр сведений  устройств через Web интерфейс шлюза по адресу http://ipadress (80 порт). Возможность отображения источника питания, уровня заряда батареи, доступных [EndPoint устройств](https://community.nxp.com/thread/332332)  в web-интерфейсе.
2) Создание локальных автоматизаций внутри шлюза (Simplelink)[]
3) Возможность написания сценариев на языке [Lua](https://ru.wikipedia.org/wiki/Lua) [Книга по Lua на русском языке](https://www.htbook.ru/kompjutery_i_seti/programmirovanie/programmirovanie-na-yazyke-lua)
4.	Возможность создания групп для управления несколькими устройствами одновременно (в разработке).
5.	Возможность задавать имя устройству. Если вы планируете использовать  шлюз с локальными системами автоматизации, рекомендуется установить галочку отправки адреса вместо устройств.
5.	Возможность удаления устройства. 
6.	Возможность отображения маршрутов в web-интерфейсе (в разработке).
8.	Возможность установить (прямые связи (Bind) между устройствами ZigBee без участия координатора)[/bind.md] для управления конечными устройствами.
9.	Возможность управлять аппаратными светодиодами (адресными или RGB). 
```
Необходимо отправить в JSON значение в топик ZigBeeGW/led следующего содержания:
{"mode":"manual","hex":"#FFFFFF"}

mode - устанавливает режим, допустимы значения off, manual и auto
hex - значение цвета в RGB Hex формате
```
10.	Возможность управлять звуком (при наличии распаянного усилителя) (в разработке)
11.	Возможность изменить PanId и номер канала.
12.	Возможность задать имя шлюза в сети.
13.	Возможность перехода шлюза в режим АР при нажатии аппаратной кнопки в течение 2-5 секунд после подачи питания.
14.	Список поддерживаемых устройств постоянно обновляется (информация находится в файле converters.txt в архиве с прошивкой)
 
# Первый запуск.
После прошивки и перезагрузки устройства, шлюз создает точку доступа без пароля в формате zgwABCD. После подключения к ней, автоматически открывается страница настроек (если не открылась, можно зайти по адресу 192.168.1.1) и прописать настройки для подключения к домашней точке доступа и к MQTT серверу (но его можно указать и позже), нажимаем перезагрузку и шлюз подключится к точке доступа и начнет слать сообщения в MQTT.

# Web-интерфейс.
 
1.	Zigbee – настройки Zigbee.
2.	WiFi Setup – настройки WiFi.
3.	Link Setup – настройки сервера MQTT
4.	HW Setup – настройки аппаратной части шлюза.
5.	Log – отображение лога шлюза.
6.	Update – обновление прошивки шлюза по ОТА.
7.	Reset – сброс настроек к первоначальному состоянию.
8.	Reboot – перезагрузка шлюза.



# 1.	Zigbee
 
nwkAddr – Адрес устройства в сети. Координатор всегда имеет адрес 0x0000.
FriendlyName - Дружественное имя устройства в сети.
ieeeAddr – Физический адрес устройства в сети.
ManufName – Наименование производителя.
ModelId – Условное обозначение модели устройства.
LinkQuality – Оценки качества связи.
Interview – Процесс получения исходных данных от устройства при первой привязке устройства к шлюзу.
LastSeen – Время, которое прошло с момента последнего сообщения от устройства.
Routes – Адрес устройства, являющегося маршрутизатором.
PS – Заряд батареи в %, при условии, что устройство питается от батареи. Если устройство питается от сети, то отображается значок «≈»
Actions: управление устройствами. Удалить устройство, задать имя

Back – вернуться на предыдущую страницу.
Refresh – Обновить текущую страницу.
Groups – создание, редактирование, удаление групп. (На стадии разработки)
 
Setup – настройка номера канала и PanId.  Возможность выбора формата передаваемых данных MQTT.
 
Start Join – запуск режима спаривания устройств в течение 255 секунд.

#2.	WiFi Setup
 








#3.	Link Setup
 

 










#4.	HW Setup
 
 В данном меню можно выбрать тип модуля Zigbee TI или NXP, а также указать номера GPIO модуля ESP32: 
1.	RX
2.	TX
3.	Аппаратной кнопки. (если кнопка притянута к земле, а при нажатии на нее на вход ESP32 подается 3.3В, то обязательно нужно ставить галку PullUp.
4.	Красный светодиод. (зажигается, когда приходит сообщение от конечного zigbee устройства)
5.	Зеленый светодиод. (зажигается, когда активен режим Join)
6.	Синий светодиод.

 


#5.	Log 
 
#6.	Update 
 
#7.	Reset 
Очистка памяти модуля Zigbee, удаление привязанных устройств.
 
Координатор + шлюз для модернизации оригинального шлюза Xiaomi.
(круглая плата)
Для данной платы используется та же прошивка, соответственно на нее распространяются те же правила, что и для простого шлюза SLS. Описано в FAQ.
Однако есть несколько особенностей, которые относятся только к этой плате.
Так как она собрана по схеме уважаемого @Jager, следует иметь ввиду следующее:
1.	При первом запуске, после настройки параметров WiFi, обязательно нужно зайти в настройки HW Setup и выставить настройки GPIO следующим образом:
 
PullUp – галка нужна обязательно, дабы исключить постоянные зависания шлюза при старте, которые устраняются коротким нажатием кнопки SW1.
2.	
 
#FAQ.
Вопрос: Какое оборудование необходимо для шлюза на 2530
Ответ: Вот подборка с Aliexpress для сборки шлюза без пайки:
1.	Модуль ESP32 16Mb Flash 8Mb SRAM для прошивки по OTA или Модуль ESP32 c 4Mb flash для прошивки без OTA
2.	Модуль CC2530 без усилителя но с внешней антенной
3.	Отладчик СС Debubber для прошивки модуля CC2530
Вопрос: Какое оборудование необходимо для шлюза на 2538?
Ответ: Вот подборка с Aliexpress для сборки шлюза с минимумом пайки (только проводки):
1.	Модуль ESP32 16Mb Flash 8Mb SRAM для прошивки по OTA или Модуль ESP32 c 4Mb flash для прошивки без OTA
2.	Модуль CC2538 с усилителем СС2592
3.	Программатор J-Link V9


Вопрос: Какую прошивку выбрать для модуля ZigBee
Ответ: Все зависит от того какой у вас модуль и усилитель.
Прошивка должна быть обязательно основана на Z-Stack 3.0.

Для прошивки через CC Debugger:
Прошивка для модуля CC2530 без усилителя
Прошивка для модуля CC2530 с усилителем СС2591
Прошивка для модуля CC2530 с усилителем СС2592

Для прошивки через J-Link:
Прошивка для модуля CC2538 с усилителем СС2592

Вопрос: Как прошить ESP32
Ответ:
Для первоначальной прошивки:
1. Загрузить архив с прошивальщиком (full) 
2. Подключить ESP32 к компьютеру через USB
3. Запустить прошивку через Flash.bat
4. Иногда батник неверно определяет порт, тогда можно дописать в батник --port COM7
Для дальнейшего обновления:
1. Загрузить архив с актуальной версией прошивки 
2. Распаковать его в любую папку
3. В веб интерфейсе выбрать на странице Update файл firmware.bin
4. Нажать Start update.

Вопрос: Как прошить CC2530
Ответ:
Либо перейти по ссылке


Вопрос: Как прошить CC2538
Ответ:
Либо перейти по ссылке




Вопрос: Как добавлять устройства.
Ответ: Есть два способа:
1. Включить режим присоединения на странице ZigBee в веб-интерфейсе (кнопка Start Join)
2. Можно послать значение true / false в топик ZigBeeGW/bridge/config/permit_join

Вопрос: Как задавать правила SimpleBind
Ответ: Есть два формата записи:
1.	DstDeviceId
2.	Cond, DstDeviceId, DstStateName, DstStateValue (Разделение запятыми, пробелы допускаются)
где:
•	Cond - значение при котором будет выполняться правило
•	DstDeviceId - Идентификатор устройства которому будем отправлять команду
•	DstStateName - Имя состояния которое будем отправлять
•	DstStateValue - Значение которое будем отправлять
Перед значением в поле Cond можно использовать знаки сравнения. (>, <, =, !, >=, <=, !=, <>)
Можно использовать несколько правил, разделяя их точкой с запятой.
Примеры:
•	single, lamp_1, state, TOGGLE - Для кнопки, при одиночном нажатии переключает режим lamp_1
•	ON, 0x00158D00007350D9, state, OFF; OFF, 0xABCD, state, ON - Для выключателя, инвертирует режим для реле
•	single, door_lock, state, LOCK; double, door_lock, state, UNLOCK - Закрывает замок при клике, открывает при двойном
•	torsher_lamp - Передает в torsher_lamp текущее состояние
•	<40, humidifier, state, ON; >60, humidifier, state, OFF - Для датчика влажности, включает увлажнитель если влажность меньше 40% и выключает если больше 60%

Пример:
left, PTVO, state_bottom_left, TOGGLE; right, PTVO, state_bottom_right, TOGGLE

Вопрос: Как задать цвет лампочке или RGB контроллеру
Ответ:
Необходимо отправить в состояние color json объект содержащий один из вариантов задания цвета:
1. В родном формате CIE 1931: {"x": 0.8, "y": 0.04}
2. В формате RGB: {"r": 0, "g": 255, "b": 0}
3. В формате RGB HEX: {"hex": "#RRGGBB"}
4. Тон, насыщенность: {"hue": 23525, "saturation": 80}
5. Тон: {"hue": 1665}
6. Насыщенность: {"saturation": 220}

Пример:
Отправка в топик ZigBeeGW/0x00158D00011D8CB1/set значения: {"color":{"r":0,"g":255,"b":0}}


Вопрос: Как задать цветовую температуру лампочке
Ответ:
Необходимо отправить в состояние color_temp значение в Майред единицах измерения.
Формула для преобразования: M = 1000000 / K где K - температура в Кельвинах.

Пример:
Цветовая температура 4000К, задаем в ZigBeeGW/lamp_1/set/color_temp значение 250

Вопрос: Как управлять аппаратными светодиодами?
Ответ:
Необходимо отправить в JSON значение в топик ZigBeeGW/led следующего содержания:
{"mode":"manual","hex":"#FFFFFF"}

mode - устанавливает режим, допустимы значения off, manual и auto
hex - значение цвета в RGB Hex формате

