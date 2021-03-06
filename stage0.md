# Конструкции

* [Фильтры](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Фильтры)
* [Создание переменных](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Создание-переменных)
* [Операторы сравнения](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Операторы-сравнения)
* [Комментарии](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Комментарии)
* [Цикл For](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Цикл-for)
* [Чередование Cycle](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#Чередование-cycle)
* [Capture](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#capture)
* [Include](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#include)
* [raw](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#raw)
* [Help - аналог var_dump](https://github.com/liquid-hub/insales-liquid-examples/blob/master/stage0.md#help---аналог-var_dump)

В шаблонизаторе есть 2 основные конструкции:

> {{ code }} - вывод информации

> {% code %} - теги

## Фильтры

Фильтры преобразовывают выходные данные

```twig
Привет, {{ name | upcase }}! => Привет, ПОЛЬЗОВАТЕЛЬ!
{{ 'sales' | append: '.jpg' }} => sales.jpg

<link href="{{ 'shop.css' | asset_url }}" rel="stylesheet" type="text/css" />
=> <link href="//assets3.insales.ru/assets/1/7120/23504/v_1479117076/build/shop.css" rel="stylesheet" type="text/css" />

<img src="{{ 'image.png' | file_url }}" alt="Изображение из раздела файлов">
=> <img src="https://static-eu.insales.ru/files/1/7105/3611585/original/image.png" alt="Изображение из раздела файлов">

Получить дату
{{ 'now' | date: "%Y" }}
```

## Создание переменных

```twig
{% assign new_var = 'Строка' %}
{{ new_var }} => Строка
{% assign new_var = 'СТРОКА' | downcase  %}
{{ new_var }} => строка
```

## Операторы сравнения

```twig
{% assign product_price = 10 %}
{% assign product_title = 'Телефон' %}
{% if product_price > 9 %}
  Дороже 9
  {% else %}
  Дешевле или равно 9
{% endif %}

{% unless product_title contains 'Телефон' %}
  Это не телефон
  {% else %}
  Это телефон  
{% endunless %}
```

## Комментарии

```twig
{% comment %} закомментированный текст {% endcomment %}
```

## Чередование Cycle

```twig
{% cycle 'odd', 'even' %}<br />
{% cycle 'odd', 'even' %}<br />
{% cycle 'odd', 'even' %}<br />
{% cycle 'odd', 'even' %}

odd
even
odd
even
```

## Цикл For

```twig
{% for item in array %}
  {{ item }}
{% endfor %}
```
> При обходе массива доступны дополнительные переменные:

```
forloop.length      # => количество элементов в массиве
forloop.index       # => номер текущей итерации
forloop.index0      # => номер текущей итерации (считая от нуля)
forloop.rindex      # => сколько элементов осталось
forloop.rindex0     # => сколько элементов осталось (считая от нуля)
forloop.first       # => первая итерация?
forloop.last        # => последняя итерация?
```

## Capture

Capture объединяет несколько переменных в одну строку

```
{% capture my_var %}<p>Первая строка</p>{% endcapture %}
{% capture my_var %}<p>Вторая строка</p>{% endcapture %}
{{ my_var }}
=> <p>Вторая строка</p>

{% capture my_var %}<p>Первая строка</p>{% endcapture %}
{% capture my_var %}{{ my_var }}<p>Вторая строка</p>{% endcapture %}
{{ my_var }}
=> <p>Первая строка</p>
=> <p>Вторая строка</p>


{% assign array = 'первый второй третий' | split: ' ' %}
{% for item in array %}
    {% capture text_array %}[{{ item }}]{% endcapture %}
{% endfor %}
{{ text_array }} => [третий]

{% for item in array %}
  {% capture text_array2 %}{{ text_array2 }}[{{ item }}]{% endcapture %}
{% endfor %}
{{ text_array2 }} => [первый][второй][третий]
```

## Include
Тег include позволяет вставлять сниппеты шаблона в произвольные места лайаута, шаблонов и других сниппетов.

```twig
{% include 'head' %}
{% include 'header' %}
При включении сниппета можно передать параметры через запятую
{% include 'footer', show_phone: true %}
```

## raw

Временно отключить обработку тегов, чтобы избежать конфликтов синтаксиса.

```
{% raw %}
In Handlebars, {{ this }} will be HTML-escaped, but {{{ that }}} will not.
{% endraw %}
=>
In Handlebars, {{ this }} will be HTML-escaped, but {{{ that }}} will not.
```

## Help - аналог var_dump

Чтобы проверить доступность переменных в шаблоне, используйте тег {% help %}.

```twig
{% help %}
{% help 'collections' %}
{% for product in collection.products %}
  {% help product%}
{% endfor %}
```

[<< Назад](https://github.com/liquid-hub/insales-liquid-examples/blob/master/readme.md)
