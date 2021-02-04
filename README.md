### UPDATED 31/01/2021 :octopus: - dev.env | prod.env
- возможность прмиенять метод калькуляции `add` к конкретной опции, а не только глобально

### UPDATED 29/12/2020
- новый параметр занчения `passToHpOrder`
- возможность использовать множители и другие правила ценообразования

### UPDATED 09/12/2020
- тип опции `hp-event`

### UPDATED 12/11/2020
- `richComment` для типов опций `checkbox` и `radio`

### UPDATED 31/10/2020
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
Ключи объекта опции:

`id` - тип: строка
- обязательный
- идентификатор опции, должен быть уникальным

`title` - тип: строка
- обязательный
- название опции, сохраняется в ордере для администраторов и клиентов
- если `title` опции начинается с символа `_`, то данная опция и ее значение не будет отображены в корзине и ордере для клиента клиента, но будет передана в ордер администраторам

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
- возможные значения: `is-full`, `is-half`, `is-third`
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

`priceRule` - тип: объект
- содержит правила расчёта цены для всех значений данной опции. По сути `value` наследуют это значение и при необходимости может быть переназначено на уровне`value`.

### Ключи `priceRule`

`method` - тип: строка
- по умолчанию `add`
- значения `add` | `multiply`
- метод калькуляции, умножение или сложение
- при умножении можно использовать значения меньше 1, пример: множитель 0.5 даст -50%, в то время как множитель 1.5 - даст +50% к стоимости
- при сложении можно использовать отрицательные значения

`affectOptions` - тип: булевый или массив :octopus:
- по умолчанию `true`
- при `false` не применяется к опциям
- при `true` применяется ко всем опциям без исключения
- при массиве - массив ID опций или значений опций в любой комбинации. При указании и ID опции и ID ее значения мультипликатор сработает для этой опции только один раз

`affectBasePrice` - тип: булевый
- по умолчанию `true`
- только для метода `multiply`
- влияеит ли мультипликатор на базовую цену товара

`affectQuantity` - тип: булевый
- по умолчанию `true`
- только для метода `add`
- влияеит ли на количество товара

`viewMode` - тип: строка
- по умолчанию `percent`
- значения `percent` | `number`
- только для метода `multiply`
- режим отображения цены для опций-мультипликаторов
- при `percent` отображает добавочную цену в процентах
- при `number` отображает вклад данной опции в конечную цену продукта

