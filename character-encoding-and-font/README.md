[TOC]

# 1. 字符编码的历史

## 1.1 背景知识

1.  在计算机中，使用0和1的序列来存储字符.

2.  世界有很多种语言，各个语言使用的字符基本上各不相同。

3.  字符集(Character Set)，顾名思义，即字符的集合
     ​	有的字符集很小，如ASCII字符集，只包含128个有效字符.
     ​	有的字符集很大，大到足够包含一个国家或文化所需的所有字符，如中国的GB2312字符集,GBK字符集等，日本的JIS字符集等
     ​	有的字符集超大，如Unicode字符集，计划包含全世界所有的字符

4.  任何一个字符集都必须包含以下两条信息：
     ​	规定该字符集包含了哪些字符
     ​	确定每个字符在这个字符集中的什么位置

5.  将字符集中的字符一一映射到01序列的过程，称为编码
     ​	有的字符集已经自带编码方案,如ASCII字符集，GB2312字符集等
     ​	有的字符集本身不指定编码方案,允许存在多种不同的编码方案来编码该字符集。像UTF-8，UTF-16，UTF-32等，都是常见的Unicode字符集编码方案.

6.  代码页(Code Page)是字符集编码的具体实现，是操作系统内的"字符-01序列"映射表，通过查询某个字符集的Code Page，操作系统可以实现"字符-字节"的编码和解码。
     操作系统中内置了多种代码页,如Windows使用936代码页来解码GBK字符集。每个代码页中可能包含一张或多张码表,这些码表中记录了该字符集中的字符对应的二进制编码，操作系统根据当前使用的代码页，将一个二进制序列在该代码页进行查找，解码为对应的字符。或将一个字符编码为对应的二进制序列



## 1.2 起源

### 	1.2.1 ASCII字符集

由于计算机是美国人发明的，因此，最早只有128个字符被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为`ASCII`编码

ASCII码是一个自带编码方案的字符集，其编码特点如下：
-   一共包含128个字符，包含33个控制字符和95个可打印字符。(顺便说一下，空格属于可打印字符)
-   它是一个单字节编码系统。每个字符编码后的长度为一个字节,即8bit。
-   ASCII字符集的编码方案很简单，将字符在ASCII字符集中的位置转换为二进制,并在高位补0即可。如大写字母A在ASCII字符集中的位置是65，在计算机中将会编码为 0100 0001 .
-   ASCII字符集的编码方案理论上可以表示256个字符，但实际只使用了128个，最高位为1的128个编码没有对应的字符。

