title: 发一个Python版的星际译王
tags:
  - Python
  - sdcv
id: 548
categories:
  - 编程相关
date: 2009-02-26 17:35:55
---

sdcv-0.4.2版本的代码有3480行，而图形界面的星际译王更是有N多的代码，俺也写了一个就十几行的Python代码，速度当然比不上sdcv，能差十倍，不过，请注意，sdcv查一个单词是0.01秒,我这个是0.1秒，我不觉得有人能感觉出来差别。

其实我的核心是C语言写的，之所以在速度上没有sdcv快是没有做那么些优化，俺只写了80多行，而且还有不少代码是用作Python封装的。我以前写[文章](http://cocobear.github.io/2008/12/31/dict-of-stardict/)说过sdcv代码写的很麻烦，用C语言可以很简洁的写出来，现在确实做到了，只是结合了Python。我觉得这种模式挺不错，影响速度的核心使用C语言去写，然后主逻辑框架使用Python写，不仅效率不会受影响，开发的速度也提高了不少。（我这个例子Python并没做什么）


	static PyObject * lookup(PyObject *self, PyObject *args)
	{
	    int fd;
	    char *file_prefix;
	    long file_size;
	    long wc;
	    char *word;

	    char *data;
	    const char *p;
	    long index_size;
	    int offset;
	    int size;
	    int flag = 0;
	    unsigned char *buf;

	    if (!PyArg_ParseTuple(args, "slls:lookup",
	                &file_prefix, &file_size, &wc, &word)) {
	        return NULL;
	    }
	    char file_name[256];
	    strcpy(file_name, file_prefix);
	    strcat(file_name, ".idx");
	    if ((fd = open(file_name, O_RDONLY)) < 0) {
	        printf("open failedn");
	        return NULL;
	    }
	    data = (char *)mmap( NULL, file_size, PROT_READ, MAP_SHARED, fd, 0);
	    p = data;
	    int i;
	    for (i=0;i<wc;i++) {
	        index_size = strlen(p) + 1 + 2*sizeof(int);
	        if (strcmp(word, p) == 0) {
	            flag = 1;
	        }
	        if (flag == 1) {
	            offset = ntohl(*(int *)(p + strlen(p) + 1));
	            size   = ntohl(*(int *)(p + strlen(p) + 1 + sizeof(int)));
	            //printf("offset=%dnsize%dn",offset,size);
	            /*gzFile zfile;
	            zfile = gzopen("./dic/stardict-langdao-ec-gb-2.4.2/langdao-ec-gb.dict.dz", "rb");
	            gzseek(zfile, offset, SEEK_SET);
	            buf = (unsigned char *)malloc(*size+1);
	            memset(buf, ' ', size+1);
	            gzread(zfile, buf, size);
	            printf("%sn", buf);
	            */
	            close(fd);

	            if ((fd = open("./dic/stardict-langdao-ec-gb-2.4.2/langdao-ec-gb.dict", O_RDONLY)) < 0) {
	                return NULL;
	            }
	            lseek(fd, offset, SEEK_SET);
	            buf = (unsigned char *)malloc(size+1);
	            memset(buf, ' ', size+1);
	            read(fd, buf, size);
	            //printf("%sn",buf);
	            close(fd);
	            return Py_BuildValue("s", buf);
	        }
	        p += index_size;
	    }

	    return Py_BuildValue("s","");
	}

	static struct PyMethodDef lookup_methods[] = {
	    {"lookup", lookup, 1, "lookup(file_prefix, file_size, wc, word)"},
	    {NULL, NULL}
	};

	void initlookup()
	{
	    (void) Py_InitModule("lookup", lookup_methods);
	}


使用C语言对Python进行扩展挺方便的，http://gashero.yeax.com/?p=38#id7这里有个不错的文档。
完了俺整理整理也放在google code上去。

上面的代码中dict文件需要是未gzip压缩过的，如果在压缩过的我使用被注释掉的那段代码在seek的时候速度很慢，又没办法用mmap，所以暂时就先只使用未压缩过的。
## Comments

**[okidogi](#5302 "2009-02-26 22:14:01"):** 代码看起来有些晃眼睛...

**[草儿](#5299 "2009-02-26 19:38:27"):** 靠，手机看你的博客真费劲

**[shuge.lee](#5304 "2009-02-27 00:44:57"):** 倒是希望有个Ubiquity+sd for broswer或Lingoes+sd for desktop

