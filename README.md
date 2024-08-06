### v2.2.0 UPDATED 05/08/2024

- `passToDeal` - Ключ опциии - при формировании дила из товара скрывает отмеченную опцию из описания.

### v2.1.0 UPDATED 22/04/2024

- `schedule` на уровне опции

### v2.0.1 UPDATED 25/07/2023

- `Mixins` получили возможность использовать `scopes`

### v2.0.0 UPDATED 22/07/2023

- `Mixins` [Примеси конфигов](#mixins) - добавочные конфиги, которые можно включать в товары и управлять централизованно
- `Extensions` [Расширения конфигов](#extensions) - "прайс-листы на стероидах",расширение конфигов, для гибкого управления ценами в поворяющихся опциях
  - ключ `scopes` для всех `option` и `value`
- Обновление `checkbox-rich` и `radio-rich`
  - обновлена вёрстка
  - добавлено свойство `cols`
- Свойство `disabled` для `range`, `range-double`

### v1.6.0 - 1.7.0 UPDATED 14/06/2023

- новый тип опции `portal` и массив `portals` для всех `value`
  - порталы служат альтернативой для кастом-хтмл, в случае если кастом-хтмл нужно привязать к опции или к ее значениям. Содержимое портала аналогично кастом-хтмлу, но задаётся напрямую в велью, что позволяет не прописывать отдельно механику связи парент-чайлд, а просто наследовать ее. Одна опция может иметь множество порталов и пробрасывать их в разные места конфига.
- `checkbox`/`radio` тип лейаута `list`
- `image` для `switch`/`button`

### v1.4.0 - 1.5.0 UPDATED 16/02/2022

- добавление инпутов в опции-слайдеры
- возможность вынести объявление parent на уровень опции
- возможность динамически заблокировать опции целиком с указанием дополнительного комментария

### v1.3.0 UPDATED 11/04/2021

- тип опций `custom-html`

### v1.2.0 UPDATED 07/04/2021

- типы опций `radio-rich`, `checkbox-rich`
- ключ опции `cssClass`

### UPDATED 31/01/2021

- возможность применять метод калькуляции `add` к конкретной опции, а не только глобально

### UPDATED 29/12/2020

- новый параметр значения `passToHpOrder`
- возможность использовать множители и другие правила ценообразования

### UPDATED 09/12/2020

- тип опции `hp-event`

### UPDATED 12/11/2020

- `richComment` для типов опций `checkbox` и `radio`

### UPDATED 31/10/2020

- Скрытие опции в корзине для клиента
- Чёрный/белый списки для `parent`

---

# Product Options Plugin

Добавляет фронтенд часть опций в товары и корзину.

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
- комбинируется c `required`
- определяет минимальное требуемое количество заполненных значений (values) опции

`maxLimit` - тип: числовой

- работает для типов: checkbox
- комбинируется c `required`
- определяет максимально возможное количество заполненных значений (values) опции

`quick` - тип: массив

- только для типов `select-custom`, `select-key`, `select-dungeon`
- содержит ID "быстрых опций", которые отрисовываются рядом с элементом списка

`translations` - тип: объект

- объект перевода, ключами объекта являются языковые локали магазина `de`, `fr`...
- каждый ключ является в свою очередь объектом, который замещает собой любые вышеописанные ключи кроме `id`, `title` и `values`

`cssClass` - тип: строка

- добавляет CSS-класс к блоку опции, может быть использован для дополнительной стилизации, например формирование дополнительны отступов - `'my-4'` и т.д.

`priceRule` - тип: объект

- содержит правила расчёта цены для всех значений данной опции. По сути `value` наследуют это значение и при необходимости может быть переназначено на уровне `value`.

`parents` - тип: массив

- включает ID и/или массивы ID родительских элементов опций или их значений
- Если элементом является ID, то значение доступно для выбора, если данный ID отмечен выбранным.
- Если элементом является массив, то значение доступно для выбора, если все ID массива отмечены выбранными.
- Значение может иметь альтернативный (расширенный) синтаксис в виде объекта, который используется, в том числе, для указания недоступности опции при выбранном паренте (новый режим)
- расширенный синтаксис включает ключи `values` - строка/массив ID опций (или значений), `disabled` - `false*|true` - ключ включающий блокировку опции, `disabledMessage` - текст сообщения при блокировке
- Важно: режим "недоступности опции" не наследуется, если от опции зависят другие опции, они будут доступны для выбора, и для них потребуется отдельно указывать условия

**Пример**

_В этом примере при паренте `someParentId` опция видима, но недоступна для выбора, и пользователь видит соответствующий комментарий, при `someAnotherParentId` опция доступна_

```
"parents": [{
    "values": "someParentId",
    "disabled": true,
    "disabledMessage": "This option is disabled"
  },
  "someAnotherParentId",
]
```

`parentsMode` - тип: строка

- возможные значения: `whitelist`, `blacklist`
- по умолчанию: `whitelist`
- `blacklist` - в режиме blacklist делает опцию невидимой для всех значений в ключе `parents`

`schedule` - тип: массив объектов

- при добавлении ключа в опцию управляет доступностью опции по расписанию (часовая зона `Europe/Berlin`)

```json
"schedule": [
  {
    "days": [1, 2, 3, 4, 5, 6, "2024-04-24"],
    "periods": [
      ["01:00", "02:42"],
      ["03:50", "03:51"]
    ]
  },
  {
    "days": [7],
    "periods": [
      ["01:00", "23:45"]
    ]
  }
]
```
- `days` - массив дат или дней недели для которых составлено расписание.
- Дата задаётся в строковом формате "YYYY-MM-DD"
- День недели задаётся в числовом формате N (в пределах от 1 до 7, где 1 - это понедельник)
- Повторное объявление дня или даты не допускается. При повторном объявлении одного и того же дня недели или или даты конфиг будет проигнорирован
- Дата имеет больший приоритет над днём недели. Если конфиг содержит одновременно и дату и соответствующий дате день недели, то будет использован конфиг даты.

- `periods` - массив временных отсечек для объявляемого правила
- время старта и окончания в формате "HH:MM"
- допускается запись с секундами вида "HH:MM:SS"
- `"00:00"` как первое значение периода будет интерпретировано как начало дня, а при указании вторым значением периода - его окончание

### Ключи `priceRule`

`method` - тип: строка

- по умолчанию `add`
- значения `add` | `multiply`
- метод калькуляции, умножение или сложение
- при умножении можно использовать значения меньше 1, пример: множитель 0.5 даст -50%, в то время как множитель 1.5 - даст +50% к стоимости
- при сложении можно использовать отрицательные значения

`affectOptions` - тип: булевый или массив

- по умолчанию `true`
- при `false` не применяется к опциям
- при `true` применяется ко всем опциям без исключения
- при массиве - массив ID опций или значений опций в любой комбинации. При указании и ID опции и ID ее значения мультипликатор сработает для этой опции только один раз

`affectBasePrice` - тип: булевый

- по умолчанию `true`
- только для метода `multiply`
- влияет ли мультипликатор на базовую цену товара

`affectQuantity` - тип: булевый

- по умолчанию `true`
- только для метода `add`
- влияет ли на количество товара

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
- может быть автоматически переопределено при переключении родительских опций

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

- включает ID и/или массивы ID родительских элементов опций или значений ~~(именно значений опций) для данного значения из **других опций**~~.
- Если элементом является ID, то значение доступно для выбора, если данный ID отмечен выбранным.
- Если элементом является массив, то значение доступно для выбора, если все ID массива отмечены выбранными.
- Значение может иметь альтернативный (расширенный) синтаксис в виде объекта, который используется, в том числе, для указания недоступности опции при выбранном паренте (новый режим)
- расширенный синтаксис включает ключи `values` - строка/массив ID опций (или значений), `disabled` - `false*|true` - ключ включающий блокировку опции, `disabledMessage` - текст сообщения при блокировке
- Важно: режим "недоступности опции" не наследуется, если от опции зависят другие опции, они будут доступны для выбора, и для них потребуется отдельно указывать условия

`parentsMode` - тип: строка

- возможные значения: `whitelist`, `blacklist`
- по умолчанию: `whitelist`
- `blacklist` - в режиме blacklist делает опцию невидимой для всех значений в ключе `parents`

`series` - тип: строка

- идентификатор группы родственных значений **внутри** группы. Служит для фиксации значения выбора пользователя при переключении родительских элементов.

~~`passToHpOrder` - тип: булевый~~

~~- дефолтное значение `true`~~
~~- отвечает за то, будет ли значение передано в ордер на платформе~~

`passToHpDeal` - тип: булевый

- при значении `false` скрывает отмеченную опцию из описания дила (в ордер и корзину опция продлжает передаваться)
- дефолтное значение `true`

`priceRule` - тип: объект

- содержит правила расчёта цены для текущего значения опции, имеет более высокий приоритет перед правилами назначенными на уровне `option`.

`portals` - тип: массив объектов

- содержит объекты вида:
  - `target` - строка, обязательный, ID портала куда будет передан контент
  - `content` - строка, обязательный, экранированный HTML
  - `show` - "onSelect" | "onShow"\* - режим передачи данных в портал, в случае "onShow" (дефолтное значение) контент будет виден в случае если опция доступна для выбора, "onSelect" - в случае если опция выбрана
  - `order` - number - определяет порядок отображения в случае множественной передачи нескольких порталов в один целевой ID
- может содержать несколько порталов с разными условиями одновременно
- несколько порталов могут быть переданы в один целевой ID

[Пример конфига c порталами](#пример-конфига-с-порталами)

#### `scopes` - тип: массив строк

- определяет порядок перезаписи опции через [конфиги расширения](#extensions)
- содержит перечень экстеншенов, которые будут прверяться на предмет наличия подменных значний
- имеет 3 зарезерсированных значния `product`, `game`, `store` а также любые экстеншены из соотвествующего раздера админки из поля `slug`
- если подменное значение добавлено в поле `optionsConfigExtension` (бывший прайс-лист товара), то оно будет иметь наивысшее наивысший приоритет, даже если оно не объявлено в `scopes`
- `product` - явное использование значения в этом массиве позволяет переопределить последовательность проверки

#### Варианты использования с примерами:

- `"scopes": [ "game", "store" ]` - в этом случае сначала будет проверен экстеншен продукта (`optionsConfigExtension`), затем производителя, если не найдёт подмены там - будет искать в экстеншен сейлЧеннел, если не нашёл - возьмёт из оригинального конфига
- `"scopes": [ "SOME_EXTENSION_NAME", "game", "product" ]` - тут сначала проверит экстеншен с идентифкатором `SOME_EXTENSION_NAME`, не найдёт там - будет искать в производителе, затем в продукте (`optionsConfigExtension`), и только потом в оригинальном конфиге
- `"scopes": [ "store" ]` - будет искать снчала в экстеншене продукта (`optionsConfigExtension`) затем в сейлЧеннел

[Подробно про сроздание экстеншенов с примерами конфигов](#extensions)

### Опции и специальные ключи значений:

## `checkbox`

Содержит одно или несколько значений ключа `values`, множественный выбор значения в группе.

`image` - тип: строка

- url картинки, которая замещает собой элемент

`enabled` - тип: булевый

- фиксирует опцию как отмеченную, не прибавляет цену

`itemLink` - тип: строка

- принимает HTML, не совместим с режимом `image`
- _пример_ `<a href=\"https://de.wowhead.com/achievement=14194/hallen-der-hingabe\" target=\"_blank\" title=\"Halls of Devotion\" rel=\"noopener noreferrer\">Hallen der Hingabe</a>`
- не забывать экранировать кавычки `"` => `\"`
- ссылки внутри этого элемента НЕ кликабельны, служат для генерации тултипов по наведению на них.
- при нажатии на элемент происходит отметка опции

`richComment` - тип: строка

- экранированный html,
- может содержать ссылку
- рендерится строчно, после обычного комментария
- не совместим с режимом `image`

`layout` - тип: строка

- возможные значения: `list`
- "костыль" для работы с режимом `image`, возвращает списочный вид при использовании картинок

## `checkbox-rich`

Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.
По сути аналогичен элементу `checkbox`, но имеет другой внешний вид и ряд дополнительных ключей:

`image` - тип: строка

- url картинки, которая замещает собой элемент

`richComment` - тип: строка

- экранированный html,
- может содержать ссылку

`imagePosition` - тип: строка

- позиция картинки в плашке опции
- возможные значения: `left`, `right`
- по умолчанию `left`

`priceGhost` - тип: число

- добавочная "зачёркнутая" цена товара
- работает так же как аналогичная цена у продукта, указывается разница цены "до/после" для возможности калькуляции валют/множителей и проч. _Пример: если зачёркнутая цена должна быть 15, а цена со скидкой - 10, то в этом ключе указывается цифра 5, сами цены будут скалькулированы._

`cols` - тип: число, дефолтное значение `3`

- указывает на число колонок для вёрстки элемента в для больших экранов, для мобил принудительно будет установлена одна колонка

## `button`

Содержит одно или несколько значений ключа `values`, множественный выбор значения в группе.

`image` - тип: строка

- url картинки, которая будет использована как иконка

## `switch`

Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

`image` - тип: строка

- url картинки, которая будет использована как иконка

## `radio`

Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.

`image` - тип: строка

- url картинки, которая замещает собой элемент

`richComment` - тип: строка

- экранированный html,
- может содержать ссылку
- рендерится строчно, после обычного комментария
- не совместим с режимом `image`

`layout` - тип: строка

- возможные значения: `list`
- "костыль" для работы с режимом `image`, возвращает списочный вид при использовании картинок

## `radio-rich`

Содержит одно или несколько значений ключа `values`, выбор одного значения в группе.
По сути аналогичен элементу `radio`, но имеет другой внешний вид и ряд дополнительных ключей:

`image` - тип: строка

- url картинки, которая замещает собой элемент

`richComment` - тип: строка

- экранированный html,
- может содержать ссылку

`imagePosition` - тип: строка

- позиция картинки в плашке опции
- возможные значения: `left`, `right`
- по умолчанию `left`

`priceGhost` - тип: число

- добавочная "зачёркнутая" цена товара
- работает так же как аналогичная цена у продукта, указывается разница цены "до/после" для возможности калькуляции валют/множителей и проч. _Пример: если зачёркнутая цена должна быть 15, а цена со скидкой - 10, то в этом ключе указывается цифра 5, сами цены будут скалькулированы._

`cols` - тип: число, дефолтное значение `3`

- указывает на число колонок для вёрстки элемента в для больших экранов, для мобил принудительно будет установлена одна колонка

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

- url картинки, которая замещает собой ползунок на соответствующем значении

`input` - тип: булевый

- по умолчанию `false`
- добавляет текстовый к инпут слайдеру
- рекомендуется использовать только с опциями имеющими числовое выражение

`disabled` - позволяет сделать слайдер неактивным

- слайдер в неактивном состоянии участвует в ценообразовании, для неактивного "пустого" в плане цены слайдера следует предвыбрать в нём значения с нулевой ценой

## `hidden`

- может применяться для передачи комментария по продукту в ордер или модификации цены

## `text-input`

- для передачи информации со свободным вводом

## `custom-html`

- для вставки расширенных комментариев и пояснений со свободной вёрсткой, завязанных на конкретных опциях, с поддержкой `parents` и проч.

`richComment ` - тип: строка

- содержит экранированный html

## `portal`

- для вставки расширенных комментариев и пояснений со свободной вёрсткой, завязанных на конкретных опциях, с поддержкой `parents` и проч.
- не содержит контент, но предоставляет другим опциям возможность передавать
- служит удобной альтернативой `custom-html` для рендера комментариев и описаний со сложной вёрсткой, содержимое портала хранится и наследуется вместе с опцией, тогда как портал является только слотом для его отображения

[Пример полного конфига c порталами](#пример-конфига-с-порталами)

```json
{
  "id": "portals",
  "type": "portal",
  "size": "is-full",
  "values": [
    {
      "name": "_hidden",
      "id": "portal1"
    },
    {
      "name": "_hidden",
      "id": "portal2"
    }
  ]
}
```

## `hp-event`

Содержит одно или несколько значений ключа `values`, единичный выбор значения в группе. Может являться парентом, а также иметь парентов. Может иметь собственный ценовой модификатор как обычная опция, а также быть или не быть `required`. Механика опции в целом аналогична `radio`.

`hpId` - тип: строка

- ID товара на платформе

`faction` - тип: строка

- принимает значения `horde` и `alliance`
- фильтрует эвенты по признаку фракции
- _переключатель фракций реализован в примере ниже_

`hideFaction` - тип: булевый

- по умолчанию `false`
- скрывает значение фракции эвента на плашках

`reserveDelay` - тип: число

- по умолчанию `0`
- единицы измерения - минуты
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
      "parents": [["keyValue103", "playMethodValueSelf"]]
    },
    {
      "hpId": "f9b084fb659744f584b4b770651fbe99",
      "id": "slot-reserve0-h",
      "faction": "horde",
      "parents": [["keyValue103", "playMethodValueAcc"]]
    },
    {
      "hpId": "393c28fd77754d3bb59de3d2a9a4606f",
      "id": "slot-reserve1",
      "parents": ["keyValue136"]
    },
    {
      "hpId": "03847083cb24423fa7fbc2940814e973",
      "id": "slot-reserve2",
      "parents": [["keyValue114", "playMethodValueAcc", "loot-option-x3"]]
    },
    {
      "hpId": "e40326add71d4defb5ac9974cc3f8af7",
      "id": "slot-reserv32",
      "parents": ["keyValue125"]
    }
  ]
}
```

## Mixins

Примеси - отдельные конфиги, которые будут подмешаны к основному конфигу товара.

- Создаются в отдельном разделе админки.
- Синтаксис полностью совпадает с оригинальным конфигов.
- Не может содержать другие миксины.
- В конфиге товара задаётся на уровне опции:

```json
[
  ...,
  {
    "mixin": "MIXIN_SLUG_NAME",
  }
  ...
]
```

### Ключи

- `mixin` - тип: строка - идентификатор примеси объявленный при создании конфига-мексина в соотвествующем поле `slug`
- `isolation` - тип: булевое, дефолтное `true`
  - опциональный ключ, обеспечивает изолированный режим, когда ID опций и значений миксина будут изолированы от конфига в который он примешивается, для гарантии того что конфиг-примесь не будет содержат дубли ID конфига-товара.

## Extensions

Расширения - отдельные конфиги, которые перезаписывают свойства,

- Создаются в отдельном разделе админки.
- Могут переопределять примеси.
- В конфиге товара задаётся через [`scopes`](#scopes---тип-массив-строк)

Имеют сходный синтаксис, но есть ряд различий:

- Опции записываются без значний (без `values`)
- Значения записываются на том же уровне что и опция
- Не могут преопрелять `scopes`

Пример структуры экстенда:

```
[
  { ...опция_1 },
  { ...значение_опции_1 },
  { ...значение_опции_1 },
  { ...опция_2 },
  { ...значение_опции_2 },
  { ...значение_опции_2 },
  ...
]
```

### Пример конфига товара

```json
[
  {
    "id": "Difficulty",
    "title": "Difficulty",
    "scopes": ["some_not_active_extension_slug", "ext-d4"],
    "size": "is-half",
    "type": "switch",
    "priceRule": {
      "affectQuantity": false,
      "method": "multiply",
      "affectOptions": true
    },
    "values": [
      {
        "id": "Softcore",
        "price": 2,
        "name": "Softcore",
        "default": true,
        "scopes": ["ext-d4", "game"]
      },
      {
        "id": "Hardcore",
        "price": 3,
        "name": "Hardcore",
        "default": false,
        "scopes": ["ext-d4", "product"]
      }
    ]
  }
]
```

### Пример конфига экстенда

slug записи: `ext-d4`

```json
[
  {
    "title": "Difficulty from ext-d4",
    "size": "is-full",
    "id": "Difficulty",
    "priceRule": {
      "affectQuantity": false,
      "method": "add",
      "affectOptions": true
    }
  },
  {
    "price": 100,
    "name": "Softcore from ext-d4",
    "default": false,
    "id": "Softcore"
  },
  {
    "price": 300,
    "name": "Hardcore from ext-d4",
    "default": true,
    "id": "Hardcore"
  }
]
```

## Parents и Series

_В этом примере опция будет доступна при: (`parentId1`) ИЛИ (`parentId3` И `parentId4`) ИЛИ (`parentId5`)_

```json
"parents": [
  "parentId1",
  ["parentId3", "parentId4"],
  "parentId5"
]
```

_В этом примере при переключении родительского значения с `parentId1` на `parentId2` текущее значение опции будет унаследовано за счёт объединения значений в группу `siblings`, если опция `id1` была выбрана, то опция `id2` унаследует это значение._

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

Практическое применение: если опция `B` может принимать разную цену в зависимости от значения другой опции `A`. В этом случае опция `B` наполняется однотипными значениями `values`, но с разными ценами и значениями `parents`. А для того, чтобы выбор пользователя не сбивался, такие родственные значения объединяются в одну группу `series`.

В примере ниже значение чекбокса `Within Timer` будет сохраняться при переключении селектора ключа, за счёт того, что им объявлена одна группа `"series": "timerOptionYes"`, и каждый раз будет доступна только одна из этих опций, так как они привязаны к разным `parent`.

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
        "parents": ["keyValue10"]
      },
      {
        "name": "Within Timer",
        "price": 12,
        "id": "timerOptionYes2",
        "series": "timerOptionYes",
        "parents": ["keyValue11"]
      },
      {
        "name": "Within Timer",
        "price": 15,
        "id": "timerOptionYes3",
        "series": "timerOptionYes",
        "parents": ["keyValue12"]
      },
      {
        "name": "Some option",
        "price": 5,
        "default": false,
        "id": "someId1",
        "parents": ["keyValue11", "keyValue12"]
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
        "parents": [["keyValue11", "someId1"]]
      }
    ]
  }
]
```

## Комментирование

Формат JSON не поддерживает комментарии, тем не менее можно использовать следующий синтаксис для комментирования значений внутри объекта настроек не теряя при этом в функциональности:

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
      "parents": [["keyValue11", "someId1"]]
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
        "price": 10.0,
        "default": false,
        "id": "items1"
      },
      {
        "name": "Verschlüsselte Schrift aus Ny'alotha",
        "itemLink": "<a href=\"https://ptr.wowhead.com/item=169223/ashjrakamas-shroud-of-resolve\" title=\"Ashjra'kamas, Shroud of Resolve\" target=\"_blank\" rel=\"noopener noreferrer\">Ashjra'kamas, Shroud of Resolve</a>",
        "comment": "comment",
        "price": 20.0,
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
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "default": false,
        "id": "playMethodValueSelf0asd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "disabled": true,
        "default": false,
        "id": "playMethodValueSelf0asd00",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
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
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 3,
        "default": false,
        "id": "timerOptionYes2zxc",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 4,
        "default": false,
        "id": "timerOptionYes3zxc",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes4zxc",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes5zxc",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes6zxc",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
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
    "quick": ["keyValue103", "keyValue125"],
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
    "quick": ["keyValue10", "keyValue15"],
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
        "parents": ["keyValue10", "keyValue11", "keyValue12"]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 0,
        "default": false,
        "id": "timerOptionYes10000",
        "series": "timerOptionYes",
        "disabled": true,
        "parents": ["keyValue10", "keyValue11", "keyValue12"]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 3,
        "default": false,
        "id": "timerOptionYes2",
        "series": "timerOptionYes",
        "parents": ["keyValue13"]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 4,
        "default": false,
        "id": "timerOptionYes3",
        "series": "timerOptionYes",
        "parents": ["keyValue14"]
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes4",
        "series": "timerOptionYes",
        "parents": ["keyValue15"]
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
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 3,
        "default": false,
        "id": "loottrade10x1",
        "parents": ["keyValue10"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 6,
        "default": false,
        "id": "loottrade10x2",
        "parents": ["keyValue10"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 4,
        "default": false,
        "id": "loottrade11x1",
        "parents": ["keyValue11", "keyValue12", "keyValue13"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 8,
        "default": false,
        "id": "loottrade11x2",
        "parents": ["keyValue11", "keyValue12", "keyValue13"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 5,
        "default": false,
        "id": "loottrade14x1",
        "parents": ["keyValue14", "keyValue15"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 10,
        "default": false,
        "id": "loottrade14x2",
        "parents": ["keyValue14", "keyValue15"]
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
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 3,
        "default": false,
        "id": "loottrade10x13",
        "parents": ["keyValue10"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 6,
        "default": false,
        "id": "loottrade10x24",
        "series": "test",
        "parents": ["keyValue10"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 4,
        "default": false,
        "id": "loottrade11x15",
        "parents": ["keyValue11", "keyValue12", "keyValue13"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 8,
        "default": false,
        "id": "loottrade11x26",
        "series": "test",
        "parents": ["keyValue11", "keyValue12", "keyValue13"]
      },
      {
        "name": "+1 Loot-Trader",
        "comment": "There will be 1 player with the same armor type in your group who will trade you all items he MAY get as reward from the end of dungeon chest and thus this item IS NOT GUARANTEED. ",
        "price": 5,
        "default": false,
        "id": "loottrade14x17",
        "parents": ["keyValue14", "keyValue15"]
      },
      {
        "name": "+2 Loot-Traders",
        "comment": "There will be 2 players with the same armor type in your group who will trade you all items they may get as reward from the end of dungeon chest and thus the items ARE NOT GUARANTEED.",
        "price": 10,
        "default": false,
        "series": "test",
        "id": "loottrade14x28",
        "parents": ["keyValue14", "keyValue15"]
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
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op2",
        "comment": "Comment op2",
        "price": 12,
        "mark": true,
        "default": false,
        "id": "rangeSlider02",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op3",
        "comment": "Comment op3",
        "price": 13,
        "mark": false,
        "default": false,
        "id": "rangeSlider03",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op4",
        "comment": "Comment op4",
        "price": 14,
        "mark": true,
        "default": false,
        "id": "rangeSlider04",
        "series": "rangeSlider04",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op4a",
        "comment": "Comment op4a",
        "price": 14,
        "mark": true,
        "default": false,
        "series": "rangeSlider04",
        "id": "rangeSlider04-1",
        "parents": ["keyValue14"]
      },
      {
        "name": "op5a",
        "comment": "Comment op5a",
        "price": 15,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider05-1",
        "parents": ["keyValue14"]
      },
      {
        "name": "op5b",
        "comment": "Comment op5b",
        "price": 0,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider05-2",
        "parents": ["keyValue15"]
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
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op2",
        "price": 1,
        "mark": " ",
        "default": false,
        "id": "rangeSlider12",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op3",
        "price": 1,
        "mark": false,
        "default": false,
        "id": "rangeSlider13",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op4",
        "price": 1,
        "mark": true,
        "default": true,
        "id": "rangeSlider14",
        "series": "rangeSlider04",
        "parents": ["keyValue10", "keyValue11", "keyValue12", "keyValue13", "keyValue14", "keyValue15"]
      },
      {
        "name": "op4a",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider04",
        "id": "rangeSlider14-1",
        "parents": ["keyValue14"]
      },
      {
        "name": "op5a",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider15-1",
        "parents": ["keyValue14"]
      },
      {
        "name": "op5b",
        "price": 1,
        "mark": true,
        "default": false,
        "series": "rangeSlider05",
        "id": "rangeSlider15-2",
        "parents": ["keyValue15"]
      }
    ]
  },
  {
    "title": "Radio Rich Option",
    "id": "playMethodasd123",
    "type": "radio-rich",
    "size": "is-full",
    "cssClass": "my-4",
    "required": true,
    "values": [
      {
        "name": "Account Sharing",
        "comment": "Our player logs in to your character and plays during the boost",
        "price": 5,
        "priceGhost": 15,
        "default": false,
        "id": "playMethodValueAccasd123",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg",
        "imagePosition": "right",
        "richComment": "We will run Mythic+ dungeon guaranted within timer <a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "priceGhost": 15,
        "default": false,
        "id": "playMethodValueSelf0asd123",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg",
        "richComment": "We will run Mythic+ dungeon guaranted within timer <a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>"
      },
      {
        "name": "Selfplay",
        "comment": "Join our players team and play yourself during the boost",
        "price": 0,
        "priceGhost": 15,
        "disabled": true,
        "default": false,
        "id": "playMethodValueSelf0asd00123",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg",
        "imagePosition": "right",
        "richComment": "We will run Mythic+ dungeon guaranted within timer <a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>"
      }
    ]
  },
  {
    "title": "Checkbox Rich Option",
    "comment": "Choose your run whether within timer or not",
    "id": "timerOptionzxcasd",
    "size": "is-full",
    "type": "checkbox-rich",
    "invalidMessage": "Custom message",
    "required": true,
    "minLimit": 2,
    "maxLimit": 3,
    "cssClass": "my-4 border border-success",
    "values": [
      {
        "name": "Within Timer",
        "price": 0,
        "default": false,
        "id": "timerOptionYes1zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg",
        "imagePosition": "right",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "richComment": "We will run Mythic+ dungeon guaranted within timer <a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 3,
        "priceGhost": 15,
        "default": false,
        "id": "timerOptionYes2zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "price": 4,
        "priceGhost": 15,
        "default": false,
        "id": "timerOptionYes3zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "richComment": "We will run Mythic+ dungeon guaranted within timer <a href=\"https://google.com\" target=\"_blank\" rel=\"noopener noreferrer\">Link</a>"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "default": false,
        "id": "timerOptionYes4zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "priceGhost": 15,
        "default": false,
        "id": "timerOptionYes5zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
      },
      {
        "name": "Within Timer",
        "comment": "We will run Mythic+ dungeon guaranted within timer",
        "price": 5,
        "priceGhost": 15,
        "default": false,
        "id": "timerOptionYes6zxcasd",
        "image": "https://wow.zamimg.com/images/wow/icons/large/ability_revendreth_warrior.jpg"
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
      "affectOptions": ["opt1", "opt1-1"]
    },
    "values": [
      {
        "name": "0",
        "price": 0,
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
  },
  {
    "title": "opt6",
    "comment": "multi, affectBasePrice-, affectQuantity-, allOpt+",
    "id": "opt7",
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
        "id": "opt7-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt7-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt7-2"
      }
    ]
  },
  {
    "title": "opt8",
    "id": "opt8",
    "comment": "multi, affectBasePrice-, affectQuantity-, opt2+",
    "type": "checkbox",
    "priceRule": {
      "method": "add",
      "affectOptions": ["opt2"]
    },
    "values": [
      {
        "name": "0.5",
        "price": 0.5,
        "id": "opt8-0"
      },
      {
        "name": "3",
        "price": 3,
        "id": "opt8-1"
      },
      {
        "name": "5",
        "price": 5,
        "id": "opt8-2"
      }
    ]
  }
]
```

---

## Пример конфига с порталами

```json
[
  {
    "id": "portals0",
    "type": "portal",
    "size": "is-full",
    "values": [
      {
        "name": "",
        "id": "portal0"
      }
    ]
  },
  {
    "title": "Select End game Build",
    "id": "build",
    "size": "is-full",
    "type": "radio-rich",
    "required": true,
    "values": [
      {
        "price": 0,
        "name": "Whirlwind",
        "image": "https://mmonster.co/media/89/0c/e9/1686847139/wirlwind.webp",
        "default": true,
        "id": "Whirlwind",
        "portals": [
          {
            "target": "portal1",
            "content": "<div>Whirlwind - portal1 - order1 - onSelect</div>",
            "show": "onSelect",
            "order": 1
          },
          {
            "target": "portal1",
            "content": "<div>Whirlwind - portal1 - order2 - always</div>",
            "order": 2
          },
          {
            "target": "portal2",
            "content": "<div>Whirlwind - portal2 - order1 - always</div>",
            "order": 1
          }
        ]
      },
      {
        "price": 0,
        "name": "Rend",
        "image": "https://mmonster.co/media/9c/13/3a/1686847139/rend.webp",
        "id": "Rend",
        "richComment": "Lorem ipsum dolor sit amet consectetur adipisicing elit. Impedit dolores commodi cupiditate veniam. Amet non exercitationem odio? Animi cupiditate reprehenderit, aliquam ab quia quo illo ducimus consectetur tenetur veniam error?",
        "portals": [
          {
            "target": "portal0",
            "content": "<div>Rend - portal0 - onSelect - order1</div>",
            "show": "onSelect",
            "order": 1
          },
          {
            "target": "portal1",
            "content": "<div>Rend - portal1 - onSelect - order1</div>",
            "show": "onSelect",
            "order": 1
          },
          {
            "target": "portal2",
            "content": "<div>Rend - portal2 - onSelect - order1</div>",
            "show": "onSelect",
            "order": 1
          }
        ]
      },
      {
        "price": 0,
        "name": "HotA",
        "image": "https://mmonster.co/media/c4/b3/6c/1686847139/hota.webp",
        "id": "HotA",
        "portals": [
          {
            "target": "portal1",
            "content": "<div>HotA - default - order2</div>",
            "order": 2
          }
        ]
      }
    ]
  },
  {
    "id": "portals1",
    "type": "portal",
    "size": "is-full",
    "values": [
      {
        "name": "",
        "id": "portal1"
      },
      {
        "name": "",
        "id": "portal2"
      }
    ]
  }
]
```
