# Введение

<Bit/>

VuePress состоит из двух частей: минималистичный генератор статических сайтов с системой пользовательских тем, основанной на Vue, и тема по умолчанию, оптимизированная для написания технической документации. Он был создан для поддержки потребностей в документировании собственных проектов экосистемы Vue.

Каждая страница, сгенерированная VuePress, имеет собственный пре-рендеренный статичный HTML, обеспечивающий великолепную скорость загрузки и дружелюбность к SEO. Однако, как только страница будет загружена, Vue использует статический контент и превращает его в полноценное одностраничное приложение (SPA). Дополнительные страницы загружаются по требованию, по мере навигации пользователя по сайту.

## Как он работает

Сайт на VuePress в действительности является SPA, работающим на [Vue](http://vuejs.org/), [Vue Router](https://github.com/vuejs/vue-router) и [webpack](http://webpack.js.org/). Если вы уже использовали Vue, вы заметите аналогичный опыт разработки при написании страниц или разработке пользовательских тем (вы даже можете использовать Vue DevTools для отладки своей пользовательской темы!).

Во время сборки мы создаём серверную версию приложения и рендерим соответствующий HTML, виртуально открывая каждый маршрут. Этот подход вдохновлён командой `nuxt generate` фреймворка [Nuxt](https://nuxtjs.org/) и другими проектами, такими как [Gatsby](https://www.gatsbyjs.org/).

Каждый файл markdown компилируется с помощью [markdown-it](https://github.com/markdown-it/markdown-it) и затем обрабатывается как шаблон компонента Vue. Это позволяет вам напрямую использовать Vue внутри ваших markdown файлов и предоставляет богатые возможности, если вам необходимо добавить динамический контент.

## Возможности

- [Встроенные расширения markdown](./markdown.md) оптимизированные для технической документации
- [Возможность использовать Vue внутри файлов markdown](./using-vue.md)
- [Система пользовательских тем, основанная на Vue](./custom-themes.md)
- [Автоматическая генерация Service Worker](../config/README.md#serviceworker)
- [Интеграция Google Analytics](../config/README.md#ga)
- ["Последнее обновление" на основе Git](../default-theme-config/README.md#посnеднее-обновnение)
- [Поддержка интернационализации](./i18n.md)
- Стандартная тема:
  - Адаптивный шаблон
  - [Опциональная главная страница](../default-theme-config/README.md#гnавная-страница)
  - [Простой поиск по заголовкам страниц из коробки](../default-theme-config/README.md#встроенный-поиск)
  - [Поиск Algolia](../default-theme-config/README.md#поиск-algolia)
  - Настраиваемая [панель навигации](../default-theme-config/README.md#панеnь-навигации) и [боковая панель](../default-theme-config/README.md#боковая-панеnь)
  - [Авто-генерируемые ссылки на GitHub и на редактирование страницы](../default-theme-config/README.md#git-репозиторий-и-ссыnки-на-редактирование-страниц)

## В планах

VuePress всё ещё в разработке. Есть несколько вещей, которые в настоящее время не поддерживаются, но их разработка в планах:

- Поддержка плагинов
- Поддержка блогов

Пулл-реквесты приветствуются!

## Почему не ...?

### Nuxt

Nuxt способен делать то, что умеет VuePress, но ориентирован именно на создание приложений. VuePress ориентирован на статичные сайты, где фокус уделяется содержанию, и предоставляет из коробки возможности, специально разработанные для удобства написания технической документации.

### Docsify / Docute

Оба являются отличными проектами, и также работают на Vue. Кроме того, они оба полностью завязаны на runtime и поэтому не дружелюбны к SEO. Если вы не заботитесь о SEO и не хотите разбираться с установкой зависимостей — это всё равно отличный выбор.

### Hexo

Hexo отлично справляется с документацией Vue — на самом деле нам, вероятно, ещё предстоит пройти долгий путь для миграции с него на нашем основном сайте. Самая большая проблема в том, что система тем очень статична и основывается на строках — мы действительно хотим использовать Vue как для шаблона так и для интерактивности. Кроме того, рендеринг Hexo файлов markdown не самый гибкий в конфигурировании.

### GitBook

Мы использовали GitBook для документаций большинства наших смежных проектов. Основная проблема с GitBook заключается в том, что скорость перезагрузки на этапе разработки невыносима длительна при большом количестве файлов. Тема по умолчанию имеет довольно ограниченную структуру навигации, а система тем, опять же, не основана на Vue. Команда GitBook больше ориентирована на превращение проекта в коммерческий продукт, а не в инструмент с открытыми исходными кодами.
