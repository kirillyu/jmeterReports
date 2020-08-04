# Описание репозитория
Данный репозиторий содержит в себе темплейт для ApacheJmeter, который поволяет генерировать и заливать отчет в Confluence. К темплейту прилагается нужное окружение для ApacheJmeter, в которое входит папка lib из корня - в ней содержатся все используемые плагины. Там же расположен chromedriver.exe, необходимый для работы темплейта. Репозиторий, помимо этого, включает в себя все необходимые для рендера графиков и анализа результатов дашборды Grafana, которые полностью интегрированы с данным темплейтом. 
## Требования к среде использования
0. JDK, желательно 1.8+ (Java)
1. ApacheJmeter 5.3
2. InfluxDB 1.8
3. Grafana 7.0.0 + Grafana Image Renderer Plugin
4. Установленный Google Chrome на машине, где будет генерироваться отчет
5. Confluence (не Cloud версия)
## Grafana
На выходе вы получите несколько Grafana дашбордов:

**1. Test Trends**

<img src="https://user-images.githubusercontent.com/9977326/89025122-f32d2580-d32e-11ea-88d4-de8cc93ce4ba.png" width="100%"></img> 

Дашборд позволяет переключаться между проектами и смотреть суммарные метрики по каждому из них.

* Левая верхняя таблица - показывает тренд изменений среднего времени отклика и процента ошибок в длительных тестах (Stability). 
* Правая верхняя - аналогична левой, но показывает результаты для коротких тестов (Stress).
* Нижняя левая - содержит лог запусков по этому проекту (показывает только запуски длительность, которых превышает 3 часа - значение настраиваемое). Также имеются ссылки на подробные метрики выбранного запуска. При нажатии на ссылку попадаем в дашборд №2 - [Test Metrics](#metrics).
* Нижняя правая - содержит все запуски по всем проектам с фильтром аналогично левой таблице.

<a name="metrics"/>**2. Test Metrics**

<img src="https://user-images.githubusercontent.com/9977326/89027755-c6c7d800-d333-11ea-97b4-3cec54facd4c.png" width="100%"></img> 
<img src="https://user-images.githubusercontent.com/9977326/89029561-68045d80-d337-11ea-8120-4d95a5427635.PNG" width="100%"></img> 
<img src="https://user-images.githubusercontent.com/9977326/89029704-aa2d9f00-d337-11ea-95c3-7f9e222786b8.PNG" width="100%"></img>
<img src="https://user-images.githubusercontent.com/9977326/89030566-676cc680-d339-11ea-904f-0d97978a48d1.png" width="100%"></img>
<img src="https://user-images.githubusercontent.com/9977326/89030707-b155ac80-d339-11ea-9e2f-832b9b9e22b4.PNG" width="100%"></img>


На этом дашборде собрана полная информация по выбранному тесту. Вручную можно выбрать:
* Проект
* Количество отображаемых запусков
* Нужный запуск
* Конкретную операцию этого запуска. По ней повятся детальные метрики в конце дашборда
Этот дашборд используется для осмотра результатов и их анализа.

**3. Render Dash**

Данный дашборд аналогичен дашборду №2 - [Test Metrics](#metrics), но он содержит определенные фильтры и настройки, которые снижают наглядность результатов, но необходимы для рендеринга. К примеру, на нем отключена пагинация в таблицах и графики содержат топ 5 операций, остальные вынесены в "Other".

# Jmeter

В самом ApacheJMeter для использования темплейта, после его [установки](#install), достаточно просто нажать на вкладку File, выбрать Templates..., кликнуть в выпадающем списке на "Add autogenerated reports..." и нажать на Merge. Мержить можно к уже готовым скриптам, после чего оформить нужные Thread Groups аналогично Main.
<img src="https://user-images.githubusercontent.com/9977326/89032614-b583c900-d33d-11ea-8b84-b8d00d7fb20a.png" width="100%"></img>
После добавления получится такой тест-план:

<img src="https://user-images.githubusercontent.com/9977326/89038166-7f981200-d348-11ea-83fd-45921fd38171.png" width="100%"></img>

<a name="install"/><h1> Установка и настройка</h1> </a>

* Превоначально необходимо поставить сам Apache Jmeter, получить актуальную версию можно по <a href="https://jmeter.apache.org/download_jmeter.cgi">ссылке</a>.
* Далее нужны <a href="https://dl.influxdata.com/influxdb/releases/influxdb-1.8.0_windows_amd64.zip">InfluxDB</a> и <a href="https://dl.grafana.com/oss/release/grafana-7.0.0.windows-amd64.msi">Grafana</a> - ссылка для Windows
* Настраивать ничего не нужно на данном этапе. Из папки с InfluxDB запускаем файл indluxd - по дефолту Influx будет на порту 8086, кастомизация происходит в файле influxdb.conf
* Для запуска Grafana достаточно стартануть файл grafana-server - она запустится на порту 3000, конфигурационный файл defaults.ini
* Confluence - корпоративная система, подразумевается, что он уже развернут в вашей компании. Если это не так - можно взять его <a href="https://www.atlassian.com/ru/software/confluence/download">отсюда</a>, но настройки потребуется провести самостоятельно. Никаких кастомных настроек не требуется, просто наличие рабочего SPACE и пользователя, который имеет **доступ к API**
* Устанавливаем <a href="https://www.google.com/intl/ru_ru/chrome/">Chrome</a> на машину, с которой будет генерироваться отчет

Желательно, чтобы машина, с которой будет подаваться нагрузка была минимально обременена сервисами, особенно Grafana, а тем более Confluence - иначе велик риск нехватки ресурсов для проведения нагрузки.

После того как инструментарий развернут, можно переходить к его интеграции с данным репозиторием. Это делается в несколько довольно простых шагов:

1. Скачиваем репозиторий
2. Содержимое папки JmeterUpdate переносим с заменой в папку apache-jmeter-..., то есть корень вашего Apache Jmeter. Важно, что в папке /bin находится chromedriver.exe для Windows, с ним бывают некоторые проблемы. Если они возникнут или используется другая операционная система необходимо попробовать <a href="https://chromedriver.chromium.org/downloads">другие версии драйвера</a>.
3. Первичная настройка Grafana заключается в подключении нужного Data Source. Configuration -> Data Sources -> Add Data Source -> InfluxDB -> вводим параметры подключения с указанием машины, на которую поставлен InfluxDB и порта (по дефолту 8086). Database в нашем случае - jmeter. После этого нажимаем Save & Test.

<img src="https://user-images.githubusercontent.com/9977326/89129564-f2e78280-d506-11ea-92b7-ee3cff6bade9.png" width="100%"></img>

4. Импортируем все дашборды из репозитория в Grafana. Правильно делается это так: в интфейсе Grafana слева есть вкладка Dashboards, в ней нужно выбрать Manage. Далее на открывшейся странице выбираем Import, после чего нажать на Upload.json, туда по очереди загрузить 3 .json файла из репозитория, в каждом из которых необходимо указать Data Source из пункта 3.

<img src="https://user-images.githubusercontent.com/9977326/89063549-06f97b80-d371-11ea-9184-bbd72ea94e02.png" width="100%"></img>

5. Чтобы в Grfana заработали перекрестные ссылки между дашбордами, в случае если она развернута не на localhost, необходимо прописать хост Grafana в переменных двух дашбордов: Test Trends и Test Metrics. 

<img src="https://user-images.githubusercontent.com/9977326/89130169-71462380-d50b-11ea-9563-3d67b745bccf.png" width="100%"></img>

6. Также нам понадобится от Grafana это ключ API. Который необходимо сгенерировать.

<img src="https://user-images.githubusercontent.com/9977326/89258206-62a55c80-d630-11ea-89ea-7889b3d4a34f.png" width="100%"></img>

7. Последним этапом настройки Grafana будет установка плагина рендера графиков. Его можно взять <a href="https://grafana.com/grafana/plugins/grafana-image-renderer/installation">тут</a>. Там же описана его установка. После нее нужно перезапустить Grafana (необъяснимо, но факт - иногда этот плаигн не требуется).

8. Теперь к Confluence. Для его использования нам нужно создать страницу-папку. В любом месте нашего пространства и с любой степенью вложенности. В эту папку, как дочерние элементы будут создаваться страницы с отчетами. Всё создается простым нажатием кнопки "Создать" вверху интерфейса.

<img src="https://user-images.githubusercontent.com/9977326/89263547-1f4feb80-d63a-11ea-87b4-debfba1fc886.png" width="100%"></img>

9. Далее внутри этой папки создаем еще одну страницу, это будет шаблон нашего отчета, которым сейчас необходимо минимально наполнить. Я подготовил черновой вариант, который вы можете попробовать у себя и изменить его как вам будет удобно. Ключевым моментом являются названия картинок графиков - они должны заранее совпадать с названиями файлов, выгружаемых при рендере из Grafana в блоке Jmeter Metrics - по дефолту это так. Добавляется черновик следующим образом:
* Переходим на страницу шаблона и нажимаем редактировать
* В верхней панели редактора нажимаем "+" ("Вставить прочий контент") или просто нажимаем ctrl + shift + d.
* В открышееся окно вставляем разметку типа "Confluence wiki":
>`VERSION_NUM`
>
>`Дата формирования отчета: RELEASE_DATE`
>
>`Информация о проекте:`
>
>`***`
>
>`***`
>
>`Сводная таблица:`
>
>`ALL_TABLE`
>
>`Ошибки:`
>
>`ERROR_TABLE`
>
>`Тест проходил с TIME_FROM до RELEASE_DATE`
>
>`Метрики теста:`
>
>`!VUsers.png!`
>
>`!ErrorPerSec.png!`
>
>`!TotalTransactions.png!`
>
>`!EachOperation.png!`
>
>`!90.png!`
>
>`!95.png!`
>
>`!99.png!`
>
>`!TransactionsResponseTime.png!`

 После добавления черновик шаблона будет выглядить вот так:
 

<img src="https://user-images.githubusercontent.com/9977326/89263550-20811880-d63a-11ea-9c29-52794881da72.png" width="100%"></img>

 Такой шаблон после теста будет сформирован в вот такую страницу:
 

<img src="https://user-images.githubusercontent.com/9977326/89263553-20811880-d63a-11ea-8052-457d7a2ed8e2.png" width="100%"></img>


8. Осталось настроить только JMeter
