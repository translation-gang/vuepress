---
home: true
heroImage: /hero.png
actionText: Введение →
actionLink: /ru/guide/
footer: MIT Licensed | Copyright © 2018-present Evan You
---

<div style="text-align: center">
  <Bit/>
</div>

<div class="features">
  <div class="feature">
    <h2>Простой</h2>
    <p>Минимальная настройка с markdown-ориентированной структурой проекта позволяет сосредоточиться на написании.</p>
  </div>
  <div class="feature">
    <h2>Сделано на Vue</h2>
    <p>Наслаждайтесь опытом разработки на Vue + webpack, используйте компоненты Vue в markdown, и создавайте собственные темы с Vue.</p>
  </div>
  <div class="feature">
    <h2>Производительный</h2>
    <p>VuePress генерирует пре-рендеренный статический HTML для каждой страницы, и запускается как SPA после загрузки страницы.</p>
  </div>
</div>

### Просто как 1, 2, 3

``` bash
# устанавливаем
yarn global add vuepress # OR npm install -g vuepress

# создаём markdown-файл
echo '# Hello VuePress' > README.md

# начинаем писать
vuepress dev

# собираем в статичные файлы
vuepress build
```

::: warning ПРИМЕЧАНИЕ СОВМЕСТИМОСТИ
VuePress для работы требуется Node.js >= 8.
:::
