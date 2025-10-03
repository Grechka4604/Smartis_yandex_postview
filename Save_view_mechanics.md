# endpoint для сохранения
POST endpoint `/postviews/show/yandex-direct` предназначен для сохранения данных о просмотре рекламного объявления Яндекс Директ. 

### Структура POST-запроса

Тело запроса (JSON):

```json
{
  "yandexUUID":"UUID в система Яндекса",
  "pid":"наш peaople_id",
  "ad_id": "id объявления в Директе",
  "campaign_id": "id кампании в Директе",
  "keyword": "ключевая фраза показа",
  "device_type": "тип устройства пользователя (desktop/mobile/tablet)",
  "source": "значение utm_source (например, 'yandex')",
  "source_type": "тип источника/канала (например, cpc, search, display)",
  "region_name": "название региона показа",
  "yclid": "идентификатор клика yclid от Яндекса",
  "utm_source": "Источник перехода (обязательный, обычно 'yandex')",
  "utm_medium": "Тип продвижения (обязательный: cpc, search, display)",
  "utm_campaign": "Название кампании или campaign_id (обязательный)",
  "utm_content": "Дополнительная информация (опционально)",
  "utm_term": "Ключевое слово (если применимо)"
}
```


### Обязательные параметры

- yandexUUID
- pid


### Описание полей

| Поле | Назначение | Обязательность |
| :-- | :-- | :-- |
| yandexUUID | UUID в система Яндекса | Да |
| pid | наш peaople_id | Да |
| ad_id | ID объявления | Нет |
| campaign_id | ID кампании | Нет |
| keyword | Ключевая фраза показа | Нет |
| device_type | Тип устройства (desktop/mobile/tablet) | Нет |
| source | utm_source, источник (например, "yandex") | Нет |
| source_type | utm_medium, тип трафика (cpc, search, display) | Нет |
| region_name | Регион показа | Нет |
| yclid | Идентификатор клика Яндекса | Нет |
| utm_source | Дублирует source, обязателен; рекомендовано передавать оба | Нет |
| utm_medium | Тип продвижения (например, "cpc", "search") | Нет |
| utm_campaign | Название/Id кампании | Нет |
| utm_content | Доп. инфо для различия объявлений | Нет |
| utm_term | Ключевое слово | Нет |

### Пример payload

```json
{
  "ad_id": "123456",
  "campaign_id": "654321",
  "keyword": "купить квартиру",
  "device_type": "mobile",
  "source": "yandex",
  "source_type": "cpc",
  "region_name": "Москва",
  "yclid": "78910-abc",
  "utm_source": "yandex",
  "utm_medium": "cpc",
  "utm_campaign": "polet_v_kosmos",
  "utm_content": "position_type1.1",
  "utm_term": "новостройки"
}
```


# таблица для сохранения

### Таблица: yandex_direct_views

| Поле | Тип данных | Описание | Особенности |
| :-- | :-- | :-- | :-- |
| id | BIGINT UNSIGNED AUTO_INCREMENT | Первичный ключ | PRIMARY KEY |
| yandexUUID | VARCHAR(64) | UUID в системе Яндекса | NOT NULL |
| pid | VARCHAR(64) | Внутренний идентификатор пользователя | NOT NULL |
| ad_id | VARCHAR(32) | Идентификатор объявления в Директе |  |
| campaign_id | VARCHAR(32) | Идентификатор кампании в Директе |  |
| keyword | VARCHAR(255) | Ключевая фраза показа |  |
| device_type | VARCHAR(32) | Тип устройства (desktop/mobile/tablet) |  |
| source | VARCHAR(64) | Источник перехода (utm_source) |  |
| source_type | VARCHAR(32) | Тип источника/канала |  |
| region_name | VARCHAR(128) | Название региона показа |  |
| yclid | VARCHAR(64) | Идентификатор клика Яндекса |  |
| utm_source | VARCHAR(64) | Obязательный параметр, обычно 'yandex' | |
| utm_medium | VARCHAR(32) | Обязательный (cpc/search/display) | |
| utm_campaign | VARCHAR(128) | Обязательный (название/id кампании) | |
| utm_content | VARCHAR(255) | Доп. информация (опционально) |  |
| utm_term | VARCHAR(255) | Ключевое слово (опционально) |  |
| created_at | DATETIME | Дата и время создания записи | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | DATETIME | Дата и время обновления записи | NOT NULL, ON UPDATE CURRENT_TIMESTAMP |


