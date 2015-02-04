---
layout: post
title: "solution of poj 1001 Exponentiation"
description: "Here is the solution of poj 1001 Exponentiation. It describes my train of thought of solving the problem of multiplication of googol."
keywords: "googol multiplication、 大数乘法、 poj 1001、 exponentiation"
category: Algorithm
tags: [googol multiplication, 大数乘法, poj 1001, exponentiation, c++, algorithm]
---

{% include JB/setup %}

Link of this problem: [POJ 1001 Exponentiation](http://poj.org/problem?id=1001)

####Problem:

											1001  Exponentiation
								Time Limit: 500MS		Memory Limit: 10000K

<!-- more -->
								
####Description

	Problems involving the computation of exact values of very large magnitude and precision are common. For example, the computation of the national debt is a taxing experience for many computer systems. 

	This problem requires that you write a program to compute the exact value of Rn where R is a real number ( 0.0 < R < 99.999 ) and n is an integer such that 0 < n <= 25.

####Input

	The input will consist of a set of pairs of values for R and n. The R value will occupy columns 1 through 6, and the n value will be in columns 8 and 9.

####Output

	The output will consist of one line for each line of input giving the exact value of R^n. Leading zeros should be suppressed in the output. Insignificant trailing zeros must not be printed. Don't print the decimal point if the result is an integer.

####Sample Input

	95.123 12
	0.4321 20
	5.1234 15
	6.7592  9
	98.999 10
	1.0100 12

####Sample Output

	548815620517731830194541.899025343415715973535967221869852721
	.00000005148554641076956121994511276767154838481760200726351203835429763013462401
	43992025569.928573701266488041146654993318703707511666295476720493953024
	29448126.764121021618164430206909037173276672
	90429072743629540498.107596019456651774561044010001
	1.126825030131969720661201


##我的思路

	一拿到这个问题，我抱着侥幸的心理试了试直接使用double类型来计算，不过结果也就不用说，当然是无法达到目的。
	既然如此，就必须使用大数(googol)。

	而带着小数点的大数我在之前也没接触过，顿时我就犯了难，顿觉无爱，顿觉好忧桑。
	功夫不负有心人，我查阅了一些资料后发现小数点其实很好解决，那就是用个变量来保存它的位置。

	我是这样处理的：有多少位小数，那个位置的值就是多少。


	小数点问题有思路后，我首先考虑的就是效率问题了。如何做才能稳准狠地计算出结果呢？


	一想到要做大数的幂运算，大多数人首先先想到的肯定是要作很多乘法运算了，这无疑就大大降低了效率，而像我这样有着些许强迫症的人是不会允许这样的情况出现的，除非实在没办法。

	我们如何提高效率呢？

	可很明显我这样说就代表了我有个较好的化的计算方法，在这里和大家分享一下，这也是本程序的亮点之一，同时若道友有更好的优化方案，希望指出，感激不尽。

	比如说，我们要计算 R^n, 常规思路是作 n-1 次乘法，优化思路就是将它作如下拆解:

		 			   R^5 = R^1 * R^4,
		 			   R^9 = R^1 * R^8,
		 			   R^7 = R^1 * R^2 * R^4
			 		   ………………

	敏感一些的同学可能就会发现，这样的拆解就类似于用少数有限个二进制数就可以表示某一区间中任意值，如 0000 0001, 0000 0010, 0000 0100, 0000 1000, 0001 0000, ……, 1000 0000 这8个二进制就可以表示 [0, 2^8) 中的任意非负整型值。

	回到题目，由以上分析我们可以感觉到，要作指数区间为(0, 25] 的幂运算，我们只需先计算好 R^1, R^2, R^4, R^8, R^16，由这几个数，我们就可以通过很少次数的乘法运算来快速计算出高指数的幂了，如 R^25 = R^1 * R^8 * R^16, 原本需要计算24次乘法，现在只需作 （2 + 构造这一数组所需次数）的少数运算。

	那么构造这一基数组是否需要很多步骤？当然不会，我直接写都能写出来：
			R^2 = R^1 * R^1,
			R^4 = R^2 * R^2,
			R^8 = R^4 * R^4,
			R^16 = R^8 * R^8
	最多仅需4次乘法运算。

	相信经过以上的优化，时间复杂度将会降低不少。



	另外还要说的一点就是最基础的大数乘法，我这有两种思路：一种就是先写个加法，然后写个乘法，因为乘法要用到加法；
												  而另一种呢，就是直接写乘法。

	下面对两者进行比较：

	这两个思路的基本思路都是使用竖式计算。

	前者：将乘法与加法分开，优点是便于书写，便于想；缺点是乘法进行一次遍历，加法在每次作乘时还要进行遍历，无形间降低了效率。

	后者：将乘法与加法结合，优点是只用遍历乘法，在作乘的过程中夹杂加法，将二者结合，当然所谓夹杂不是简单的将加法函数里边的代码拷贝到乘法中，具体情况见稍后贴出的代码；缺点就很显然的是，比较难想，不益于书写，写时思路很容易乱，当然这个缺点对一些道友来说也是优点，因为利于学习。


##乘加分离的乘法运算

	思路图：

![method of separation](/assets/images/poj1001/method_of_separation.png)

	代码：

<script src="https://gist.github.com/gustfu/cd606b2414dbf1e6d295.js"></script>

	结果：

![result_of_separation](/assets/images/poj1001/result_of_separation.png)

##乘加结合的乘法运算
	思路图：

![method of combination](/assets/images/poj1001/method_of_combination.png)

	代码:

<script src="https://gist.github.com/gustfu/41ed9e2c420580e3c95a.js"></script>

	结果：

![result_of_separation](/assets/images/poj1001/result_of_combination.png)

##完整代码

[i][color=#008800]001 [/color][/i][color=#008080]#include <iostream>[/color]
[i][color=#008800]002 [/color][/i][color=#008080]#include <vector>[/color]
[i][color=#008800]003 [/color][/i][color=#008080]#include <string>[/color]
[i][color=#008800]004 [/color][/i][color=#008080]#include <algorithm>[/color]
[color=#f810b0]005 [/color][b][color=#000080]using[/color][/b] [b][color=#000080]namespace[/color][/b] [color=#000000]std[/color];
[i][color=#008800]006 [/color][/i]
[i][color=#008800]007 [/color][/i][color=#008080]#define SEPARATION 1[/color]
[i][color=#008800]008 [/color][/i]
[i][color=#008800]009 [/color][/i][b][color=#000080]class[/color][/b] [color=#000000]BigFloat[/color]
[color=#f810b0]010 [/color][color=#000000]{[/color]
[i][color=#008800]011 [/color][/i][b][color=#000080]private[/color][/b][color=#000000]:[/color]
[i][color=#008800]012 [/color][/i]    [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>[/color] [color=#000000]value[/color]; [i][color=#008800]//数字部分[/color][/i]
[i][color=#008800]013 [/color][/i]    [b][color=#000080]unsigned[/color][/b] [b][color=#000080]int[/color][/b] [color=#000000]pos_decimalPoint[/color]; [i][color=#008800]//小数点位置[/color][/i]
[i][color=#008800]014 [/color][/i]
[color=#f810b0]015 [/color][b][color=#000080]public[/color][/b][color=#000000]:[/color]
[i][color=#008800]016 [/color][/i]    [color=#000000]BigFloat[/color]() [color=#000000]:[/color] [color=#000000]pos_decimalPoint[/color]([color=#0000ff]0[/color]) [color=#000000]{};[/color]
[i][color=#008800]017 [/color][/i]    [b][color=#000080]inline[/color][/b] [color=#000000]BigFloat[/color]([color=#000000]string[/color] [color=#000000]r[/color]); [i][color=#008800]// 初始化 result, 小数点位置为 输入的 R 所在的下标， 无小数点则为 0[/color][/i]
[i][color=#008800]018 [/color][/i]
[i][color=#008800]019 [/color][/i][color=#008080]#if SEPARATION[/color]
[color=#f810b0]020 [/color]    [b][color=#000080]inline[/color][/b] [color=#000000]BigFloat[/color] [b][color=#000080]operator[/color][/b][color=#000000]+=[/color]([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>[/color] [color=#000000]&[/color][color=#000000]other[/color]);
[i][color=#008800]021 [/color][/i]    [b][color=#000080]inline[/color][/b] [color=#000000]BigFloat[/color] [b][color=#000080]operator[/color][/b][color=#000000]*[/color]([color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]other[/color]);
[i][color=#008800]022 [/color][/i][color=#008080]#else[/color]
[i][color=#008800]023 [/color][/i]    [b][color=#000080]inline[/color][/b] [color=#000000]BigFloat[/color] [b][color=#000080]operator[/color][/b] [color=#000000]*[/color] ([color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]bf[/color]);
[i][color=#008800]024 [/color][/i][color=#008080]#endif[/color]
[color=#f810b0]025 [/color]    [b][color=#000080]inline[/color][/b] [color=#000000]BigFloat[/color] [b][color=#000080]operator[/color][/b] [color=#000000]=[/color] ([color=#000000]BigFloat[/color] [color=#000000]bf[/color]);
[i][color=#008800]026 [/color][/i]    [b][color=#000080]friend[/color][/b] [color=#000000]ostream[/color] [color=#000000]&[/color] [b][color=#000080]operator[/color][/b] [color=#000000]<<[/color] ([color=#000000]ostream[/color] [color=#000000]&[/color][color=#000000]output[/color][color=#000000],[/color] [color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]bf[/color]);
[i][color=#008800]027 [/color][/i][color=#000000]};[/color]
[i][color=#008800]028 [/color][/i]
[i][color=#008800]029 [/color][/i][b][color=#000080]int[/color][/b] [color=#000000]main[/color]()
[color=#f810b0]030 [/color][color=#000000]{[/color]
[i][color=#008800]031 [/color][/i]    [b][color=#000080]unsigned[/color][/b] [b][color=#000080]int[/color][/b] n; [i][color=#008800]//指数[/color][/i]
[i][color=#008800]032 [/color][/i]    [color=#000000]string[/color] [color=#000000]baseNum[/color]; [i][color=#008800]//基数[/color][/i]
[i][color=#008800]033 [/color][/i]
[i][color=#008800]034 [/color][/i]    [b][color=#000080]while[/color][/b] ([color=#000000]cin[/color] [color=#000000]>>[/color] [color=#000000]baseNum[/color] [color=#000000]>>[/color] n)
[color=#f810b0]035 [/color]    [color=#000000]{[/color]
[i][color=#008800]036 [/color][/i]        [color=#000000]vector[/color][color=#000000]<[/color][color=#000000]BigFloat[/color][color=#000000]>[/color] [color=#000000]seqOfBigFloat[/color]; [i][color=#008800]// 构造数列存储 R^1, R^2, R^4, R^8 ~~~ 采用优化的乘法[/color][/i]
[i][color=#008800]037 [/color][/i]        [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]bool[/color][/b][color=#000000]>[/color] [color=#000000]flag[/color]; [i][color=#008800]// 判断某位置是否需要 seqOfBigFloat 中对应位置的数[/color][/i]
[i][color=#008800]038 [/color][/i]
[i][color=#008800]039 [/color][/i]        [color=#000000]seqOfBigFloat[/color][color=#000000].[/color][color=#000000]push_back[/color]([color=#000000]BigFloat[/color]([color=#000000]baseNum[/color]));
[color=#f810b0]040 [/color]        [b][color=#000080]do[/color][/b]
[i][color=#008800]041 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]042 [/color][/i]            [color=#000000]flag[/color][color=#000000].[/color][color=#000000]push_back[/color](n [color=#000000]&[/color] [color=#0000ff]1[/color]);
[i][color=#008800]043 [/color][/i]        [color=#000000]}[/color] [b][color=#000080]while[/color][/b] (n [color=#000000]>>=[/color] [color=#0000ff]1[/color]);
[i][color=#008800]044 [/color][/i]
[color=#f810b0]045 [/color]        [i][color=#008800]// 将数列存入 seqOfBigFloat[/color][/i]
[i][color=#008800]046 [/color][/i]        [b][color=#000080]unsigned[/color][/b] [b][color=#000080]int[/color][/b] [color=#000000]sizeOfFlag[/color] [color=#000000]=[/color] [color=#000000]flag[/color][color=#000000].[/color][color=#000000]size[/color]();
[i][color=#008800]047 [/color][/i]        [b][color=#000080]for[/color][/b] ([b][color=#000080]unsigned[/color][/b] [color=#000000]i[/color] [color=#000000]=[/color] [color=#0000ff]0[/color]; [color=#000000]i[/color] [color=#000000]<[/color] [color=#000000]sizeOfFlag[/color] [color=#000000]-[/color] [color=#0000ff]1[/color]; [color=#000000]i[/color][color=#000000]++[/color])
[i][color=#008800]048 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]049 [/color][/i]            [color=#000000]seqOfBigFloat[/color][color=#000000].[/color][color=#000000]push_back[/color]([color=#000000]seqOfBigFloat[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]][/color] [color=#000000]*[/color] [color=#000000]seqOfBigFloat[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]]);[/color]
[color=#f810b0]050 [/color]        [color=#000000]}[/color]
[i][color=#008800]051 [/color][/i]
[i][color=#008800]052 [/color][/i]        [i][color=#008800]// 计算[/color][/i]
[i][color=#008800]053 [/color][/i]        [color=#000000]BigFloat[/color] [color=#000000]result[/color]([color=#0000ff]"1"[/color]);
[i][color=#008800]054 [/color][/i]        [b][color=#000080]for[/color][/b] ([b][color=#000080]unsigned[/color][/b] [color=#000000]i[/color] [color=#000000]=[/color] [color=#0000ff]0[/color]; [color=#000000]i[/color] [color=#000000]<[/color] [color=#000000]sizeOfFlag[/color]; [color=#000000]i[/color][color=#000000]++[/color])
[color=#f810b0]055 [/color]        [color=#000000]{[/color]
[i][color=#008800]056 [/color][/i]            [b][color=#000080]if[/color][/b] ([color=#000000]flag[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]])[/color]
[i][color=#008800]057 [/color][/i]            [color=#000000]{[/color]
[i][color=#008800]058 [/color][/i]                [color=#000000]result[/color] [color=#000000]=[/color] [color=#000000]result[/color] [color=#000000]*[/color] [color=#000000]seqOfBigFloat[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]];[/color]
[i][color=#008800]059 [/color][/i]            [color=#000000]}[/color]
[color=#f810b0]060 [/color]        [color=#000000]}[/color]
[i][color=#008800]061 [/color][/i]
[i][color=#008800]062 [/color][/i]        [i][color=#008800]// 输出[/color][/i]
[i][color=#008800]063 [/color][/i]        [color=#000000]cout[/color] [color=#000000]<<[/color] [color=#000000]result[/color] [color=#000000]<<[/color] [color=#000000]endl[/color];
[i][color=#008800]064 [/color][/i]
[color=#f810b0]065 [/color]    [color=#000000]}[/color]
[i][color=#008800]066 [/color][/i]
[i][color=#008800]067 [/color][/i]    [b][color=#000080]return[/color][/b] [color=#0000ff]0[/color];
[i][color=#008800]068 [/color][/i][color=#000000]}[/color]
[i][color=#008800]069 [/color][/i]
[color=#f810b0]070 [/color][color=#000000]BigFloat[/color][color=#000000]::[/color][color=#000000]BigFloat[/color]([color=#000000]string[/color] [color=#000000]r[/color])
[i][color=#008800]071 [/color][/i][color=#000000]{[/color]
[i][color=#008800]072 [/color][/i]    [color=#000000]pos_decimalPoint[/color] [color=#000000]=[/color] [color=#0000ff]0[/color];
[i][color=#008800]073 [/color][/i]
[i][color=#008800]074 [/color][/i]    [i][color=#008800]//放入数值，并确定小数点的位置[/color][/i]
[color=#f810b0]075 [/color]    [b][color=#000080]for[/color][/b] ([b][color=#000080]unsigned[/color][/b] [color=#000000]i[/color] [color=#000000]=[/color] [color=#0000ff]0[/color][color=#000000],[/color] [color=#000000]lengthOfR[/color] [color=#000000]=[/color] [color=#000000]r[/color][color=#000000].[/color][color=#000000]length[/color](); [color=#000000]i[/color] [color=#000000]<[/color] [color=#000000]lengthOfR[/color]; [color=#000000]i[/color][color=#000000]++[/color])
[i][color=#008800]076 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]077 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]r[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]][/color] [color=#000000]==[/color] [color=#800080]'.'[/color])
[i][color=#008800]078 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]079 [/color][/i]            [color=#000000]pos_decimalPoint[/color] [color=#000000]=[/color] [color=#000000]lengthOfR[/color] [color=#000000]-[/color] [color=#000000]i[/color] [color=#000000]-[/color] [color=#0000ff]1[/color];
[color=#f810b0]080 [/color]        [color=#000000]}[/color]
[i][color=#008800]081 [/color][/i]        [b][color=#000080]else[/color][/b]
[i][color=#008800]082 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]083 [/color][/i]            [color=#000000]value[/color][color=#000000].[/color][color=#000000]push_back[/color]([color=#000000]r[/color][color=#000000][[/color][color=#000000]i[/color][color=#000000]][/color] [color=#000000]-[/color] [color=#800080]'0'[/color]);
[i][color=#008800]084 [/color][/i]        [color=#000000]}[/color]
[color=#f810b0]085 [/color]    [color=#000000]}[/color]
[i][color=#008800]086 [/color][/i]
[i][color=#008800]087 [/color][/i]    [i][color=#008800]//去掉 leading 0[/color][/i]
[i][color=#008800]088 [/color][/i]    [b][color=#000080]for[/color][/b] ([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]iterator[/color] [color=#000000]it[/color] [color=#000000]=[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color](); [color=#000000]it[/color] [color=#000000]!=[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]end[/color](); [color=#000000]++[/color][color=#000000]it[/color])
[i][color=#008800]089 [/color][/i]    [color=#000000]{[/color]
[color=#f810b0]090 [/color]        [b][color=#000080]if[/color][/b] ([color=#000000]*[/color][color=#000000]it[/color] [color=#000000]!=[/color] [color=#0000ff]0[/color])
[i][color=#008800]091 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]092 [/color][/i]            [color=#000000]value[/color][color=#000000].[/color][color=#000000]erase[/color]([color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#000000]it[/color]);
[i][color=#008800]093 [/color][/i]            [b][color=#000080]break[/color][/b];
[i][color=#008800]094 [/color][/i]        [color=#000000]}[/color]
[color=#f810b0]095 [/color]    [color=#000000]}[/color]
[i][color=#008800]096 [/color][/i]    [i][color=#008800]//去掉 trailing 0[/color][/i]
[i][color=#008800]097 [/color][/i]    [b][color=#000080]for[/color][/b] ([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]reverse_iterator[/color] [color=#000000]rit[/color] [color=#000000]=[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color](); [color=#000000]rit[/color] [color=#000000]!=[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color](); [color=#000000]++[/color][color=#000000]rit[/color])
[i][color=#008800]098 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]099 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]*[/color][color=#000000]rit[/color] [color=#000000]!=[/color] [color=#0000ff]0[/color] || [color=#000000]rit[/color] [color=#000000]-[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]() [color=#000000]==[/color] [color=#000000]pos_decimalPoint[/color])
[color=#f810b0]100 [/color]        [color=#000000]{[/color]
[i][color=#008800]101 [/color][/i]            [color=#000000]pos_decimalPoint[/color] [color=#000000]-=[/color] [color=#000000]rit[/color] [color=#000000]-[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]();
[i][color=#008800]102 [/color][/i]            [color=#000000]value[/color][color=#000000].[/color][color=#000000]erase[/color]([color=#000000]rit[/color][color=#000000].[/color][color=#000000]base[/color][color=#000000](),[/color] [color=#000000]value[/color][color=#000000].[/color][color=#000000]end[/color]());
[i][color=#008800]103 [/color][/i]            [b][color=#000080]break[/color][/b];
[i][color=#008800]104 [/color][/i]        [color=#000000]}[/color]
[color=#f810b0]105 [/color]    [color=#000000]}[/color]
[i][color=#008800]106 [/color][/i][color=#000000]}[/color]
[i][color=#008800]107 [/color][/i]
[i][color=#008800]108 [/color][/i][color=#008080]#if SEPARATION[/color]
[i][color=#008800]109 [/color][/i][i][color=#008800]// adding[/color][/i]
[color=#f810b0]110 [/color][color=#000000]BigFloat[/color] [color=#000000]BigFloat[/color][color=#000000]::[/color][b][color=#000080]operator[/color][/b][color=#000000]+=[/color]([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>[/color] [color=#000000]&[/color][color=#000000]other[/color])
[i][color=#008800]111 [/color][/i][color=#000000]{[/color]
[i][color=#008800]112 [/color][/i]    [b][color=#000080]short[/color][/b] [color=#000000]carry[/color] [color=#000000]=[/color] [color=#0000ff]0[/color]; [i][color=#008800]// 进位数[/color][/i]
[i][color=#008800]113 [/color][/i]    [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>[/color] [color=#000000]&[/color][color=#000000]result[/color] [color=#000000]=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color];
[i][color=#008800]114 [/color][/i]    [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]reverse_iterator[/color] [color=#000000]rit_result[/color][color=#000000],[/color]
[color=#f810b0]115 [/color]        [color=#000000]rit_other[/color];
[i][color=#008800]116 [/color][/i]
[i][color=#008800]117 [/color][/i]    [b][color=#000080]for[/color][/b] ([color=#000000]rit_result[/color] [color=#000000]=[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]rbegin[/color][color=#000000](),[/color] [color=#000000]rit_other[/color] [color=#000000]=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]rbegin[/color]();
[i][color=#008800]118 [/color][/i]        [color=#000000]rit_other[/color] [color=#000000]!=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]119 [/color][/i]        [color=#000000]++[/color][color=#000000]rit_result[/color][color=#000000],[/color] [color=#000000]++[/color][color=#000000]rit_other[/color])
[color=#f810b0]120 [/color]    [color=#000000]{[/color]
[i][color=#008800]121 [/color][/i]        [i][color=#008800]// 当 result 中没有空间时，扩展空间[/color][/i]
[i][color=#008800]122 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]rit_result[/color] [color=#000000]==[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]rend[/color]())
[i][color=#008800]123 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]124 [/color][/i]            [color=#000000]result[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]result[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#0000ff]0[/color]);
[color=#f810b0]125 [/color]            [color=#000000]rit_result[/color] [color=#000000]=[/color] [color=#000000]--[/color][color=#000000]result[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]126 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]127 [/color][/i]        [color=#000000]carry[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]rit_result[/color] [color=#000000]+[/color] [color=#000000]*[/color][color=#000000]rit_other[/color] [color=#000000]+[/color] [color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color];
[i][color=#008800]128 [/color][/i]        [color=#000000]*[/color][color=#000000]rit_result[/color] [color=#000000]=[/color] [color=#000000]carry[/color] [color=#000000]%[/color] [color=#0000ff]10[/color];
[i][color=#008800]129 [/color][/i]    [color=#000000]}[/color]
[color=#f810b0]130 [/color]    [i][color=#008800]//结束循环后 other 中所有数都参与了计算[/color][/i]
[i][color=#008800]131 [/color][/i]    [b][color=#000080]while[/color][/b] ([color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color])
[i][color=#008800]132 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]133 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]rit_result[/color] [color=#000000]==[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]rend[/color]())
[i][color=#008800]134 [/color][/i]        [color=#000000]{[/color]
[color=#f810b0]135 [/color]            [color=#000000]result[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]result[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#0000ff]0[/color]);
[i][color=#008800]136 [/color][/i]            [color=#000000]rit_result[/color] [color=#000000]=[/color] [color=#000000]--[/color][color=#000000]result[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]137 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]138 [/color][/i]
[i][color=#008800]139 [/color][/i]        [i][color=#008800]// 这里由于能够保证 other 中所有数值都参与了计算，所以不考虑它[/color][/i]
[color=#f810b0]140 [/color]        [color=#000000]carry[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]rit_result[/color] [color=#000000]+[/color] [color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color];
[i][color=#008800]141 [/color][/i]        [color=#000000]*[/color][color=#000000]rit_result[/color] [color=#000000]=[/color] [color=#000000]carry[/color] [color=#000000]%[/color] [color=#0000ff]10[/color];
[i][color=#008800]142 [/color][/i]    [color=#000000]}[/color]
[i][color=#008800]143 [/color][/i]
[i][color=#008800]144 [/color][/i]    [b][color=#000080]return[/color][/b] [color=#000000]*[/color][b][color=#000080]this[/color][/b];
[color=#f810b0]145 [/color][color=#000000]}[/color] [i][color=#008800]// end of adding[/color][/i]
[i][color=#008800]146 [/color][/i]
[i][color=#008800]147 [/color][/i][i][color=#008800]// multiplication[/color][/i]
[i][color=#008800]148 [/color][/i][color=#000000]BigFloat[/color] [color=#000000]BigFloat[/color][color=#000000]::[/color][b][color=#000080]operator[/color][/b][color=#000000]*[/color]([color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]other[/color])
[i][color=#008800]149 [/color][/i][color=#000000]{[/color]
[color=#f810b0]150 [/color]    [b][color=#000080]int[/color][/b] [color=#000000]num_0[/color] [color=#000000]=[/color] [color=#0000ff]0[/color]; [i][color=#008800]// 在作加法时需要在加数后面添加的 0 的个数[/color][/i]
[i][color=#008800]151 [/color][/i]    [color=#000000]BigFloat[/color] [color=#000000]result[/color]; [i][color=#008800]// 保存结果[/color][/i]
[i][color=#008800]152 [/color][/i]
[i][color=#008800]153 [/color][/i]    [b][color=#000080]for[/color][/b] ([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]reverse_iterator[/color] [color=#000000]rit_this[/color] [color=#000000]=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]();
[i][color=#008800]154 [/color][/i]        [color=#000000]rit_this[/color] [color=#000000]!=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]();
[color=#f810b0]155 [/color]        [color=#000000]++[/color][color=#000000]rit_this[/color][color=#000000],[/color]
[i][color=#008800]156 [/color][/i]        [color=#000000]++[/color][color=#000000]num_0[/color]) [i][color=#008800]// 乘数每乘完一位后增加0的个数[/color][/i]
[i][color=#008800]157 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]158 [/color][/i]        [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>[/color] [color=#000000]addend[/color]([color=#000000]num_0[/color][color=#000000],[/color] [color=#0000ff]0[/color]); [i][color=#008800]// 初始化加数，并添加一定个数的 0[/color][/i]
[i][color=#008800]159 [/color][/i]        [b][color=#000080]unsigned[/color][/b] [color=#000000]carry[/color] [color=#000000]=[/color] [color=#0000ff]0[/color]; [i][color=#008800]// 进位数[/color][/i]
[color=#f810b0]160 [/color]
[i][color=#008800]161 [/color][/i]        [b][color=#000080]for[/color][/b] ([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]reverse_iterator[/color] [color=#000000]rit_other[/color] [color=#000000]=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]();
[i][color=#008800]162 [/color][/i]            [color=#000000]rit_other[/color] [color=#000000]!=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]163 [/color][/i]            [color=#000000]++[/color][color=#000000]rit_other[/color])
[i][color=#008800]164 [/color][/i]        [color=#000000]{[/color]
[color=#f810b0]165 [/color]            [color=#000000]carry[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]rit_this[/color] [color=#000000]*[/color] [color=#000000]*[/color][color=#000000]rit_other[/color] [color=#000000]+[/color] [color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color]; [i][color=#008800]// 计[/color][/i]
[i][color=#008800]166 [/color][/i]            [color=#000000]addend[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]addend[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#000000]carry[/color] [color=#000000]%[/color] [color=#0000ff]10[/color]);   [i][color=#008800]// 算[/color][/i]
[i][color=#008800]167 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]168 [/color][/i]        [i][color=#008800]// 处理留下的 carry[/color][/i]
[i][color=#008800]169 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color])
[color=#f810b0]170 [/color]        [color=#000000]{[/color]
[i][color=#008800]171 [/color][/i]            [color=#000000]addend[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]addend[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#000000]carry[/color] [color=#000000]/[/color] [color=#0000ff]10[/color]);
[i][color=#008800]172 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]173 [/color][/i]
[i][color=#008800]174 [/color][/i]        [color=#000000]result[/color] [color=#000000]+=[/color] [color=#000000]addend[/color]; [i][color=#008800]// 将计算所得的加数与结果相加[/color][/i]
[color=#f810b0]175 [/color]    [color=#000000]}[/color]
[i][color=#008800]176 [/color][/i]
[i][color=#008800]177 [/color][/i]    [color=#000000]result[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]+[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color]; [i][color=#008800]// 计算小数点[/color][/i]
[i][color=#008800]178 [/color][/i]
[i][color=#008800]179 [/color][/i]    [b][color=#000080]return[/color][/b] [color=#000000]result[/color];
[color=#f810b0]180 [/color][color=#000000]}[/color] [i][color=#008800]// end of multiplication[/color][/i]
[i][color=#008800]181 [/color][/i]
[i][color=#008800]182 [/color][/i][color=#008080]#else[/color]
[i][color=#008800]183 [/color][/i]
[i][color=#008800]184 [/color][/i][color=#000000]BigFloat[/color] [color=#000000]BigFloat[/color][color=#000000]::[/color][b][color=#000080]operator[/color][/b] [color=#000000]*[/color] ([color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]other[/color])
[color=#f810b0]185 [/color][color=#000000]{[/color]
[i][color=#008800]186 [/color][/i]    [color=#000000]BigFloat[/color] [color=#000000]result[/color];
[i][color=#008800]187 [/color][/i]    [b][color=#000080]int[/color][/b] [color=#000000]i[/color][color=#000000],[/color] [color=#000000]j[/color];
[i][color=#008800]188 [/color][/i]    [b][color=#000080]short[/color][/b] [color=#000000]carrySum[/color][color=#000000],[/color] [i][color=#008800]// 和的进位数[/color][/i]
[i][color=#008800]189 [/color][/i]        [color=#000000]carryPro[/color]; [i][color=#008800]// 积的进位数[/color][/i]
[color=#f810b0]190 [/color]
[i][color=#008800]191 [/color][/i]    [color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]reverse_iterator[/color] [color=#000000]ritSelf[/color][color=#000000],[/color]
[i][color=#008800]192 [/color][/i]                                    [color=#000000]ritOther[/color][color=#000000],[/color]
[i][color=#008800]193 [/color][/i]                                    [color=#000000]ritResult[/color];
[i][color=#008800]194 [/color][/i]
[color=#f810b0]195 [/color]    [b][color=#000080]for[/color][/b] ([color=#000000]ritSelf[/color] [color=#000000]=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color][color=#000000](),[/color] [color=#000000]j[/color] [color=#000000]=[/color] [color=#0000ff]0[/color];
[i][color=#008800]196 [/color][/i]        [color=#000000]ritSelf[/color] [color=#000000]!=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]197 [/color][/i]        [color=#000000]++[/color][color=#000000]ritSelf[/color][color=#000000],[/color] [color=#000000]j[/color][color=#000000]++[/color])
[i][color=#008800]198 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]199 [/color][/i]        [color=#000000]carrySum[/color] [color=#000000]=[/color] [color=#000000]carryPro[/color] [color=#000000]=[/color] [color=#0000ff]0[/color];
[color=#f810b0]200 [/color]        [b][color=#000080]for[/color][/b] ([color=#000000]ritOther[/color] [color=#000000]=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color][color=#000000](),[/color] [color=#000000]i[/color] [color=#000000]=[/color] [color=#000000]j[/color];
[i][color=#008800]201 [/color][/i]            [color=#000000]ritOther[/color] [color=#000000]!=[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]();
[i][color=#008800]202 [/color][/i]            [color=#000000]++[/color][color=#000000]ritOther[/color][color=#000000],[/color] [color=#000000]i[/color][color=#000000]++[/color])
[i][color=#008800]203 [/color][/i]        [color=#000000]{[/color]
[i][color=#008800]204 [/color][/i]            [color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]() [color=#000000]+[/color] [color=#000000]i[/color];
[color=#f810b0]205 [/color]            [b][color=#000080]if[/color][/b] ([color=#000000]ritResult[/color] [color=#000000]==[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]())
[i][color=#008800]206 [/color][/i]            [color=#000000]{[/color]
[i][color=#008800]207 [/color][/i]                [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#0000ff]0[/color]);
[i][color=#008800]208 [/color][/i]                [color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]() [color=#000000]-[/color] [color=#0000ff]1[/color];
[i][color=#008800]209 [/color][/i]            [color=#000000]}[/color]
[color=#f810b0]210 [/color]            [color=#000000]carryPro[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]ritSelf[/color] [color=#000000]*[/color] [color=#000000]*[/color][color=#000000]ritOther[/color] [color=#000000]+[/color] [color=#000000]carryPro[/color] [color=#000000]/[/color] [color=#0000ff]10[/color];
[i][color=#008800]211 [/color][/i]            [color=#000000]carrySum[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]ritResult[/color] [color=#000000]+[/color] [color=#000000]carryPro[/color] [color=#000000]%[/color] [color=#0000ff]10[/color] [color=#000000]+[/color] [color=#000000]carrySum[/color] [color=#000000]/[/color] [color=#0000ff]10[/color];
[i][color=#008800]212 [/color][/i]            [color=#000000]*[/color][color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]carrySum[/color] [color=#000000]%[/color] [color=#0000ff]10[/color];
[i][color=#008800]213 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]214 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]carrySum[/color] [color=#000000]/[/color] [color=#0000ff]10[/color] || [color=#000000]carryPro[/color] [color=#000000]/[/color] [color=#0000ff]10[/color])
[color=#f810b0]215 [/color]        [color=#000000]{[/color]
[i][color=#008800]216 [/color][/i]            [b][color=#000080]do[/color][/b] 
[i][color=#008800]217 [/color][/i]            [color=#000000]{[/color]
[i][color=#008800]218 [/color][/i]                [color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rbegin[/color]() [color=#000000]+[/color] [color=#000000]i[/color];
[i][color=#008800]219 [/color][/i]                [b][color=#000080]if[/color][/b] ([color=#000000]ritResult[/color] [color=#000000]==[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]())
[color=#f810b0]220 [/color]                [color=#000000]{[/color]
[i][color=#008800]221 [/color][/i]                    [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#0000ff]0[/color]);
[i][color=#008800]222 [/color][/i]                    [color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]result[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]rend[/color]() [color=#000000]-[/color] [color=#0000ff]1[/color];
[i][color=#008800]223 [/color][/i]                [color=#000000]}[/color]
[i][color=#008800]224 [/color][/i]                [color=#000000]carrySum[/color] [color=#000000]=[/color] [color=#000000]*[/color][color=#000000]ritResult[/color] [color=#000000]+[/color] [color=#000000]carrySum[/color] [color=#000000]/[/color] [color=#0000ff]10[/color] [color=#000000]+[/color] [color=#000000]carryPro[/color] [color=#000000]/[/color] [color=#0000ff]10[/color];
[color=#f810b0]225 [/color]                [color=#000000]*[/color][color=#000000]ritResult[/color] [color=#000000]=[/color] [color=#000000]carrySum[/color] [color=#000000]%[/color] [color=#0000ff]10[/color];
[i][color=#008800]226 [/color][/i]                [color=#000000]carryPro[/color] [color=#000000]=[/color] [color=#0000ff]0[/color];
[i][color=#008800]227 [/color][/i]                [color=#000000]i[/color][color=#000000]++[/color];
[i][color=#008800]228 [/color][/i]            [color=#000000]}[/color] [b][color=#000080]while[/color][/b] ([color=#000000]carrySum[/color] [color=#000000]/[/color] [color=#0000ff]10[/color]);
[i][color=#008800]229 [/color][/i]        [color=#000000]}[/color]
[color=#f810b0]230 [/color]    [color=#000000]}[/color]
[i][color=#008800]231 [/color][/i]
[i][color=#008800]232 [/color][/i]    [color=#000000]result[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]=[/color] [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]+[/color] [color=#000000]other[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color];
[i][color=#008800]233 [/color][/i]
[i][color=#008800]234 [/color][/i]    [b][color=#000080]return[/color][/b] [color=#000000]result[/color];
[color=#f810b0]235 [/color][color=#000000]}[/color]
[i][color=#008800]236 [/color][/i][color=#008080]#endif[/color]
[i][color=#008800]237 [/color][/i]
[i][color=#008800]238 [/color][/i][color=#000000]BigFloat[/color] [color=#000000]BigFloat[/color][color=#000000]::[/color][b][color=#000080]operator[/color][/b] [color=#000000]=[/color] ([color=#000000]BigFloat[/color] [color=#000000]bf[/color])
[i][color=#008800]239 [/color][/i][color=#000000]{[/color]
[color=#f810b0]240 [/color]    [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]value[/color] [color=#000000]=[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color];
[i][color=#008800]241 [/color][/i]    [b][color=#000080]this[/color][/b][color=#000000]->[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]=[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color];
[i][color=#008800]242 [/color][/i]    [b][color=#000080]return[/color][/b] [color=#000000]*[/color][b][color=#000080]this[/color][/b];
[i][color=#008800]243 [/color][/i][color=#000000]}[/color]
[i][color=#008800]244 [/color][/i]
[color=#f810b0]245 [/color][color=#000000]ostream[/color] [color=#000000]&[/color] [b][color=#000080]operator[/color][/b] [color=#000000]<<[/color] ([color=#000000]ostream[/color] [color=#000000]&[/color][color=#000000]output[/color][color=#000000],[/color] [color=#000000]BigFloat[/color] [color=#000000]&[/color][color=#000000]bf[/color])
[i][color=#008800]246 [/color][/i][color=#000000]{[/color]
[i][color=#008800]247 [/color][/i]    [b][color=#000080]if[/color][/b] ([color=#000000]bf[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]>[/color] [color=#0000ff]0[/color])
[i][color=#008800]248 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]249 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]bf[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]>[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]size[/color]())
[color=#f810b0]250 [/color]        [color=#000000]{[/color]
[i][color=#008800]251 [/color][/i]            [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color][color=#000000](),[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color] [color=#000000]-[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]size[/color][color=#000000](),[/color] [color=#0000ff]0[/color]);
[i][color=#008800]252 [/color][/i]        [color=#000000]}[/color]
[i][color=#008800]253 [/color][/i]        [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]insert[/color]([color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]end[/color]() [color=#000000]-[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]pos_decimalPoint[/color][color=#000000],[/color] [color=#800080]'.'[/color]);
[i][color=#008800]254 [/color][/i]    [color=#000000]}[/color]
[color=#f810b0]255 [/color]
[i][color=#008800]256 [/color][/i]    [i][color=#008800]/*[/color][/i]
[i][color=#008800]257 [/color][/i][i][color=#008800]    for_each(bf.value.begin(), bf.value.end(),[/color][/i]
[i][color=#008800]258 [/color][/i][i][color=#008800]        [&](short v)[/color][/i]
[i][color=#008800]259 [/color][/i][i][color=#008800]        {[/color][/i]
[color=#f810b0]260 [/color][i][color=#008800]            if (v == '.')[/color][/i]
[i][color=#008800]261 [/color][/i][i][color=#008800]                output << '.';[/color][/i]
[i][color=#008800]262 [/color][/i][i][color=#008800]            else[/color][/i]
[i][color=#008800]263 [/color][/i][i][color=#008800]                output << v;[/color][/i]
[i][color=#008800]264 [/color][/i][i][color=#008800]        }[/color][/i]
[color=#f810b0]265 [/color][i][color=#008800]    );*/[/color][/i]
[i][color=#008800]266 [/color][/i]    
[i][color=#008800]267 [/color][/i]    [b][color=#000080]for[/color][/b] ([color=#000000]vector[/color][color=#000000]<[/color][b][color=#000080]short[/color][/b][color=#000000]>::[/color][color=#000000]iterator[/color] [color=#000000]it[/color] [color=#000000]=[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]begin[/color](); [color=#000000]it[/color] [color=#000000]!=[/color] [color=#000000]bf[/color][color=#000000].[/color][color=#000000]value[/color][color=#000000].[/color][color=#000000]end[/color](); [color=#000000]++[/color][color=#000000]it[/color])
[i][color=#008800]268 [/color][/i]    [color=#000000]{[/color]
[i][color=#008800]269 [/color][/i]        [b][color=#000080]if[/color][/b] ([color=#000000]*[/color][color=#000000]it[/color] [color=#000000]==[/color] [color=#800080]'.'[/color])
[color=#f810b0]270 [/color]            [color=#000000]output[/color] [color=#000000]<<[/color] [color=#800080]'.'[/color];
[i][color=#008800]271 [/color][/i]        [b][color=#000080]else[/color][/b]
[i][color=#008800]272 [/color][/i]            [color=#000000]output[/color] [color=#000000]<<[/color] [color=#000000]*[/color][color=#000000]it[/color];
[i][color=#008800]273 [/color][/i]    [color=#000000]}[/color]
[i][color=#008800]274 [/color][/i]    
[color=#f810b0]275 [/color]    [b][color=#000080]return[/color][/b] [color=#000000]output[/color];
[i][color=#008800]276 [/color][/i][color=#000000]}[/color]
[/font]


##最后的思考与总结

	这道题目其实弄起来也不难，但涉及到诸多细节上面的东西，需要我们仔细一些。

	特别是要注意这样的数：
		100.00
		00.001
	这样的数处理起来不难，但容易忽略


	最后呢希望大家指出自己的疑点，相互学习

	转载请注明出处!