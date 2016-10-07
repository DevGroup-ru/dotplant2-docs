# Working with Images

> To work with downloadable images in DotPlant2 implemented module `Images`. It supports various types of data storage (local file system, ftp, sftp, Amazon S3) and contains a number of functions (create thumbnails, add watermark).

you need to make base module settings for the correct operation of the application. This is done in 'Settings/Config` CMS admin panel tab `Images`.

Shape setup is divided into several sections: the basic configuration and file system components.

Consider the basic settings:

* `Use watermark` - includes support for watermarks;
* `Default thumbnail size` -  size format {width} x {height} pixel thumbnail image, which is created by default;
* `No image src"` -  a reference to the image displayed when the image does not exist in the file system, or can not be accessed;
* `Thumbnails directory` - the directory in which the thumbnails are created;
* `Watermark directory` - the directory in which the loading watermarks;
* `Default Component` - a component of the default file system for storing images.

Blocks FS components are configured similarly to [creocoder/yii2-flysystem](https://github.com/creocoder/yii2-flysystem).

Additional functionality is configured in 'Images` administrative panel.
