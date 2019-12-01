# homework_2
## 9.11 
### 问：对6种创建和初始化 vector 对象的方法，每一种都给出一个实例。 解释每个 vector 包含什么值。

## 9.12 
### 问：对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

我们可以用接受两个迭代器创建拷贝的构造函数去复制一个容器的某一部分内容；但是如果使用接受一个容器创建其拷贝的方法，只能复制整个容器。

当一个容器初始化为另一个容器的拷贝时，两个容器的容器类型和元素类型都必须相同。当传递迭代器参数来拷贝一个范围时，就不要求容器类型和

元素类型是相同的。

## 9.13
### 问：如何从一个list<int>初始化一个vector<double>?从一个vector<int>又该如何创建？编写代码验证你的答案。
	
```C++
#include<iostream>
#include<vector>
#include<list>

using namespace std;

int main(){
	list<int> ilist = {1,2,3,4,5};
	vector<int> ivec = {5,4,3,2,1};

	vector<double> ivec_1(ilist.begin(),ilist.end());

	vector<double> ivec_2(ivec.begin(),ivec.end());

	cout << ivec_1.capacity() << " " << ivec_1.size() << " " <<ivec_1[0]
		<< " " << ivec_1[ivec_1.size()-1] << " " << endl;
	cout << ivec_2.capacity() << " " << ivec_2.size() << " " <<ivec_2[0]
                << " " << ivec_2[ivec_2.size()-1] << " " << endl;
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/912.png)

## 9.20
### 问：编写程序，从一个list<int>拷贝元素到两个deque中。值为偶数的所有元素拷贝到一个deque中，而奇数值元素都拷贝到另一个deque中。  

```C++
#include <iostream>
#include <list>
#include <deque>

using namespace std;

int main(){
	list<int> ilist = {1,2,3,4,5};
	deque<int> jishu, oushu;
	
	for (auto i = ilist.cbegin(); i != ilist.cend(); i++){
		if (*i & 1)
			jishu.push_back(*i);
		else
			oushu.push_back(*i);
	}
	cout << "jishu: " ;
	for (auto i = jishu.cbegin(); i != jishu.cend(); i++){
		cout << *i << " ";
	}
	cout << endl;

        cout << "oushu: " ;
        for (auto i = oushu.cbegin(); i != oushu.cend(); i++){
                cout << *i << " ";
        }       
        cout << endl;
	
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/920.png)

## 9.29
### 问：假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？

会默认初始化75个元素添加到vec的末尾。

会将vec中的后90个元素删除。

