# Типы переменных

* [Строки](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage1.md#Строки)
* [Числа](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage1.md#Числа)
* [Объекты](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage1.md#Объекты)
* [Массивы](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage1.md#Массивы)
* [Логический тип данных](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage1.md#Логический-тип-данных)

## Строки

> Строками могут быть текстовые поля бэк-офиса, поля содержащие html разметку, поля типа `select` и так далее.

> [Строки в списке отмечены лейблами - string,html](http://liquidhub.ru/collection/shpargalka-liquid)

```twig
{{ title }} - название магазина

{{ page.title }} - название страницы

{{ product.title }} - название продукта

{{ product.first_image.medium_url }} - ссылка изображение

{% assign string_variable = 'Переменная с текстом' %}

{{ string_variable }} - Локальная переменная

```

> Строки можно приводить к числам.

```twig
{% assign number = 220 %}
{% assign string_number = '239' | times: 1 %}
{% if string_number > number %}
  Строка умноженная на 1 приводится к числу.
  Данный код отработает
{% endif %}

{% assign string_number2 = '239' %}
{% if string_number2 > number %}
  Код выдаст ошибку при сравнении строки с числом.
{% endif %}
```

> Из строки можно создать массив.

```twig
{% assign string_for_array = '1|2|3|4' | split: '|' %}

{% for i in string_for_array %}
  {{ i }}
{% endfor %}
=> 1 2 3 4

{% comment %}page.url = page/contacts{% endcomment %}
{% assign page_handle = page.url | split: '/' | last %}
{{ page_handle }}
=> contacts
```

> К строке можно применить фильтр или метод size, таким образом можно проверить строку на пустоту

```twig
{% assign new_string = '' %}

{% if new_string == '' %}
  Строка пуста
{% endif %}

{% if new_string.size == 0 %}
  Строка пуста
{% endif %}
```

> Корректно передать строку в JS можно с помощью фильтра `json`.

> Фильтр `json` корректно формирует данные (Присваивая строку из бэк-офиса, есть вероятность получить конфликт ковычек)

```html
<script type="text/javascript">
  var myString = {{ product.description | json }};
</script>
```
> Из строки можно ссылку на файл темы (`asset_url`) или из раздела файлы (`file_url`)

```html
<link href="{{ 'shop.css' | asset_url }}" rel="stylesheet" type="text/css" />
=> <link href="//assets3.insales.ru/assets/1/7120/23504/v_1479117076/build/shop.css" rel="stylesheet" type="text/css" />

<img src="{{ 'image.png' | file_url }}" alt="Изображение из раздела файлов">
=> <img src="https://static-eu.insales.ru/files/1/7105/3611585/original/image.png" alt="Изображение из раздела файлов">
```

## Числа

Числами могут быть id, цены, количество и т.д.

Помимо математических операций числа можно подставлять в строки.

```twig
{% assign number = 2 %}
{{ number | plus: 2 }}
=> 4

{{ product.price | money }}
=> 1 000 руб.

{{ 2000 | money }}
=> 2 000 руб.

{{ product.price | money | prepend: 'Цена: ' }}
=> Цена: 1 000 руб.
```

## Объекты

Объекты это структура данных похожая на ассоциативные массивы. Значение объектов можно получить по ключу.

> Практически все выводимые из бэк-офиса данные заключаются в объекты. Например account, product, blog и т.д..

```twig
{{ объект.ключ.значение }}
{{ объект.ключ.ключ.значение }}

{{ account.phone }}
{{ product.title }}
{{ product.properties.handle.characteristics.first.name }}
```

> Объекты могут быть итерируемыми

```twig
{% for property in product.properties %}
  {{property.name}}: {% for item in property.characteristics %}{{item.name}},{% endfor %}
{% endfor %}
```

## Массивы

Массив это набор элементов расположенных в памяти непосредственно друг за другом, что позволяет обращаться к элементам по числовому индексу.

```twig
{% assign array = 'первый второй третий' | split: ' ' %}
{{ array[0] }} => первый
{{ array[1] }} => второй
{{ array[2] }} => третий
{{ array[3] }} => (ничего не выведет)

{% for item in array %}
  [{{ item }}][{{ forloop.index }}]
{% endfor %}
=> [первый][1] [второй][2] [третий][3]

{% for item in array %}
  [{{ item }}][{{ forloop.index0 }}]
{% endfor %}
=> [первый][0] [второй][1] [третий][2]
```
> Элементы массива можно сортировать по значениям элементов. Например по title

```twig
<!-- products title = "a", "b", "A", "B" -->
{% assign products = collection.products | sort: 'title' %}
{% for product in products %}
  {{ product.title }}
{% endfor %}

#=> A B a b
```

> Итерирование можно сдвинуть параметром `offset` и ограничить параметром `limit`

```twig
{% assign array = 'первый второй третий четвертый' | split: ' ' %}
{% for item in array limit: 1 offset: 1 %}
  [{{ item }}][{{ forloop.index0 }}]
{% endfor %}
=> [второй][0] [третий][1]
```

## Логический тип данных

Логический тип данных, или булев тип, или булевый тип (от англ. Boolean или logical data type) — примитивный тип данных в информатике, принимающий два возможных значения, иногда называемых истиной (true) и ложью (false).

```twig
{% assign product_available = false %}
{% if product_available %}
  Товар доступен
  {% else %}
  Товар не доступен
{% endif %}
=> Товар не доступен
```

> К булевой переменной можно применить фильтр replace

```twig
{% assign product_available = true %}
{{ product_available | replace: 'true', 'is-available' | replace: 'false', 'not-available' }}
=> is-available
{% assign product_available = false %}
{{ product_available | replace: 'true', 'is-available' | replace: 'false', 'not-available' }}
=> not-available
```

[<< Назад](https://github.com/liquid-hub/insales-liquid-examples/blob/master/readme.md)
