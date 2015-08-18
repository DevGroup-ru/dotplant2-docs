# Создание AssetBundle

Поскольку DotPlant2 построена на базе Yii Framework 2 - мы стараемся придерживаться всех стандартов и паттернов разработки, которые задает этот фреймворк.

Именно поэтому мы не будем размещать ссылки на наши стили и скрипты непосредственно в HTML-коде шаблонов.
Для этих вещей придуманы AssetBundle(или комплекты ресурсов).
Подробнее о них вы можете прочесть в официальном Guide - https://github.com/yiisoft/yii2/blob/master/docs/guide-ru/structure-assets.md

Если вы создавали тему с помощью `./yii admin/create-theme`, то у вас уже есть готовый файл для комплекта ресурсов - `web/theme/module/assets/ThemeAsset.php`.

> Поскольку тема - это модуль, там также применяются пространства имён.
> 
> Например для файла ThemeAsset пространство имён будет `app\web\theme\module\assets`

В данном руководстве мы будем использовать [SASS](http://sass-lang.com/) для процессинга CSS файлов. 

Исходные коды scss файлов мы поместим в папку `web/theme/scss`(её нужно будет создать).

Для удобства запустим в терминале из папки `web/theme` следующую команду:

``` bash
$ sass --watch scss:css
>>> Sass is watching for changes. Press Ctrl-C to stop.
```

Эта команда запускает специальный процесс sass-процессора, который смотрит за всеми файлами внутри `web/theme/scss` и компилиррует их при сохранении изменений в соответствующие css-файлы в папку `web/theme/css`.

Создадим файл `web/theme/scss/main.scss` с любым валидным scss-кодом, например:

``` css
body {
  background: red;
}
```

В запущеном ранее терминале мы увидим, что файл успешно обработался:

``` bash
>>> New template detected: scss/main.scss
      write css/main.css
      write css/main.css.map
```

Теперь самое время добавить готовый css файл в наш комплект ресурсов. В файле `ThemeAsset.php` добавляем ссылку на css:

``` php
public $css = [
        'css/main.css',
    ];
```

По-умолчанию, в теме создается свой layout-файл: `web/theme/views/modules/basic/layouts/main.php`. В этом файле уже подключается наш ThemeAsset, поэтому перейдя на http://localhost/ мы можем успешно примененный стиль из main.css(в примере выше - красный фон).

> SASS генерирует кеш-файлы, которые вы скорее всего не захотите видеть в репозитории. Добавьте в `web/theme/.gitignore` строчку `.sass-cache` и эти файлы не будут вам мешать.
