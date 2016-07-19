# Organisation of work and workspace

This guide presents general guidelines for the development of projects on DotPlant2 team and calculations on the server battle.

## Recommended applications for development

- IDE - [PhpStorm](https://www.jetbrains.com/phpstorm/)
- Shell to work with the Git - [SourceTree](https://www.sourcetreeapp.com) (win/mac) or [SmartGIT](http://www.syntevo.com/smartgit/) (win/mac/linux)
- [OpenServer](http://open-server.ru/) or Windows system or Apache/PHP/MySQL for Linux/MacOSX

## Application Framework - Developer working computer

As a rule, the development site to dotplant2 occurs without modifying core files.

The first thing you need to clone the repository dotplant2:

``` bash
git clone https://github.com/DevGroup-ru/dotplant2.git
```

Now you need to make a basic installation DotPlant2 according to [instructions](../install.md).

## Site Specific Functionality

All site specific functionality, including the design theme of your website, we recommend that you keep it in a separate folder `application/web/theme`.

To create this folder, and base frame there is a special console command:

``` bash
./yii admin/create-theme
```

More information about the theme folder structure can be found in the section [Using custom theme](custom-theme.md).

For convenient operation, you must create a repository for the theme:

``` bash
cd application/web/theme
git init
git remote add origin git@your.git.host:vendor/repo.git # эта команда добавит remote origin к текущему репозиторию темы
```

Now your site specific functionality and design theme is under version control system.

> Always replace your git.host:vendor/repo.git to the appropriate value for your repository!

## Roll-out to the production server

The first roll out you must:
1. Clone core DotPlant2
2. Clone folder in `application/web/theme` your repository with the theme
3. Move all the configuration files from `application/config` in local machine to production server, in this case:
	* Replace access to the database or create a db-local.php with access to a specific server
    * Replace from everywhere (including in configurable-state), `serverName` variable to production server domain
4. Copy the database from the local machine to production for example through a simple SQL-dump.
5. Align the necessary rights (`chmod -R 777 application/web/assets applicaiton/runtime`) 
6. Install the composer-based - `cd application; php ../composer.phar install`

> Some people prefer not to install the composer, depending directly on the server and copy them to the local machine (by copying the contents of a folder `application/vendor`). Such an embodiment example, but may result in conflicts.

## Update on production server

Update DotPlant2 CMS core is performed in the following order.

From the folder where it was cloned dotplant2:

``` bash
git pull         # обновить git репозиторий
cd application
composer update  # обновить зависимости composer
./yii migrate    # применить миграции
```

Next, `application/web/theme` folder need to be updated with your personal repository of site specific functionality and theme:

``` bash
git pull
```

In some cases, it may be necessary to clear the cache and assets.
To do this, in the backend there is a button in the upper right corner with eraser icon.

Alternatively, you can clean the cache console command:

``` bash
cd application
./yii cache/flush cache
```

> Keep in mind that changes to the database and downloaded files in your local machine do not fall into the version control system. At the same time, constantly copying the database server on the production server also not desirable - may be lost change orders and completed application forms, which have been produced directly on site.

## The total work process

1. 1. Update DotPlant2 on the local machine to make composer update and update (git pull) site specific functionality repository 
2. Develop a site specific functionality
3. Update DotPlant2 on the local machine and make the composer update
4. Check that everything works, and **only** then do git commit site specific functionality repository (`application/web/theme`)
5. Make git pull from the site specific functionality repository to update the changes made by other developers site
6. If necessary, resolve conflicts and make git push
7. Carry out the update to production server (see paragraph above)
8. If you change the config again, changes in server too
9. Check the operation of the production version of the site and its critical components (order form, contact page, shopping cart, etc.)
