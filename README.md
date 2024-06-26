MD5-algorithm-optimization
基于SecureRandom的MD5算法优化/java/大学信息安全课程设计



摘要

随着信息技术的飞速发展,人们在获得信息技术所提供的便利的同时,也面临着数据安全问题, 这时信息安全在人们生活中起着举足轻重的作用。MD5加密算法为不可逆技术,一般应用与用户注册加密,因为一般的密码可以通过输入MD5值去查找明文密码,存在一定弊端。所以本作品采用Java中的SecureRandom随机生成字符串去改变加密后的MD5值，使其即使获取到MD5值也得不到明文密码。

 
第一章 作品概述

当我们需要使用密码信息用于确认身份时，如果直接将密码信息以明码的方式保存在数据库当中，不采用任何保密措施，系统管理员就很容易能得到原来的密码信息，而这些信息一旦泄露，密码就会很容易被不法分子所破译。所以为了增加密码信息的安全性，需要对数据库中需要保密的信息进行加密，即使可以获取整个数据库，如果没有解密算法，也不能得到原来的密码信息。只有在明文相同的情况下，才能等到相同的密文。当前国内外密码技术主要分为数学和非数学密码技术。而对于MD5算法，目前已经有很多的网站针对MD5，进行了暴力枚举，通过保存的MD5值与他人的密码进行对比，从而推算出他人的账户密码。我们所设计的算法可以很好地解决这个问题，我们的算法是基于SecureRandom对MD5的算法优化，我们在MD5生成的固定输出字符串（将任意长度的字符串经过计算得到固定长度的输出）的基础上在其中间增加了随机生成的一段变长的字符串，并且这个算法是不可逆的，这样就可以把用户的密码以MD5值和随机字符串一同保存起来，在用户输入密码的时候，系统是把用户输入的密码计算成MD5值，然后再去和系统中截取出的MD5值进行比较，如果俩者相同，就可以认定密码是正确的，否则会认定密码错误。通过这样的步骤，系统在不知道用户密码明码的情况下就可以确定用户登录系统账号的合法性。这样不但可以避免用户的密码类似被具有系统管理员权限的用户知道，而且还在一定程度上增加了密码被破解的难度,提高了密码的安全性.



 
第二章 作品设计与实现                              

1.	系统设计方案
为了防止通过获取MD5值得到明文密码，我们可以通过如下步骤实现改进：
1)	用户输入密码，得到明文，通过Java对明文进行MD5加密，得到暗文。
2)	将加密后的暗文，通过Java中的字符串截取子串，将暗文分为两半。
3)	随机生成长度为n的字符串，n由该暗文的第一次出现数字的位置所确定。
4)	将前半暗文和生成的随机字符串进行拼接，再将其与后半的暗文进行拼接，得到新的暗文，将其存放的相应数据库中。

2.	实现原理
1)	通过Java实现MD5加密。Java中的MessageDigest类，实现信息摘要算法的功能，可以接收任意大小的数据,并输出固定长度的Hash值。从而实现对明文进行MD5加密处理。其中：
a)	getInstance()方法，返回实现指定摘要算法的MessageDigest对象。
b)	digest()方法，对于给定数量的更新数据 ,digest() 方法只能被调用一次.在调用digest() 方法之后,MessageDigest对象被重新设置成初始状态。
c)	update() 方法，MessageDigest对象在开始时会被初始化，对象通过调用update() 方法处理数据。
2)	字符串截取。Java的字符串截取子串有很多，这里用substring()方法，其中substring(int begin,int end)是截取索引从begin到end-1形成新的子串。
3)	生成随机字符串。通过Java的随机生成数，可以实现随机字符串。其中生成随机数的类有很多，这里使用SecureRandom类，因为提供加密的强随机数生成器，收集了一些随机事件，比如鼠标点击，键盘点击等等，而不是仅仅默认使用系统当前时间的毫秒数作为种子，有规律可寻。 
4)	字符串连接。可以直接用”+”(字符串连接符)将前后半暗文和随机生成的字符串进行。

3.	算法流程图

![image](https://github.com/HarveyJI/MD5-algorithm-optimization/assets/78439035/6639e778-4f9e-432a-9253-43d3bcaf748c)


4.	实现功能
1)	优化了MD5算法,即使获取MD5值也不能通过查找相关映射数据库得到明文密码。
2)	计算速度快。加密速度快，不需要秘钥。
3)	检查文件的完整性，一旦文件被更改，MD5值也是不同的。
4)	防止看到明文，存放的是MD5值。
5)	防止抵赖，用于数字签名。

5.	评价指标
1）	压缩性: 对于任意长度的输入, 输出长度总是相同的。
2）	容易计算: 线性时间复杂度。
3）	抗修改性: 对原数据的一点点修改都会导致最终结果的巨大变化。
4）	抗碰撞性: 已知原数据和MD5值很难生成与原数据不同但MD5值相同的数据。
5）	更难暴力破解。 




第三章 作品测试与分析

1.	测试方案
用自己电脑ecplise环境，运行输入明文，运行程序得出暗文，去有关MD5在线破解网站去试着查询明文。

2.	测试环境
Java环境：ecplise
对抗网站：www.somd5.com

3.	测试数据:
明文:2042159112
暗文: 76e331o8lLJKjM361a405ef683b0b3a1b54c4fcf

4.	结果分析
1)	因为改变了明文加密后的MD5值，故通过原来的错误的MD5值找不到与值对应的明文密码或错误的明文密码。
2)	因为不同的明文加密后不仅MD5值不同，且长度不固定，大大提高的暴力破解的难度。

![image](https://github.com/HarveyJI/MD5-algorithm-optimization/assets/78439035/6765f4c8-937b-4301-ad78-c3b2f6fce88b)

