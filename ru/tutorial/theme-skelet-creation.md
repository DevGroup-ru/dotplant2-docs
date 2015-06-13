# Создание скелета темы

Поскольку каждый сайт индивидуален - ему необходима своя тема оформления.

В DotPlant2 в понятие темы мы также вкладываем весь сайто-специфичный функционал. Поэтому помимо оформления, в папке web/theme мы также будем хранить необходимые контроллеры и модели.

Для создания скелета темы вполните следующую команду из папки application:

``` bash
$ ./yii admin/create-theme
```

Эта команда создаст папку `web/theme` с необходимой структурой:

```
css
fonts
images
index.html
js
module
resources
views
```

Поскольку это сайтоспецифичные файлы - они не попадают в основной репозиторий dotPlant и эта папка находится в .gitignore.

Для удобства вы можете создать git-репозиторий для темы:

``` bash
$ cd web/theme
$ git init
Initialized empty Git repository in /Users/bethrezen/git-my/dotplant2/application/web/theme/.git/

$ git add *
$ git remote add origin git@YOUR_GIT_HOST:YOUR_THEME_REPO.git

$ git commit -m "first commit :grinning:"
[master (root-commit) e341004] first commit :grinning:
 27 files changed, 70 insertions(+)
 create mode 100644 css/index.html
 create mode 100644 fonts/index.html
 create mode 100644 images/index.html
 create mode 100644 index.html
 create mode 100644 js/index.html
 create mode 100644 module/.htaccess
 create mode 100644 module/assets/ThemeAsset.php
 create mode 100644 module/components/.keep
 create mode 100644 module/config/web.php
 create mode 100644 module/controllers/.keep
 create mode 100644 module/migrations/.keep
 create mode 100644 module/models/.keep
 create mode 100644 module/views/.keep
 create mode 100644 module/widgets/.keep
 create mode 100644 resources/index.html
 create mode 100644 resources/product-images/.gitignore
 create mode 100644 resources/product-images/index.html
 create mode 100644 views/.htaccess
 create mode 100644 views/modules/basic/default/.keep
 create mode 100644 views/modules/basic/layouts/main.php
 create mode 100644 views/modules/page/page/.keep
 create mode 100644 views/modules/shop/cart/.keep
 create mode 100644 views/modules/shop/product-compare/.keep
 create mode 100644 views/modules/shop/product/.keep
 create mode 100644 views/templates/.keep
 create mode 100644 views/widgets/form/.keep
 create mode 100644 views/widgets/navigation/.keep

$ git push -u origin master
Counting objects: 22, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (13/13), done.
Writing objects: 100% (22/22), 2.11 KiB | 0 bytes/s, done.
Total 22 (delta 0), reused 0 (delta 0)
To git@YOUR_GIT_HOST:YOUR_THEME_REPO.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

Таким образом, весь ваш сайто-специфичный функционал будет в системе контроля версия и работать над ним в команде будет удобнее.