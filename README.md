### UPDATED 31/10/2020 - апдейты отмечены :octopus:
- Скрытие опции в корзине для клиента
- Чёрный/белый списки для `parent`

---

# Объект конфига
Конфиг опций представляет из себя массив объектов JSON

```
[           <-- начало массива
  {...},    <-- опция 1
  {...},    <-- опция 2
  ...
]           <-- конец массива
```

## Опция
Каждый объект, в совою очередь, содержит в себе все необходимые сведения об опции и ее значениях

Ключи объекта опции:

`id` - тип: строка
- обязательный
- идентификатор опции, должен быть уникальным

`title` - тип: строка
- обязательный
- название опции, сохраняется в ордере для администраторов и клиентов
- :octopus: если `title` опции начинается с символа `_`, то данная не будет отображена в корзине клиента, но будет передана в ордер администраторам

`label` - тип: строка
- если заполнено, замещает собой поле `title` для клиента (на фронте магазина)

`comment` - тип: строка
- комментарий к опции, отображение зависит от типа опции

`type` - тип: строка
- обязательный
- возможные значения: `checkbox`, `radio`, `switch`, `text-input`, `button`, `range`, `range-double`, `hidden`, `select-custom`, `select`, `select-key` (устаревший), `select-raid` (устаревший)

`values` - тип: массив, обязательный
- Массив значений опций, будут описаны отдельно в контексте типов
- Сама опция видима и доступна для выбора если есть хотя бы одно значение в группе values (исключение типы `hidden` и `text-input`).

`size` - тип: строка
- возможные значения: `is-full`, `is-half`
- по умолчанию `is-half`
- определяет вёрстку элемента в десктопной версии, какое пространство по ширине он занимает, половину блока или целый блок, в мобильной версии игнорируется

`required` - тип: булевый
- по умолчанию `false`
- определяет является ли опция обязательной для заполнения

`invalidMessage` - тип: строка
- по умолчанию `This option is required`
- Сообщение при ошибке валидации опции

`minLimit` - тип: числовой
- работает для типов: checkbox
- комбириуется c `required`
- определяет минимальное требуемое количество заполненных значений (values) опции

`maxLimit` - тип: числовой
- работает для типов: checkbox
- комбириуется c `required`
- определяет максимально возможное количество заполненных значений (values) опции

`quick` - тип: массив
- только для типов `select-custom`, `select-key`, `select-dungeon`
- содержит ID "быстрых опций", которые отрисовываются рядом с элементом списка

`translations` - тип: объект
- объект перевода, ключами объекта являются языковые локали магазина `de`, `fr`...
- каждый ключ является в свою очередь объектом, который замещает собой любые вышеописанные ключи кроме `id`, `title` и `values`

### Общие значения опций

`id` - тип: строка
- обязательный
- идентификатор опции, должен быть уникальным

`name` - тип: строка
- обязательный
- название значения опции, сохраняется в ордере для администраторов и клиентов

`label` - тип: строка
- если заполнено, замещает собой поле `name` для клиента (на фронте магазина)

`translations` - тип: объект
- объект перевода, ключами объекта являются языковые локали магазина `de`, `fr`...
- каждый ключ является в свою очередь объектом, который замещает собой любые поля кроме `id`, `price`, `name`

`default` - тип: булевый
- по умолчанию `false`
- определяет начальное положение опции **при загрузке страницы**
- может быть автоматически переопределно при переключении родительских опций

`price` - тип: числовой
- цена опции в базовой валюте магазина
- может быть отрицательной
- если равна 0, то не отображается на фронте

`priceLabel` - тип: числовой
- замещает собой цену
- может использоваться для скрытия реальной стоимости опций при установке значения `0`

`disabled` - тип: булевый
- значение заблокировано для выбора, но доступно для просмотра

`parents` - тип: массив
- включает ID и/или массивы ID родительских элементов (именно значений опций) для данного значения из **других опций**.
- Если элементов является ID, то значение доступно для выбора, если данный ID отмечен выбранным.
- Если элементом является массив, то значение доступно для выбора, если все ID массива отмечены выбранными.

`parentsMode` :octopus: - тип: строка
- возможные значения: `whitelist`, `blacklist`
- по умолчанию: `whitelist`
- `blacklist` - в режие blacklist делает опцию невидимой для всех значений в ключе `parents`

`series` - тип: строка
- идентификатор группы родственных значений **внутри** группы. Служит для фиксации значения выбора пользователя при переключении родительских элементов.

### Опции и специальные ключи значений:

## `checkbox`
Содержит одно или несколько значений ключа `values`, множественный выбор значения в группе.

`image` - тип: строка
- url картинки, которая замещает собой элемент

`enabled` - тип: булевый
- фиксирует опцию как отмеченную, не прибавляет цену

