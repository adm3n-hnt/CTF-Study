# Web

## [极客大挑战 2019]EasySQL 1

网页环境<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269227979-fd564186-7adb-4eaf-9e3e-8424fdd19fa4.png#averageHue=%230c0c0c&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u1f8820fd&originHeight=934&originWidth=1913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569569&status=done&style=none&taskId=u5bf2ca64-b145-4ca8-9fea-76843b29330&title=&width=1530.4)<br />可以看到是一个登录窗口，题目已经说了是SQL注入<br />首先试一下弱口令，admin,123<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697269500707-e261da2b-33fa-446f-b907-e898575facae.png#averageHue=%23100909&clientId=uf24ad900-2f88-4&from=paste&height=747&id=u4b689535&originHeight=934&originWidth=1899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576337&status=done&style=none&taskId=ue42bb3eb-811a-4bb8-a5ad-0a9c898c1fb&title=&width=1519.2)<br />提示用户名密码错了<br />既然是第一道题，那应该是很简单了<br />判断是数字型还是字符型

1. 数字型

`and 1=1 回显正常`<br />`and 1=2 返回异常，存在数字型注入可能`

2. 字符型

`1 返回正常`<br />`1' 返回异常，存在字符型注入可能`<br />经过测试，发现username和password两个参数都可以分别注入和同时注入<br />构造payload<br />`url?usename=1'&password=1'`<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270426620-acb3876c-9f0d-4dad-b7b5-b7e2f50b557f.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=750&id=uac410499&originHeight=938&originWidth=1916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=578355&status=done&style=none&taskId=u9824586b-054d-4e84-998e-a4f99a53025&title=&width=1532.8)<br />回显报错，存在SQL字符型注入<br />**小试一下万能密码注入**<br />构造payload<br />`url?usename=1' or '1'='1&password=1' or '1'='1`<br />**回显flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1697270808644-23610741-6cba-4ccc-ac0d-20688612546d.png#averageHue=%230b0a0a&clientId=u50427f58-e377-4&from=paste&height=746&id=u61a5ec44&originHeight=933&originWidth=1905&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=588001&status=done&style=none&taskId=u34c8a5ee-5cfa-462d-8975-2dd088f61f5&title=&width=1524)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/36016220/1697271707206-5d837e64-976e-44fc-b710-05a0ca10dd88.jpeg)

## buuctf[极客大挑战 2019]Havefun 1

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

