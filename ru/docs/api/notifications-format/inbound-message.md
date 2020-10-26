# Формат входящего сообщения

Уведомление данного формата приходит при получении входящего сообщения: текст, изображение, видео, голосовое сообщение, документ, контакт, геопозиция.

## Формат уведомления

```json
{
    "type": "notification-type",
    "account": {
        "id": "account-id",
        "wa_id": "account-wa-id"
    },
    "messages": [
        {
            "from": "sender-wa-id",
            "id": "message-id",
            "timestamp": "message-timestamp",
            "type": "text | image | video | voice | document | contacts | location",

            "text": {
                "body": "text-message-content"
            },

            "image": {
                "id": "media-id",
                "mime_type": "media-mime-type",
                "file_extension": "source-file-extension",
                "caption": "image-caption"
            },

            "video": {
                "id": "media-id",
                "mime_type": "media-mime-type",
                "file_extension": "source-file-extension",
                "caption": "video-caption"
            },

            "voice": {
                "id": "media-id",
                "mime_type": "media-mime-type",
                "file_extension": "source-file-extension"
            },

            "document": {
                "id": "media-id",
                "mime_type": "media-mime-type",
                "file_extension": "document-file-extension",
                "filename": "document-file-name"
            },

            "contacts": {
                "vcard": "vcard-data"
            },

            "location": {
                "link": "location-link"
            }

        }
    ],
    "contacts": [
        {
            "profile": {
                "name": "sender-profile-name"
            },
            "wa_id": "sender-wa-id"
        }
    ]
}
```

> Тело уведомления приведено в качестве примера. В примере перечислены все возможные варианты входящих сообщений. Тело действительного ответа может содержать только один объект сообщения: `text`, `image`, `video`, `voice`, `document`, `contacts`, `location`.

## Параметры уведомления {#notification-parameters}

Параметр | Тип | Описание
----- | ----- | -----
`type` | **string** | Тип уведомления. Для входящих сообщений поле принимает значение `inbound_message`
`account` | **object** | [Объект аккаунта](). Содержит данные аккаунта, который получил уведомление
`messages` | **object** | [Объект сообщения](). Содержит данные полученного сообщения
`contacts` | **object** | [Объект контакта](). Содержит данные отправителя сообщения

### Объект `messages` {#notification-object-messages}

Параметр | Тип | Описание
----- | ----- | -----
`from` | **string** | [Идентификатор корреспондента или группы](../chat-id.md) - отправитель сообщения
`id` | **string** | Идентификатор полученного сообщения
`timestamp` | **integer** | Время получения сообщения в UNIX-формате
`type` | **string** | Тип полученного сообщения. Возможные значения: `text`, `image`, `video`, `voice`, `document`, `contacts`, `location`
`text` | **object** | [Объект текстового сообщения](#notification-object-messages-text)
`image` | **object** | [Объект с данными изображения](#notification-object-messages-image)


#### Объект `text` {#notification-object-messages-text}

Параметр | Тип | Описание
----- | ----- | -----
`body ` | **string** | Текст полученного сообщения. Может содержать несколько URL и [форматирование](../formatting.md). Максимальная длина текстового сообщения составляет 4096 символов. Поддерживаются символы emoji 😃


#### Объект `image` {#notification-object-messages-image}

Параметр | Тип | Описание
----- | ----- | -----
`id` | **string** | Идентификатор файла изображения из облачного хранилища `media`. Для загрузки файла используйте метод [Получение медиаданных](../media/download.md)
`mime_type` | **string** | [MIME](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_MIME-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2) тип файла
`file_extension` | **string** | Расширение полученного файла, например `jpeg`
`caption` | **string** | Описание полученного изображения. Отображается в чате под изображением

#### Объект `video` {#notification-object-messages-video}

Параметр | Тип | Описание
----- | ----- | -----
`id` | **string** | Идентификатор видео-файла из облачного хранилища `media`. Для загрузки файла используйте метод [Получение медиаданных](../media/download.md)
`mime_type` | **string** | [MIME](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_MIME-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2) тип файла
`file_extension` | **string** | Расширение полученного файла, например `mp4`
`caption` | **string** | Описание полученного видео. Отображается в чате под видео

#### Объект `voice` {#notification-object-messages-voice}

Параметр | Тип | Описание
----- | ----- | -----
`id` | **string** | Идентификатор файла голосового сообщения из облачного хранилища `media`. Для загрузки файла используйте метод [Получение медиаданных](../media/download.md)
`mime_type` | **string** | [MIME](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_MIME-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2) тип файла
`file_extension` | **string** | Расширение полученного файла, например `ppt`

#### Объект `document` {#notification-object-messages-document}

Параметр | Тип | Описание
----- | ----- | -----
`id` | **string** | Идентификатор файла документа из облачного хранилища `media`. Для загрузки файла используйте метод [Получение медиаданных](../media/download.md)
`mime_type` | **string** | [MIME](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_MIME-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2) тип файла
`file_extension` | **string** | Расширение полученного файла, например `pdf`
`filename` | **string** | Полное имя файла документа, указанное при отправке


#### Объект `contacts` {#notification-object-messages-contacts}

Параметр | Тип | Описание
----- | ----- | -----
`vcard ` | **string** | Данные карточки контакта. Например: "\nN:;GreenAPI\nFN:GreenAPI\nTEL;WAID=79001234567:+7 900 123 4567\nTEL;WAID=79001234567:+7 900 123 4567\nX-AB-LABEL:\nX-AB-LABEL:\n"


#### Объект `location` {#notification-object-messages-location}

Параметр | Тип | Описание
----- | ----- | -----
`link ` | **string** | Ссылка на геообъект на картах maps.google.com. Например: https://maps.google.com/maps?q=55.7416,37.6201&z=17&hl=ru


## Примеры {#notifications-example}

### Получено текстовое сообщение {#notification-example-text}

```json
{
    "type": "inbound_message",
    "account": {
        "id": 22123456,
        "wa_id": "79001234567"
    },
    "messages": [
        {
            "from": "79001234568",
            "id": 1234,
            "timestamp": 1603666324,
            "text": {
                "body": "I use Green-API to get this message from you!"
            },
            "type": "text"
        }
    ],
    "contacts": [
        {
            "profile": {
                "name": "Andrew"
            },
            "wa_id": "79001234568"
        }
    ]
}
```