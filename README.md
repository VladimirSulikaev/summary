# Владимир Суликаев 
![image](https://github.com/user-attachments/assets/478001f8-0f72-435e-ada8-fa546ca235e8)

- Email: vladimir@sulikaev.ru
- Telegram: https://t.me/nuckydreen_mbt

## Аналитик данных

### О себе 

Аналитик данных с опытом работы в SQL (PostgreSQL, MySQL, Clickhouse), Excel (Power Query, Power Pivot), Python (Pandas, Seaborn). Специализируюсь на обработке данных, построении отчётов и визуализации (Superset, Power BI).

## Навыки и технологии

- SQL: ``PostgreSQL, MySQL, Clickhouse (написание запросов, работа с БД)``  
- Python: ``Pandas, Numpy, Matplotlib (анализ данных, визуализация)``
- BI-инструменты: ``Superset, Power BI, Excel``
- Excel & Google Sheets: ``Power Query, Power Pivot, формулы``  
- Английский: ``Intermediate (B1)``

В этом репозитории вы можете найти некоторые из моих проектов, выполненных во время практики.
<br>

## Мои проекты

#### Проект 6: Построение ETL-пайплайна через бота в телеграм (Python, Airflow)

<ol>

![image](https://github.com/user-attachments/assets/70d0881c-d6ff-4c39-a3d4-65f854c5fe0a)

<p> Задача: Создать удобный дейли отчёт по витринам в Clickhouse </p>
<p> Инструменты: Python, Airflow, Clickhouse, GitLab </p>
<p> Результаты: Создал DAG в Airflow, подключил к боту в телеграм который показывает основные метрики за неделю и вчерашний день, избавляет от ежедневной ad-hoc аналитики, предоставляя важные метрики каждый день и помогает заметить сразу просадки в показателях. </p>

    import matplotlib.pyplot as plt
    import seaborn as sns
    import pandas as pd
    import pandahouse as ph
    import io
    
    import telegram
    
    connection = {
        'host': 'https://clickhouse.lab.karpov.courses',
        'password': 'dpo_python_2020',
        'user': 'student',
        'database': 'simulator_20250320'}
    
    def test_report(chat = None):
        chat_id = chat or -938659451

        my_token = '7848904672:AAGe-boDiGL_yAB4ss75Q-fAUWYnqlYeUmY'
        bot = telegram.Bot(token = my_token)
        
        query = '''
            select toDate(time) as date
                 , countIf(action = 'view') as views
                 , countIf(action = 'like') as likes
                 , likes/views as CTR
                 , uniqExact(user_id) as DAU
            from simulator_20250320.feed_actions
            where date between today() - 7 and today() - 1
            group by date
            order by date
                '''
        data = ph.read_clickhouse(query, connection = connection)
        
        sns.set(rc = {'figure.figsize':(21,9)}, style = 'whitegrid') # наводим красоту
        
        # карточка
        plt.subplot(2, 2, 1)
        plt.title('Ежедневный отчёт KPI', fontsize = 25, pad = 20)
        plt.figtext(0.12, 0.81, f"Данные за {str(max(data['date'].dt.date))}", ha = "left", fontsize = 18)
        plt.figtext(0.13, 0.75, f"Активных пользователей = {data['DAU'].loc[6]}", ha = "left", fontsize = 15)
        plt.figtext(0.13, 0.70, f"Просмотров = {data['views'].loc[6]}", ha = "left", fontsize = 15)
        plt.figtext(0.13, 0.65, f"Лайков = {data['likes'].loc[6]}", ha = "left", fontsize = 15)
        plt.figtext(0.13, 0.60, f"CTR = {round(data['CTR'].loc[6] * 100,2)} %", ha = "left", fontsize = 15)
        plt.axis('off')
        
        # DAU
        plt.subplot(2, 2, 2)
        sns.barplot(x=data['date'].dt.date, y=data['DAU'], color = 'b')
        plt.title('Активные пользователи (DAU)', fontsize = 25, pad = 20)
        plt.ylim(bottom = min(data['DAU']) - min(data['DAU']) * .05, top = max(data['DAU']) * 1.01)
        plt.xlabel(''), plt.ylabel('')
        
        # VIEWS / LIKES
        plt.subplot(2, 2, 3)
        sns.lineplot(x=data['date'].dt.date, y=data['views'], color = 'b', label = 'views')
        sns.lineplot(x=data['date'].dt.date, y=data['likes'], color = 'r', label = 'likes')
        plt.figtext(0.3, 0.03, 'Просмотры и лайки по дням', ha = "center", fontsize = 25)
        plt.ylim(bottom = 0)
        plt.xlabel(''), plt.ylabel('')
        
        # CTR
        plt.subplot(2, 2, 4)
        sns.barplot(x=data['date'].dt.date, y=data['CTR'], color = 'b')
        plt.figtext(0.72, 0.03, 'Конверсия из просмотра в лайк (CTR)', ha = "center", fontsize = 25)
        plt.ylim(bottom = min(data['CTR']) - min(data['CTR']) * .05, top = max(data['CTR']) * 1.05)
        plt.xlabel(''), plt.ylabel('')
    
        # вывод
        plot_object = io.BytesIO()
        plt.savefig(plot_object)
    
        plot_object.seek(0)
    
        plot_object.name = 'test_plot.png'
        plt.close()
        
        bot.sendPhoto(chat_id = chat_id, photo = plot_object)
        
        # Второй график
        
        query_2 = '''
        select event_date 
             , sum(messages_received) as messages_received
             , sum(messages_sent) as messages_sent
             , sum(users_received) as users_received
             , sum(users_sent) as users_sent
             , users_sent / users_received as SRR 
        from test.sulikaev_test_7
        where event_date >= today() - 7
        group by event_date
        '''
        # Прописываем все колонки для удобства копирования
        data_2 = ph.read_clickhouse(query_2, connection = connection)
        
        # карточка + Senter-to-Reciever Ratio (SRR) как своя метрика
        plt.subplot(2, 2, 1)
        plt.title('Ежедневный отчёт по мессенджеру', fontsize = 25, pad = 20)
        plt.figtext(0.12, 0.81, f"Данные за {str(max(data_2['event_date'].dt.date))}", ha = "left", fontsize = 18)
        plt.figtext(0.13, 0.75, f"{round((data_2['users_sent'].loc[6] / data['DAU'].loc[6] * 100),2) }% пользователей отправили сообщения", ha = "left", fontsize = 15)
        plt.figtext(0.13, 0.70, f"{round((data_2['users_received'].loc[6] / data['DAU'].loc[6] * 100),2) }% пользователей получили сообщения", ha = "left", fontsize = 15)
        plt.figtext(0.13, 0.63, f"{round(data_2['SRR'].loc[6] * 100,2)}% коэффицент охвата рассылки", ha = "left", fontsize = 15)
        plt.axis('off')
        
        # Сообщения
        
        plt.subplot(2, 2, 2)
        plt.title('Активность сообщений', fontsize = 25, pad = 20)
        sns.lineplot(x=data_2['event_date'].dt.date, y=data_2['messages_sent'], color = 'b', label = 'отправлено')
        sns.lineplot(x=data_2['event_date'].dt.date, y=data_2['messages_received'], color = 'r', label = 'получено')
        plt.xlabel(''), plt.ylabel('')
        
        # SRR
        
        plt.subplot(2, 2, 3)
        plt.figtext(0.3, 0.03, 'Коэффицент охвата рассылки (SRR)', ha = "center", fontsize = 25)
        sns.lineplot(x=data_2['event_date'].dt.date, y=data_2['SRR'], color = 'b')
        plt.ylim(bottom = min(data_2['SRR']) - min(data_2['SRR']) * .1, top = max(data_2['SRR']) * 1.1)
        plt.xlabel(''), plt.ylabel('')
        
        # Пользователи
        
        plt.subplot(2, 2, 4)
        plt.figtext(0.72, 0.03, 'Активность пользователей', ha = "center", fontsize = 25)
        sns.lineplot(x=data_2['event_date'].dt.date, y=data_2['users_sent'], color = 'b', label = 'отправители')
        sns.lineplot(x=data_2['event_date'].dt.date, y=data_2['users_received'], color = 'r', label = 'получатели')
        plt.xlabel(''), plt.ylabel('')
        
        # отправка
    
        plot_object = io.BytesIO()
        plt.savefig(plot_object)
    
        plot_object.seek(0)
    
        plot_object.name = 'test_plot.png'
        plt.close()
        
        bot.sendPhoto(chat_id = chat_id, photo = plot_object)
    
    from datetime import datetime, timedelta
    import requests
    
    from airflow.decorators import dag, task
    
    default_args = {
        'owner': 'v-sulikaev',
        'depends_on_past': False,
        'retries': 2,
        'retry_delay': timedelta(minutes=5),
        'start_date': datetime(2025, 5, 3),
    }
    
    schedule_interval = '0 8 * * *'
    
    @dag(default_args = default_args, schedule_interval = schedule_interval, catchup = False)
    def svs_report():
        
        @task
        def make_report():
            test_report()
        
        make_report()
        
    svs_report = svs_report()

</ol>

![image](https://github.com/user-attachments/assets/f200c18b-2883-409e-b149-7d169a4dad22)

#### Проект 5: Анализ Retention пользователей после рекламной кампании (Superset, SQL)

<ol>

<p> Задача: Оценить Retention пользователей, привлечённых рекламной кампанией и выявить какие пользователи не смогли воспользоваться лентой новостей. </p>
<p> Инструменты: Superset, Clickhouse, GitLab </p>
<p> Результаты: Retention оказался аномально низким. На 1-й день после акции осталось только 8,6% пользователей (обычно 30-50%). На 7-й день удержание составило 7,5% (обычно 20-25%). Среднее отклонение по удержанию – 50-70%. 
  
Выявлены технические проблемы в России: В крупных городах (Москва, Новосибирск) сервис не работал совсем. Пользователи из этих регионов отсутствуют в базе. Проблема передана в отдел разработки.</p>

</ol>

![image](https://github.com/user-attachments/assets/f200c18b-2883-409e-b149-7d169a4dad22)

#### Проект 4: Дашборд метрики ленты новостей и чатов (SQL, Superset)

<ol>

<p> Задача: разработка дашборда для анализа пользовательской активности в ленте новостей и чатах </p>
<p> Инструменты: Superset, Clickhouse, GitLab </p>
<p> Результаты: обнаружен рост аудитории (DAU/WAU/MAU) → помог скорректировать стратегию контента, проанализирована вовлечённость (CTR, взаимодействия) → выявлены ключевые точки для улучшения UX, определены пики активности → оптимизировано расписание публикаций, разработаны фильтры для сегментации аудитории → улучшена персонализация контента </p>

</ol>

![image](https://github.com/user-attachments/assets/71d39ad3-24d8-4a78-a748-109cf37645a1)

#### Проект 3: Анализ вовлечённости в геймдеве (SQL)

<ol>

<p> Задача: анализ активности пользователей и их платежей </p>
<p> Инструменты: PostgreSQL, Google Sheets </p>
<p> Результаты: выявлен рост DAU в 3 раза, подтверждена гипотеза о более лояльных игроках </p>

</ol>

> <a href="https://youtu.be/PwAE9W_19KI?si=0RXfYwele5kSs8Vo">Ссылка на видео с разъяснением каждого кейса на YouTube (более подробно) </a>

#### Проект 2: Сборка тестовой БД (SQL, Excel)

<ol>

<p> Задача: генерация данных для тестов </p>
<p> Инструменты: Excel, PostgreSQL </p>
<p> Результаты: создана тестовая база, автоматизирован генератор данных </p>

</ol>

> <a href="https://docs.google.com/spreadsheets/d/1y_QszM6TqXtx8qt5ZaN0iQ0RGP2bjiWztiBEuIKIk4A/edit?usp=sharing">Ссылка на исходный файл БД в Google Sheets (из него создал 3 .csv файла для sqliteonline) </a>

#### Проект 1: Анализ нагрузки серверов (Excel, SQL)

<ol>

<p> Задача: определить причину перегрузки </p>
<p> Инструменты: Excel, SQL </p>
<p> Результаты: перераспределение нагрузки привело к снижению проблем на 30% </p>

</ol>

> <a href="https://youtu.be/mQ5jHFjSQNA?si=x8RbixMC_DtVjV9I">Ссылка на видео с разъяснением каждого кейса на YouTube (более подробно)</a>
