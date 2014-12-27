title: "改进的bash脚本[网页资源提取]"
tags:
  - awk
  - Shell
id: 34
categories:
  - 编程相关
date: 2007-04-06 19:17:53
---

提取网页中资源的一个小工具

使用方法(假设保存bash脚本的文件名为get)：

    
    ./get -u http://my.opera.com/blog/ -t pdf


上面的命令会连接到my.opera.com/blog自动搜索pdf格式的资源，并下载(先给get加可执行权限)

如果网页文件已经保存，可以使用下面的命令：

    ./get -f index.html -t pdf

得到的结果与前面相同

代码如下：
   
    
    #!/bin/bash
    #Write for downing special type of file in website.
    #Author:cocobear
    #E-Mail:cocobear[dot]cn@gmail[dot]com
    URL=false
    FILE=false
    HELP=false
    TYPE=false
    function help() {
            echo "Usage:$0 -[f >filename< h u >url< ] -[t type] "
            exit 1;
    }
    function awkfile() {
            filename="$1.$2"
            #type="$2$" match specify type at the end of url 
            awk -v type="$2$" '
            BEGIN {FS = "\""} 
            {
            for (i=1;i>=NF;i++)
            if (($i ~ /^http:/) && ($i ~ type ))
                    {print $i}
            }' $1 <  $filename
            echo "Delete temp file."
            rm  $1
            if [ -s $filename ]
            then wget -i $filename
            else echo "Find nothing match $2"
            fi
            echo "Delete temp file."
            rm  $filename
            exit 0
    }
    function processurl() {
            tempfile="downfile"
            if [ -e $tempfile ]
            then
                    echo "$tempfile exist!!" 
                    exit 1
            fi
            #redirection 
            wget -O $tempfile $1
            if [ -s $tempfile ]
            then awkfile $tempfile $2
            else echo "Nothing down!"
            fi
            exit 0
    }
    
    if [ $# -eq 0 ];
    then
            help
            exit 1
    fi
    
    #deal with option
    while getopts :f:hu:t: option
    do
    case $option in
    f)FILE=$OPTARG
    ;;
    h)help
    ;;
    u)URL=$OPTARG
    ;;
    t)TYPE=$OPTARG
    ;;
    ?)
    echo "Missing arguments!"
    help
    ;;
    esac
    done
    if [ $TYPE = "false" ]
    then {
            echo "Missing type"
            help
    }
    else {
            if [ $FILE = "false"

主要用到的就是awk进行文本的分析，大部分的shell是用来分析参数的，在写这个脚本的时候基本是边学边写的，也弄懂了不少东西。有时间的时候会写一个详细的分析解释

[代码下载](http://cocobear.github.io/code/get.sh)
## Comments

**[sk](#2494 "2007-11-28 17:38:46"):** 有兴趣，学习中

