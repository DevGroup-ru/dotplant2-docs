# Creating thumbnails

> Thumbnails are created automatically when images are downloaded, but if you have added a new size of the thumbnails or the watermark, the generation will occur at the time of the request. In this case, it is recommended to generate them immediately to accelerate the `cold start` CMS.

In the `Pictures/Create thumbnail` there is only one button. It carries only one meaning - to execute thumbnail generation. The task will be performed in the background. To do this, [configure background tasks](background-tasks.md).