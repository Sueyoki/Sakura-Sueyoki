---
title: pip不是外部或内部命令以及python没反应问题
author: Sueyoki
avatar: https://cdn.jsdelivr.net/gh/Sueyoki/CDN@master/img/custom/avatar.png
authorLink: hojun.cn
authorAbout: 一只古灵古灵的精怪
authorDesc: 寒鸦向暖，枯木逢春~
categories: 随想
date: 2021-08-22 22:16:01
comments: true
tags: 
 - web
 - 悦读
keywords: Sakura
description: pip不是外部或内部命令以及python没反应问题
photos: https://cdn.jsdelivr.net/gh/Sueyoki/CDN@1.2/img/cover/(7).jpg.webp
---



# pip不是外部或内部命令以及python没反应问题



不知道发生了什么python崩溃了

大概形容就是，在cmd下输入

python  //没输出也没反应

pip   //pip不是外部或内部命令

一般是环境变量坏了，具体原因不知道为什么坏了，重新配叭~

win10搜索可以直接搜 环境变量>>点编辑系统环境变量

![image-20210823222210252](https://gitee.com/Sueyoki/ctf/raw/master/202108232222003.png)

这里上下有两个path，虽然系统环境变量的优先级高一点，但还是建议两个都改一下

![image-20210823222423139](https://gitee.com/Sueyoki/ctf/raw/master/202108232224907.png)



你要双击那个path，进去以后点新建

![image-20210823222635510](https://gitee.com/Sueyoki/ctf/raw/master/202108232226906.png)

最下面的两个分别是我的pip路径和python3.8路径

D:\f盘\python\pycharm\PyCharm Community Edition 2020.1.2\help\python3.8\Scripts  //这是pip

D:\f盘\python\pycharm\pycharm community edition 2020.1.2\help\python3.8\              //这是python

把这两条都加进系统变量里的path和用户变量里的path就好用了

## 关于pip报错：Failed building wheel for ...  

## 在Github直接下载相关python库安装

![image-20210823223238130](https://gitee.com/Sueyoki/ctf/raw/master/202108232232930.png)

点击download zip

我这里下好直接放桌面解压了

![image-20210823223520748](https://gitee.com/Sueyoki/ctf/raw/master/202108232235810.png)

打开cmd，输入

cd C:\Users\苏圆琪\Desktop\pyunit-prime-master\pyunit-prime-master

取决于你的解压目录，然后

```
python setup.py install
```

![image-20210823223753923](https://gitee.com/Sueyoki/ctf/raw/master/202108232238910.png)

安装就成功了



PS: 吐槽一下，这个pyUnit-Prime库死活装不上，setup.py里面东西不对，懒得给作者发邮件，大素数生成抄了个别的算法，这是个128位素数生成器

https://www.cnblogs.com/Withinlover/p/13914000.html

```python
from random import randint
import time

def miller_rabin(p):
    if p == 1: return False
    if p == 2: return True
    if p % 2 == 0: return False
    m, k, = p - 1, 0
    while m % 2 == 0:
        m, k = m // 2, k + 1
    a = randint(2, p - 1)
    x = pow(a, m, p)
    if x == 1 or x == p - 1: return True
    while k > 1:
        x = pow(x, 2, p)
        if x == 1: return False
        if x == p - 1: return True
        k = k - 1
    return False

def is_prime(p, r = 40):
    for i in range(r):
        if miller_rabin(p) == False:
            return False
    return True

if __name__ == '__main__':
    t=1
    T = time.perf_counter()
    for _ in range(2):  #其中’_’ 是一个循环标志，也可以用i，j 等其他字母代替，下面的循环中不会用到，这里是循环2次
        index = 128
        print(index, "位质数: ", end="")
        num = 0
        for i in range(index):
            num = num * 2 + randint(0, 1) #随机生成0-1之间int

        while is_prime(num) == False:
            num = num + 1
        print(num)
        if t==1 :
            p=num
            t=t-1
        else:
            q=num

        print("----------------------------")
    print("用时：", time.perf_counter() - T)
    print("n=",p*q)

```

这里顺便生成了一个rsa算法公私钥验证，奇奇怪怪的东西混进来了hhh~

```python
import gmpy2
c=12051279450352771 #c要比n小
e=65537
n=8308362313255798397
p=4042430201
q=2055288997
d = gmpy2.invert(e, (p-1)*(q-1))
m=pow(c,d,n)
print(m)
cipher=pow(m,e,n)
print(cipher)
```

![image-20210823234655004](https://gitee.com/Sueyoki/ctf/raw/master/202108232346604.png)