# Контрольное мероприятие № 5

## Проектирование HDL-описания компонента интегральной схемы

### Предварительное задание

Убедиться в работоспособности схемы, спроектированной в курсе "Цифровая схемотехника".

### Немного теории

Полный поток проектирования интегральной схемы делится разделить на два этапа:

1. логическое проектирование;
2. физическое проектирование.

На этапе логического проектирования с целью уменьшения выхода негодных изделий производится [прототипирование](https://fpga-systems.ru/blog/dlja_chego_my_ispolzuem_prototipirovanie_fpga/2022-11-22-14) устройства на ПЛИС. Определяются временные ограничения и проверяется, может ли устройство работать в "железе".

В соответствие с этим сначала формулируется техническое задание и анализируется. На основе анализа (или обзора литературных источников) формируется алгоритм работы разрабатываемого устройства, а затем - функциональная схема устройства. На основе этого анализа проводится HDL-описание разрабатываемого устройства. Для каждого разрабатываемого модуля проводится верификация, для этого пишется тестовое испытание (testbench).

При условии правильной работы устройства на уровне HDL (функциональная RTL верификация, не путать с [формальной](https://fpga-systems.ru/formal-verification-with-symbiyosys)) производится её логический синтез для соответствующей ПЛИС, после чего синтезированная схема также на уровне функциональных блоков (модулей) проходит верификацию в виде моделирования тестового испытания на уровне логических вентилей (gate-level верификация). Адекватность синтеза определяется совпадением результатов моделирования предыдущего этапа и данного.

После наступает этап размещения и трассировки для формирования файла прошивки ПЛИС.

### Основное задание

1. Для разрабатываемого устройства приведите *функциональную* или *структурную* схему устройства и *описание* принципа его работы. Здесь же приведите *таблицу*, описывающую входные и выходные сигналы, а также их активных уровней на основе примера, приведённого ниже (пример разработан для суммирующего счётчика с предустановкой его начального значения). Пример для ориентирования: [Описание микросхемы с двумя UART-модулями](https://www.farnell.com/datasheets/390385.pdf).

![Счётчик](files/figure/image_cnt.png)

| Сигнал | Направление | Функционал | Примечание |
|--------| ------------|------------|------------------|
| clk    | Вход        | Тактовый сигнал | Схема работает по переднему фронту |
| rstn   | Вход        | Общий сброс, асинхронный | Логический ноль |
| D[3:0] | Вход        | Предустановка значения счётчика | При активном сигнале `Mode` по приходу переднего фронта сигнала `clk` происходит установка значения счётчика |
| mode   | Вход        | Выбирает режим работы счётчика: предустановить значение счётчика или разрешить считать | 
| Q[3:0] | Выход       | Определяет выходное значение счётчика во время его работы. Если реализован режим ввода начального значения, то после первого переднего фронта синхросигнала он равен этому значению, иначе равен предыдущему значению, увеличенному на 1 со сбросом в 0 при достижении числа 15.
| VSS    | Вход        | Контакт "земли" для ядра микросхемы | Равно 0 В  |
| VDD    | Вход        | Контакт "питания" для ядра микросхемы | Равно 1,1 В|

2. Приведите пример временной диаграммы, демонстрирующей работоспособность устройства во всех (или типичных) режимах его работы из предыдущего семестра. Прокомментируйте диаграмму.
3. Приведите поведенческое HDL-описание модулей, составляющих устройство.
4. Для каждого модуля приведите его модуль тестирования (testbench) с демонстрацией работоспособности модуля и описанием того, на что читатель должен обратить внимание при просмотре временной диаграммы.
5. Для полной схемы приведите её модуль тестирования и также продемонстрируйте её работоспособность с описанием того, на что читатель должен обратить внимание при просмотре временной диаграммы.
6. Для каждого модуля в пп. 4-5 приведите его RTL-представление и технологическое отображение для ПЛИС семейства Cyclone IV EP4CE6E22C8.
7. Приведите результат моделирования для RTL-моделирования и Gate-level-моделирования. Покажите, что оно совпадает с моделированием RTL-уровня.
7. Укажите, сколько комбинационной логики и регистров использовано в качестве ресурсов ПЛИС.
