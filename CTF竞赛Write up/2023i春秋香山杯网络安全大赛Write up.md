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
