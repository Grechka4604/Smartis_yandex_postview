# 1. Первичное описание
```
1 (core)
стандартное размещение чужого пикселя на сайте нашего клиента через Smartis.TagManager по наличию включенной интеграции
 <img src="https://an.yandex.ru/mapuid/<CookieMatchTag>/pid" style no-referer>


2 
яндекс дергает наш пиксель 
https://tracker.smartcallback.ru/matching
мы видим/сохраняем нашу куку в браузере пользователя
редиректим на пиксель яндекса тот же самый
 <img src="https://an.yandex.ru/mapuid/<CookieMatchTag>/pid" style no-referer>


3
пользователь видит рекламу
яндекс показывает пиксель 
<img src="https://verify.yandex.ru/verify_smartis/verify?param1=1&param2=2..." > - мы должны предоставить вид параметров ?param1=1&param2=2...

яндекс понимает что verify_smartis = мы, находит в своих настройках что надо дернуть наш адрес регистрации события показа
https://tracker.smartis.bi/show?param1=1&param2=2...&Yandexuid=XXXX&ExtUID=pid
+в headers доп параметры
	- UserAgent
	- X-Yabs-Request-Id
	- IP без последнего октета в хедерах

мы это событие сохраняем в нашей базе


4
пользователь кликает по рекламе
направляют юзера на https://verify.yandex.ru/verify_smartis/click?param1=1&param2=2...
яндекс видит пиксель на клик с verify_smartis - редиректит их в нас 
https://tracker.smartis.bi/click?param1=1&param2=2...&Yandexuid=XXXX&ExtUID=pid
+в headers доп параметры
	- UserAgent
	- X-Yabs-Request-Id
	- IP без последнего октета в хедерах
? мы находим ссылку куда отправить пользователя на сайт рекламодателя
редиректим пользователя по этой ссылке транслируя utm метки
? альтернатива если яд не присылает ссылок - сделать частое обновление справочника объявлений - проставлять там нашу ссылку на клик + сохранять маппинг ссылка на клик=> ссылка куда редирект делать
```

# 2. Сценарии и участиники
## 1. Матчинг на нашем трафике
*Основные участники*
1. Сайт нашего клиента (Браузер)
2. YandexMapModule
3. SmartisTrackerModule
   
*Сценарий*
1. Размещаем пиксель на сайте застройщика
2. Пользователь преходит на сайт
3. Тут мы pid в любом случае получаем (единственное надо передалть SmartCallback на новую ручку)
3. Пиксель прогружается
4. Браузер отправляет запрос /an.yandex.ru/mapuid/<CookieMatchTag>/pid
5. YandexMapModule возвращает ответ



## 2. Матчинг на трафике Яндекса
*Основные участники*
1. Страницы Яндекса (Браузер)
2. SmartisTrackerModule
3. YandexMapModule

*Сценарий*
1. Пользователь просматривает страницу Яндекс
2. Браузер вызывает /matching
3. SmartisTrackerModule видит/сохраняет нашу куку в браузере пользователя
   1. Проверка наличия pid
      1. Есть
         1. Переходим к следующему шагу
      2. Нет
         1. Дергаем ручку /pid/create
         2. Получаем pid
         3. Переходим к следующему шагу
4. SmartisTrackerModule возаращает 302 + Редирект на /an.yandex.ru/mapuid/<CookieMatchTag>/pid

## 3. Фиксация просмотров
*Основные участники*
1. Ресурс Яндекса
2. YandexVerifyModule
3. SmartisTrackerModule
4. YandexPostview_table (вопрос она где лежит)

*Сценарий*
1. Зашиваем в рекламное объявление специальныую треккинговую ссылку вместо стандартной (https://verify.yandex.ru/verify_smartis/verify?param1=1&param2=2...)
2. Посетитель видит рекламное объявление 
3. Писксель прогружается, вызывая https://verify.yandex.ru/verify_smartis/verify?param1=1&param2=2...
4. Яндекс идентифицирует PARTNER_NAME
5. Вызывает соответствующий метод https://tracker.smartis.bi/show?param1=1&param2=2...&Yandexuid=XXXX&ExtUID=pid
+в headers доп параметры
	- UserAgent
	- X-Yabs-Request-Id
	- IP без последнего октета в хедерах
6. Smartis cохраняет запись в БД



# 3. Endpoints

*base_url*: https://tracker.smartcallback.ru

## 1. /pid/сreate
- Создает новую запись в таблице people
- Возаращает pid

## 2. /matching
- ищем pid в куках
- pid есть?
  - да -> подолжаем
  - нет
    - вызываем /pid/сreate
- возвращаем 302 + редирект на https://an.yandex.ru/mapuid/<CookieMatchTag>/pid

## 3. /show
- Получаем (?)
- Заносим запись в postview.yandex_views