---
title: События
permalink: rest_events.html
sidebar: evorest
product:
---

# Общее введение
TODO: взять главные мысли и разнести по другим разделам.

| Термин        | Описание      | Пример|
| ------------- |:-------------:| -----:|
| Ресурс        | обозначается английским словом в единственном числе | product |
| ПодРесурс        | ::= Ресурс{.ПодРесурс}      |   product.quantity |
| ТипОперации   | ::= { read, write }      |    read |
| OAuth scope | ::= Ресурс:ТипОперации | product-image:write |
| ТипСобытия | ::= to_camel_case(Ресурс)Event | ProductImageEvent |
| ТипДействия | ::= {created, updated, removed, etc} | created |

Условием для возникновения события является изменение состояния некого ресурса или прочего внутреннего состояния системы.

Для получения событий приложение должно явно указать в настройках интеграции, изменения каких ресурсов приложение хочет получать, а также дополнительно иметь разрешение на чтение соответствующих ресурсов. Например, для получения событий _DeviceEvent_, приложение должно быть подписано на изменения ресурса _device_ и иметь разрешение _device:read_.

Изменение состояния подресурса приводит к возникновению события, соответствующего ресурсу. Например, изменение _device.firmware_ приводит к возникновению событий _DeviceEvent_. Причем модель события определяется существующими разрешениями приложения: если у приложения есть разрешение _device.firmware:read_, то в модели будет присутствовать значение поля _firmware_.

События могут быть получены через [Events API] или посредством [Webhook]. В последнем случае разработчик должен указать настройки доставки сообщений.

## Схема событий

Каждое событие удовлетворяет следующей схеме:

```json
{
    "action": "ТипДействия",
    "id": "уникальный идентификатор события",
    "payload": {

    },
    "timestamp": 1534431847922,
    "type": "ТипСобытия"
}
```

Модель _payload_ полностью идентична модели ресурса, при получении через rest api.
Например, получаемая через GET /devices/{id} модель данных долностью идентична модели данных события _DeviceEvent_.

Самыми распространенными типами действий являются _created_, _updated_ и _removed_.
Модель данных для всех типов действий одного типа событий всегда идентична.

## Типы событий и их содержание

### StoreEvent {#StoreEvent}

Событие об изменении магазина.
Доступные типы действий: _created_, _updated_ и _removed_.

```json
{
    "action": "updated",
    "id": "ded7bf3a-3c51-4f0e-9872-05f75aab041d",
    "payload": {
        "address": "Новый адрес 1534431862462",
        "created_at": "2018-04-17T10:11:49.670+0000",
        "id": "20180417-2396-4075-808C-50046017B78F",
        "name": "Новое имя 1534431862462",
        "updated_at": "2018-08-16T15:04:22.548+0000",
        "user_id": "02-200000000000108"
    },
    "timestamp": 1534431863189,
    "type": "StoreEvent"
}
```

### DeviceEvent {#DeviceEvent}

Событие об изменении терминала.
Доступные типы действий: _created_, _updated_ и _removed_.

```json
{
    "action": "updated",
    "id": "5e84621d-e5c7-4348-84ce-981bfaac0978",
    "payload": {
        "created_at": "2018-07-16T16:26:31.160+0000",
        "id": "20180716-D877-40E8-8033-350054333E4B",
        "name": "Новое имя 1534431843089",
        "store_id": "20180417-2396-4075-808C-50046017B78F",
        "updated_at": "2018-08-16T15:04:03.455+0000",
        "user_id": "02-200000000000108"
    },
    "timestamp": 1534431843930,
    "type": "DeviceEvent"
}
```

### EmployeeEvent {#EmployeeEvent}

Событие об изменении сотрудника.
Доступные типы действий: _created_, _updated_ и _removed_.

