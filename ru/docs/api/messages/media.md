# Отправка сообщений с медиаданными

`/v1/messages`

Для отправки изображений, видео, аудио и документов корреспонденту или в группу используйте узел `messages`.

В Green API поддерживается два варианта отправки файлов:
1. [Отправка файлов через узел `media`](#media)
2. [Отправка файлов по ссылке](#link)

## 1. Отправка файлов через узел `media` {#media}
Для отправки сообщения с файлом требуется предварительно выгрузить файл в облачное хранилище `media` согласно документации [Отправка медиаданных](../media/upload.md). Поле выгрузки будет получен идентификатор файла `your-media-id`, который следует указать при отправке сообщения в поле `id`.
Используйте этот вариант отправки, если файл расположен локально на сервере и недоступен из сети Интернет.

## 2. Отправка файлов по ссылке {#link}
Если файл уже опубликован в сети Интернет и доступен для скачивания по ссылке, то при отправке сообщения достаточно указать ссылку на этот файл в поле `link`. В этом случае выгружать файл в облачное хранилище `media` не требуется.

## Запрос {#request}

Для отправки изображения, видео, аудио или документа требуется выполнить запрос по адресу:
```
POST https://api.green-api.com/v1/messages
```

```json
{
    "recipient_type": "individual" | "group",
    "to": "whatsapp-id" | "whatsapp-group-id",
    "type":  "image" | "video" | "audio" | "document",

    "image": {
        "link": "http(s)://the-url",
        "provider": {
            "name" : "provider-name"
        },
        "caption": "your-image-caption"
    },
    
    "image": {
        "id": "your-media-id",
        "caption": "your-image-caption"
    },

    "video": {
        "link": "http(s)://the-url",
        "provider": {
            "name" : "provider-name"
        },
        "caption": "your-video-caption"
    },

    "video": {
        "id": "your-media-id",
        "caption": "your-video-caption"    
    },    

    "audio": {
        "link": "http(s)://the-url",
        "provider": {
            "name" : "provider-name"
        }
    },

    "audio": {
        "id": "your-media-id"  
    },      

    "document": {
        "link": "http(s)://the-url",
        "provider": {
            "name" : "provider-name"
        },
        "caption": "your-document-caption",
        "filename": "your-document-filename"
    },
    
    "document": {
        "id": "your-media-id",
        "caption": "your-document-caption",
        "filename": "your-document-filename"
    },

}
```

> Тело запроса приведено в качестве примера. В примере перечислены все возможные варианты отправки медиаданных. Тело действительного запроса может содержать только один объект медиаданных см. ниже.

### Параметры запроса {#request-parameters}

Параметр | Тип | Обязательный | Описание
----- | ----- | ----- | -----
`preview_url`* | **boolean** | Нет | Если сообщение содержит URL, то данная опция активирует предпросмотр содержимого по URL. Подробнее в документации [Отправка URL в текстовых сообщениях](https://developers.facebook.com/docs/whatsapp/api/messages/text#urls)
`recipient_type`* | **string** | Нет | Определяет тип получателя - корреспондент или группа. Возможные значения: `individual` - отправка корреспонденту; `group` - отправка в группу. Тип получателя определятся автоматически по значению поля `to`, поэтому указывать параметр не обязательно
`to` | **string** | Да | [Идентификатор корреспондента или группы](../chat-id.md) - получатель сообщения
`type` | **string** | Нет | Тип отправляемого сообщения. При отправке текстового сообщения указывать параметр не обязательно. Значение по умолчанию: `text`
`text ` | **object** | Да | Объект текстового сообщения

> \* Параметр можно не указыать, он используется для совместимости с официальной версией [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp)

Объект `text`

Параметр | Тип | Обязательный | Описание
----- | ----- | ----- | -----
`body ` | **string** | Да | Текст сообщения. Может содержать несколько [URL](https://developers.facebook.com/docs/whatsapp/api/messages/text?locale=ru_RU#urls) и [форматирование](https://developers.facebook.com/docs/whatsapp/api/messages/text?locale=ru_RU#formatting). Максимальная длина текстового сообщения составляет 4096 символов. Поддерживаются символы emoji 😃

### Пример тела запроса {#request-example-body}

Отправка сообщения корреспонденту:
```json
{
    "to": "79001234567",
    "type":"text",    
    "text": {
        "body": "I use Green-API to send this message to you!"
    }    
}
```

Отправка сообщения в группу:
```json
{
    "to": "79001234567-1581234048",
    "type":"text",    
    "text": {
        "body": "I use Green-API to send this message to the Group!"
    }    
}
```
## Ответ {#response}

При успешном ответе возвращается код HTTP `201`

### Поля ответа {#response-parameters}

Поле | Тип |  Описание
----- | ----- | -----
`messages` | **array** | Массив идентификаторов отправленных сообщений 


Массив `messages`

Поле | Тип |  Описание
----- | ----- | -----
`id ` | **string** | Идентификатор отправленного сообщения 

### Пример тела ответа {#response-example-body}

```
201 Created
```

```json
{
    "messages": [
        {
            "id": "1234"
        }
    ],
    "meta": {
        "api_status": "stable",
        "version": "2.0.1"
    }
}
```

### Ошибки {#errors}

Перечень общих для всех методов ошибок смотрите в разделе [Стандартные ошибки](../common-errors.md)

В случае ошибки возвращается код HTTP `400` с подробным описанием ошибки в теле ответа.

### Пример тела ответа с ошибкой {#response-example-body-error}

```json
{
    "errors": [
        {
            "code": 82,
            "details": "Outgoing messages limit exceeded",
            "title": "Превышено колличество исходящих сообщений"
        }
    ],
    "meta": {
        "api_status": "stable",
        "version": "2.0.1"
    }
}
```

## Пример кода на Python  {#request-example-python}

```python
###
```