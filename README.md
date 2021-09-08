
ESP-OT это проект контроллера на основе ESP8266/ESP32 для управления работой котла с помощью адаптера (http://ihormelnyk.com/opentherm_adapter)
или его самодельных аналогов и протокола Opentherm (http://ihormelnyk.com/opentherm_library).

В папке \ESP-OT-Lite находится исходный код ядра прошивки, обеспечивающий подключение к WiFi сети и взаимодействие с котлом, 
без веб-интерфейса. Данный код при желании может быть самостоятельно дополнен веб-интерфейсом и доработан под свои потребности.

В папке \ESP-OT-Lite-v2 находится обновленная версия кода ядра прошивки, переписанная с целью более гибкой организации работы с конкретным котлом.
Так при запуске происходит однократный опрос всех ID на предмет получения разумного ответа от котла и маркировкой ID, на которые котел не отвечает.
В дальнейшей работе с котлом используются только "читаемые" ID. Таким образом код прошивки подстраивается под конкретную модель котла.
Первичный опрос занимает около 15 сек, но работа основного цикла при этом не блокируется, т.к. все запросы делаются в асинхронном режиме.
Алгоритм опроса "читаемых" ID настраивается в коде указанием интервала опроса для каждого ID, либо указанием необходимости только однократного опроса.
Расшифровка ответов котла в приведенном коде сделана только для тех ID, которые поддерживает мой котел. Расшифровка других ID может быть добавлена самостоятельно.
В частности, в процессе тестирования выяснилось, что мой котел поддерживает запрос "Fan Speed" (ID=35), которого нет в описании протокола Opentherm.
Получить отчет о конфигурации вашего котла, а также о поддерживаемых ID, возможно с помощью кнопки "Opentherm report" в бесплатной версии прошивки.

В папке \ESP-OT-Free находится собранный и готовый для работы вариант прошивки для тестирования, обеспечивающий
подключение к выбранной WiFi сети, отображение и ручное управление основными параметрами работы котла через встроенный веб-интерфейс, 
формирование отчета поддерживаемых котлом DataID, отображение и управление параметрами работы термостата. 
Единственное ограничение - рестарт прошивки каждые 24 часа. 
ESP-OT-Free.D1-mini.bin - прошивка для контроллера Wemos D1 mini (NodeMCU). Подключение OT адаптера: выход адаптера (входной сигнал контроллера от котла) - пин D2 (GPIO-04), 
вход адаптера (выходной сигнал контроллера на котел) - пин D1 (GPIO-05). Подключение датчика DS18B20 - пин D5 (GPIO-14). Период опроса датчика 30 сек.
ESP-OT-Free.D1-mini32.bin - прошивка для контроллера ESP32 D1 mini. Подключение OT адаптера: выход адаптера (входной сигнал контроллера от котла) - пин GPIO-21, вход адаптера (выходной сигнал контроллера на котел) - пин GPIO-22. Подключение датчика DS18B20 - пин GPIO-18. Период опроса датчика 30 сек.
После запуска прошивки делается попытка использовать данные последнего удачного подключения к WiFi сети (моргает светодиод).
При отсутствии таких данных, либо при невозможности подключения запускается режим точки доступа (горит светодиод), пароль 12345678.
В дальнейшем возможно подключение к другой WiFi сети с помощью веб-страницы "Configure WiFi".
Прошивка также может быть собрана для других типов контроллеров. 

В папке \HomeAssistant выложены YAML файлы для настройки MQTT топиков контроллеров в HomeAssistant - Сенсоры, Двоичные Сенсоры и Переключатели. 
Предварительно нужно в configuration.yaml прописать путь к ним и в файлах заменить имя хоста в топиках на свое.

Полная версия контроллера собирается под задачи заказчика и дополнительно может включать в себя следующие возможности:
- чтение проводных датчиков температуры воздуха, теплоносителя, подключенных непосредственно к контроллеру,
- чтение беспроводных датчиков, в том числе по протоколам BLE (датчики умных домов) и RF-433 (выносные датчики метеостанций),
- получение данных с датчиков и с других устройств по сети (MQTT, HTTP GET),
- управление отопительным и другим оборудованием (насос, клапан двухпозиционный, клапан с сервоприводом),
- алгоритмы термостатирования (автоматическое управление работой котла на основе заданных параметров),
- отправка всех параметров работы контроллера на MQTT брокер для дальнейшего сбора, отображения и анализа информации,
- управление работой котла и термостата через MQTT брокер,
- использование OLED дисплея для отображения текущих параметров работы котла и управление с помощью сенсорных кнопок,
- использование GSM модуля для информирования и управления работой котла с помощью SMS сообщений,
- сохранение всех параметров настройки и работы контроллера в файле с возможностью редактирования через веб-интерфейс,
- взаимодействие с другими контроллерами в составе комплексной системы управления отоплением.

Работа с BLE устройствами (датчики) возможна в двух режимах: в режиме подключения к устройству (1) и в режиме приема рекламных пакетов (2).
В первом случае работа возможна только с одним BLE устройством, но такой режим не прерывает основную работу прошивки.
Во втором случае возможен прием рекламных пакетов с любого кол-ва BLE устройств, но такой режим требует периодического активного сканирования радиоэфира, 
что оказывает непосредственное влияние на основную работу прошивки.

Весь код для работы перечисленных функций (за исключением используемых сторонних библиотек) разработан и протестирован 
автором в реальных условиях и гарантирует надежную работу контроллера в режиме Fail-Safe.
Возможна сборка контроллера с адаптером и дисплеем (при необходимости) в стандартном корпусе на DIN-рейку.
Полная версия ПО контроллера предоставляется в виде готовой прошивки на коммерческой основе.

Для связи с автором писать на почту esp-ot@mail.ru

