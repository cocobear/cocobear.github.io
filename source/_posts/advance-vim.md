title: vim进阶
tags:
  - Linux
  - Vim
id: 194
categories:
  - Linux
date: 2007-09-30 13:46:38
---

/*摘自：http://edt1023.sayya.org/vim/index.html
&nbsp; *
&nbsp; *里面有很多非常好用的我用蓝色标记了一下，看看你都知道这些好用的功能吗？
 &nbsp;*
 &nbsp;*/

Ctrl+f  即 PageDown 翻頁（Forward，向前、下翻頁）。
Crtl+b  即 PageUp 翻頁（Backward，向後、上翻頁)。

0       是數目字 0 而不是英文字母 o。移至行首，（含空白字元）。
W       移动一个符，但會忽略一些標點符號。
e       移至後一個字字尾。
E       同上，但會忽略一些標點符號。
b       移至前一個字字首。
B       同上，但會忽略一些標點符號。
H       移至螢幕頂第一個非空白字元。
M       移至螢幕中間第一個非空白字元。
L       移至螢幕底第一個非空白字元。

)       移至下一個句子（sentence）首。
(       移至上一個句子（sentence）首。sentence（句子）是以 . ! ? 為區格。
}       移至下一個段落（paragraph）首。
{       移至上一個段落（paragraph）首。paragraph（段落）是以空白行為區格。

R进入替换模式

I       在行首開始輸入文字。此之行首指第一個非空白字元處，要從真正的第一個字元處開始輸人文字，可使用 0i 或 gI(Vim 才有)。
A       在行尾開始輸入文字。這個好用，您不必管游標在此行的什麼地方，只要按 A 就會在行尾等著您輸入文字。
O       在游標所在行上開一新行來輸入文字。
J       將下一行整行接至本行(Joint)。

dG      刪至檔尾。
d1G     刪至檔首。或 dgg(只能用於 Vim)。
D       刪至行尾，或 d$（含游標所在處字元）。
d0      刪至行首，或 d^（不含游標所在處字元）。請回憶一下 $ 及 ^ 所代表的意義，您就可以理解 d$ 及 d^ 的動作，這就是 vi(m) 可愛之處。

5J      將五行合併成一行。
5i A    然後按 Ecs，插入五個 A。中文也可以！
2i sys Esc      插入 syssys。中文也可以！

<font color=lightblue>:ce(nter) 本行文字置中。注意是冒號命令！
:ri(ght)        本行文字靠右。
:le(ft) 本行文字靠左。所謂置中、靠左右，是參考 textwidth(tw) 的設定。如果 tw 沒有設定，預設是80，就是以 80 個字元為總寬度為標準來置放。當然您也可以如 sw 一樣馬上重設。</font>
gqap    整段重排，或 gqip，在段落中位何地方都可以使用。和中文的配合見下述。
gqq     本行重排。
gqG     全文重排，是以游標所在處的段落開始重排至檔尾。以空白行為段落的間隔。

<font color=lightblue>» 整行向右移一個 shiftwidth（預設是 8 個字元，可重設）。这个好啊，写代码时缩进就方便了。
«       整行向左移一個 shiftwidth（預設是 8 個字元，可重設）。</font>

:[range]s/pattern/string/[c,e,g,i]5.1 
range   指的是範圍，1,7 指從第一行至第七行，1,$ 指從第一行至最後一行，也就是整篇文章，也可以 % 代表。還記得嗎？ % 是目前編輯的文章，# 是前一次編輯的文章。
pattern 就是要被替換掉的字串，可以用 regexp 來表示。
string  將 pattern 由 string 所取代。
c       confirm，每次替換前會詢問。
e       不顯示 error。
g       globe，不詢問，整行替換。
i       ignore 不分大小寫。

<font color=lightblue>mx        x 代表 26 個小寫英文字母，這樣游標所在處就會被 mark。</font>
`x      回到書籤原設定位置。` 是 backward quote，就是 Tab 鍵上面那一個。
'x      回到書籤設定行行首。' 是 forward quote，是 Enter 鍵隔壁那一個。

<font color=lightblue># 或 Ctrl+^       編輯前一個檔案，用於兩檔互相編輯時相當好用。這種用法不管是 argument list 或 buffer list 檔案間皆可使用。還記得嗎？# 代表的是前一次編輯的檔案。</font>

:f 或 Ctrl+g    顯示目前編輯的檔名、是否經過修改及目前游標所在之位置。
:f 檔名 改變編輯中的檔名。(file)
<font color=lightblue>:r 檔名   在游標所在處插入一個檔案內容。(read)</font>

<font color=lightblue>vim -x [檔名] 加密编辑一个文件，每次编辑时要求输入密码，否则为乱码。也可以在编辑时使用:X命令来设置密码，不过要注意，设置以后需要保存并退出。</font>

Ctrl+w n        即 :new。開一空的新視窗。
Ctrl+w s        即 :sp(lit)，會開一新視窗，且原檔分屬兩個視窗。
Ctrl+w f        開一新視窗，並編輯游標所在處之 word 為檔名的檔案。
Ctrl+w q        即 :q 結束分割出來的視窗。
Ctrl+w o        即 :only! 使游標所在之視窗，成為目前唯一顯示的視窗其它視窗會隱藏起來。
Ctrl+w j        移至下視窗。
Ctrl+w k        移至上視窗。還記得 hjkl 的按鍵移動方式嗎？

:sh(ell)        執行 shell。使用 exit 回到 vim。
<font color=lightblue>:r !commond       這個就妙了！會在游標所在處次一行插入外部指令 commond 執行後的輸出內容。例如 :r !date 就會插入日期時間。這在 elvis 是會插入在游標所在處那一行。</font>
:n,mw !commond  以 n 至 m 行內之資料，當做外部指令 commond 的 input。這算是相當高級的用法了，初學者大概還用不上，不過印象中留有一個這樣的功能，以後總是會用得上的。
<font color=lightblue>K 大寫 K 會顯示游標所在處之 word 的 man page 系統線上使用手冊。</font>
## Comments

**[cocobear](#1869 "2007-10-01 22:56:25"):** 确实，以前还要换个地方man一下。

**[luguo](#1867 "2007-10-01 15:35:27"):** K这个尤其特别地好用！！哈哈～！

**[dream](#2070 "2007-10-22 20:09:10"):** 太伟大了，真是编辑器之神～～

**[Julius](#12444 "2012-01-16 00:32:18"):** I certainly knew about many of this, but having said that, I still considered it had been useful. Good blog!

**[felmHeittevem](#15265 "2012-09-27 15:05:56"):** lLkvlxxh [Phen375 Does It Really Work](http://buyphen375.postbit.com) jTjlfyov

