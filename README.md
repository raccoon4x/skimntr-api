# SKIMNTR

API мониторинга горнолыжных курортов

## Getting Started

## API Endpoints

All endpoints will start with /api/1/

## API Reference

#### Получение данных

Сервер отвечает JSON строкой следующей структурой.

```json
{
  "version": "0.1",
  "status": 200,
  "data": []
}
```

В массиве `data` возвращаются данные, если есть. Если данных нет, возвращается пустой массив.

- `version`: Версия, которая вернула данные
- `status`: HTTP код ответа сервера
- `data`: Массив возвращаемых данных

#### Ошибки

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

Ошибки возвращаются в массиве `error`:
- `message`: Сообщение об ошибке
- `code`: Код ошибки

# Methods

### GET regions

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
          "region_name": "\u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c"
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


### GET resorts/<region_id>

Возвращает список курортов в указанном регионе

#### Response

```json
{
  "version": "0.1",
  "status": 200,
  "data": [
    {
      "id": 1,
      "name": "\u0413\u043e\u0440\u043d\u043e\u043b\u044b\u0436\u043d\u044b\u0439 \u043a\u043b\u0443\u0431 \u041b\u0435\u043e\u043d\u0438\u0434\u0430 \u0422\u044f\u0433\u0430\u0447\u0435\u0432\u0430",
      "status": false,
      "weekday_price": 30,
      "weekend_price": 60,
      "number_of_tracks": 5,
      "webcams": true,
      "image_url": "https:\/\/enot.dev\/images\/image.jpg",
      "url": "http:\/\/www.shukolovo.ru\/"
    }
  ]
}
```

- `id`: Идентификатор курорта
- `name`: Название курорта