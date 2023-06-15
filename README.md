# SKIMNTR

SKIMNTR это API для мониторинга горнолыжных курортов. Данная документация описывает доступные методы и форматы ответов.

## Начало работы

API запросы осуществляются через HTTPS протокол, используя методы GET, POST, PUT, DELETE. Ответы возвращаются в формате JSON.

## Base URL

`https://enot.dev/skimon/`

## Конечные точки API

Все конечные точки API начинаются с `/api/1/`

## Авторизация

Каждый запрос, использующий PUT, POST, DELETE методы, должны быть авторизованы по токену.

- Headers:
  - Content-Type: application/x-www-form-urlencoded
  - Authorization: Bearer <token>

## Справочник API

#### Получение данных

Сервер возвращает ответ в формате JSON со следующей структурой.

```json
{
  "version": "0.1",
  "status": 200,
  "data": []
}
```

Массив `data` содержит запрошенные данные. Если данных нет, то возвращается пустой массив.

- `version`: Версия API, которая вернула данные
- `status`: HTTP код ответа сервера
- `data`: Массив запрашиваемых данных

#### Обработка ошибок


```json
{
  "version": "0.1",
  "status": 200,
  "error": {
    "message": "method not found",
    "code": 4
  }
}
```

Если произошла ошибка, то сервер возвращает ее в массиве error:
- `message`: Сообщение об ошибке
- `code`: Код ошибки

# Методы API

### GET /regions

Возвращает список регионов

#### Response

```json
{
  "version": "0.1",
  "status": 200,
  "data": [
    {
      "cluster_id": 1,
      "cluster_name": "\u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u044b\u0439 \u0440\u0430\u0439\u043e\u043d",
      "regions": [
        {
          "region_id": 1,
          "region_name": "\u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c",
          "region_alias": "msk"
        }
      ]
    }
  ]
}
```

- `cluster_id`: Идентификатор кластера региона
- `cluster_name`: Название кластера региона
- `regions`: Массив областей, которые входят в кластер
  - `region_id`: Идентификатор региона
  - `region_name`: Название региона
  - `region_alias`: Псевдоним, аббревиатура региона


### GET /resorts/<region_id>

Возвращает список курортов в указанном регионе

#### Response

```json
{
  "version": "0.1",
  "status": 200,
  "data": [
    {
      "id": 1,
      "alias": "shukolovo",
      "name": "\u0413\u043e\u0440\u043d\u043e\u043b\u044b\u0436\u043d\u044b\u0439 \u043a\u043b\u0443\u0431 \u041b\u0435\u043e\u043d\u0438\u0434\u0430 \u0422\u044f\u0433\u0430\u0447\u0435\u0432\u0430",
      "status": false,
      "weekday_price": 30,
      "weekend_price": 60,
      "number_of_tracks": 5,
      "webcams": true,
      "image_url": "https:\/\/enot.dev\/images\/image.jpg",
      "url": "http:\/\/www.shukolovo.ru\/",
      "lat": 56.2307,
      "lon": 37.4938,
      "working_hours": [
        {
          "day_of_week":1,
          "open_time":"10:00",
          "close_time":"24:00"
        }
      ],
      "live_streams": [
        {
          "webcam_id": 1,
          "type": "code",
          "data": "..."
        }
      ]
    }
  ]
}
```

- `id`: Идентификатор курорта
- `alias`: Псевдоним курорта
- `name`: Название курорта
- `status`: Статус курорта, открыт (true) или закрыт (false)
- `weekday_price`: Стоимость в будни 
- `weekend_price`: Стоимость в выходные
- `number_of_tracks`: Количество трасс
- `webcams`: Наличие веб камер на курорте
- `image_url`: Изображение для карточки курорта
- `url`: Вебсайт курорта
- `lat`: Географическая широта
- `lon`: Географическая долгота
- `working_hours`: Часы работы. 
  - `Массив с данными`
    - `day_of_week`: День недели, от 1 до 7.
      - `1`: Понедельник
      - `2`: Вторник
      - `3`: Среда
      - `4`: Четверг
      - `5`: Пятница
      - `6`: Суббота
      - `7`: Воскресенье
    - `open_time`: Время открытия
    - `close_time`: Время закрытия
- `live_streams`: Массив с данными о прямой трансляции с курорта
  - `Массив с данными`
    - `webcam_id`: Идентификатор трансляции
    - `type`: Тип трансляции. Может содержать следующие значени:
      - `code`: HTML код плеера
      - `iframe`: Ссылка на трансляцию для блока Iframe
    - `data`: Данные трансляции, код плеера или ссылка


# С версии API 0.5

### PUT /resorts/

Обновляет информацию о курорте.

#### Request

- Request Body:

| Параметр         | Тип     | Обязательный | Описание                    |
|------------------|---------|--------------|-----------------------------|
| resort_id        | integer | YES          | ID курорта для обновления   |
| region_id        | integer | NO           | ID региона                  |
| name             | string  | NO           | Название курорта            |
| status           | bool    | NO           | Статус курорта              |
| number_of_tracks | integer | NO           | Количество склонов          |
| webcams          | bool    | NO           | Наличие вебкамер курорта    |
| url              | string  | NO           | URL вебсайта курорта        |
| lat              | float   | NO           | Географическая широта       |
| lon              | float   | NO           | Географическая долгота      |

#### Response

```json
{
    "result": true,
    "message": "ok"
}
```