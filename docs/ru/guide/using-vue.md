# Используем Vue в Markdown

## Ограничения доступа к API браузера

Поскольку приложение VuePress при серверном рендеринге использует Node.js, в том числе когда генерирует статическую сборку, то любое использование Vue должно соответствовать [требованиям написания универсального кода](https://ssr.vuejs.org/ru/universal.html). Вкратце, убедитесь что обращаетесь к API браузера / DOM только в хуках `beforeMounted` или `mounted`.

Если вы используете компоненты, несовместимые с SSR (например, содержащие пользовательские директивы), можете обернуть их встроенным компонентом `<ClientOnly>`:

``` md
<ClientOnly>
  <NonSSRFriendlyComponent/>
</ClientOnly>
```

Обратите внимание, это не исправит компоненты или библиотеки, которые обращаются к API браузера **при импорте** — в случае использования кода, который предполагает окружение браузера при импорте, вам необходимо использовать динамические импорты в нужных хуках жизненного цикла:

``` vue
<script>
export default {
  mounted () {
    import('./lib-that-access-window-on-import').then(module => {
      // использование кода
    })
  }
}
</script>
```

## Шаблонизация

### Интерполяции

Каждый файл markdown сначала компилируется в HTML и затем обрабатывается в качестве шаблона компонента Vue с помощью `vue-loader`. Это значит, что вы можете использовать интерполяции Vue в тексте:

**Входные данные**

``` md
{{ 1 + 1 }}
```

**Результат**

<div class="language-text"><pre><code>{{ 1 + 1 }}</code></pre></div>

### Директивы

Директивы работают также:

**Входные данные**

``` md
<span v-for="i in 3">{{ i }} </span>
```

**Результат**

<div class="language-text"><pre><code><span v-for="i in 3">{{ i }} </span></code></pre></div>

### Доступ к данным сайта & страницы

У компилируемого компонента нет приватных данных, но имеет доступ к [мета-данным сайта](./custom-themes.md#мета-данные-сайта-и-страницы). Например:

**Входные данные**

``` md
{{ $page }}
```

**Результат**

``` json
{
  "path": "/using-vue.html",
  "title": "Используем Vue в Markdown",
  "frontmatter": {}
}
```

## Необработанный вывод данных

По умолчанию, вставки фрагментов кода оборачиваются с помощью `v-pre`. Если вы хотите отобразить оригинальное содержимое или Vue-специфичный синтаксис внутри блоков фрагментов кода или просто текст, вам необходимо обернуть параграф с помощью пользовательского контейнера `v-pre`:

**Входные данные**

``` md
::: v-pre
`{{ Это будет отображено как есть }}`
:::
```

**Результат**

::: v-pre
`{{ Это будет отображено как есть }}`
:::

## Использование компонентов

Любые файлы `*.vue` обнаруженные в каталоге `.vuepress/components` будут автоматически зарегистрированы как [глобальные](https://ru.vuejs.org/v2/guide/components-registration.html#Глобальная-регистрация), [асинхронные](https://ru.vuejs.org/v2/guide/components-dynamic-async.html#Асинхронные-компоненты) компоненты. Например:

```
.
└─ .vuepress
   └─ components
      ├─ demo-1.vue
      ├─ OtherComponent.vue
      └─ Foo
         └─ Bar.vue
```

Внутри любого markdown файла вы можете напрямую использовать компоненты (имя компонента определяется по имени файла):

``` md
<demo-1/>
<OtherComponent/>
<Foo-Bar/>
```

<demo-1></demo-1>

<OtherComponent/>

<Foo-Bar/>

::: warning ВАЖНО
Убедитесь, что имя пользовательского компонента содержит дефис, либо именуется в PascalCase. В противном случае он будет рассматриваться как встроенный элемент и оборачиваться внутрь тега `<p>`, что приведёт к ошибке гидратации, потому что `<p>` не позволяет размещать блоковые элементы внутри себя.
:::

### Использование пре-процессоров

VuePress использует встроенную конфигурацию webpack для следующих пре-процессоров: `sass`, `scss`, `less`, `stylus` и `pug`. Всё что вам нужно сделать, это установить соответствующие зависимости. Например, для использования `sass`, установите в проект:
 
``` bash
yarn add -D sass-loader node-sass
```

Теперь вы можете использовать синтаксис в файлах markdown и компонентах темы:

``` vue
<style lang="sass">
.title
  font-size: 20px
</style>
```

Использование `<template lang="pug">` требует установки `pug` и `pug-plain-loader`:

``` bash
yarn add -D pug pug-plain-loader
```

::: tip СОВЕТ
Если вы используете Stylus, вам не требуется устанавливать `stylus` и `stylus-loader` в ваш проект, потому что VuePress уже использует Stylus внутри себя.
  
Для пре-процессоров, которые не поддерживаются по умолчанию, вы можете [изменить встроенную конфигурацию webpack](../config/README.md#configurewebpack) вдобавок к установке требуемых зависимостей.
:::

## Использование Script & Style

Иногда вам может потребоваться применить JavaScript или CSS только к текущей странице. В таких случаях вы можете напрямую указать блоки `<script>` или `<style>` на корневом уровне файла markdown, и они будут извлечены из скомпилированного HTML и использованы в качестве секций `<script>` и `<style>` итогового однофайлового компонента Vue.

<p class="demo" :class="$style.example"></p>

<style module>
.example {
  color: #41b883;
}
</style>

<script>
export default {
  mounted () {
    document.querySelector(`.${this.$style.example}`)
      .textContent = 'Это отображается с помощью инлайнового скрипта и стилизуется инлайновым CSS'
  }
}
</script>