```json
{
    "action": "updated",
    "id": "5e84621d-e5c7-4348-84ce-981bfaac0978",
    "payload": {
        "id": "20180417-A1FF-4089-8048-D71414E1568D",
        "role": "ADMIN",
        "name": "Администратор",
        "stores": [
          "20180417-2396-4075-808C-50046017B78F"
        ],
        "user_id": "02-200000000000108",
        "created_at": "2018-04-17T10:11:49.244+0000",
        "updated_at": "2018-07-16T16:00:10.662+0000"
    },
    "timestamp": 1534431843930,
    "type": "EmployeeEvent"
}
```

### DocumentEvent {#DocumentEvent}

Событие об создании документа.
Доступные типы действий: _created_.

```json
{
    "action": "created",
    "id": "a7619739-3548-450c-8752-34bd565b7f1e",
    "payload": {
        "body": {},
        "close_date": "2018-08-16T15:04:08.221+0000",
        "close_user_id": "02-200000000000108",
        "device_id": "20180716-D877-40E8-8033-350054333E4B",
        "extras": {},
        "id": "a7619739-3548-450c-8752-34bd565b7f1e",
        "number": 42,
        "session_id": "74e23e47-f393-4927-bb56-87ba127ad700",
        "session_number": 42,
        "store_id": "20180417-2396-4075-808C-50046017B78F",
        "time_zone_offset": 10800000,
        "type": "OPEN_SESSION",
        "user_id": "02-200000000000108",
        "version": "V2"
    },
    "timestamp": 1534431848373,
    "type": "DocumentEvent"
}
```

### ProductEvent {#ProductEvent}

Событие об изменении продукта.
Доступные типы действий: _created_, _updated_ и _removed_.


```json
{
    "action": "created",
    "id": "0e748db2-877d-4c12-bf27-9f040c09eae3",
    "payload": {
        "allow_to_sell": true,
        "cost_price": 1,
        "created_at": "2018-08-16T15:04:11.472+0000",
        "id": "4be33817-bfb2-4e9f-90d0-3286c8c39f7e",
        "measure_name": "литр",
        "name": "Простой новый продукт 1534431849835",
        "price": 1,
        "store_id": "20180417-2396-4075-808C-50046017B78F",
        "tax": "NO_VAT",
        "type": "NORMAL",
        "updated_at": "2018-08-16T15:04:11.472+0000",
        "user_id": "02-200000000000108"
    },
    "timestamp": 1534431851496,
    "type": "ProductEvent"
}
```

### ProductGroupEvent {#ProductGroupEvent}

Событие об изменении группы продуктов.
Доступные типы действий: _created_, _updated_ и _removed_.

TODO: // json model

### ProductImageEvent {#ProductImageEvent}

Событие об изменении изображения продукта.
Доступные типы действий: _created_ и _removed_.

```json
{
    "action": "created",
    "id": "28774f47-48d4-4e63-8952-481cd5a1db44",
    "payload": {
        "created_at": "2018-08-16T15:04:20.672+0000",
        "id": "7ad64074-db99-41a7-a798-c9ccc5c81bf4",
        "meta": {
            "extension": "png",
            "file_size": 132385,
            "height": 300,
            "width": 300
        },
        "product_id": "251d1a99-f94f-4716-a37b-fd55c7a65edb",
        "store_id": "20180417-2396-4075-808C-50046017B78F",
        "updated_at": "2018-08-16T15:04:20.672+0000",
        "url": "https://static-test.evotor.ru/images/IBgEFyOWQHWAjFAEYBe3jwJR0amflPRxaje/1Vx6Ze2wetZAdNuZQaenmMnMxcgb9A",
        "user_id": "02-200000000000108"
    },
    "timestamp": 1534431860945,
    "type": "ProductImageEvent"
}
```

### SystemEvent {#SystemEvent}

Системное событие, доступное через [Event API].
Используется исключительно в технических целях, чтобы предотвратить разрыв клиентского соединения при [Event Streaming].
Доступные типы действий: _initialized_, _ping_

```json
{
    "action": "ping",
    "id": "35f9916b-ecf5-48e6-bffb-95ae85a35e37",
    "timestamp": 1534431886387,
    "type": "SystemEvent"
}
```
