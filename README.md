# homework_2
---
### 9.11 
- 问：对6种创建和初始化`vector`对象的方法，每一种都给出一个实例。 解释每个`vector`包含什么值。


```C++
vector<int> ivec;  //0
vector<int> ivec(10); //0 
vector<int> ivec(10,1); //1
vector<int> ivec{0,1,2,3,4}; //0,1,2,3,4
vector<int> ivec(other_vec); //same as other vec
vector<int> ivec(other_vec.begin(),other_vec.end()); //same as other_vec
```

---
---


### 9.12 


- 问：对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

1. 我们可以用接受两个迭代器创建拷贝的构造函数去复制一个容器的某一部分内容；但是如果使用接受一个容器创建其拷贝的方法，只能复制整个容器。

1. 当一个容器初始化为另一个容器的拷贝时，两个容器的容器类型和元素类型都必须相同。当传递迭代器参数来拷贝一个范围时，就不要求容器类型和

元素类型是相同的。

---
---

### 9.13


- 问：如何从一个`list<int>`初始化一个`vector<double>`?从一个`vector<int>`又该如何创建？编写代码验证你的答案。
	
```C++
  1 /*
  2  * Copyright [2019]
  3  */
  4 
  5 #include<iostream>
  6 #include<vector>
  7 #include<list>
  8 
  9 int main() {
 10     std::list<int>ilist = {1 , 2 , 3 , 4 , 5};
 11     std::vector<int>ivec = {5 , 4 , 3 , 2 , 1};
 12 
 13     std::vector<double>ivec_1(ilist.begin() , ilist.end());
 14     std::vector<double>ivec_2(ivec.begin() , ivec.end());
 15     
 16     std::cout << ivec_1.capacity() << " " << ivec_1.size() << " " <<ivec_1[0] << " " << ivec_1[ivec_1.size()-1] << " " << std::endl;
 17     std::cout << ivec_2.capacity() << " " << ivec_2.size() << " " <<ivec_2[0] << " " << ivec_2[ivec_2.size()-1] << " " << std::endl;
 18     
 19     return 0;
 20 }
            
```
![login](https://github.com/UpCris/homework_2/blob/master/912.png)

---
---


### 9.20


- 问：编写程序，从一个`list<int>`拷贝元素到两个`deque`中。值为偶数的所有元素拷贝到一个`deque`中，而奇数值元素都拷贝到另一个`deque`中。  

```C++
#include<iostream>
#include<list>
#include<deque>

using namespace std;

int main() {
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


---
---


### 9.29


- 问：假定`vec`包含25个元素，那么`vec.resize(100)`会做什么？如果接下来调用`vec.resize(10)`会做什么？

1. 会默认初始化75个元素添加到vec的末尾。

1. 会将`vec`中的后90个元素删除。

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

---
---


### 9.43


- 问：编写一个函数，接受三个`string`参数是`s`、`oldVal` 和`newVal`。使用迭代器及`insert`和`erase`函数将`s`中所有`oldVal`替换为`newVal`。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

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


---
---


### 9.52


- 问：使用`stack`处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从`stack`中`pop`对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）push到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。

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
---
---
```
![login](https://github.com/UpCris/homework_2/blob/master/952.png)

---
---


### 10.3

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

---
---


### 10.15


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

---
---


### 10.34


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

---
---


### 11.12


```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

using namespace std;

int main(){
	vector<pair<string,int>>ivec;
	string s;
	int v;
	while (cin >> s && cin>> v)
		ivec.push_back(pair<string,int>(s,v));
	
	for (const auto &d : ivec)
		cout << d.first << " " << d.second << " " << endl;

	return 0;
}

```
![login](https://github.com/UpCris/homework_2/blob/master/pair.png)

---
---

### 11.17


- 问：假定 c 是一个`string`的`multiset`，`v`是一个`string`的`vector`，解释下面的调用。指出每个调用是否合法：

```C++
copy(v.begin(), v.end(), inserter(c, c.end()));//将v中元素依次插入到c的尾部
copy(v.begin(), v.end(), back_inserter(c));//将v中元素依次插入到c的尾部
copy(c.begin(), c.end(), inserter(v, v.end()));//将c中元素依次插入到v的尾部
copy(c.begin(), c.end(), back_inserter(v));//将c中元素依次插入到v的尾部
```
---
---
### 13.12

三次析构函数的调用。

-函数结束时：

1. 局部变量`item1`,`item2`的生命期结束，被销毁，`Sales_data`的析构函数被调用。

1. 参数`accum`的生命期结束，被销毁，调用`Sales_data`的析构函数。

---
---


### 13.18


```C++
#include <iostream>
#include <string>

using namespace std;

class Employee {
	private:
		static int sn;
	public:
		Employee () { mysn = sn++; }
		Employee ( const string &s ) { name = s; mysn = sn++;}

		const string &get_name() { return name;}
		int get_mysn() { return mysn; }

	private:
		string name;
		int mysn;
};

int Employee::sn = 0;

void f(Employee &s){
	cout << s.get_name() << ":" << s.get_mysn() << endl;
}

int main(int argc,char **argv){
	Employee a("赵"), b("林"), c("叶 ");
	f(a);
	f(b);
	f(c);
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/1318.png)

---
---


### 13.46


- 问：什么类型的引用可以绑定到下面的初始化器上？


```C++
int f() { return 1; }
	vector<int> vi(100);
	int && ri = f();       //返回非引用类型的函数，生成右值
	int & r2 = vi[0];      //vi[0]具有持有状态，左值
	int & r3 = r1;         //变量是左值
	int && r4 = vi[0] * f();  //表达式求值过程中创建的临时对象是右值

```
---
---
### 13.49


- 问：为你的StrVec、String和Message类添加一个移动构造函数和一个移动赋值运算符。


```C++
	StrVec(StrVec &&s) noexcept : elements(s.elements), first_free(s.first_free), cap(s.cap)
	{ s.elements = s.first_free = s.cap = nullptr; }
	StrVec operator=(StrVec &&rhs)noexcept
	{
		if (this != &rhs) {
			free();
			elements = rhs.elements;
			first_free = rhs.first_free;
			cap = rhs.cap;
			rhs.elements = rhs.cap = rhs.first_free = nullptr;
		}
		return *this;
	}
```


```C++
	String(String &&s) :str(s.str) noexcept
	{
		s.str.clear();
	}
	String operator=(String &&rhs) noexcept
	{
		if (this != &rhs) {
			str.clear();
			str = rhs.str;
		}
		return *this;
	}
```

```C++
Message::Message(Message &&m) :contents(std::move(m.contents)) 
{
	move_Folders(&m);
}
Message Message::operator=(Message &&rhs)
{
	if (this != &rhs) {
		remove_from_Folders();
		contents = std; :move(rhs.contens);
		move_Folders(&rhs);
	}
	return *this;
}
```
---
---

### 13.58


- 问： 编写新版本的Foo类，其sorted函数中有打印语句，来验证你对前面两题的答案是否正确。


```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Foo{
	public:
		Foo sorted() &&;
		Foo sorted() const &;

	private:
		vector<int> data;
};

Foo Foo::sorted() &&
{
	cout << "右值引用版本" << endl;
	sort( data.begin() , data.end());
	return *this;
}

Foo Foo::sorted() const &{
	cout << "左值引用版本" << endl;
	Foo ret(*this);
	return ret.sorted();
}

int main(int argc, char **argv){
	Foo f;
	f.sorted();
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/%E5%B7%A6%E5%8F%B3%E5%80%BC%E7%9A%84%E5%BC%95%E7%94%A8.png)



### 14.3


- (a) 都不是 (b)string © vector (d) string

---
---

### 14.20


```C++
istream& Sales_data::operator>>( istream &is, Sales_data &rhs ){
    is >> rhs.bookNo;
    is >> rhs.units_sold;
    is >> rhs.selling_price;
    is >> rhs.sales_price;
    if( is && !selling_price )
        discount = sales_price / selling_price;
    else
        rhs = Sales_data();
    return is;
}
 
ostream& Sales_data::operator<<( ostream &os, const Sales_data &rhs ){
    os << rhs.bookNo << " "
        << rhs.units_sold << " "
        << rhs.selling_price << " "
        << rhs.sales_price << " "
        << rhs.discount; //不添加换行，换行自行添加，增加操作的灵活性。
    
    return os;
}
 
Sales_data& Sales_data::operator+=( const Sales_data &rhs ){
//同一本书的零售价应该是相同的，所以selling_price应该相同。
    sales_price = ( sales_price * units_sold + rhs.sales_price * rhs.units_sold )
                / ( units_sold + rhs.units_sold );
    units_sold += rhs.units_sold;
    if( selling_price != 0 )
        discount = sales_price / selling_price;
        
    return *this;
}
 
Sales_data Sales_data::operator+( const Sales_data &lhs, const Sales_data &rhs ){        
//默认认为两个Sales_data对象的bookNo相同
    Sales_data tmp( lhs );
    tmp += rhs;
    
    return tmp;
}

```
---
---


### 14.38


```C++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

class strlenis
{
	public:
		strlenis(int len ) : len (len){}
		bool operator() (const string &str) {return str.length () == len ;}

	private:
		int len;
};

void readstr (istream &is, vector<string> &vec)
{
	string word;
	while (is >> word)
	{
		vec.push_back(word);
	}
}

int main(){
	vector<string> vec;
	readstr(cin , vec);
	const int minlen = 1;
	const int maxlen = 10;
	for ( int i = minlen; i <= maxlen; i++){
		strlenis slenis(i);
		cout << "len: " << i << ",cnt: " << count_if(vec.begin(), vec.end(),slenis) << endl;
	}
	return 0;
}
```

![login](https://github.com/UpCris/homework_2/blob/master/%E8%AE%A1%E5%8D%95%E8%AF%8D%E9%95%BF%E5%BA%A6%E5%92%8C%E6%95%B0%E7%9B%AE.png)

---
---


### 14.52


- 问：在下面的加法表达式中分别选用了哪个operator？列出候选函数、可行函数及为每个可行函数的实参执行的类型转换。


```C++
struct longDouble {
	//用于演示的成员operator+; 在通常情况下+s是个非成员
	longDouble operator+(const SmallInt&);
	//其他成员与14.9.2节一致
};
longDouble operator+(longDouble&, double);
SmallInt si;
longDouble ld;
ld = si + ld;
ld = ld + si;
```
1. 对于ld=si+ld，由于LongDouble不能转换为SmallInt，因此Smallint的成员operator+和friend operator都不可行。

2. 由于Smallint不能转换为LongDouble，LongDouble的成员operator+和非成员operator+也都不可行。

3. 由于SmallInt可以转换为int， LongDouble了可以转换为float和double，所以内置的operator+(int, float)和operator+(int, double)都可行，会产生二义性。

4. 对于ld=ld+si，类似上一个加法表达式，由于Smallint不能转换为double，LongDouble也不能转换为SmallInt，因此SmallInt的成员operator+和两个非成员operator+都不匹配。

5. LongDouble的成员operator+可行，且为精确匹配。

6. SmallInt可以转换为int，longDouble可以转换为float和double，因此内置的operator+(float, int)和operator(double, int)都可行。但它们都需要类型转换，因此LongDouble的成员operator+优先匹配。

---
---


### 15.12


- 问:有必要将一个成员函数同时声明成override和final吗？ 

1. 如果希望编译器帮助我们检查是否覆盖了相应的虚函数，同时，禁止派生的类覆盖该函数，那么就有必要同时声明成override和final。
2. 而且这样能使得我们的意图更加清晰。

---
---


### 15.16


- 改写你在15.2.2节（第533页）练习中编写的数量受限的折扣策略，令其继承Disc_quote。
```C++
class Limit_quote : public Disc_quote
{
public:
    Limit_quote() = default;
    Limit_quote(const std::string& b, double p, std::size_t max, double disc):
        Disc_quote(b, p, max, disc)  {   }

    double net_price(std::size_t n) const override
    { return n * price * (n < quantity ? 1 - discount : 1 ); }
};
```
---
---


### 15.20


```C++
#include <iostream>

using namespace std;

class base
{
	public:
		void pub_mem();
	protected:
		int prot_mem;
	private:
		char priv_mem;
};

struct pub_derv:public base
{
	int f() { return prot_mem; }
	void memfcn (base &b){
		b = *this;
		cout << "pub_derv" << endl;
	}
};

struct priv_derv:private base
{
        int f1() { return prot_mem; }
        void memfcn (base &b){
                b = *this;
                cout << "priv_derv" << endl;
        }
};

struct prot_derv:protected base
{
        int f2() { return prot_mem; }
        void memfcn (base &b){
                b = *this;
                cout << "prot_derv" << endl;
        }
};

struct derived_from_public:public pub_derv{
	int use_base() { return prot_mem;}
	void memfcn(base &b)
	{
		b = *this;
		cout << "derived_from_public" << endl;
	}
};

struct derived_from_protected:protected prot_derv{
        int use_base() { return prot_mem;}
        void memfcn(base &b)
        {
                b = *this;
                cout << "derived_from_protected" << endl;
        }
};

int main( int argc, const char *argv[]){
	pub_derv d1;
	priv_derv d2;
	prot_derv d3;
	derived_from_public dd1;
	derived_from_protected dd3;
	base base_1;
	base *p = new base;
	p = &d1;
	p = &dd1;
	d1.memfcn(base_1);
	d2.memfcn(base_1);
	d3.memfcn(base_1);
	dd1.memfcn(base_1);
	return 0;
}
```
![login](https://github.com/UpCris/homework_2/blob/master/1520.png)

---
---


### 15.30


- 编写你自己的Basket类，用它计算上一个练习中交易记录的总价格。
```C++
class Basket
{
public:
    void add_item(const shared_ptr<Quote> &sales)
    {
        items.insert(sales);
    }
    double total_receipt (std::ostream&) const;     // 打印每本书的总价和购物篮中所有书的总价
private:
    static bool compare(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs)
    {
        return lhs->isbn() < rhs->isbn();
    }
    // multiset保存多个报价，按照compare成员排序
    std::multiset<std::shared_ptr<Quote>, decltype(compare)*> items{compare};
};

double Basket::total_receipt(std::ostream &os) const
{
    double sum = 0.0;

    for (auto iter = items.cbegin(); iter != items.cend(); iter=items.upper_bound(*iter))
    {
        sum += print_total (os, **iter, items.count(*iter));
    }
    os << "Total Sale: " << sum << endl;
    return  sum;
}
```
---