`itemLink` - тип: строка
- принимает HTML, не совсемтим с режиомом `image`
- _пример_ `<a href=\"https://de.wowhead.com/achievement=14194/hallen-der-hingabe\" target=\"_blank\" title=\"Halls of Devotion\" rel=\"noopener noreferrer\">Hallen der Hingabe</a>`

## `button`
Содержит одно или несколько значений ключа `values`, множественный выбор значения в группе.

## `switch`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

## `radio`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

`image` - тип: строка
- url картинки, которая замещает собой элемент

## `select`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

## `select-custom`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

`image` - тип: строка
- url картинки, которая прикрепляется к элементу

## `range` и `range-double`
`mark` - тип: булевый или строка
- по умолчанию `false`
- отображается ли значение на шкале рендж-слайдера
- при строчном режиме замещает собой значение на шкале

`image` - тип: строка
- url картинки, которая замещает собой ползунок на соотвествующем значении

## `hidden`
- может применяться для передачи комментария по продукту в ордер или модификации цены

## `text-input`
- для передачи информации со свободным вводом

## Parents и Series

_В этом примере опция будет достуна при: (`parentId1`) ИЛИ (`parentId3` И `parentId4`) ИЛИ (`parentId5`)_

```json
"parents": [
  "parentId1",
  ["parentId3", "parentId4"],
  "parentId5"
]
```

_В этом примере при переклчюении родительского значения с `parentId1` на `parentId2` текущее значение опции будет унаследовано засчёт объединения значений в гурппу `siblings`, если опция `id1` была выбрана, то опция `id2` унаследует это значение._

```json
"values": [
  {
    "id": "id1",
    "parents": ["parentId1"],
    "series": "siblings"
  },
  {
    "id": "id2",
    "parents": ["parentId2"],
    "series": "siblings"
  }
]
```

Практическое применение: если опция `B` может принимать разную цену в зависимости от значения другой опции `A`. В этом случае опция `B` наполянется однотипными значениями `values`, но с разными ценами и значениями `parents`. А для того, чтобы выбор пользователя не сбивался, такие родственные значения объединяются в одну группу `series`.

В примере ниже значение чекбокса `Within Timer` будет сохраняться при переключении селектора ключа, засчёт того, что им объявлена одна группа `"series": "timerOptionYes"`, и каждый раз будет доступна только одна из этих опций, так как они привязаны к разным `parent`.

При этом `Personal Loot` станет доступным для выбора только в случае отметки обоих его `parent`

```json
[
  {
    "title": "Key",
    "id": "key",
    "type": "select-key",
    "values": [
      {
        "name": "MYTHIC +10",
        "price": 17.95,
        "default": true,
        "id": "keyValue10"
      },
      {
        "name": "MYTHIC +11",
        "price": 19.95,
        "id": "keyValue11"
      },
      {
        "name": "MYTHIC +12",
        "price": 21.95,
        "id": "keyValue12"
      }
    ]
  },
  {
    "title": "Timer Option",
    "id": "timerOption",
    "type": "checkbox",
    "values": [
      {
        "name": "Within Timer",
        "price": 10,
        "default": true,
        "id": "timerOptionYes1",
        "series": "timerOptionYes",
        "parents": [
          "keyValue10"
        ]
      },
      {
        "name": "Within Timer",
        "price": 12,
        "id": "timerOptionYes2",
        "series": "timerOptionYes",
        "parents": [
          "keyValue11"
        ]
      },
      {
        "name": "Within Timer",
        "price": 15,
        "id": "timerOptionYes3",
        "series": "timerOptionYes",
        "parents": [
          "keyValue12"
        ]
      },
      {
        "name": "Some option",
        "price": 5,
        "default": false,
        "id": "someId1",
        "parents": [
          "keyValue11",
          "keyValue12"
        ]
      },
      {
        "name": "Some another option",
        "price": 20,
        "default": false,
        "id": "someId2"
      }
    ]
  },
  {
    "title": "Personal Loot",
    "id": "personalLoot",
    "type": "checkbox",
    "values": [
      {
        "name": "Yes",
        "price": 10,
        "default": true,
        "id": "someId3",
        "series": "timerOptionYes",
        "parents": [
          ["keyValue11", "someId1"]
        ]
      }
    ]
  }
]

```

## Комментирование

Формат JSON не поддерживает комментарии, тем не менее можно использовать следующий синтаксис для комментриварония значений внутри объекта настроек не теряя при этом в функциональности:

```json
{
  "title": "Personal Loot",
  "//label": " -> это значение будет проигнорировано <- ",
  "label": "Visible label",
  "id": "personalLoot",
  "type": "checkbox",
  "values": [
    {
      "name": "Yes",
      "price": 10,
      "default": true,
      "id": "someId3",
      "series": "timerOptionYes",
      "parents": [
        ["keyValue11", "someId1"]
      ]
    }
  ]
}
```
