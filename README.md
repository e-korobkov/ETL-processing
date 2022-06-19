# Что делает программа
Соединяет данные из нескольких источничников.
Вычисляет фичи.


# Задача разделена на части:
- Загрузка данных из 1С с сохранением на диск;
- Предобработка данных и сохранение на диск: замена пропусков, изменение типов данных, удаление определенных данных и т.п.;
- Обработка контактных данных с сохранение результата на диск: объединение данных из разных Справочников 1С в один файл;
- Объединение продаж и контактов Партнеров - вычисление «количества касаний» на сделку;
- Выделение фич.

Выбран сценарий с сохранением данных 1С на диск, т.к. позволяет проводить исследования без необходимости каждый раз ожидать завершения предыдущих этапов.

## Пояснения к каждому этапу:
#### Пояснения к [«1. Загрузка данных из 1С.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/1.%20%D0%97%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%BA%D0%B0%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85%20%D0%B8%D0%B7%201%D0%A1.ipynb)
  Подход к получению данных из 1С можно сказать типовой [используется в другом проекте](https://github.com/e-korobkov/Load_data_from_1C), имеет структуру:
- Текст запроса к 1С выполненный в синтаксисе 1С;
- Подключение к 1С – поднимается COM соединение;
- Выполнение запроса – выполняется запрос к 1С, обрабатываю результат;
- Сохраняю в файл.

#### Пояснения к [«2. Пред. обработка данных.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/2.%20%D0%9F%D1%80%D0%B5%D0%B4.%20%D0%BE%D0%B1%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85.ipynb)
  Данные скаченные из 1С требуют обработки, а именно:
- Приведения строк в фактическим типам данных;
- Замену пропусков;
- Перекодировки значений;
- Удаления;
- и т.п.

#### Пояснения к [«3. Обработка контактных данных Партнеров.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/3.%20%D0%9E%D0%B1%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B0%D0%BA%D1%82%D0%BD%D1%8B%D1%85%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85%20%D0%9F%D0%B0%D1%80%D1%82%D0%BD%D0%B5%D1%80%D0%BE%D0%B2.ipynb)
  Контактные данные Партнеров предварительно загружены из 1С в 2 файла (контакты Партнеров и Контактные лица партнеров).
Оба файла приводятся к единой структуре и объединяются.

#### Пояснения к [«4. Объединение продаж, контактов Партнеров + вычисление касаний.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/4.%20%D0%9E%D0%B1%D1%8A%D0%B5%D0%B4%D0%B8%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%BF%D1%80%D0%BE%D0%B4%D0%B0%D0%B6%2C%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B0%D0%BA%D1%82%D0%BE%D0%B2%20%D0%9F%D0%B0%D1%80%D1%82%D0%BD%D0%B5%D1%80%D0%BE%D0%B2%20%2B%20%D0%B2%D1%8B%D1%87%D0%B8%D1%81%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%B0%D1%81%D0%B0%D0%BD%D0%B8%D0%B9.ipynb)
На этом этапе происходит расчет кол-ва взаимодействий (телефонные переговоры и e-mail) на дату Заказа покупателя, по следующему алгоритму:
  > Цикл по файлу с данными о продажах
  >> Получаем дату Заказа покупателя
  >> Получаем контактные данные Партнера
  >>> Считаем кол-во e-mail и звонков (по соответствующим файлам) с ограничениями из подпунктов a & b

#### Пояснения к [«5. Выделение фич.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/5.%20%D0%92%D1%8B%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5%20%D1%84%D0%B8%D1%87.ipynb)
Структура кода:
- Функции расчета фич – получают на вход:
  + df – DataFrame данных полученных;
  + period - с каким шагом периода будем группировать данные;
  + grupBaza - столбец основание группировки;
  + grupPriznak - столбец с дополнительным разрезом группировки;
  + metodGrup – сумма или количество;
  + newColunmName - имя нового столбца, в который добавим данные/
- Загрузим данные полученные из 1С [см. файл «1. Загрузка данных из 1С.ipynb»](https://github.com/e-korobkov/ETL-processing/blob/master/1.%20%D0%97%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%BA%D0%B0%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85%20%D0%B8%D0%B7%201%D0%A1.ipynb)
- Преобразуем данные, например 'nan' -> None
- Выполним расчет фич
  + Заказ - как основной признак
    + Количество уникальной номенклатуры по Заказу
    + Количество уникальных Видов номенклатуры по Заказу
  + Партнер - как основной признак
    + Количество запросов (Заказов) в заданный период (частота обращений)
    + Количество реализаций в заданный период (частота продаж)
    + Количество реализаций в заданный период по номенклатуре (Заказываемый ассортимент)
    + Количество уникальных Видов номенклатуры по Партнеру
    + Заказано на сумму
    + Количество входящих/исходящих звонков
    + Количество входящей/исходящей почты
  + БизнесРегион - как основной признак
    + Ассортимент Номенклатуры
    + Количество Заказов
    + Количество Реализаций
    + Количество Партнеров
    + Количество сотрудников (менеджеров), работающих с Бизнес регионе
    + Сумма Заказа по Бизнес Региону
    + Сумма Реализаций по Бизнес Региону
    + Звонков в/из Бизнес региона
    + Кол-во входящей/исходящей эл-почты
  + Основной Менеджер - как основной признак
    + Количество уникальной номенклатуры по Менеджеру
    + Количество вида номенклатуры Партнеров по Менеджеру
    + Количество уникальных Партнеров по Менеджеру
    + География поставок Менеджера
    + Количество Заказов в заданный период по Менеджеру (загрузка менеджера)
    + Количество Реализаций в заданный период по Менеджеру (загрузка менеджера)
  + Номенклатура - как основной признак
    + Количество Регионов с поставляемой Номенклатурой
    + Количество запросов на Номенклатуру (популярность Номенклатуры)
    + Количество продаж на Номенклатуру (популярность Номенлатуры)
  + КредитИлиПредоплата - как основной признак
    + Количество Заказов по виду оплаты
    + Количество Реализ по виду оплаты
    + Количество ВидовНоменлатуры по виду оплаты
    + Количество Номенлатуры по виду оплаты
    + Количество БизнесРегионов по виду оплаты
- Сохранения файла с фичами

***Важно***

*Могут ли фичи подглядывать в результат? КОНЕЧНО МОГУТ!!! Давайте разберем фичу «Кол_воРеализацПартнерВНеделю» - Кол-во реализаций по Партнеру в неделю:*
- *"Клиент А" сделал 1 заказ и в туже неделю сделана реализация - в таком сценарии, к примеру forest 100% "угадает" правильный ответ. И к такому клиенту применение фичи равносильно записи ответов в обучающий набор*
- *"Клиент Б" сделал 6 одинаковых Заказов и 3 реализации, в такой ситуации для точного предсказания необходимо опираться на другие фичи…*
- *А в ситуации, где клиенту выставляется много счетов, но он не покупает, такая фича явно отражает "фактическое положение вещей" и может быть использована.
Конечно, агрегация «неделя» довольно спорная, но агрегация «квартал» или, к примеру «год», может быть использована*

*В общем вывод такой, фичи основанные на "Подглядывании" можно использовать, но только, с очень хорошим пониманием всех нюансов и специфики конкретного набора данных.
Я же такие фичи рассчитал, а использовать их или нет, решаю в каждом конкретном случае.*
