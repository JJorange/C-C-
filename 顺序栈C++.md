```c++
#include<stdlib.h>
#include<iostream>
#include<assert.h>

using namespace std;
const int stackIncreament = 20;//扩容大小

template<class T>
class Stack
{
public:
	//构造函数
	Stack(int sz = 50)
	{
		top = -1;
		maxSize = sz;
		elements = new T[maxSize];
		assert(elements != NULL);
	}
	~Stack()						//析构函数
	{
		delete[]elements;
	}
	void push(const T& x);			//压栈操作
	bool pop(T& x);					//出栈操作
	bool getTop(T& x)				//取栈顶元素
	{
		if (IsEmpty() == true)
			return false;
		x = elements[top];
		return true;
	}
	//判空
	bool IsEmpty()const
	{
		return (top == -1) ? true : false;
	}
	//判满
	bool IsFull()const
	{
		return (top == maxSize - 1) ? true : false;
	}
	//返回栈中元素个数
	int getSize()const
	{
		return top + 1;
	}
	//将栈置空
	void makeEmpty()
	{
		top = -1;
	}



private:
	T *elements;				//存放栈中元素的栈数组
	int top;					//栈顶指针
	int maxSize;				//栈最大可容纳元素个数
	void overFloeProcess();		//栈的溢出处理
};

//私有函数，扩充栈的存储空间
template<class T>
void Stack<T>::overFloeProcess()
{
	T * newArray = new T[maxSize + stackIncreament];
	if (newArray == NULL)
	{
		cerr << "内存分配失败！" << endl;
		exit(1);
	}
	for (int i = 0; i <= top; i++)
	{
		newArray[i] = elements[i];
	}
	maxSize += stackIncreament;
	delete[]elements;
}

//压栈操作
template<class T>
void Stack<T>::push(const T& x)
{
	while (IsFull() == true){
		overFloeProcess();
	}
	elements[++top] = x;
}

//出栈操作
template<class T>
bool Stack<T>::pop(T& x)
{
	if (IsEmpty() == true)
		return false;
	x = elements[top--];//返回栈顶元素值
	return true;
}

//括号匹配
void PrintMatchedPairs(char * exprssion, int size)
{
	if (exprssion == NULL)
		return;
	Stack<int> s(size);
	int x;//pop返回的值
	int n;//栈中元素的个数
	for (int i = 1; i <= size; i++)
	{
		if (exprssion[i-1] == '(')
			s.push(i);
		else if (exprssion[i-1] == ')')
		{
			if (s.pop(x) == true)
				cout << x << " 与 " << i << "匹配" << endl;
			else
				cout << "没有与第" << i << "个右括号匹配的左括号！" << endl;
		}
	}
	while (s.IsEmpty() == false)
	{
		s.pop(x);
		cout << "没有与第" << x << "个右括号匹配的左括号！" << endl;
	}
}


int main()
{
	char * c = "(0)ii(ooio(o)))";
	cout << strlen(c) << endl;
	PrintMatchedPairs(c, strlen(c));
	return 0;
}


```
