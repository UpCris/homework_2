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