[Пример конфига c ценами](#пример-конфига-с-ценовыми-правилами)

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

`priceLabel` - тип: числовой, строка
- замещает собой цену
- может использоваться для скрытия реальной стоимости опций при установке значения `0`

`disabled` - тип: булевый
- значение заблокировано для выбора, но доступно для просмотра

`parents` - тип: массив
- включает ID и/или массивы ID родительских элементов (именно значений опций) для данного значения из **других опций**.
- Если элементов является ID, то значение доступно для выбора, если данный ID отмечен выбранным.
- Если элементом является массив, то значение доступно для выбора, если все ID массива отмечены выбранными.

`parentsMode` - тип: строка
- возможные значения: `whitelist`, `blacklist`
- по умолчанию: `whitelist`
- `blacklist` - в режие blacklist делает опцию невидимой для всех значений в ключе `parents`

`series` - тип: строка
- идентификатор группы родственных значений **внутри** группы. Служит для фиксации значения выбора пользователя при переключении родительских элементов.

`passToHpOrder` - тип: булевый
- дефолтное значение `true`
- отвечат за то, будет ли значение передано в ордер на плтформе

`priceRule` - тип: объект
- содержит правила расчёта цены для текущего значения опции, имеет более высокий приоритет перед правилами назначенными на уровне `option`.

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
- не забывать экранировать кавычки `"` => `\"`
- ссылки внутри этого элемента НЕ кликабельны, служат для генреции тултипов по наведению на них. 
- при нажатии на элемент происходит отметка опции

`richComment` - тип: строка
- экранированный html, 
- может содержать ссылку
- рендерится строчно, после обычного комментария
- не совместим с режимом `image`

## `button`
Содержит одно или несколько значений ключа `values`, множественный выбор значения в группе.

## `switch`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

## `radio`
Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

`image` - тип: строка
- url картинки, которая замещает собой элемент

`richComment` - тип: строка
- экранированный html, 
- может содержать ссылку
- рендерится строчно, после обычного комментария
- не совместим с режимом `image`

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

## `hp-event`
Содержит одно или несколько значений ключа `values`, единичный выбор значения в группе. Может являться парентом, а также иметь парентов. Может иметь собственный ценовой модификатор как обычная опция, а также быть или не быть `required`. Механика опции в целом аналогична `radio`.

`hpId` - тип: строка
- ID товара на платформе

`faction` - тип: строка
- принимает значения `horde` и `alliance`
- фильтрует эвенты по признаку фракции
- *переключатель фракций реалзован в примере ниже*

`hideFaction` - тип: булевый
- по умолчанию `false`
- скрывает значение фракции эвента на плашках

`reserveDelay` - тип: число
- по умолчанию `0`
- едицицы измерения - минуты
- определяет отсрочку, за которую эвент становится недоступным для добавления в корзину

`disabled` - тип: булевый
- по умолчанию `false`
- блокировка выбора даты и эвента

`disabledMessage` - тип: строка
- комментарий причины блокировки выбора эвента/слота

```json
  {
    "title": "Slot reserve",
    "id": "slot",
    "size": "is-full",
    "type": "hp-event",
    "required": true,
    "values": [
     {
        "hpId": "f9b084fb659744f584b4b770651fbe99",
        "id": "slot-reserve0-a",
        "faction": "alliance",
        "parents": [
          ["keyValue103", "playMethodValueSelf"]
        ]
      },
     {
        "hpId": "f9b084fb659744f584b4b770651fbe99",
        "id": "slot-reserve0-h",
        "faction": "horde",
        "parents": [
          ["keyValue103", "playMethodValueAcc"]
        ]
      },
      {
        "hpId": "393c28fd77754d3bb59de3d2a9a4606f",
        "id": "slot-reserve1",
        "parents": [
          "keyValue136"
        ]
      },
      {
        "hpId": "03847083cb24423fa7fbc2940814e973",
        "id": "slot-reserve2",
        "parents": [
          [
            "keyValue114",
            "playMethodValueAcc",
            "loot-option-x3"
          ]
        ]
      },
      {
        "hpId": "e40326add71d4defb5ac9974cc3f8af7",
        "id": "slot-reserv32",
        "parents": [
          "keyValue125"
        ]
      }
    ]
  }
```

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

## Пример конфига

```json
[
  {
    "title": "Items group",
    "id": "items",
    "size": "is-full",
    "type": "checkbox",
    "values": [
      {
        "name": "Hallen der Hingabe",
        "itemLink": "<a href=\"https://de.wowhead.com/achievement=14194/hallen-der-hingabe\" target=\"_blank\" title=\"Halls of Devotion\" rel=\"noopener noreferrer\">Hallen der Hingabe</a>",
        "comment": "comment",
        "richComment": "<a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>",
        "price": 10.00,
        "default": false,
        "id": "items1"
      },
      {
        "name": "Verschlüsselte Schrift aus Ny'alotha",
        "itemLink": "<a href=\"https://ptr.wowhead.com/item=169223/ashjrakamas-shroud-of-resolve\" title=\"Ashjra'kamas, Shroud of Resolve\" target=\"_blank\" rel=\"noopener noreferrer\">Ashjra'kamas, Shroud of Resolve</a>",
        "comment": "comment",
        "price": 20.00,
        "default": false,
        "id": "items2",
        "parents": ["simple-select8"]
      }
    ]
  },
  {
    "title": "custom select image",
    "label": "",
    "comment": "Comment for select",
    "size": "is-half",
    "id": "select-simple",
    "type": "select",
    "values": [
      {
        "name": "simple-select1",
        "price": 17.95,
        "default": true,
        "id": "simple-select7"
      },
      {
        "name": "simple-select2",
        "price": 19.95,
        "default": false,
        "id": "simple-select8",
        "comment": "option comment"
      },
      {
        "name": "simple-select3",
        "price": 21.95,
        "default": false,
        "id": "simple-select9"
      },
      {
        "name": "simple-select4",
        "price": 23.95,
        "default": false,
        "id": "simple-select10"
      },
      {
        "name": "simple-select5",
        "price": 26.95,
        "default": false,
        "id": "simple-select11"
      }
    ]
  },
  {
    "title": "Switch",
    "id": "switch",
    "size": "is-half",
    "type": "switch",
    "values": [
      {
        "name": "Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "default": false,
        "id": "playMethodValueAcc5"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "default": true,
        "id": "playMethodValueSelf10"
      }
    ]
  },
  {
    "title": "Switch",
    "id": "switch2",
    "size": "is-full",
    "type": "switch",
    "values": [
      {
        "name": "Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "priceLabel": 0,
        "default": true,
        "id": "1playMethodValueAcc5"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "priceLabel": 0,
        "default": false,
        "id": "2playMethodValueSelf10"
      },
      {
        "name": "Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "default": false,
        "id": "3playMethodValueAcc5"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "default": false,
        "id": "4playMethodValueSelf10"
      },
      {
        "name": "Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "default": false,
        "id": "5playMethodValueAcc5"
      }
    ]
  },
  {
    "title": "Image Radio",
    "id": "playMethodasd",
    "size": "is-half",
    "type": "radio",
    "required": true,
    "values": [
      {
        "name": "Account Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "default": false,
        "id": "playMethodValueAccasd",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "default": false,
        "id": "playMethodValueSelf0asd",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "disabled": true,
        "default": false,
        "id": "playMethodValueSelf0asd00",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      }
    ]
  },
  {
    "title": "Timer Option",
    "comment": "Choose your run whether within timer or not",
    "id": "timerOptionzxc",
    "size": "is-full",
    "type": "checkbox",
    "invalidMessage": "Custom message",
    "required": true,
    "minLimit": 2,
    "maxLimit": 3,
    "values": [
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 0,
        "default": false,
        "id": "timerOptionYes1zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 3,
        "default": false,
        "id": "timerOptionYes2zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 4,
        "default": false,
        "id": "timerOptionYes3zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes4zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes5zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes6zxc",
        "image": "https://static-shpf.mageworx.com/img/uploads/productoptions/3212/b7eddc21b66a8e47ffdbd714453f69b4.png"
      }
    ]
  },
  {
    "title": "Widget Notification",
    "id": "widget",
    "type": "hidden",
    "values": [
      {
        "name": "WIDGET",
        "comment": "",
        "price": 0,
        "default": true,
        "id": "widget"
      }
    ]
  },
  {
    "title": "Input",
    "comment": "Comment for input",
    "id": "input3",
    "required": true,
    "size": "is-full",
    "type": "text-input",
    "values": [
      {
        "name": "",
        "price": 0,
        "default": true,
        "id": "input4"
      }
    ]
  },
  {
    "title": "Input",
    "comment": "Comment for input",
    "id": "input",
    "size": "is-half",
    "type": "text-input",
    "values": [
      {
        "name": "",
        "price": 0,
        "default": true,
        "id": "input1"
      }
    ]
  },
  {
    "title": "custom select image",
    "label": "",
    "comment": "Comment for select",
    "size": "is-full",
    "id": "select-custom36",
    "type": "select-custom",
    "values": [
      {
        "name": "custom-select1",
        "price": 17.95,
        "default": true,
        "id": "custom-select37"
      },
      {
        "name": "custom-select2",
        "price": 19.95,
        "default": false,
        "id": "custom-select38",
        "comment": "option comment"
      },
      {
        "name": "custom-select3",
        "price": 21.95,
        "default": false,
        "id": "custom-select39"
      },
      {
        "name": "custom-select4",
        "price": 23.95,
        "default": false,
        "id": "custom-select310"
      },
      {
        "name": "custom-select5",
        "price": 26.95,
        "default": false,
        "id": "custom-select311"
      }
    ]
  },
  {
    "title": "custom select image",
    "label": "",
    "comment": "Comment for select",
    "size": "is-half",
    "id": "select-custom6",
    "type": "select-custom",
    "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/wow-farm-transmog_91e71ca3-3d64-41a1-8309-5061fd572a0f.png?4880513842382269742",
    "values": [
      {
        "name": "custom-select1",
        "price": 17.95,
        "default": true,
        "id": "custom-select7"
      },
      {
        "name": "custom-select2",
        "price": 19.95,
        "default": false,
        "id": "custom-select8",
        "comment": "option comment"
      },
      {
        "name": "custom-select3",
        "price": 21.95,
        "default": false,
        "id": "custom-select9"
      },
      {
        "name": "custom-select4",
        "price": 23.95,
        "default": false,
        "id": "custom-select10"
      },
      {
        "name": "custom-select5",
        "price": 26.95,
        "default": false,
        "id": "custom-select11"
      }
    ]
  },
  {
    "title": "custom select",
    "label": "",
    "comment": "Comment for select",
    "size": "is-half",
    "id": "select-custom",
    "type": "select-custom",
    "values": [
      {
        "name": "custom-select1",
        "price": 17.95,
        "default": true,
        "id": "custom-select1",
        "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/raid1.png?2233282437125190411"
      },
      {
        "name": "custom-select2",
        "price": 19.95,
        "default": false,
        "id": "custom-select2",
        "comment": "option comment",
        "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/trophy3.png?2233282437125190411"
      },
      {
        "name": "custom-select3",
        "price": 21.95,
        "default": false,
        "id": "custom-select3",
        "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/raid1.png?2233282437125190411"
      },
      {
        "name": "custom-select4",
        "price": 23.95,
        "default": false,
        "id": "custom-select4",
        "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/raid1.png?2233282437125190411"
      },
      {
        "name": "custom-select5",
        "price": 26.95,
        "default": false,
        "id": "custom-select5",
        "image": "https://cdn.shopify.com/s/files/1/0987/2678/files/raid1.png?2233282437125190411"
      }
    ]
  },
  {
    "title": "Key",
    "label": "Pick a key level:",
    "id": "key3",
    "size": "is-full",
    "type": "select-key",
    "quick": [
      "keyValue103",
      "keyValue125"
    ],
    "values": [
      {
        "name": "MYTHIC +10",
        "priceLabel": 17.95,
        "price": 17.95,
        "default": true,
        "id": "keyValue103"
      },
      {
        "name": "MYTHIC +11",
        "priceLabel": 19.95,
        "price": 19.95,
        "default": false,
        "id": "keyValue114"
      },
      {
        "name": "MYTHIC +12",
        "priceLabel": 21.95,
        "price": 21.95,
        "default": false,
        "id": "keyValue125"
      },
      {
        "name": "MYTHIC +13",
        "priceLabel": 23.95,
        "price": 23.95,
        "default": false,
        "id": "keyValue136"
      },
      {
        "name": "MYTHIC +14",
        "priceLabel": 26.95,
        "price": 26.95,
        "default": false,
        "id": "keyValue147"
      },
      {
        "name": "MYTHIC +15",
        "priceLabel": 29.95,
        "price": 29.95,
        "default": false,
        "id": "keyValue158"
      }
    ]
  },
  {
    "title": "Key",
    "label": "Pick a key level:",
    "id": "key2",
    "size": "is-half",
    "type": "select-key",
    "quick": [
      "keyValue10",
      "keyValue15"
    ],
    "values": [
      {
        "name": "MYTHIC +10",
        "priceLabel": 17.95,
        "price": 17.95,
        "default": true,
        "id": "keyValue10"
      },
      {
        "name": "MYTHIC +11",
        "priceLabel": 19.95,
        "price": 19.95,
        "default": false,
        "id": "keyValue11"
      },
      {
        "name": "MYTHIC +12",
        "priceLabel": 21.95,
        "price": 21.95,
        "default": false,
        "id": "keyValue12"
      },
      {
        "name": "MYTHIC +13",
        "priceLabel": 23.95,
        "price": 23.95,
        "default": false,
        "id": "keyValue13"
      },
      {
        "name": "MYTHIC +14",
        "priceLabel": 26.95,
        "price": 26.95,
        "default": false,
        "id": "keyValue14"
      },
      {
        "name": "MYTHIC +15",
        "priceLabel": 29.95,
        "price": 29.95,
        "default": false,
        "id": "keyValue15"
      }
    ]
  },
  {
    "title": "Play Method",
    "id": "playMethod",
    "size": "is-half",
    "type": "radio",
    "values": [
      {
        "name": "Account Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "default": false,
        "id": "playMethodValueAcc"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "default": true,
        "id": "playMethodValueSelf0"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "disabled": true,
        "default": true,
        "id": "playMethodValueSelf000"
      }
    ]
  },
  {
    "title": "Timer Option",
    "comment": "Choose your run whether within timer or not",
    "id": "timerOption",
    "size": "is-full",
    "type": "checkbox",
    "values": [
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 0,
        "default": false,
        "id": "timerOptionYes1",
        "richComment": "<a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>",
        "series": "timerOptionYes",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12"
        ]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 0,
        "default": false,
        "id": "timerOptionYes10000",
        "series": "timerOptionYes",
        "disabled": true,
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12"
        ]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 3,
        "default": false,
        "id": "timerOptionYes2",
        "series": "timerOptionYes",
        "parents": [
          "keyValue13"
        ]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 4,
        "default": false,
        "id": "timerOptionYes3",
        "series": "timerOptionYes",
        "parents": [
          "keyValue14"
        ]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes4",
        "series": "timerOptionYes",
        "parents": [
          "keyValue15"
        ]
      }
    ]
  },
  {
    "title": "Loot Trading",
    "id": "loottrade",
    "size": "is-full",
    "type": "radio",
    "values": [
      {
        "name": "Personal Loot",
        "comment": "Run with Personal Loot option means that quantity of items you will get fully depends on your luck, which we wish you most of all :)",
        "price": 0,
        "default": true,
        "id": "loottrade10No",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 3,
        "default": false,
        "id": "loottrade10x1",
        "parents": [
          "keyValue10"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 6,
        "default": false,
        "id": "loottrade10x2",
        "parents": [
          "keyValue10"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 4,
        "default": false,
        "id": "loottrade11x1",
        "parents": [
          "keyValue11",
          "keyValue12",
          "keyValue13"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 8,
        "default": false,
        "id": "loottrade11x2",
        "parents": [
          "keyValue11",
          "keyValue12",
          "keyValue13"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 5,
        "default": false,
        "id": "loottrade14x1",
        "parents": [
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 10,
        "default": false,
        "id": "loottrade14x2",
        "parents": [
          "keyValue14",
          "keyValue15"
        ]
      }
    ]
  },
  {
    "title": "Loot Trading",
    "id": "loottrade1",
    "size": "is-half",
    "type": "radio",
    "values": [
      {
        "name": "Personal Loot",
        "comment": "Run with Personal Loot option means that quantity of items you will get fully depends on your luck, which we wish you most of all :)",
        "price": 0,
        "default": true,
        "id": "loottrade10No2",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 3,
        "default": false,
        "id": "loottrade10x13",
        "parents": [
          "keyValue10"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 6,
        "default": false,
        "id": "loottrade10x24",
        "series": "test",
        "parents": [
          "keyValue10"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 4,
        "default": false,
        "id": "loottrade11x15",
        "parents": [
          "keyValue11",
          "keyValue12",
          "keyValue13"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 8,
        "default": false,
        "id": "loottrade11x26",
        "series": "test",
        "parents": [
          "keyValue11",
          "keyValue12",
          "keyValue13"
        ]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 5,
        "default": false,
        "id": "loottrade14x17",
        "parents": [
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 10,
        "default": false,
        "series": "test",
        "id": "loottrade14x28",
        "parents": [
          "keyValue14",
          "keyValue15"
        ]
      }
    ]
  },
  {
    "title": "Range Slider Name",
    "label": "Range Slider Label",
    "id": "rangeSlider00",
    "size": "is-half",
    "type": "range",
    "values": [
      {
        "name": "op1",
        "comment": "Comment op1",
        "price": 5,
        "mark": true,
        "default": true,
        "id": "rangeSlider01",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op2",
        "comment": "Comment op2",
        "price": 12,
        "mark": true,
        "default": false,
        "id": "rangeSlider02",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op3",
        "comment": "Comment op3",
        "price": 13,
        "mark": false,
        "default": false,
        "id": "rangeSlider03",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op4",
        "comment": "Comment op4",
        "price": 14,
        "mark": true,
        "default": false,
        "id": "rangeSlider04",
        "series": "rangeSlider04",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op4a",
        "comment": "Comment op4a",
        "price": 14,
        "mark": true,
        "default": false,
        "series": "rangeSlider04",
        "id": "rangeSlider04-1",
        "parents": [
          "keyValue14"
        ]
      },
      {
        "name": "op5a",
        "comment": "Comment op5a",
        "price": 15,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider05-1",
        "parents": [
          "keyValue14"
        ]
      },
      {
        "name": "op5b",
        "comment": "Comment op5b",
        "price": 0,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider05-2",
        "parents": [
          "keyValue15"
        ]
      }
    ]
  },
  {
    "title": "Title Slider Double",
    "label": "Label Slider Double",
    "id": "rangeSlider01",
    "size": "is-full",
    "comment": "Choose your run whether within timer or not",
    "type": "range-double",
    "values": [
      {
        "name": "op1",
        "price": 1,
        "mark": true,
        "default": true,
        "id": "rangeSlider11",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op2",
        "price": 1,
        "mark": " ",
        "default": false,
        "id": "rangeSlider12",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op3",
        "price": 1,
        "mark": false,
        "default": false,
        "id": "rangeSlider13",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op4",
        "price": 1,
        "mark": true,
        "default": true,
        "id": "rangeSlider14",
        "series": "rangeSlider04",
        "parents": [
          "keyValue10",
          "keyValue11",
          "keyValue12",
          "keyValue13",
          "keyValue14",
          "keyValue15"
        ]
      },
      {
        "name": "op4a",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider04",
        "id": "rangeSlider14-1",
        "parents": [
          "keyValue14"
        ]
      },
      {
        "name": "op5a",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider15-1",
        "parents": [
          "keyValue14"
        ]
      },
      {
        "name": "op5b",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider15-2",
        "parents": [
          "keyValue15"
        ]
      }
    ]
  }
]
```

## Пример локализации

```json
[
  {
    "title": "Items group TITLE",
    "label": "Items group LABEL",
    "translations": {
      "de": {
        "label": "Items group LABEL DE"
      }
    },
    "id": "items",
    "size": "is-full",
    "type": "checkbox",
    "values": [
      {
        "name": "Hallen der Hingabe",
        "label": "labelEN",
        "itemLink": "<a href=\"https://ptr.wowhead.com/item=169223/ashjrakamas-shroud-of-resolve\" title=\"Ashjra'kamas, Shroud of Resolve\" target=\"_blank\" rel=\"noopener noreferrer\">Ashjra'kamas, Shroud of Resolve</a>",
        "comment": "comment",
        "price": -10.00,
        "default": false,
        "id": "items1",
        "translations": {
          "de": {
            "label": "label DE",
            "comment": "comment DE",
            "itemLink": "<a href=\"https://de.wowhead.com/achievement=14194/hallen-der-hingabe\" target=\"_blank\" title=\"Halls of Devotion\" rel=\"noopener noreferrer\">Hallen der Hingabe</a>"
          }
        }
      }
    ]
  }
]
```
---

## Пример конфига с ценовыми правилами

```json
[
  {
    "title": "opt1",
    "id": "opt1",
    "comment": "No config",
    "type": "checkbox",
    "values": [
      {
        "name": "0",
        "id": "opt1-0"
      },
      {
        "name": "+3",
        "price": 3,
        "id": "opt1-1"
      },
      {
        "name": "+5",
        "price": 5,
        "id": "opt1-2"
      }
    ]
  },
  {
    "title": "opt2",
    "comment": "add, affectQuantity-",
    "id": "opt2",
    "type": "checkbox",
    "priceRule": {
      "affectQuantity": false
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt2-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt2-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt2-2"
      }
    ]
  },
  {
    "title": "opt3",
    "id": "opt3",
    "comment": "multi, affectBasePrice+, affectQuantity+, all-options+",
    "type": "checkbox",
    "priceRule": {
      "method": "multiply",
      "viewMode": "number"
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt3-0"
      },
      {
        "name": "3",
        "price": 3,
        "comment": "rewritten viewMode",
        "id": "opt3-1",
        "priceRule": {
          "viewMode": "percent"
        }
      },
      {
        "name": "5",
        "comment": "rewritten calcType",
        "price": 5,
        "id": "opt3-2",
        "priceRule": {
          "method": "add"
        }
      }
    ]
  },
  {
    "title": "opt4",
    "comment": "multi, affectBasePrice+, affectQuantity+, opt1+",
    "id": "opt4",
    "type": "checkbox",
    "priceRule": {
      "method": "multiply",
      "affectOptions": ["opt1"]
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt4-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt4-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt4-2"
      }
    ]
  },
  {
    "title": "opt5",
    "id": "opt5",
    "comment": "multi, affectBasePrice-, affectQuantity-, opt2+",
    "type": "checkbox",
    "priceRule": {
      "method": "multiply",
      "affectOptions": ["opt2"],
      "affectBasePrice": false,
      "affectQuantity": false
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt5-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt5-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt5-2"
      }
    ]
  },
    {
    "title": "opt6",
    "comment": "multi, affectBasePrice-, affectQuantity-, allOpt+",
    "id": "opt6",
    "type": "checkbox",
    "priceRule": {
      "method": "multiply",
      "affectBasePrice": false,
      "affectQuantity": false,
      "viewMode": "number"
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt6-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt6-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt6-2"
      }
    ]
  }
]
```
