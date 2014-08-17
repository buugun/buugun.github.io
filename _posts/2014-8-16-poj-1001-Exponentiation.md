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
	既然如此，就必须使用__大数(googol)__。

	而带着小数点的大数我在之前也没接触过，顿时我就犯了难，顿觉无爱，顿觉好忧桑。
	功夫不负有心人，我查阅了一些资料后发现小数点其实很好解决，那就是用个变量来保存它的位置。

	我是这样处理的：有多少位小数，那个位置的值就是多少。


	小数点问题有思路后，我首先考虑的就是效率问题了。如何做才能稳准狠地计算出结果呢？


	一想到要做大数的幂运算，大多数人首先先想到的肯定是要作很多__乘法运算__了，这无疑就大大降低了效率，而像我这样有着些许强迫症的人是不会允许这样的情况出现的，除非实在没办法。

	我们如何__提高效率__呢？

	可很明显我这样说就代表了我有个较好的化的计算方法，在这里和大家分享一下，这也是本程序的__亮点__之一，同时若道友有更好的优化方案，希望指出，这段代码我会保存到github，会用的话直接在上面修改就是，不会用的话直接联系我邮箱便可，感激不尽。

	比如说，我们要计算 R^n, 常规思路是作 n-1 次乘法，优化思路就是将它作如下拆解:

		 			   R^5 = R^1 * R^4,
		 			   R^9 = R^1 * R^8,
		 			   R^7 = R^1 * R^2 * R^4
			 		   ………………

	敏感一些的同学可能就会发现，这样的拆解就类似于用少数有限个二进制数就可以表示某一区间中任意值，如 0000 0001, 0000 0010, 0000 0100, 0000 1000, 0001 0000, ……, 1000 0000 这8个二进制就可以表示 [0, 2^8) 中的__任意__非负整型值。

	回到题目，由以上分析我们可以感觉到，要作指数区间为(0, 25] 的幂运算，我们只需先计算好 R^1, R^2, R^4, R^8, R^16，由这几个数，我们就可以通过很少次数的乘法运算来快速计算出高指数的幂了，如 R^25 = R^1 * R^8 * R^16, 原本需要计算24次乘法，现在只需作 （2 + 构造这一数组所需次数）的少数运算。

	那么构造这一基数组是否需要很多步骤？当然不会，我直接写都能写出来：
			R^2 = R^1 * R^1,
			R^4 = R^2 * R^2,
			R^8 = R^4 * R^4,
			R^16 = R^8 * R^8
	最多仅需4次乘法运算。

	相信经过以上的优化，时间复杂度将会降低不少。



	另外还要说的一点就是最基础的__大数乘法__，我这有两种思路：一种就是先写个加法，然后写个乘法，因为乘法要用到加法；
													  而另一种呢，就是直接写乘法。

	下面对两者进行比较：

	这两个思路的基本思路都是使用竖式计算。

	前者：将乘法与加法分开，__优点__是便于书写，便于想；__缺点__是乘法进行一次遍历，加法在每次作乘时还要进行遍历，无形间降低了效率。

	后者：将乘法与加法结合，__优点__是只用遍历乘法，在作乘的过程中夹杂加法，将二者结合，当然所谓夹杂不是简单的将加法函数里边的代码拷贝到乘法中，具体情况见稍后贴出的代码；__缺点__就很显然的是，比较难想，不益于书写，写时思路很容易乱，当然这个缺点对一些道友来说也是优点，因为利于学习。

##乘加分离的乘法运算
思路图：

![method of separation](/assets/images/poj1001/method_of_separation.png)

