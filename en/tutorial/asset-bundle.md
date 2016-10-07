# Work with the resources and AssetBundle

Since DotPlant2 is based on Yii Framework 2 - we try to adhere to all standards and design patterns that sets the framework.

That is why we do not post links to our styles and scripts directly into the HTML-code templates.
For these things invented AssetBundle (or resource kits).
Learn more about them you can read in the [official guide Yii2] (https://github.com/yiisoft/yii2/blob/master/docs/guide/structure-assets.md).

If you created a theme with the help of `./yii admin/create-theme`, then you already have a file for a set of resources - `web/theme/module/assets/ThemeAsset.php`.

> As the topic - this module, there is also a namespace apply.
>
> For example, to ThemeAsset namespace file is `app\web\theme\module\assets`

If you created a theme through the console command `./yii admin/create-theme`, then you have already configured scenarios [gulp](http://gulpjs.com/) for comfortable work.

> To set gulp is very simple: `npm install --global gulp`

Gulp compiles the necessary resources in the `dist/` folder. It is in this folder is set by default  `ThemeAsset`.

Initially, the structure of the source files to gulp as follows:
* `images` - source files of images of the site. In the future they will be compressed into a folder gulp means `dist/images`.
* `sass` - source files [SASS](http://sass-lang.com). They are compiled gulp means in accordance css files in the folder `dist/styles`. At the start there is a file `main.scss`, which is connected to a ThemeAsset like `styles/main.css`.
* `js` - javascript source files that are compiled into a folder gulp means `dist/scripts`.

For css and js files will be automatically created minified version, such as  `dist/styles/main.min.css`.

The first time after generation of the threads need to run the theme folder `gulp`.

In the future, when working is recommended to use `gulp watch`.
This command launches a special gulp mode, in which all changes to scss, js, and the pictures are automatically recorded, and in the browser is updated by means of [livereload](https://github.com/vohof/gulp-livereload), if exposed YII_DEBUG.