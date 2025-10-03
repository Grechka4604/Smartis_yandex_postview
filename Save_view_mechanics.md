<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Нужно описать POST endpoint /postviews/show/yandex-direct

Что будем отправлять
{ad_id}
{campaign_id}
{keyword}
{device_type}
{source}
{source_type}
{region_name}
{yclid}
utm_source
Источник перехода или платформа продвижения, обязательный параметр
utm_source=yandex— объявления размещаются в Директе
utm_medium
Тип продвижения, обязательный параметр
cpc, search— контекстная реклама; display— медийная реклама
utm_campaign
Название кампании, обязательный параметр;
utm_campaign=polet_v_kosmos или utm_campaign={campaign_id}
utm_content
Дополнительная информация, которая помогает различать объявления, необязательный параметр;
Можно использовать несколько значений, разделяя их "|" или ".": utm_content={position_type}.{position}
utm_term

POST endpoint `/postviews/show/yandex-direct` предназначен для сбора и передачи данных о просмотре рекламного объявления Яндекс Директ. В запросе должны передаваться параметры, отражающие источник, кампанию, объявление и условия показа по стандарту UTM и уникальные параметры Яндекса.

### Структура POST-запроса

Тело запроса (JSON):

```json
{
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

- `utm_source` — указывает платформу, например, "yandex" (для объявлений в Яндекс.Директ).
- `utm_medium` — тип продвижения: "cpc", "search" (контекст), "display" (медийка).
- `utm_campaign` — название/идентификатор кампании.
- Остальные поля наполняются по ситуации — для более глубокой аналитики и валидации событий перехода на сайт из Яндекс.Директа.


### Описание полей

| Поле | Назначение | Обязательность |
| :-- | :-- | :-- |
| ad_id | ID объявления | Нет |
| campaign_id | ID кампании | Нет |
| keyword | Ключевая фраза показа | Нет |
| device_type | Тип устройства (desktop/mobile/tablet) | Нет |
| source | utm_source, источник (например, "yandex") | Да |
| source_type | utm_medium, тип трафика (cpc, search, display) | Да |
| region_name | Регион показа | Нет |
| yclid | Идентификатор клика Яндекса | Нет |
| utm_source | Дублирует source, обязателен; рекомендовано передавать оба | Да |
| utm_medium | Тип продвижения (например, "cpc", "search") | Да |
| utm_campaign | Название/Id кампании | Да |
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

Все значения можно склеивать с помощью серверной логики (например, получать часть меток из источника или принимать все как отдельные поля).[^1][^2][^6]
<span style="display:none">[^10][^3][^4][^5][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://yandex.ru/dev/direct

[^2]: https://yandex.ru/dev/direct/doc/ru/concepts/overview

[^3]: https://yandex.com/dev/direct

[^4]: https://yagla.ru/blog/kontekstnaya-reklama/api-yandeksdirekt/

[^5]: https://yandex.cloud/ru/docs/api-design-guide/concepts/endpoints

[^6]: https://yandex.cloud/ru/docs/search-api/concepts/post-request

[^7]: https://tichiy.ru/wiki/rabota-s-yandeks-direct-api/

[^8]: https://qna.habr.com/q/79622

[^9]: https://www.youtube.com/watch?v=ZuGLwm9l0zo

[^10]: https://github.com/ArtemBuskunov/API_YD