代码：

	// adding
	BigFloat BigFloat::operator+=(vector<short> &other)
	{
		short carry = 0; // 进位数
		vector<short> &result = this->value;
		vector<short>::reverse_iterator rit_result,
			rit_other;

		for (rit_result = result.rbegin(), rit_other = other.rbegin();
			rit_other != other.rend();
			++rit_result, ++rit_other)
		{
			// 当 result 中没有空间时，扩展空间
			if (rit_result == result.rend())
			{
				result.insert(result.begin(), 0);
				rit_result = --result.rend();
			}
			carry = *rit_result + *rit_other + carry / 10;
			*rit_result = carry % 10;
		}
		//结束循环后 other 中所有数都参与了计算
		while (carry / 10)
		{
			if (rit_result == result.rend())
			{
				result.insert(result.begin(), 0);
				rit_result = --result.rend();
			}
	
			// 这里由于能够保证 other 中所有数值都参与了计算，所以不考虑它
			carry = *rit_result + carry / 10;
			*rit_result = carry % 10;
		}

		return *this;
	} // end of adding

	// multiplication
	BigFloat BigFloat::operator*(BigFloat &other)
	{
		int num_0 = 0; // 在作加法时需要在加数后面添加的 0 的个数
		BigFloat result; // 保存结果

		for (vector<short>::reverse_iterator rit_this = this->value.rbegin();
			rit_this != this->value.rend();
			++rit_this,
			++num_0) // 乘数每乘完一位后增加0的个数
		{
			vector<short> addend(num_0, 0); // 初始化加数，并添加一定个数的 0
			unsigned carry = 0; // 进位数

			for (vector<short>::reverse_iterator rit_other = other.value.rbegin();
				rit_other != other.value.rend();
				++rit_other)
			{
				carry = *rit_this * *rit_other + carry / 10; // 计
				addend.insert(addend.begin(), carry % 10);   // 算
			}
			// 处理留下的 carry
			if (carry / 10)
			{
				addend.insert(addend.begin(), carry / 10);
			}

			result += addend; // 将计算所得的加数与结果相加
		}

		result.pos_decimalPoint = this->pos_decimalPoint + other.pos_decimalPoint; // 计算小数点

		return result;
	} // end of multiplication

结果：

![result_of_separation](/assets/images/poj1001/result_of_separation.png)

##乘加结合的乘法运算
思路图：

![method of combination](/assets/images/poj1001/method_of_combination.png)

代码:
	BigFloat BigFloat::operator * (BigFloat &other)
	{
		BigFloat result;
		int i, j;
		short carrySum, // 和的进位数
			carryPro; // 积的进位数

		vector<short>::reverse_iterator ritSelf,
										ritOther,
										ritResult;

		for (ritSelf = this->value.rbegin(), j = 0;
			ritSelf != this->value.rend();
			++ritSelf, j++)
		{
			carrySum = carryPro = 0;
			for (ritOther = other.value.rbegin(), i = j;
				ritOther != other.value.rend();
				++ritOther, i++)
			{
				ritResult = result.value.rbegin() + i;
				if (ritResult == result.value.rend())
				{
					result.value.insert(result.value.begin(), 0);
					ritResult = result.value.rend() - 1;
				}
				carryPro = *ritSelf * *ritOther + carryPro / 10;
				carrySum = *ritResult + carryPro % 10 + carrySum / 10;
				*ritResult = carrySum % 10;
			}
			if (carrySum / 10 || carryPro / 10)
			{
				do 
				{
					ritResult = result.value.rbegin() + i;
					if (ritResult == result.value.rend())
					{
						result.value.insert(result.value.begin(), 0);
						ritResult = result.value.rend() - 1;
					}
					carrySum = *ritResult + carrySum / 10 + carryPro / 10;
					*ritResult = carrySum % 10;
					carryPro = 0;
					i++;
				} while (carrySum / 10);
			}
		}

		result.pos_decimalPoint = this->pos_decimalPoint + other.pos_decimalPoint;

		return result;
	}

结果：

![result_of_separation](/assets/images/poj1001/result_of_combination.png)

