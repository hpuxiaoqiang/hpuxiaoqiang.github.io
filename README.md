# hpuxiaoqiang.github.io

**Attention**
hexo-env分支，只用于存放博客的hexo环境文件。这样 有利于我们在多台电脑上对博客进行更新。当在一台新的电脑上是，只需要将远程的hexo-env分支拉取最新的环境文件即可。

因此每次更新博客时，需要先将hexo环境文件推送到hexo-env分支，然后再发布文章。

推送环境文件到hexo-env分支
1. git add . 
2. git commit -m "description"
3. git push origin hexo-env
推送博客静态文件到master分支
4. hexo g
5. hexo d
