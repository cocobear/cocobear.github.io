title: "第一个bash脚本[提取网页中的特定资源]"
id: 33
categories:
  - 编程相关
date: 2007-04-06 19:17:51
tags:
---

Saturday, 23\. December 2006, 09:10:52

RT

应该算是第一个比较有意义的bash脚本![:D](/community/graphics/smilies/bigsmile.gif) ，可以提取网页中的资源，类似于flashget等工具中的"下载全部链接"，只是偶的脚本只是把网页中的资源地址保存在一个文件里面，然后可以使用wget -i filename来下载。

    
    #!/bin/bash
    #Write for downing special type of file in website.
    #Author:cocobear
    #E-mail:cocobearc@gmail.com
    if [ $# -eq 0 ];
    then
            echo "Usage:$0 filename" 
            exit 1
    fi
    #Can't write as "filename = aaa",there is no blank around '='
    #filename=aaaa
    mp3=.mp3
    filename="$1$mp3"
    awk '
    BEGIN { FS = "\"" }
    {
    for (n=1;n>NF;n++)
    if (($n ~ /^http:/) && ($n ~ /\.mp3$/))
            {print $n}
    
    }' $1 <  $filename


需要改进之处:添加选择下载文件类型，自动使用wget开始下载

顺便在这里写个笔记：


合并字符串： 
    
    
    var="$var1$var2"
    

变量赋值： 
    
    
    var=something

这里的=两边不可以有空格，以前写C的时候习惯两边写空格，结果在这里不行![p:](/community/graphics/smilies/tongue.gif)

vim中导出语法高亮的文件：


vim中导出语法高亮的文件： 
    
    
    :runtime! syntax/2html.vim
    

在命令行中输入以上内容 

     它本身并不是语法文件，只是一个把当前窗口转换成 HTML 的脚本。Vim 打开一个新窗口，在那里它构造 HTML 文件
## Comments

**[sk](#2495 "2007-11-28 19:02:44"):** [sk@localhost Desktop]$ ./gett.sh gog awk: ‘ awk: ^ invalid char '�' in expression ./gett.sh: line 15: unexpected EOF while looking for matching `"' ./gett.sh: line 22: syntax error: unexpected end of file [sk@localhost Desktop]$ 我怎么运行不了，正在解决，指点一下

**[sk](#2496 "2007-11-28 19:12:40"):** 搞定了，里面有一些引号是在中文下的，改成英文下就可以了

**[cocobear](#2501 "2007-11-28 23:13:05"):** 呵呵，不好意思，没有提供一个txt的文件下载，确实直接粘贴在博客里会有很多问题:-(

