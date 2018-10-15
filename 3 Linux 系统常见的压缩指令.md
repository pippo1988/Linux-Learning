
# 8.2 Linux 系统常见的压缩指令
&ensp;&ensp;&ensp;&ensp;在 Linux 的环境中，压缩文件案的扩展名大多是：『*.tar, *.tar.gz, *.tgz, *.gz, *.Z, *.bz2, *.xz』，为什么会有这样的扩展名呢？不是说Linux 的扩展名没有什么作用吗？   
&ensp;&ensp;&ensp;&ensp;这是因为 Linux 支持的压缩指令非常多，且不同的指令所用的压缩技术并不相同，当然彼此之间可能就无法互通压缩/解压缩文件案啰。所以，当你下载到某个压缩文件时，自然就需要知道该文件是由哪种压缩指令所制作出来的，好用来对照着解压缩啊！ 也就是说，虽然Linux 文件的属性基本上是与文件名没有绝对关系的， 但是为了帮助我们人类小小的脑袋瓜子，所以适当的扩展名还是必要的！ 底下我们就列出几个常见的压缩文件案扩展名吧：  
|文件后缀名|文件来源|  
|:----|:---|  
|*.Z|compress 程序压缩的文件|
|*.zip|zip 程序压缩的文件|
|*.gz|gzip 程序压缩的文件|
|*.bz2|bzip2 程序压缩的文件|
|*.xz|xz 程序压缩的文件|
|*.tar|tar 程序打包的数据|
|*.tar.gz|tar 程序打包，并且经过 gzip 压缩|
|*.tar.bz2|tar 程序打包，并且经过 bzip2 的压缩|
|*.tar.xz|tar 程序打包，并且经过 xz 的压缩|  
&ensp;&ensp;&ensp;&ensp;Linux 上常见的压缩指令就是gzip, bzip2 以及最新的xz ，至于compress 已经退流行了。为了支持windows 常见的zip，其实Linux 也早就有zip 指令了！ gzip 是由GNU 计划所开发出来的压缩指令，该指令已经取代了compress 。后来 GNU 又开发出bzip2 及xz 这几个压缩比更好的压缩指令！不过，这些指令通常仅能针对一个文件来压缩与解压缩，如此一来， 每次压缩与解压缩都要一大堆文件，岂不烦人？此时，那个所谓的『打包软件, tar』就显的很重要啦！  
&ensp;&ensp;&ensp;&ensp;这个tar 可以将很多文件『打包』成为一个文件！甚至是目录也可以这么玩。不过，单纯的tar 功能仅是『打包』而已，亦即是将很多文件集结成为一个文件， 事实上，他并没有提供压缩的功能，后来，GNU 计划中，将整个tar 与压缩的功能结合在一起，如此一来提供使用者更方便并且更强大的压缩与打包功能！ 底下我们就来谈一谈这些在 Linux 底下基本的压缩指令吧！  
*******
## 8.2.1 gzip, zcat/zmore/zless/zgrep
&ensp;&ensp;&ensp;&ensp;gzip 可以说是应用度最广的压缩指令了！目前gzip 可以解开compress, zip 与gzip 等软件所压缩的文件。至于 gzip 所建立的压缩文件为*.gz 的檔名喔！让我们来看看这个指令的语法吧：  
```bash
[pippo@Centos7 ~]$ gzip --help 
Usage: gzip [OPTION]... [FILE]...
Compress or uncompress FILEs (by default, compress FILES in-place).
# 压缩或者解压缩文件（默认压缩完不保留原文件）
Mandatory arguments to long options are mandatory for short options too.

  -c, --stdout      write on standard output, keep original files unchanged
# 将压缩的数据输出到标准输出，保持原文件不被改变
  -d, --decompress  decompress
# 解压文件
  -t, --test        test compressed file integrity
# 可以用来检验一个压缩文件的一致性，查看文件有无错误
  -v, --verbose     verbose mode
# 可以显示出原文件/压缩文件案的压缩比等信息
  -# --fast --best  Regulate the speed of compression using the specified digit  #
# # 为数字的意思，代表压缩等级，-1 最快，但是压缩比最差、-9 最慢，但是压缩比最好！预设是 -6
With no FILE, or when FILE is -, read standard input.
# 如果没有文件或者文件位置为 - ，读取标准输入
```  
```bash
范例一：找出 /etc 底下 (不含子目录) 容量最大的文件，并将它复制到 /tmp ，然后以 gzip 压缩 
[pippo@Centos7 ~]$ ls -ldS /etc/* |head -1
-rw-r--r--.  1 root root   670293 Jun  7  2013 /etc/services
[pippo@Centos7 ~]$ cp /etc/services /tmp
pippo@Centos7 ~]$ gzip -v /tmp/services 
/tmp/services:	 79.7% -- replaced with /tmp/services.gz
[pippo@Centos7 ~]$ ll /etc/services /tmp/services*
-rw-r--r--. 1 root  root  670293 Jun  7  2013 /etc/services
-rw-r--r--. 1 pippo pippo 136088 Oct 13 13:07 /tmp/services.gz

范例二：由于 services 是文本文件，请将范例一的压缩文件的内容读出来！
[pippo@Centos7 ~]$ zcat /tmp/services.gz |less
# 由于 services 这个原本的文件是是文本文件，因此我们可以尝试使用 zcat/zmore/zless 去读取！
# 此时屏幕上会显示 servcies.gz 解压缩之后的源文件内容！

范例三：将范例一的文件解压缩 
[pippo@Centos7 tmp]$ gzip -d  services.gz 
# 也可以使用 gunzip 来进行解压缩，等同于 gzip -d
# 与 gzip 相反， gzip -d 会将原本的 .gz 删除，回复到原本的 services 文件。

范例四：将范例三解开的 services 用最佳的压缩比压缩，并保留原本的文件
[pippo@Centos7 tmp]$ gzip -9 -c services >services.gz

范例五：由范例四再次建立的 services.gz 中，找出 http 这个关键词在哪几行？
zgrep -n 'http' services.gz |less
```
&ensp;&ensp;&ensp;&ensp;其实gzip 的压缩已经优化过了，所以虽然gzip 提供1~9 的压缩等级，不过使用默认的6 就非常好用了！ 因此上述的范例四可以不要加入那个 -9 的选项。范例四的重点在那个-c 与> 的使用啰！-c 可以将原本要转成压缩文件的资料内容，将它变成文字类型从屏幕输出， 然后我们可以透过大于(>) 这个符号，将原本应该由屏幕输出的数据，转成输出到文件而不是屏幕，所以就能够建立出压缩挡了。只是档名也要自己写， 当然最好还是遵循 gzip 的压缩文件名要求较佳喔！！更多的> 这个符号的应用，我们会在bash 章节再次提及！  
&ensp;&ensp;&ensp;&ensp;cat/more/less 可以使用不同的方式来读取纯文本档，那个zcat/zmore/zless 则可以对应于cat/more/less 的方式来读取纯文本档被压缩后的压缩文件！由于 gzip 这个压缩指令主要想要用来取代compress 的，所以不但compress 的压缩文件案可以使用gzip 来解开，同时zcat 这个指令可以同时读取compress 与gzip 的压缩文件呦！  
&ensp;&ensp;&ensp;&ensp;另外，如果你还想要从文字压缩文件当中找数据的话，可以透过 egrep 来搜寻关键词喔！而不需要将压缩文件解开才以grep 进行！ 这对查询备份中的文本文件数据相当有用！  
*********
## 8.2.2 bzip2, bzcat/bzmore/bzless/bzgrep
&ensp;&ensp;&ensp;&ensp;若说 gzip 是为了取代compress 并提供更好的压缩比而成立的，那么bzip2 则是为了取代gzip 并提供更佳的压缩比而来的。bzip2 真是很不错用的东西～这玩意的压缩比竟然比gzip 还要好～至于bzip2 的用法几乎与gzip 相同！看看底下的用法吧！  
```bash
[pippo@Centos7 tmp]$ bzip2 --help 
bzip2, a block-sorting file compressor.
# bzip2 一个以块为单位的文件压缩处理工具
常见选项参数
   -d --decompress     force decompression
# 解压缩文件
   -z --compress       force compression
# 压缩文件，默认选项，看不添加
'   -k --keep           keep (do not delete) input files'
# 保留源文件，不删除原来的文件
   -f --force          overwrite existing output files
# 覆盖已经存在的输出文件
   -t --test           test compressed file integrity
# 测试压缩文件的完整性
   -c --stdout         output to standard out
# 输出结果到标准输出
   -q --quiet          suppress noncritical error messages
# 忽略非关键的错误信息
   -v --verbose        be verbose (a 2nd -v gives more)
# 可以显示出原文件/压缩文件案的压缩比等信息
   -s --small          use less memory (at most 2500k)
# 使用更少的内存（最少2500K）
   -1 .. -9            set block size to 100k .. 900k
   --fast              alias for -1
   --best              alias for -9

bzip2, default action is to compress.
# bzip2 默认压缩
bunzip2,  default action is to decompress.
# bunzip2 默认解压缩
bzcat, default action is to decompress to stdout.
# bzcat 默认解压缩并输出到标准输出
   If no file names are given, bzip2 compresses or decompresses
   from standard input to standard output. 
# 如果不给定文件名，bzip2 将会处理标准输入输出
```
```bash
范例一：将刚刚 gzip 范例留下来的 /tmp/services 以 bzip2 压缩
[pippo@Centos7 tmp]$ bzip2 -v services
  services:  5.409:1,  1.479 bits/byte, 81.51% saved, 670293 in, 123932 out.
[pippo@Centos7 tmp]$ ll services*
-rw-r--r--. 1 pippo pippo 123932 Oct 13 13:07 services.bz2
-rw-rw-r--. 1 pippo pippo 135489 Oct 13 14:07 services.gz
# 此时 services 会变成 services.bz2 之外，也可以发现 bzip2 的压缩比要较 gzip 好
# 压缩率由 gzip 的 79% 提升到 bzip2 的 81% 

范例二：将范例一的文件内容读出来！
[pippo@Centos7 tmp]$ bzmore services.bz2

范例三：将范例一的文件解压缩
[pippo@Centos7 tmp]$ bzip2 -d services.bz2

范例四：将范例三解开的 services 用最佳的压缩比压缩，并保留原本的文件
[pippo@Centos7 tmp]$ bzip2 -9 -k services
```
&ensp;&ensp;&ensp;&ensp;看上面的范例，你会发现到bzip2 连选项与参数都跟gzip 一模一样！只是扩展名由.gz 变成.bz2而已！其他的用法都大同小异，所以鸟哥就不一一介绍了！ 你也可以发现到 bzip2 的压缩率确实比gzip 要好些！不过，对于大容量文件来说，bzip2 压缩时间会花比较久喔！至少比gzip 要久的多！这没办法～要有更多可用容量，就得要花费相对应的时间！还 OK 啊！
*********  
## 8.2.3 xz, xzcat/xzmore/xzless/xzgrep
&ensp;&ensp;&ensp;&ensp;虽然 bzip2 已经具有很棒的压缩比，不过显然某些自由软件开发者还不满足，因此后来还推出了xz这个压缩比更高的软件！这个软件的用法也跟gzip/bzip2 几乎一模一样！ 那我们就来瞧一瞧！  
```bash
[pippo@Centos7 tmp]$ xz --help 
Usage: xz [OPTION]... [FILE]...
Compress or decompress FILEs in the .xz format.
# 以 .xz格式来压缩或者解压缩文件
  -z, --compress      force compression
# 压缩文件，默认选项
  -d, --decompress, --uncompress
                      force decompression
# 解压缩文件
  -t, --test          test compressed file integrity
# 测试压缩文件的完整性
'  -l, --list          list information about .xz files  '
# 列出压缩文件的信息
  -k, --keep          keep (do not delete) input files
# 保留源文件
  -f, --force         force overwrite of output file and (de)compress links
# 
  -c, --stdout, --to-stdout
                      write to standard output and do not delete input files
# 输出结果到标准输出，保留源文件
  -0 ... -9           compression preset; default is 6; take compressor *and*  decompressor memory usage into account before using 7-9!
# 设定压缩比例，默认为6，使用7-9时要考虑内存的使用情况
  -e, --extreme       try to improve compression ratio by using more CPU time; does not affect decompressor memory requirements
# 消耗 CPU 来提高压缩比，不影响内存的使用
  -T, --threads=NUM   use at most NUM threads; the default is 1; set to 0 to use as many threads as there are processor cores
# 最多使用的线程数；默认为1；设置为 0 时线程数和处理器核数一样
  -q, --quiet         suppress warnings; specify twice to suppress errors too
# 忽略警告，设置两次忽略错误
  -v, --verbose       be verbose; specify twice for even more verbose
# 显示压缩比等信息，设置两次显示更多信息
With no FILE, or when FILE is -, read standard input.
# 没有文件或者文件为 - ，读取标准输入
```
```bash
范例一：将刚刚由 bzip2 所遗留下来的 /tmp/services 透过 xz 来压缩！
[pippo@Centos7 tmp]$ xz -v services
services (1/1)
  100 %        97.3 KiB / 654.6 KiB = 0.149 
[pippo@Centos7 tmp]$ ll services*
-rw-r--r--. 1 pippo pippo 123932 Oct 13 13:07 services.bz2
-rw-rw-r--. 1 pippo pippo 135489 Oct 13 14:07 services.gz
-rw-r--r--. 1 pippo pippo  99608 Oct 13 13:07 services.xz
# 文件大小又进一步缩减，压缩比又加强了

范例二：列出这个压缩文件的信息，然后读出这个压缩文件的内容
[pippo@Centos7 tmp]$ xz -l services.xz 
Strms  Blocks   Compressed Uncompressed  Ratio  Check   Filename
    1       1     97.3 KiB    654.6 KiB  0.149  CRC64   services.xz
# 可以列出这个文件的压缩前后的容量，这样观察就方便多了！
[pippo@Centos7 tmp]$ xzless services.xz

范例三：将他解压缩吧！
[pippo@Centos7 tmp]$ xz -d services.xz

范例四：保留原文件的档名，并且建立压缩文件！
[pippo@Centos7 tmp]$ xz -k services
```
&ensp;&ensp;&ensp;&ensp;虽然xz 这个压缩比真的好太多太多了！以鸟哥选择的这个services 文件为范例，他可以将gzip 压缩比(压缩后/压缩前) 的21% 更进一步优化到15% 耶！差非常非常多！不过，xz 最大的问题是...时间花太久了！如果你曾经使用过xz 的话，应该会有发现，他的运算时间真的比gzip 久很多喔！  
&ensp;&ensp;&ensp;&ensp;透过『 time [gzip|bzip2|xz] -c services > services.[gz|bz2|xz] 』去执行运算结果，结果发现这三个指令的运行时间依序是： 0.025s, 0.058s, 0.371s， 看最后一个数字！差了10 倍的时间耶！所以，如果你并不觉得时间是你的成本考虑，那么使用xz 会比较好！如果时间是你的重要成本，那么gzip 恐怕是比较适合的压缩软件喔！  
************
# 8.3 打包指令： tar
&ensp;&ensp;&ensp;&ensp;前一小节谈到的指令大多仅能针对单一文件来进行压缩，虽然gzip, bzip2, xz 也能够针对目录来进行压缩，不过， 这两个指令对目录的压缩指的是『将目录内的所有文件"分别" 进行压缩』的动作！而不像在Windows 的系统，可以使用类似WinRAR 这一类的压缩软件来将好多数据『包成一个文件』的样式。   
&ensp;&ensp;&ensp;&ensp;这种将多个文件或目录包成一个大文件的指令功能，我们可以称呼他是一种『打包指令』啦！那 Linux有没有这种打包指令呢？是有的！那就是鼎鼎大名的tar 这个玩意儿了！ tar 可以将多个目录或文件打包成一个大文件，同时还可以透过gzip/bzip2/xz 的支持，将该文件同时进行压缩！更有趣的是，由于tar 的使用太广泛了，目前Windows 的WinRAR 也支持.tar.gz 档名的解压缩呢！ 很不错吧！所以底下我们就来玩一玩这个咚咚！  
```bash
[pippo@Centos7 tmp]$ tar --help
Usage: tar [OPTION...] [FILE]...
Examples:
  tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
# 打包文件 foo 和 bar 并且创建 archive.ar
  tar -tvf archive.tar         # List all files in archive.tar verbosely.
# 显示archive.tar内的文件的详细信息
  tar -xf archive.tar          # Extract all files from archive.tar.
# 从 archive.tar 中提取所有文件
#常见选项参数
  -c, --create               create a new archive
# 建立打包文件，可搭配 -v 来察看过程中被打包的文件
  -t, --list                 list the contents of an archive
# 察看打包文件的内容含有哪些文件名，重点在察看文件名；
  -x, --extract, --get       extract files from an archive
# 解打包或解压缩的功能，可以搭配 -C (大写) 在特定目录解开，
' -c, -t, -x 不可同时出现在一串指令列中'
  -Z, --compress, --uncompress   filter the archive through compress
# 透过 compress 的支持进行压缩/解压缩：此时档名最好为 *.tar.Z
  -z, --gzip, --gunzip, --ungzip   filter the archive through gzip
# 透过 gzip 的支持进行压缩/解压缩：此时档名最好为 *.tar.gz
  -J, --xz                   filter the archive through xz
# 透过 xz 的支持进行压缩/解压缩：此时档名最好为 *.tar.xz
  -j, --bzip2                filter the archive through bzip2
# 透过 bzip2 的支持进行压缩/解压缩：此时档名最好为 *.tar.bz2
' -z, -j, -J 不可以同时出现在一串指令列中'
  -v, --verbose              verbosely list files processed
# 在压缩/解压缩的过程中，将正在处理的文件名显示出来
  -f, --file=ARCHIVE         use archive file or device ARCHIVE
# -f 后面要立刻接要被处理的档名！ -f 最好单独写一个选项
  -C, --directory=DIR        change to directory DIR
# 这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项
      --exclude=PATTERN      exclude files, given as a PATTERN
# 排除符合模式的文件
   -T, --files-from=FILE      get names to extract or create from FILE
# 从FILE中获取要操作的文件名
   -X, --exclude-from=FILE    exclude patterns listed in FILE
# 排除符合FILE中的模式的文件
   -P, --absolute-names       do not strip leading / from file names
# 打包压缩时保留最前的 / （以绝对路径来处理）
   -p, --preserve-permissions, --same-permissions
                             extract information about file permissions (default for superuser)
# 保留备份数据的原本权限与属性，常用于备份(-c)重要的配置文件
```
&ensp;&ensp;&ensp;&ensp;其实最简单的使用tar 就只要记忆底下的方式即可：  
* 压 缩：tar -j**c**v -f filename.tar.bz2 要被压缩的文件或目录名称  
* 查 询：tar -j**t**v -f filename.tar.bz2  
* 解压缩：tar -j**x**v -f filename.tar.bz2 -C 欲解压缩到的目录  
&ensp;&ensp;&ensp;&ensp;那个filename.tar.bz2 是我们自己取的档名，tar 并不会主动的产生建立的档名喔！我们要自定义啦！所以扩展名就显的很重要了！如果不加 [-z|-j|-J] 的话，档名最好取为*.tar 即可。如果是-j 选项，代表有bzip2 的支持，因此档名最好就取为*.tar.bz2 ，因为bzip2 会产生.bz2 的扩展名之故！ 至于如果是加上了-z 的gzip 的支持，那档名最好取为*.tar.gz 喔！了解乎？  
&ensp;&ensp;&ensp;&ensp;另外，由于『-f filename 』是紧接在一起的，过去很多文章常会写成『-jcvf filename』，这样是对的，但由于选项的顺序理论上是可以变换的，所以很多读者会误认为『-jvfc filename』也可以～事实上这
样会导致产生的档名变成c ！ 因为 -fc 嘛！所以啰，建议您在学习tar 时，将『-f filename 』与其他选项独立出来，会比较不容易发生问题。  
&ensp;&ensp;&ensp;&ensp;闲话少说，让我们来测试几个常用的 tar 方法吧！  
*************
* 使用tar 加入-z, -j 或-J 的参数备份/etc/ 目录  
&ensp;&ensp;&ensp;&ensp;有事没事备份一下 /etc 这个目录是件好事！备份/etc 最简单的方法就是使用tar 啰！让我们来玩玩先：  
```shell
[root@Centos7 etc]# time  tar -cp --gzip -f /data/etc.tar.gz /etc/
tar: Removing leading '/' from member names
# 备份时移除每个文件的根目录
real	0m1.373s
[root@Centos7 etc]# time  tar -cp --bzip2 -f /data/etc.tar.bz2 /etc/
tar: Removing leading '/' from member names
real	0m4.012s
[root@Centos7 etc]# time  tar -cp --xz -f /data/etc.tar.xz /etc/
tar: Removing leading '/' from member names
real	0m16.551s
[root@Centos7 etc]#ll -hS /data/etc.tar.*
-rw-r--r--. 1 root root  11M Oct 13 17:44 /data/etc.tar.gz
-rw-r--r--. 1 root root 8.9M Oct 13 17:46 /data/etc.tar.bz2
-rw-r--r--. 1 root root 7.2M Oct 13 17:47 /data/etc.tar.xz
[root@Centos7 etc]#du -sm /etc/ 
37	/etc/
# 目录 /etc 大约有37MB
```  
&ensp;&ensp;&ensp;&ensp;压缩比越好当然要花费的运算时间越多！我们从上面可以看到，虽然使用gzip 的速度相当快，总时间花费1.373 秒钟，但是压缩率最糟糕！ 如果使用 xz 的话，虽然压缩比最佳！不过竟然花了16.551秒钟的时间耶！这还仅是备份37MBytes 的/etc 而已，如果备份的数据是很大容量的， 那你真的要考虑时间成本才行！  
&ensp;&ensp;&ensp;&ensp;至于加上『-p 』这个选项的原因是为了保存原本文件的权限与属性！我们曾在第六章的cp 指令介绍时谈到权限与文件类型(例如连结档)对复制的不同影响。同样的，在备份重要的系统数据时，这些原本文件的权限需要做完整的备份比较好。此时-p 这个选项就派的上用场了。接下来让我们看
看打包文件内有什么数据存在？  
**********
* 查阅tar 文件的数据内容(可察看文件名)，与备份文件名有否根目录的意义  
要察看由 tar 所建立的打包文件内部的档名非常的简单！可以这样做：  
```shell
[root@Centos7 etc]# tar -jtv -f /data/etc.tar.bz2 |head -2
drwxr-xr-x root/root         0 2018-10-13 14:16 etc/
-rw-r--r-- root/root       595 2018-09-06 18:28 etc/fstab
```  
&ensp;&ensp;&ensp;&ensp;如果加上-v 这个选项时，详细的文件权限/属性都会被列出来！如果只是想要知道檔名而已， 那么就将-v 拿掉即可。从上面的数据我们可以发现一件很有趣的事情，那就是每个文件名都没了根目录了！这也是上一个练习中出现的那个警告讯息『tar: Removing leading '/' from member names(移除了档名开头的'/' )』所告知的情况！
&ensp;&ensp;&ensp;&ensp;那为什么要拿掉根目录呢？主要是为了安全！我们使用 tar 备份的数据可能会需要解压缩回来使用，在 tar 所记录的文件名(就是我们刚刚使用tar -jtvf 所察看到的檔名) 那就是解压缩后的实际档名。如果拿掉了根目录，假设你将备份数据在/tmp 解开，那么解压缩的档名就会变成『/tmp/etc/xxx』。但『如果没有拿掉根目录，解压缩后的档名就会是绝对路径， 亦即解压缩后的数据一定会被放置到/etc/xxx 去！』如此一来，你的原本的/etc/ 底下的数据， 就会被备份数据所覆盖过去了！  
&ensp;&ensp;&ensp;&ensp;如果你确定你就是需要备份根目录到 tar 的文件中，那可以使用-P (大写) 这个选项，请看底下的例子分析：  
```shell
范例：将文件名中的(根)目录也备份下来，并察看一下备份档的内容档名
[root@Centos7 etc]# tar -zpPc -f /data/etc.and.root.tar.gz /etc/
[root@Centos7 etc]#tar -zt -f /data/etc.and.root.tar.gz |head -2
tar: Removing leading '/' from member names
/etc/
/etc/fstab
# 这次查阅文件名不含 -v 选项，所以仅有文件名而已,没有详细属性/权限等参数
```
&ensp;&ensp;&ensp;&ensp;有发现不同点了吧？如果加上-P 选项，那么文件名内的根目录就会存在喔！不过，鸟哥个人建议，还是不要加上-P 这个选项来备份！ 毕竟很多时候，我们备份是为了要未来追踪问题用的，倒不一定需要还原回原本的系统中！ 所以拿掉根目录后，备份数据的应用会比较有弹性！也比较安全呢！  
**********
* 将备份的数据解压缩，并考虑特定目录的解压缩动作(-C 选项的应用)  
那如果想要解打包呢？很简单的动作就是直接进行解打包嘛！
```shell
[root@Centos7 data]# tar -Jx -f etc.tar.xz 
[root@Centos7 data]# ll 
......(前面省略)......
drwxr-xr-x. 136 root  root      8192 Oct 13 14:16 etc
......(后面省略)......
```
&ensp;&ensp;&ensp;&ensp;此时该打包文件会在『本目录下进行解压缩』的动作！ 所以，你等一下就会在家目录底下发现一个名为etc 的目录啰！所以啰，如果你想要将该文件在/tmp 底下解开， 可以 cd /tmp 后，再下达上述的指令即可。不过，这样好像很麻烦呢～有没有更简单的方法可以『指定欲解开的目录』呢？ 有的，可以使用-C 这个选项喔！举例来说：  
```shell
[root@Centos7 ~]# tar -zx -f /data/etc.tar.gz -C /tmp/
[root@Centos7 ~]# ll -d /tmp/*
drwxr-xr-x. 136 root root 8192 Oct 13 14:16 /tmp/etc
```
&ensp;&ensp;&ensp;&ensp;这样一来，你就能够将该文件在不同的目录解开啰！鸟哥个人是认为，这个-C 的选项务必要记忆一下的！  
**********
* 仅解开单一文件的方法
&ensp;&ensp;&ensp;&ensp;刚刚上头我们解压缩都是将整个打包文件的内容全部解开！想象一个情况，如果我只想要解开打包文件内的其中一个文件而已， 那该如何做呢？很简单的，你只要使用-jtv 找到你要的档名，然后将该档名解开即可。我们用底下的例子来说明一下：  
```shell
# 1. 先找到我们要的档名，假设解开 shadow 文件好了：
[root@Centos7 data]# tar -jtv -f etc.tar.bz2 | grep -w 'shadow'
---------- root/root      1635 2018-09-26 16:49 etc/shadow
---------- root/root      1614 2018-09-26 16:49 etc/shadow-
# 2. 将该文件解开！语法与实际作法如下：
tar -jxv -f 打包档.tar.bz2 待解开档名
[root@Centos7 data]# tar -jxv -f etc.tar.bz2 etc/shadow
etc/shadow
[root@Centos7 data]#ll etc
total 4
----------. 1 root root 1635 Sep 26 16:49 shadow
# 此时只会解开一个文件而已！不过，重点是那个档名！你要找到正确的档名。
# 在本例中，不能写成 /etc/shadow ！因为记录在 etc.tar.bz2 内的并没有 / 之故！
```
**********
* 打包某目录，但不含该目录下的某些文件之作法   
假设我们想要打包 /etc/ /root 这几个重要的目录，但却不想要打包/root/etc* 开头的文件，因为该文件都是刚刚我们才建立的备份档嘛！ 而且假设这个新的打包文件要放置成为/root/system.tar.bz2 ，当然这个文件自己不要打包自己(因为这个文件放置在/root 底下啊！)，此时我们可以透过--exclude的帮忙！ 那个 exclude 就是不包含的意思！所以你可以这样做： 
```shell
[root@Centos7 ~]# tar -jc -f /root/system.bar.bz2 --exclude=/root/etc* --exclude=/root/system.bar.bz2 /etc/ /root/
tar: Removing leading '/' from member names
```
透过这个--exclude="file" 的动作，我们可以将几个特殊的文件或目录移除在打包之列，让打包的动作变的更简便.  
*********

