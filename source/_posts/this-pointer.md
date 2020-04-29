# C++ this pointer
在标准文档草案N4659中， this关键字的解释如下：
1. 在非静态成员函数的函数体内，关键字this是一个预定义的表达式，它的值为调用该函数的对象的地址。(例如，在class X的非静态成员函数中，this的类型为X *)。如果成员函数被定义为const, volatile或者const volatile， 则对应的this 类型为const X *, volatile X *或者 const volatile X *。


2. const 和 volatile 修饰符在存取对象及其非静态数据成员时生效。

3. 一个含有const,volatile修饰符的成员函数仅能够被至多含有相同修饰符的类对象表达式调用。
例如：

```C++
	class X {
	public:
		int getNum() const { return num; }
		void setNum(int num) {m_num = num;}
	private:
		int m_num;
   };


   void example() {
	   X m;
	   const X n;

	   m->setNum(); // ERROR
	   
	   n->setNum(); // OK
   }
```

4. 构造和析构函数不能被声明为const, volatile或者const volatile。 但在它们内部可以调用含有这些修饰符的函数。
