title: 处理了下网站
tags:
  - Linux
  - MySQL
  - Shell
id: 305
categories:
  - Linux
date: 2008-08-15 17:57:53
---

改了info域名后网站以前里很多页面里有cocobear.cn，连一些图片也显示不出来，今天就处理了一下：

把网站里一些文件做替换，先备份一下：
grep cocobear.cn ./cocobear.github.io/ -R -l --binary-files=without-match|xargs -i cp --parents {} bk/

然后全部替换：
grep cocobear.cn ./cocobear.github.io/ -R -l --binary-files=without-match | xargs sed -i 's/cocobear\.cn[^@]/cocobear\.info/g'

剩下就是数据库里的内容了，主要是wp_posts表中的post_content字段，还有guid字段，还有wp_comments中的comment_author_url，wp_postmeta中的meta_value;
操作的命令是：
mysql> update wp_posts set post_content=replace(post_content,"http://www.cocobear.cn","http://cocobear.github.io");

当然在进行数据库操作时一定要记得先备份一下：
 mysqldump -h mysql.1g50.cn -u cocobear -p cocobear > 0808151.sql

使用 mysqldump -h mysql.1g50.cn -u cocobear -p cocobear < 0808151.sql恢复似乎不太好，还是进mysql里使用source ~/0808151.sql进行恢复。
## Comments

**[Amankwah](#4038 "2008-08-15 21:03:21"):** 呵呵

**[luguo](#4039 "2008-08-16 19:44:38"):** 看来dp比老大更适合走网管路线阿～～

**[crazyfranc](#4042 "2008-08-17 22:23:58"):** pig还是当网管吧

**[cocobear](#4043 "2008-08-18 09:29:56"):** 晕………………………… 我还没把和小孔同学弄的转移网站的东西贴出来，那更“网管”！