附： [ASCII字符集的编码表在线查询](http://tool.oschina.net/commons?type=4)

​   

## 1.3 混乱

​ 	世界各地都开始使用计算机了，但是很多国家用的不是英文，他们的文字和符号里有许多是ASCII里没有的，为了在计算机保存他们的字符，他们需要创造新的字符集，来包含他们自己的字符。

### 	1.3.1 欧洲 - 拓展的ASCII字符集

​	对于一些使用拉丁字母的国家来说，他们需要在ASCII字符集的基础上补充的字符并不太多，而由于ASCII字符还剩128个未编码的空间，因此很多国家决定在ASCII字符集的基础上进行拓展，即保持前128个字符不变，在后128个位置上加入自己需要的字符。从128到255这些字符集被称“扩展字符集”。

​	对于具体在这后128个位置上补充哪些字符，不同的国家有不同的想法，基于此，ISO 组织在ASCII码基础上又制定了一系列标准用来扩展ASCII编码，它们是ISO-8859-1 ~ ISO-8859-15。其中ISO-8859-1（又称Latin-1）涵盖了大多数西欧语言字符，所有应用的最广泛。

​	这些字符集的特点是前128位基本与ASCII字符集相同，但是后128位却各不一致。

### 	1.3.2 中国 - 中文GB系列

#### 		1.3.2.1 GB2312

​	GB 2312 或 GB 2312-80 是中国国家标准简体中文字符集，又称GB0，由中国国家标准总局发布，1981年5月1日实施。
​	GB2312编码通行于中国大陆；新加坡等地也采用此编码。中国大陆几乎所有的中文系统和国际化的软件都支持GB 2312。
​	GB 2312标准共收录6763个汉字，其中一级汉字3755个，二级汉字3008个；同时收录了包括拉丁字母、希腊字母、日文平假名及片假名字母、俄语西里尔字母在内的682个字符。
​	GB 2312的出现，基本满足了汉字的计算机处理需要，它所收录的汉字已经覆盖中国大陆99.75%的使用频率。
​	对于人名、古汉语等方面出现的罕用字，GB 2312不能处理(如'镕'等)，这导致了后来GBK及GB 18030汉字字符集的出现。

​	附： [GB2312字符码表速查](http://www.knowsky.com/resource/gb2312tbl.htm)

##### 1.3.2.1.1 GB2312中的字符及其位置

​	GB2312编码对所收录字符进行了"分区"处理，共94个区，每区又包含94个位，因此共8836个码位。因此GB2312理论上可以包含8836个字符(实际上为7445个字符)。每个字符的位置通过它的区位码来描述。

-   01-09区收录除汉字外的682个字符，如一些全角数字，全角字母，日文字符等 (以及164个未使用的空码位)
-   10-15区为空白区，没有使用
-   16-55区收录3755个一级汉字，按拼音排序。(16区-54区每个码位上都有字符，55区的最后5个码位无字符。)
-   56-87区收录3008个二级汉字，按部首/笔画排序。(每个码位都有字符)
-   88-94区为空白区，没有使用。

      举例来说，

      " "(注:空格)是GB2312编码中的第一个字符，它位于01区01位，区位码即为01,01
      "啊"字是GB2312编码中的第一个汉字，它位于16区的01位，区位码就是16,01
      "齄"字是GB2312编码中的最后一个字，也是最后一个汉字，它位于87区的94位，区位码是87,94

##### 1.3.2.1.2 GB2312字符集的编码方案

​	GB2312严格来讲只是一个字符集，由于其使用的 EUC-CN 编码方案已经成为事实上的标准，因此，在提及GB2312时，也可以像ASCII字符集那样理解成自带编码方案的字符集。

​	GB2312字符集的(EUC-CN)编码方案如下：

1.  第一步：首先，找到该字符在字符集中的位置，即区位码。区位码使用两个字节来表示，第一个字节用来表示其所在的区，第二个字节用来表示其所在的位。GB2312中字符理论上的区位码范围是：01,01－94,94。

2.  第二步：将该字符的区字节和位字节分别加上**0xA0**，得到一个新的双字节，即为该字符的GB2312编码。

     例如上述的“啊”字符，在字符集中的位置为16区01位，区位码为16,01，区字节为0x10,位字节为0x01,分别加上0xA0：

     0x10 + 0xA0 = oxB0

     0x01+ 0xA0 = 0xA1

     即可得到'啊'的GB2312编码为B0A1.  同理，"齄"的GB2312编码为F7FE.

##### 1.3.2.1.3 GB2312与ASCII的关系

-   GB2312字符集是对ASCII字符集的一种拓展,因此，和1.3.1中的各种欧洲的拓展ASCII字符集一样，GB2312字符集是不包含任何ASCII字符集中的字符的。

-   同时，GB2312的编码方式也决定了，GB2312是兼容ASCII字符集的。具体分析如下：

      ​对于任何一个GB2312字符，其编码后的区字节和位字节，都是大于0xA0的，即两个字节的最高位肯定都是1，因此，当我们解码一串使用GB2312编码的二进制流时，可以通过检查当前字节的最高位是否为1来区分该字节属于GB2312的字符还是ASCII的字符：

    -   如果当前字节的最高位为0，则直接将当前字节解码为ASCII字符；
    -   如果当前字节的最高位为1，则接着判断下一个字节的最高位是否也是1，如果也为1，则把这两个字节连起来，解码为GB2312中的字符。理论上，如果前面的内容解码正常，那么不会存在当前字节最高位为1，而下一个字节的最高位却为0的情况，这也是后来的GBK字符集对GB2312字符集再次进行拓展的一个基础。
    -   由上可知，在使用gb2312编码的文本中，英文字母和数字(即ASCII中的字符)只占一个字节，其他的字符均占两个字节。这也是所谓的"英文一个字节，中文两个字节"的由来。

##### 1.3.2.1.4 总结

​	GB2312是一种自带编码方案的字符集。
​	GB2312为双字节编码,对其所包含的任意一个字符，编码后的长度都为两个字节。
​	GB2312的实际编码范围: A1A1-F7FE ，共 7445个字符
​	GB2312中的汉字编码范围为 B0A1-F7FE ，共6763个汉字

​	GB2312有一些缺陷，如不包含繁体字，缺少少数民族的字符，包含的简体汉字也并不全，甚至像"镕"这样的简体字也没有包含，这就会让某些国家领导人感到尴尬了，为了解决这些问题，人们在GB2312的基础上扩展出了GBK字符集。

#### 		1.3.2.2 GBK	

​	GB2312是对ASCII字符集的扩展，GBK则是对GB2312字符集的扩展，向下完全兼容GB2312。同时GBK收录了Unicode基本多文种平面中的所有CJK汉字。GBK还收录了GB2312不包含的汉字部首符号、竖排标点符号等字符。

​	GBK和GB2312都是双字节等宽编码

​	GBK编码方案不再要求低字节一定是127号之后的内码，只要第一个字节是大于127就固定表示这是一个汉字的开始，不管后面跟的是不是扩展字 符集里的内容
​	GBK的具体编码规则如下：//TODO

#### 		1.3.2.3 GB18030

​	2000年的GB18030是取代GBK1.0的正式国家标准。GB18030编码向下兼容GBK和GB2312。
​	GB18030收录了所有Unicode3.1中的字符，包括中国少数民族字符。GB18030虽然是国家标准，但是实际应用系统中使用的并不广泛。目前使用最广的仍是GBK编码。

​	GB18030编码是变长编码，采用单字节、双字节和4字节方案，其中单字节、双字节和GBK是完全兼容的，4字节编码的码位就是收录了CJK扩展A的6582个汉字。

​	从ASCII、GB2312、GBK到GB18030，这些编码方法是向下兼容的，即同一个字符在这些方案中总是有相同的编码，后面的标准支持更多的字符。在这些编码中，英文和中文可以统一地处理，区分中文编码的方法是高字节的最高位不为0。GB2312、GBK都属于双字节字符集 (DBCS)。

## 1.4统一

不管是欧洲的ISO-8859系列的字符集，还是中国的GB系列的字符集，都属于地区性的编码方案，这些编码方案虽然都可以兼容ASCII字符集，但是互相之间却不能兼容，

### 1.4.1 Unicode字符集



# 2. 编码的常见应用场景

## 2.1 文本文件(硬盘上)

## 2.2 程序运行时(内存中)

## 2.3 操作系统

## 2.4 终端



# 3. PHP中的编码相关函数



# 4. 字符编码的典型问题

## 4.1 记事本中的ANSI,Unicode,UTF-8是什么

## 4.2 乱码的产生

## 4.3 给定一个编码,如何反推它可能的编码方式



# 5. 常用的字符编码相关工具



# 6. 参考链接