```C++
#include<iostream>
#include<vector>

using namespace std;

int main(){
	vector<int> ivec{1,2,3,4,5};

	ivec.resize(10);
	for (auto i = ivec.cbegin(); i != ivec.cend() ; i++)
		cout << *i << " ";
	cout << endl;

	ivec.resize(3);
        for (auto i = ivec.cbegin(); i != ivec.cend() ; i++)
                cout << *i << " ";
        cout << endl;

	return 0;
}

```
![login](https://github.com/UpCris/homework_2/blob/master/929.png)

## 9.43
### 问：编写一个函数，接受三个string参数是s、oldVal 和newVal。使用迭代器及insert和erase函数将s中所有oldVal替换为newVal。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

```C++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

void replace_string( string &s, const string &oldval, const string &newval){
	auto l = oldval.size();

	if( !l)
		return;
	auto i = s.begin();
	while (i <= s.end()-1){
		auto i1 = i;
		auto i2 = oldval.begin();
		while (i2 != oldval.end() && *i1 == *i2){
			i1++;
			i2++;
		}
		if (i2 == oldval.end()){
			i = s.erase(i,i1);
			if (newval.size()){
				i2 = newval.end();
				do{
					i2--;
					i = s.insert(i,*i2);
				}while (i2>newval.begin());
			}
			i += newval.size();
		}else i++;
	}
}

int main(){
	string s ="tho thru tho!";
	replace_string(s,"thru","through");
	cout << s << endl;

	replace_string(s,"tho","though");
	cout << s << endl;

	replace_string(s,"through"," ");
	cout << s << endl;

	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/943.png)

## 9.52
### 问：使用stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从stack中pop对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）push到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。

```C++
#include <iostream>
#include <string>
#include <stack>
#include <vector>
using namespace std;
 
void calculate(stack<int>  &operd, string oper)
{
	if (oper == "-")
	{
		int  val2 = operd.top();
		operd.pop();
		int val1 = operd.top();
		operd.pop();
		int result = val1 - val2;
		operd.push(result);
	}
	else if (oper == "+")
	{
		int  val1 = operd.top();
		operd.pop();
		int val2 = operd.top();
		operd.pop();
		int result = val1 + val2;
		operd.push(result);
	}
	else if (oper == "*")
	{
		int  val1 = operd.top();
		operd.pop();
		int val2 = operd.top();
		operd.pop();
		int result = val1 * val2;
		operd.push(result);
	}
	else if (oper == "/")
	{
		int  val2 = operd.top();
		operd.pop();
		int val1 = operd.top();
		operd.pop();
		int result = val2 ? (val1 / val2) : val2; //这里除法要特别计算 除数不能为0
		operd.push(result);
	}
}
 
int priority(string op)
{
	int pri;
	if (op == "(") //（ 都小于其他 
		pri = 0;
	else if (op == "+" || op == "-")
		pri = 1;
	else if (op == "*" || op == "/")
		pri = 2;
	return pri;
}
 
vector<string> Amend(string s)
{
	size_t len = s.size();
	vector<string> exp;
	size_t pos = 0;
	while ((pos = s.find_first_of(" ()/*-+0123456789", pos)) != string::npos)
	{
		if (s[pos] == '(' || s[pos] == ')' || s[pos] == '+' || s[pos] == '-' || s[pos] == '/' || s[pos] == '*')
		{
			exp.push_back(s.substr(pos, 1));//返回字符串
			pos++;//加1继续找
		}
		else if (s[pos] == ' ')
		{
			pos++;
			continue;
		}
		else
		{
			size_t p;
			exp.push_back (to_string(stoi(s.substr(pos),&p)));
			pos = p + pos;
		}
	}
 
	return exp;
}
 
int expression(string str)
{
	stack<int> oand; //这个压入操作数 栈
	stack<string> otor; //这个是压入操作符 栈
 
	vector<string> exp = Amend(str);//修正
	size_t len = exp.size(); //修正之后的表达式大小
 
	for (size_t ix = 0; ix != len; ++ix)
	{
		string token = exp[ix]; //循环逐步读取 每个 标记
		if (token == "+" || token == "/" || token == "-" || token == "*")
		{
			if (otor.size() == 0)
				otor.push(token);
			else //进到这肯定有操作符
			{
				int pri_token = priority(token);//现在的操作符的优先级
				string stoken = otor.top();
				int pri_stack = priority(stoken);//在栈的操作符的优先级
 
				if (pri_token > pri_stack)
					otor.push(token);
				else
				{
					while (pri_token <= pri_stack) //当前的操作符小于栈里的操作符
					{
						otor.pop();
						calculate(oand, stoken); 
						if (otor.size() > 0) //如果操作符栈里还有
						{
							stoken = otor.top(); //再次读取当前操作符栈里的操作符
							pri_stack = priority(stoken); //这里计算优先级
						}
						else
							break;
					}
					otor.push(token); //压入新的
				}
			}
		}
		else if (token == "(") //直接压入（
			otor.push(token);
		else if (token == ")") // 这里计算圆括号里的表达式
		{
			if (otor.top() == "(")
			{
				otor.pop();
				break;
			}
			else
			{
				while (otor.top() != "(")
				{
					string stor = otor.top();
					calculate(oand, stor);
					otor.pop();
				}
				otor.pop();  //弹出（
			}
		}
		else
			oand.push(stoi(token)); //stoi 直接 转换 字符串 
	}
	while (otor.size() != 0) //如果再之后的里面还剩余操作符和操作数
	{
		string stor = otor.top();
		calculate(oand, stor);
		otor.pop();
	}
	return oand.top(); //返回最终值
}
 
int main()
{
	string str("(12+12)*2/2-(1+1)*2");
	cout << expression(str) << endl;
	for (auto i : Amend(str))
		cout << i << ' ';
	cout << endl;
	
	return 0;
}

```
![login](https://github.com/UpCris/homework_2/blob/master/952.png)

## 10.3

```C++
#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>
#include <numeric>

using namespace std;

int main(int argc,char *argv[]){
	

	vector<int> ivec;
	int val;
	
	while ( cin >> val)
		ivec.push_back(val);

	cout << "序列中的整数之和为" << accumulate(ivec.begin(),ivec.end(),0) << endl;
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/103.png)

## 10.15

```C++
#include <iostream>

using namespace std;

void add(int a){
	auto sum = [a] (int b) {return a+b;};

	cout << "捕获的参数 " << a << endl;
	cout << "参数的和" << sum(1) << endl; 
}

int main(int argc, char *argv []){
	add(1);
	add(2);

	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/1015.png)

## 10.34

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main(){
	vector<int> ivec{1,2,3,4,5};

	for(auto r_i = ivec.crbegin(); r_i != ivec.crend(); ++r_i)
		cout << *r_i << " ";
	cout << endl;
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/1034.png)
