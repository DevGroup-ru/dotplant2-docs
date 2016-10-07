# Create a skeleton theme

Since each site is unique - it needs its own design theme.

In DotPlant2 in the concept of the theme, we also invest in all the site-specific functionality. Therefore, in addition to registration, in a web folder / theme we will also store necessary controllers and models.

To create a theme skeleton is quite the following command from the application folder:

``` bash
$ ./yii admin/create-theme
```

This will create a folder `web / theme` with the necessary structure:

```
css
dist
fonts
gulpfile.js
images
index.html
js
module
node_modules
package.json
resources
sass
views
```

Since this site specific files - they do not fall into the main repository dotPlant and this folder is located in .gitignore.

For convenience, you can create a repository for git-themes:

``` bash
$ cd web/theme
$ git init
Initialized empty Git repository in /Users/bethrezen/git-my/dotplant2/application/web/theme/.git/

$ git add *
$ git remote add origin git@YOUR_GIT_HOST:YOUR_THEME_REPO.git

$ git commit -m "first commit :grinning:"
[master (root-commit) e341004] first commit :grinning:
 27 files changed, 70 insertions(+)


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

Thus, all your site-specific functionality will be in the version control system and work on it as a team will be more convenient.