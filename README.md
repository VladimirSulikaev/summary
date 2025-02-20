# Владимир Суликаев 
![imageedit_2_9319058207](https://github.com/VladimirSulikaev/summary/assets/150725281/a6c871c0-a9a3-48c7-b0cf-ad9ab04661a3)

- Email: vladimir@sulikaev.ru
- Telegram: https://t.me/dreenydove

## Аналитик данных

### О себе 

Имею хорошие навыки работы с Excel и его аналогами, где я умею создавать таблицы, диаграммы, формулы, меры в Power Pivot, так же преобразование данных в Power Query для меня не проблема. Хорошо работаю с SQL и с базами данных на языках PostgreSQL, MySQL, Clickhouse, умею писать запросы, фильтровать данные, анализировать результаты и визуализировать их в Power BI, Python (mayplotlib/seaborn) и в крайнем случае в Excel. Имею непосредственный опыт работы аналитиком данных в крупной международной компании. Готов работать как в команде, так и самостоятельно, а также адаптироваться к разным условиям и требованиям.

## Навыки и технологии
- Инструменты анализа данных: ``Excel``, ``Google Sheets``, ``DBeaver``, ``Spyder``, ``Jupiter Notebook``
- Языки программирования и библиотеки: ``Python``, ``Pandas``, ``Numpy``, ``Scipy``, ``Matplotlib``, ``Seaborn``
- Системы управления базами данных: ``PostgreSQL``, ``MySQL``, ``Clickhouse``
- Средства визуализации данных: ``PowerBi``, ``Excel``
- Английский язык на уровне: ``Intermediate``

В этом репозитории вы можете найти некоторые из моих проектов, выполненных во время обучения и практики (будет дополняться).
<br>

## Мои проекты
#### Проект 1: Комплекс заданий по Excel (4 задания)

<p>Что нужно было сделать:<p>

<ol>

<p> 1. Понять причину возросшей нагрузки пользователей на сервер. </p>
<p> 2. Рассмотреть периоды технических проблем на двух серверах, при определенных условиях, а именно использование программы лояльности и покупал ли человек на обычной кассе или на кассе самообслуживания.</p>
<p> 3. Рассмотреть выгодность проведённой акции, и посчитать прибыль которую получили. </p>
<p> 4. Построить график количества транзакций и среднего платежа по дням с возможностью выбора торговой точки. </p>

</ol>

<p>Выводы (итоги):<p>

<ol>

<p> 1. Запустив новую торговую точку на ул. Чертановской, новые покупатели по всей видимости начали ходить туда, а большая часть из покупателей на Симферопольском бульваре тоже начала покупать на Чертановской, поэтому будет разумно распределить нагрузку, переместив торговую точку Чертановской на первый сервер, а Симферопольский на третий. </p>
<p> 2. Выявил по построенным графикам проблемы, в связи с увеличившимся временем обслуживания, которое поднималось выше 60%. </p>
<p> 3. Средний чек вырос и перебил затраты на подарки своей чистой прибылью. </p>
<p> 4. Оптимизация выполнена успешно: Теперь данные получаем в зависимости от выбранной торговой точки, так мы можем наблюдать какая точка приносит больше прибыли, а какая меньше.
Также добавили в расчёты сумму всех платежей, их количество, и средний чек покупки, которые по формуле доставляются с данных и влияют на график. </p>
</ol>

<p> Как решал: краткое описание решения: </p>

> <a href="https://youtu.be/mQ5jHFjSQNA?si=x8RbixMC_DtVjV9I">Ссылка на видео с разъяснением каждого кейса на YouTube (более подробно)</a>

#### Проект 2: Сборка тестовой базы данных

<p> Что нужно было сделать: </p>

<ol>

<p> 1. Сгенерировать дата-сэмплы с помощью функций Excel </p>
<p> 2. Воспользоваться sqliteonline на Postgresql написать запросы соединения таблиц, и вывести данные </p>

</ol>

<p> Выводы (итоги): </p>

<ol> 
  
<p> 1. Выснил как с помощью функций Excel генерировать значения, чтобы заполнть ими таблицы. </p>
<p> 2. Создал .csv файлы для удобной работы БД с ними. </p>
<p> 3. Теперь можно тестить коды на своей базе, не затрагивая основные, с которыми работаем. </p>

</ol>

<p> Как решал: краткое описание решения: </p>

<ol> Построил в Excel три таблицы с ценами на продукты, покупками клиентов и продукт-лист с названиями и сегментами продуктов. Импортировал таблицы в sqliteonline.com, написал несколько скриптов помогающих посчитать выручку за определённый период, вывел список клиентов которые за последние 2 месяца потратили на покупки больше 50 000 рублей. </ol>

> <a href="https://docs.google.com/spreadsheets/d/1y_QszM6TqXtx8qt5ZaN0iQ0RGP2bjiWztiBEuIKIk4A/edit?usp=sharing">Ссылка на исходный файл БД в Google Sheets (из него создал 3 .csv файла для sqliteonline) </a>

#### Проект 3: Исследования в геймдеве (SQL)

<p>Что нужно было сделать:<p>

<ol>

<p> 1. Построить графики ежемесячной активности пользователей, недельной и в сутки (MAU, WAU и DAU), плюс графики вовлеченности Sticky Factor. </p>
<p> 2. Нужно проверить вовлеченность игроков зарегистрировавшихся в 2022 году, "разбудить" более старых игроков и наградить игровой валютой. </p>
<p> 3. Рассмотреть долю проблемных записей игровых сессий для дальнейшей очистки, на какие устройства они приходятся. </p>
<p> 4. Выяснить на что тратят деньги игроки, и в какое время на что приходится больше покупок. </p>
<p> 5. Проверить гипотезу что: из-за более дорогой и таргетированной рекламы мы приобрели более "лояльных" игроков, которые больше времени посвящают нашей игре. </p>
<p> 6. Рассчитать K-factor и "вирусность" игры. </p>
<p> 7. Выделить лояльные когорты пользователей по выбранным когортам. </p>

</ol>

<p>Выводы (итоги):<p>

<ol>

<p> 1. Построив графики выяснили, что в марте количество активных пользователей возрасло в среднем на 2-3 раза. </p>
<p> 2. Выявили пользователей которые зарегистрировались в 2022 году, и кто из них больше проводил времени в игре за всё время. </p>
<p> 3. Доля проблемных записей составила 12.01%, из которых 18.98% приходится на iOS и 0.8% на Android, из всех проблемных записей 97% идёт от iOS. </p>
<p> 4. Построил динамику клиентских выплат, больше всего было задоначено на внутриигровую валюту. </p>
<p> 5. Подтвердил гипотезу, среднее время в игре возрасло на 10-25 минут по выбранным когортам по месяцам, в которых использовали новую стратегию привлечения клиентов. </p>
<p> 6. Рассчитал K-factor, в среднем 1 пользователь доводит до регистрации "0.97" друга, также в среднем за каждый месяц к нам будут приходить около 280 человек только по реферальной программе. </p>
<p> 7. Выделил лояльные когорты и посчитал количество человек, которые входят в них, в дальнейшем можно использовать для проведения различных акций. </p>

</ol>

<p> Как решал: краткое описание решения: </p>

> <a href="https://youtu.be/PwAE9W_19KI?si=0RXfYwele5kSs8Vo">Ссылка на видео с разъяснением каждого кейса на YouTube (более подробно) </a>
