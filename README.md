
Контроллер на основе ESP8266/ESP32 для управления работой котла с помощью адаптера (http://ihormelnyk.com/opentherm_adapter)
или его самодельных аналогов и протокола Opentherm (http://ihormelnyk.com/opentherm_library).

В папке \ESP-OT-Lite находится исходный код ядра прошивки, обеспечивающий подключение к WiFi сети и взаимодействие с котлом, 
без веб-интерфейса. Данный код при желании может быть самостоятельно дополнен веб-интерфейсом и доработан под свои потребности.
В планах переписать алгоритм взаимодействиия с котлом, сделав его более гибким и использующим все DataID, поддерживаемые котлом.

В папке \ESP-OT-Free находится собранный и готовый для работы вариант прошивки для свободного использования, обеспечивающий
подключение к выбранной WiFi сети, а также отображение и ручное управление основными параметрами работы котла через веб-интерфейс и 
тестирование DataID, поддерживаемых котлом, с формированием веб-страницы с результатами отчета.
Прошивка собрана для контроллера Wemos D1 mini (NodeMCU). Также может быть собрана для других контроллеров.
Подключение адаптера: выход адаптера (входной сигнал контроллера от котла) - пин D1 (GPIO-05), 
вход адаптера (выходной сигнал контроллера на котел) - пин D2 (GPIO-04).
После запуска прошивки делается попытка использовать данные последнего удачного подключения к WiFi сети (моргает светодиод).
При отсутствии таких данных, либо при невозможности подключения запускается режим точки доступа (горит светодиод), пароль 12345678.
В дальнейшем возможно подключение к другой WiFi сети с помощью веб-страницы "Configure WiFi".

Полная версия собирается под задачи заказчика и дополнительно может включать в себя следующие возможности:
- чтение датчиков температуры воздуха, теплоносителя, подключенных как непосредственно к контроллеру, 
так и по сети с других устройств, в том числе по протоколу RF-433 (выносные датчики метеостанций),
- управление другим отопительным оборудованием (насос, клапан двухпозиционный, клапан с сервоприводом),
- алгоритмы термостатирования (автоматическое управление котлом на основе заданных параметров),
- отправка всех параметров работы контроллера на MQTT брокер для дальнейшего сбора, отображения и анализа информации,
- использование OLED дисплея для отображения текущих параметров работы котла и управление с помощью сенсорных кнопок,
- использование GSM модуля для информирования и управления работой котла с помощью SMS сообщений,
- сохранение всех параметров настройки и работы контроллера, а также взаимодействие с другими контроллерами в составе 
комплексной системы управления отоплением.

Весь код для работы перечисленных функций (за исключением используемых сторонних библиотек) разработан и протестирован 
автором в реальных условиях и гарантирует надежную работу контроллера в режиме Fail-Safe.
Возможна сборка контроллера с адаптером и дисплеем (при необходимости) в стандартном корпусе на DIN-рейку.
Полная версия ПО контроллера предоставляется в виде готовой прошивки на коммерческой основе.
Для связи с автором писать на почту esp-ot@mail.ru
