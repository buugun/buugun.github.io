---
layout: post
title: "solution of poj 1001 Exponentiation"
description: "Here is the solution of poj 1001 Exponentiation. It describes my train of thought of solving the problem of multiplication of googol."
keywords: "googol multiplication, 大数乘法, poj 1001, exponentiation"
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
	既然如此，就必须使用<strong>大数(googol)</strong>。

	而带着小数点的大数我在之前也没接触过，顿时我就犯了难，顿觉无爱，顿觉好忧桑。
	功夫不负有心人，我查阅了一些资料后发现小数点其实很好解决，那就是用个变量来保存它的位置。

	我是这样处理的：有多少位小数，那个位置的值就是多少。


	小数点问题有思路后，我首先考虑的就是效率问题了。如何做才能稳准狠地计算出结果呢？


	一想到要做大数的幂运算，大多数人首先先想到的肯定是要作很多<strong>乘法运算</strong>了，这无疑就大大降低了效率，而像我这样有着些许强迫症的人是不会允许这样的情况出现的，除非实在没办法。

	我们如何<strong>提高效率</strong>呢？

	可很明显我这样说就代表了我有个较好的化的计算方法，在这里和大家分享一下，这也是本程序的<strong>亮点</strong>之一，同时若道友有更好的优化方案，希望指出，这段代码我会保存到github，会用的话直接在上面修改就是，不会用的话直接联系我邮箱便可，感激不尽。

	比如说，我们要计算 R^n, 常规思路是作 n-1 次乘法，优化思路就是将它作如下拆解:

		 			   R^5 = R^1 * R^4,
		 			   R^9 = R^1 * R^8,
		 			   R^7 = R^1 * R^2 * R^4
			 		   ………………

	敏感一些的同学可能就会发现，这样的拆解就类似于用少数有限个二进制数就可以表示某一区间中任意值，如 0000 0001, 0000 0010, 0000 0100, 0000 1000, 0001 0000, ……, 1000 0000 这8个二进制就可以表示 [0, 2^8) 中的<strong>任意</strong>非负整型值。

	回到题目，由以上分析我们可以感觉到，要作指数区间为(0, 25] 的幂运算，我们只需先计算好 R^1, R^2, R^4, R^8, R^16，由这几个数，我们就可以通过很少次数的乘法运算来快速计算出高指数的幂了，如 R^25 = R^1 * R^8 * R^16, 原本需要计算24次乘法，现在只需作 （2 + 构造这一数组所需次数）的少数运算。

	那么构造这一基数组是否需要很多步骤？当然不会，我直接写都能写出来：
			R^2 = R^1 * R^1,
			R^4 = R^2 * R^2,
			R^8 = R^4 * R^4,
			R^16 = R^8 * R^8
	最多仅需4次乘法运算。

	相信经过以上的优化，时间复杂度将会降低不少。



	另外还要说的一点就是最基础的<strong>大数乘法</strong>，我这有两种思路：一种就是先写个加法，然后写个乘法，因为乘法要用到加法；
												  而另一种呢，就是直接写乘法。

	下面对两者进行比较：

	这两个思路的基本思路都是使用竖式计算。

	前者：将乘法与加法分开，<strong>优点</strong>是便于书写，便于想；<strong>缺点</strong>是乘法进行一次遍历，加法在每次作乘时还要进行遍历，无形间降低了效率。

	后者：将乘法与加法结合，<strong>优点</strong>是只用遍历乘法，在作乘的过程中夹杂加法，将二者结合，当然所谓夹杂不是简单的将加法函数里边的代码拷贝到乘法中，具体情况见稍后贴出的代码；<strong>缺点</strong>就很显然的是，比较难想，不益于书写，写时思路很容易乱，当然这个缺点对一些道友来说也是优点，因为利于学习。


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

<script src="https://gist.github.com/gustfu/fe10b9e67700216f30a7.js"></script>


##最后的思考与总结

	这道题目其实弄起来也不难，但涉及到诸多细节上面的东西，需要我们仔细一些。

	特别是要注意这样的数：
		100.00
		00.001
	这样的数处理起来不难，但容易忽略


	最后呢希望大家指出自己的疑点，相互学习

	转载请注明出处!