```shell
[root@Centos7 data]# find /etc/ -newer /etc/passwd |xargs tar -jc -f /data/etc.newer.than.passwd.tar.bz2
tar: Removing leading '/' from member names
tar: Removing leading '/' from hard link targets
[root@Centos7 data]# tar -jtv -f etc.newer.than.passwd.tar.bz2 | grep -v '/$'
# 透过这个指令可以呼叫出 tar.bz2 内的结尾非 / 的文件名
```
&ensp;&ensp;&ensp;&ensp;现在你知道这个指令的好用了吧！甚至可以进行差异文件的记录与备份呢～ 这样子的备份就会显的更容易啰！你可以这样想象，如果我在一个月前才进行过一次完整的数据备份， 那么这个月想要备份时，当然可以仅备份上个月进行备份的那个时间点之后的更新的文件即可！ 为什么呢？因为原本的文件已经有备份了嘛！干嘛还要进行一次？只要备份新数据即可。这样可以降低备份的容量啊！ 
*********
* 基本名称  tarfile tarball
&ensp;&ensp;&ensp;&ensp;另外值得一提的是，tar 打包出来的文件有没有进行压缩所得到文件称呼不同喔！如果仅是打包而已，就是『tar -cv -f file.tar 』而已，这个文件我们称呼为tarfile 。如果还有进行压缩的支持，例如『tar -jcv -f file.tar.bz2 』时，我们就称呼为tarball (tar 球？)！这只是一个基本的称谓而已，不过很多书籍与网络都会使用到这个tarball 的名称！所以得要跟您介绍介绍。  
&ensp;&ensp;&ensp;&ensp;此外，tar 除了可以将资料打包成为文件之外，还能够将文件打包到某些特别的装置去，举例来说， 磁带机(tape) 就是一个常见的例子。磁带机由于是一次性读取/写入的装置，因此我们不能够使用类似cp 等指令来复制的！ 那如果想要将 /home, /root, /etc 备份到磁带机(/dev/st0) 时，就可以使用：『tar -cv -f /dev/st0 /home /root /etc』，很简单容易吧！ 磁带机用在备份 (尤其是企业应用) 是很常见的工作喔！  
*******
* 特殊应用：利用管线命令与数据流
&ensp;&ensp;&ensp;&ensp;在 tar 的使用中，有一种方式最特殊，那就是透过标准输入输出的数据流重导向(standardinput/standard output)， 以及管线命令 (pipe) 的方式，将待处理的文件一边打包一边解压缩到目标目录去。关于数据流重导向与管线命令更详细的数据我们会在第十章bash 再跟大家介绍， 底下先来看一个例子吧！
```shell
# 将 /etc 整个目录一边打包一边在 /tmp 解开
[root@study ~]# cd /tmp
[root@study tmp]# tar -cvf - /etc | tar -xvf -
# 这个动作有点像是 cp -r /etc /tmp 
# 要注意的地方在于输出档变成 - 而输入档也变成 - ，又有一个 | 存在
# 这分别代表 standard output, standard input 与管线命令啦！
# 简单的想法中，你可以将 - 想成是在内存中的一个装置(缓冲区)
```
&ensp;&ensp;&ensp;&ensp;在上面的例子中，我们想要『将/etc 底下的资料直接copy 到目前所在的路径，也就是/tmp 底下』，但是又觉得使用cp -r 有点麻烦，那么就直接以这个打包的方式来打包，其中，指令里面的- 就是表示那个被打包的文件啦！ 由于我们不想要让中间文件存在，所以就以这一个方式来进行复制的行为。