##完整代码
	#include <iostream>
	#include <vector>
	#include <string>
	#include <algorithm>
	using namespace std;

	#define SEPARATION 0 // 使用分离的乘法否

	class BigFloat
	{
	private:
		vector<short> value; //数字部分
		unsigned int pos_decimalPoint; //小数点位置

	public:
		BigFloat() : pos_decimalPoint(0) {};
		inline BigFloat(string r); // 初始化 result, 小数点位置为 输入的 R 所在的下标， 无小数点则为 0

	#if SEPARATION
		inline BigFloat operator+=(vector<short> &other);
		inline BigFloat operator*(BigFloat &other);
	#else
		inline BigFloat operator * (BigFloat &bf);
	#endif
		inline BigFloat operator = (BigFloat bf);
		friend ostream & operator << (ostream &output, BigFloat &bf);
	};

	int main()
	{
		unsigned int n; //指数
		string baseNum; //基数

		while (cin >> baseNum >> n)
		{
			vector<BigFloat> seqOfBigFloat; // 构造数列存储 R^1, R^2, R^4, R^8 ~~~ 采用优化的乘法
			vector<bool> flag; // 判断某位置是否需要 seqOfBigFloat 中对应位置的数

			seqOfBigFloat.push_back(BigFloat(baseNum));
			do
			{
				flag.push_back(n & 1);
			} while (n >>= 1);

			// 将数列存入 seqOfBigFloat
			unsigned int sizeOfFlag = flag.size();
			for (unsigned i = 0; i < sizeOfFlag - 1; i++)
			{
				seqOfBigFloat.push_back(seqOfBigFloat[i] * seqOfBigFloat[i]);
			}

			// 计算
			BigFloat result("1");
			for (unsigned i = 0; i < sizeOfFlag; i++)
			{
				if (flag[i])
				{
					result = result * seqOfBigFloat[i];
				}
			}

			// 输出
			cout << result << endl;

		}

		return 0;
	}

	BigFloat::BigFloat(string r)
	{
		pos_decimalPoint = 0;

		//放入数值，并确定小数点的位置
		for (unsigned i = 0, lengthOfR = r.length(); i < lengthOfR; i++)
		{
			if (r[i] == '.')
			{
				pos_decimalPoint = lengthOfR - i - 1;
			}
			else
			{
				value.push_back(r[i] - '0');
			}
		}

		//去掉 leading 0
		for (vector<short>::iterator it = value.begin(); it != value.end(); ++it)
		{
			if (*it != 0)
			{
				value.erase(value.begin(), it);
				break;
			}
		}
		//去掉 trailing 0
		for (vector<short>::reverse_iterator rit = value.rbegin(); rit != value.rend(); ++rit)
		{
			if (*rit != 0 || rit - value.rbegin() == pos_decimalPoint)
			{
				pos_decimalPoint -= rit - value.rbegin();
				value.erase(rit.base(), value.end());
				break;
			}
		}
	}

	#if SEPARATION
	// adding
	BigFloat BigFloat::operator+=(vector<short> &other)
	{
		short carry = 0; // 进位数
		vector<short> &result = this->value;
		vector<short>::reverse_iterator rit_result,
			rit_other;

		for (rit_result = result.rbegin(), rit_other = other.rbegin();
			rit_other != other.rend();
			++rit_result, ++rit_other)
		{
			// 当 result 中没有空间时，扩展空间
			if (rit_result == result.rend())
			{
				result.insert(result.begin(), 0);
				rit_result = --result.rend();
			}
			carry = *rit_result + *rit_other + carry / 10;
			*rit_result = carry % 10;
		}
		//结束循环后 other 中所有数都参与了计算
		while (carry / 10)
		{
			if (rit_result == result.rend())
			{
				result.insert(result.begin(), 0);
				rit_result = --result.rend();
			}

			// 这里由于能够保证 other 中所有数值都参与了计算，所以不考虑它
			carry = *rit_result + carry / 10;
			*rit_result = carry % 10;
		}

		return *this;
	} // end of adding

	// multiplication
	BigFloat BigFloat::operator*(BigFloat &other)
	{
		int num_0 = 0; // 在作加法时需要在加数后面添加的 0 的个数
		BigFloat result; // 保存结果

		for (vector<short>::reverse_iterator rit_this = this->value.rbegin();
			rit_this != this->value.rend();
			++rit_this,
			++num_0) // 乘数每乘完一位后增加0的个数
		{
			vector<short> addend(num_0, 0); // 初始化加数，并添加一定个数的 0
			unsigned carry = 0; // 进位数

			for (vector<short>::reverse_iterator rit_other = other.value.rbegin();
				rit_other != other.value.rend();
				++rit_other)
			{
				carry = *rit_this * *rit_other + carry / 10; // 计
				addend.insert(addend.begin(), carry % 10);   // 算
			}
			// 处理留下的 carry
			if (carry / 10)
			{
				addend.insert(addend.begin(), carry / 10);
			}

			result += addend; // 将计算所得的加数与结果相加
		}

		result.pos_decimalPoint = this->pos_decimalPoint + other.pos_decimalPoint; // 计算小数点

		return result;
	} // end of multiplication

	#else

	BigFloat BigFloat::operator * (BigFloat &other)
	{
		BigFloat result;
		int i, j;
		short carrySum, // 和的进位数
			carryPro; // 积的进位数

		vector<short>::reverse_iterator ritSelf,
										ritOther,
										ritResult;

		for (ritSelf = this->value.rbegin(), j = 0;
			ritSelf != this->value.rend();
			++ritSelf, j++)
		{
			carrySum = carryPro = 0;
			for (ritOther = other.value.rbegin(), i = j;
				ritOther != other.value.rend();
				++ritOther, i++)
			{
				ritResult = result.value.rbegin() + i;
				if (ritResult == result.value.rend())
				{
					result.value.insert(result.value.begin(), 0);
					ritResult = result.value.rend() - 1;
				}
				carryPro = *ritSelf * *ritOther + carryPro / 10;
				carrySum = *ritResult + carryPro % 10 + carrySum / 10;
				*ritResult = carrySum % 10;
			}
			if (carrySum / 10 || carryPro / 10)
			{
				do 
				{
					ritResult = result.value.rbegin() + i;
					if (ritResult == result.value.rend())
					{
						result.value.insert(result.value.begin(), 0);
						ritResult = result.value.rend() - 1;
					}
					carrySum = *ritResult + carrySum / 10 + carryPro / 10;
					*ritResult = carrySum % 10;
					carryPro = 0;
					i++;
				} while (carrySum / 10);
			}
		}

		result.pos_decimalPoint = this->pos_decimalPoint + other.pos_decimalPoint;

		return result;
	}
	#endif

	BigFloat BigFloat::operator = (BigFloat bf)
	{
		this->value = bf.value;
		this->pos_decimalPoint = bf.pos_decimalPoint;
		return *this;
	}

	ostream & operator << (ostream &output, BigFloat &bf)
	{
		if (bf.pos_decimalPoint > 0)
		{
			if (bf.pos_decimalPoint > bf.value.size())
			{
				bf.value.insert(bf.value.begin(), bf.pos_decimalPoint - bf.value.size(), 0);
			}
			bf.value.insert(bf.value.end() - bf.pos_decimalPoint, '.');
		}

		/*
		for_each(bf.value.begin(), bf.value.end(),
			[&](short v)
			{
				if (v == '.')
					output << '.';
				else
					output << v;
			}
		);*/
		
		for (vector<short>::iterator it = bf.value.begin(); it != bf.value.end(); ++it)
		{
			if (*it == '.')
				output << '.';
			else
				output << *it;
		}
		
		return output;
	}

##最后的思考与总结

这道题目其实弄起来也不难，但涉及到诸多细节上面的东西，需要我们仔细一些。

特别是要注意这样的数：
	100.00
	00.001
这样的数处理起来不难，但容易忽略


最后呢希望大家指出自己的疑点，相互学习

转载请注明出处!