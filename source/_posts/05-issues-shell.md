title: shell脚本解题4
tags:
  - sed
  - Shell
id: 294
categories:
  - 编程相关
date: 2008-07-18 16:24:32
---

问题：
用sed把每行的第二个字符到第六个字符替换成星号
原文
123456
1234567
12345

要求结果

1*****
1*****7
1****

**解法：
1.**
	sed '/^../{h;s/^.\(.\{1,5\}\).*/\1/;s/./*/g;G;s/\(.*\)\n\(.\).\{1,5\}\(.*\)/\2\1\3/}'

**2.**
	sed -r '/.{6}/bb
	:a
	s/(.\**)[^*]/\1\*/
	ta
	b
	:b
	/^.\*{5}/! {
	s/(.\**)[^*]/\1\*/
	tb
	}'	
**3.**
	awk '{a=(substr($0,2,5));gsub(/./,"*",a);print substr($0,1,1)a""substr($0,7,length($0))}'

**4.**
	sed 's/^\(.\)\(.\{1,5\}\)\(.*\)/\1\n\2\n\3/' file  | sed '{n; s/./*/g;n}' | sed '{N; N; s/\n//g}'

**5.**
	sed ':a;/^.[*]\{5\}/!{s/\(.[*]*\)./\1*/;/*$/!ta}'[/bash]

问题：
比较2008-01-29和2008-02-1之间相差月份

**解法：
1.**
	echo "$date1-$date2" | awk -F'-' '{a=($1-$4)*12+($2-$5);print sqrt(a*a)}'
## Comments

**[luguo](#3899 "2008-07-18 19:24:18"):** 第1个的perl解法： perl -ne 'chomp; $i= length($_) > 6? 5: length($_)-2; substr($_, 1, $i)="*"x$i; print "$_\n";' test.txt

**[luguo](#3900 "2008-07-18 19:25:48"):** Oops! 应该是：perl -ne 'chomp; $i= length($_) > 6? 5: length($_)-1; substr($_, 1, $i)="*"x$i; print "$_\n";' test.txt

**[luguo](#3901 "2008-07-18 19:28:13"):** 或者更简单的： perl -ne '$i= length($_) > 7? 5: length($_)-2; substr($_, 1, $i)="*"x$i; print;' test.txt

**[luguo](#3902 "2008-07-18 21:21:35"):** 第2个： echo '2008-01-2007-12' | perl -ne '($a, $b, $c, $d)=split/\\-/; print abs(($a-$c)*12+$b-$d), "\n";'

