# Расширения Markdown

## Якоря заголовков

Заголовки автоматически получают ссылки-якоря. Отображение якорей может быть настроено с помощью опции [`markdown.anchor`](../config/README.md#markdown-anchor).

## Ссылки

### Внутренние ссылки

Внутренние ссылки, оканчивающиеся на `.md` или `.html`, будут преобразованы в компонент `<router-link>` для SPA-навигации.

Каждый под-каталог в вашем статичном сайте должен содержать `README.md`. Он будет автоматически сконвертирован в `index.html`.

::: tip СОВЕТ
При написании относительного пути к файлу `index.html` каталога, не забывайте закрывать их символом `/`, в противном случае вы получите ошибку 404. Например, используйте `/config/` вместо `/config`.
:::

Если вы хотите добавить ссылку на другой markdown-файл в каталоге, то не забывайте:

1.  Добавлять его с помощью `.html` или `.md`
2.  Убедиться, что написали в правильном регистре, так как путь чувствителен к нему

#### Пример

Для следующей структуры каталогов:

```
.
├─ README.md
├─ foo
│  ├─ README.md
│  ├─ one.md
│  └─ two.md
└─ bar
   ├─ README.md
   ├─ three.md
   └─ four.md
```

``` md
[Главная](/) <!-- Отправит пользователя в корневой README.md -->
[foo](/foo/) <!-- Отправит пользователя index.html в каталоге foo -->
[foo с якорем на heading](/foo/#heading) <!-- Отправит к якорю заголовка README.md каталога foo -->
[foo - one](/foo/one.html) <!-- Вы можете добавлять .html -->
[foo - two](/foo/two.md) <!-- Или вы можете добавлять .md -->
```

### Внешние ссылки

Внешние ссылки автоматически получают `target="_blank" rel="noopener noreferrer"`:

- [vuejs.org](https://vuejs.org)
- [VuePress on GitHub](https://github.com/vuejs/vuepress)

Вы можете настроить атрибуты, добавляемые к внешним ссылкам, с помощью опции [config.markdown.externalLinks](../config/README.md#markdown-externallinks).

## Front Matter

[YAML front matter](https://jekyllrb.com/docs/frontmatter/) поддерживается из коробки:

``` yaml
---
title: Blogging Like a Hacker
lang: en-US
---
```

Данные будут доступны на странице, а также во всех пользовательских и компонентах темы через свойство `$page`.

`title` и `lang` будут автоматически установлены на текущей странице. Вы можете указать дополнительные meta-теги которые требуется добавить на этой странице:

``` yaml
---
meta:
  - name: description
    content: привет
  - name: keywords
    content: супер пупер SEO
---
```

### Альтернативные форматы Front Matter

Дополнительно VuePress поддерживает JSON или [TOML](https://github.com/toml-lang/toml) front matter.

JSON front matter необходимо начинать и заканчивать фигурными скобками:

```
---
{
  "title": "Blogging Like a Hacker",
  "lang": "en-US"
}
---
```

TOML front matter необходимо явно определять как TOML:

```
---toml
title = "Blogging Like a Hacker"
lang = "en-US"
---
```

## Таблицы в стиле GitHub

**Входные данные**

```
| Таблицы          | Это                       | Круто |
| ---------------- |:-------------------------:| -----:|
| третья колонка   | выровнена по правому краю | $1600 |
| вторая колонка   | выровнена по центру       |   $12 |
| полосатые строки | приятно смотрятся         |    $1 |
```

**Результат**

| Таблицы          | Это                       | Круто |
| ---------------- |:-------------------------:| -----:|
| третья колонка   | выровнена по правому краю | $1600 |
| вторая колонка   | выровнена по центру       |   $12 |
| полосатые строки | приятно смотрятся         |    $1 |

## Эмодзи :tada:

**Входные данные**

```
:tada: :100:
```

**Результат**

:tada: :100:

Список всех доступных эмодзи можно найти [здесь](https://github.com/markdown-it/markdown-it-emoji/blob/master/lib/data/full.json).

## Содержание

**Входные данные**

```
[[toc]]
```

**Результат**

[[toc]]

Рендеринг содержания страницы можно конфигурировать с помощью опции [`markdown.toc`](../config/README.md#markdown-toc).

## Пользовательские блоки

**Входные данные**

```
::: tip
Это совет
:::

::: warning
Это предупреждение
:::

::: danger
Это опасное предупреждение
:::
```

**Результат**

::: tip
Это совет
:::

::: warning
Это предупреждение
:::

::: danger
Это опасное предупреждение
:::

Вы также можете указать заголовок блока:

```
::: danger СТОП
Опасная зона, возвращайтесь назад
:::
```

::: danger СТОП
Опасная зона, возвращайтесь назад
:::

## Подсветка строк в блоке кода

**Входные данные**

````
``` js{4}
export default {
  data () {
    return {
      msg: 'Highlighted!'
    }
  }
}
```
````

**Результат**

``` js{4}
export default {
  data () {
    return {
      msg: 'Highlighted!'
    }
  }
}
```

## Нумерация строк

Вы можете включить нумерацию строк в каждом блоке кода с помощью опции:

``` js
module.exports = {
  markdown: {
    lineNumbers: true
  }
}
```

<!-- TODO Support line numbers for specific fence block -->

- Демо:

<picture>
  <source srcset="/line-numbers-desktop.png" media="(min-width: 719px)">
  <img class="line-numbers-desktop-snap" alt="Image">
</picture>

<picture>
  <source srcset="/line-numbers-mobile.gif" media="(max-width: 719px)">
  <img class="line-numbers-mobile-snap" alt="Image">
</picture>

<style>
  @media screen and (min-width:  719px) {
    .line-numbers-mobile-snap {
       display: none;
    }
  }
  @media screen and (max-width:  719px) {
    .line-numbers-desktop-snap {
       display: none;
    }
    .line-numbers-mobile-snap {
      max-width: none!important;
      margin: 0 -1.5rem;
      width: 100vw;
    }
  }
</style>

## Импорт фрагментов кода <Badge text="Бета" type="warn"/> <Badge text="0.10.1+" type="tip"/>

Вы можете импортировать фрагменты кода из существующих файлов следующим образом:

``` md
<<< @/filepath
```

Также поддерживается [подсветка строк](#подсветка-строк-в-бnоке-кода):

``` md
<<< @/filepath{highlightLines}
```

**Входные данные**

```
<<< @/test/markdown/fragments/snippet.js{2}
```

**Результат**

<<< @/test/markdown/fragments/snippet.js{2}

::: tip СОВЕТ
  Поскольку импорт фрагментов кода будет выполнен до компиляции webpack, то вы не сможете использовать псевдонимы webpack при указании пути. По умолчанию для `@` будет значение `process.cwd()`.
:::

## Продвинутая конфигурация

VuePress использует [markdown-it](https://github.com/markdown-it/markdown-it) для рендеринга markdown. Многие из расширений выше реализованы через пользовательские плагины. Вы также можете настроить экземпляр `markdown-it` с помощью опции `markdown` в `.vuepress/config.js`:

``` js
module.exports = {
  markdown: {
    // настройки для markdown-it-anchor
    anchor: { permalink: false },
    // настройки для markdown-it-toc
    toc: { includeLevel: [1, 2] },
    config: md => {
      // используем больше markdown-it плагинов!
      md.use(require('markdown-it-xxx'))
    }
  }
}
```
