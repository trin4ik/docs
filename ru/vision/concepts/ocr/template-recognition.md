# Распознавание документов

_Распознавание документов по шаблонам находится на [стадии Preview](../../../overview/concepts/launch-stages.md) и не тарифицируется дополнительно, его использование тарифицируется [по правилам для распознавания текста](../../pricing.md)._

Вы можете извлекать стандартные поля и распознавать текст шаблонов и документов: паспорта, водительского удостоверения, свидетельства о регистрации транспортного средства (СТС), регистрационных номеров автомобилей.

## Модели распознавания {#models}

Чтобы распознать текст документа, в значении параметра `model` массива `text_detection_config` укажите одну из следующих моделей распознавания:
* `passport` — паспорт, основной разворот.
* `driver-license-front` — водительское удостоверение, лицевая сторона.
* `driver-license-back` — водительское удостоверение, обратная сторона.
* `vehicle-registration-front` — свидетельство о регистрации транспортного средства, лицевая сторона.
* `vehicle-registration-back` — свидетельство о регистрации транспортного средства, обратная сторона.
* `license-plates` — все регистрационные номера автомобилей на изображении.

### Страны, чьи документы доступны для распознавания {#countries}

{% list tabs %}

- Паспорта

  * Россия
  * Россия (вид на жительство)
  * Азербайджан
  * Армения
  * Белоруссия
  * Германия
  * Грузия
  * Израиль
  * Италия
  * Казахстан
  * Киргизия
  * Латвия
  * Молдавия
  * Таджикистан
  * Тунис
  * Туркменистан
  * Турция
  * Узбекистан
  * Украина
  * Франция

- Водительские удостоверения, СТС, регистрационные номера автомобилей

  * Россия
  * Азербайджан
  * Армения
  * Белоруссия
  * Германия
  * Греция
  * Грузия
  * Израиль
  * Казахстан
  * Киргизия
  * Латвия
  * Литва
  * Молдавия
  * Польша
  * Таджикистан
  * Туркменистан
  * Узбекистан
  * Украина
  * Швейцария
  * Эстония

{% endlist %}

## Пример {#example}

### Запрос на распознавание {#example-query}

Файл `body.json`:

```json
{
    "folderId": "<идентификатор_каталога>",
    "analyze_specs": [{
        "content": "<объект_распознавания>",
        "features": [{
            "type": "TEXT_DETECTION",
            "text_detection_config": {
                "language_codes": ["<язык_текста>"],
                "model": "<модель_распознавания>"
            }
        }]
    }]
}
```

Где:

* `folderId` — [идентификатор каталога](../../../resource-manager/operations/folder/get-id.md).
* `content` — изображение, [кодированное в Base64](../../operations/base64-encode.md).
* `language_codes` — [язык текста](supported-languages.md). Для автоматического определения языка текста укажите `*`.
* `model` — модель распознавания.

### Ответ на запрос {#example-answer}

В ответе моделей `passport`, `driver-license-front`, `driver-license-back`, `vehicle-registration-front` и `vehicle-registration-back` будет массив `entities`.

Ответ модели `license-plates` не содержит массив `entities`. Эта модель распознает все регистрационные номера автомобилей на изображении и не распознает другой текст. При этом полнота и точность распознавания регистрационных номеров автомобилей у этой модели значительно выше, чем у общей модели OCR. Результаты распознавания отображаются в [стандартном ответе text_detection](../ocr/index.md#response).

{% note warning %}

Модель `license-plates` не поддерживает [автоматическое определение языка](../../operations/ocr/text-detection-image.md#ocr-api-recognition). Чтобы использовать эту модель, обязательно укажите [язык текста](./supported-languages.md), например `ru`.

{% endnote %}

Пример вывода массива `entities` для модели `passport` в ответе сервиса:

```json
{         "entities": [
         {
          "name": "name",
          "text": "елена"
         },
         {
          "name": "middle_name",
          "text": "михайловна"
         },
         {
          "name": "surname",
          "text": "агапова"
         },
         {
          "name": "gender",
          "text": "жен"
         },
         {
          "name": "citizenship",
          "text": "rus"
         },
         {
          "name": "birth_date",
          "text": "12.05.1978"
         },
         {
          "name": "birth_place",
          "text": "гор.пенза пензенского р-на пензенской обл."
         },
         {
          "name": "number",
          "text": "0702084625"
         },
         {
          "name": "issued_by",
          "text": "отделом уфмс россии по пензенской обл. центрального района гор.пенза."
         },
         {
          "name": "issue_date",
          "text": "10.05.2011"
         },
         {
          "name": "expiration_date",
          "text": "-"
         }
        ]}
```

Список полей в массиве `entities`:

* `passport`
    * `name` — имя.
    * `middle_name` — отчество.
    * `surname` — фамилия.
    * `gender` — пол.
    * `citizenship` — гражданство.
    * `birth_date` — дата рождения.
    * `birth_place` — место рождения.
    * `number` — номер паспорта.
    * `issued_by` — кем выдан.
    * `issue_date` — дата выдачи.
    * `subdivision` — код подразделения.
    * `expiration_date` — дата окончания срока действия.
* `driver-license-front`
    * `name` — имя.
    * `middle_name` — отчество.
    * `surname` — фамилия.
    * `number` — номер водительского удостоверения.
    * `birth_date` — дата рождения.
    * `issue_date` — дата выдачи.
    * `expiration_date` — дата окончания срока действия.
* `driver-license-back`
    * `experience_from` — водительский стаж (с какого года).
    * `number` — номер водительского удостоверения.
    * `issue_date` — дата выдачи.
    * `expiration_date` — дата окончания срока действия.
    * `prev_number` — номер предыдущего водительского удостоверения.
* `vehicle-registration-front`
    * `stsfront_car_number` — автомобильный номер.
    * `stsfront_vin_number` — VIN-номер транспортного средства.
    * `stsfront_car_brand` — марка транспортного средства.
    * `stsfront_car_model` — модель транспортного средства.
    * `stsfront_car_year` — год выпуска транспортного средства.
    * `stsfront_car_chassis_number` — номер шасси.
    * `stsfront_car_trailer_number` — номер кузова, кабины или прицепа.
    * `stsfront_car_color` — цвет транспортного средства.
    * `stsfront_sts_number` — номер СТС.
* `vehicle-registration-back`
    * `stsback_car_owner` — ФИО собственника или имя юридического лица.
    * `stsback_sts_number` — номер СТС.

{% note info %}

При низкой [достоверности распознавания](./index.md#confidence) часть полей может отсутствовать в результатах распознавания.

{% endnote %}

#### Что дальше {#what-is-next}

* [Посмотрите список поддерживаемых языков и моделей](supported-languages.md)
* [Посмотрите известные ограничения в текущей версии](known-issues.md)
* [Попробуйте распознать текст на изображении](../../operations/ocr/text-detection-image.md)