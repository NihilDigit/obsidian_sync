### 用complex.h定义复数

1. 实现输入输出
```cpp
#include <iostream>
using namespace std;
```
> `iostream` 是 C++ 标准库中的一个头文件，它提供了输入输出（I/O）功能的基本支持。`iostream` 包含了定义标准输入输出流的类和对象，这些流用于从键盘读取输入和向屏幕输出文本。

> 命名空间是一种将相关的类、函数、变量等组织在一起的机制，用于避免命名冲突。`std` 是标准库中定义的一个命名空间，它包含了标准库中的所有类和函数。使用 `using namespace std;` 可以在当前作用域中直接使用 `std` 命名空间中的名称，而不需要每次都加上 `std::` 前缀。但是，过度使用 `using namespace` 可能会导致命名冲突，因此建议谨慎使用。

1. 定义类`Complex`:
```cpp
class Complex {
private:
	--snip--
public:
	--snip--
}
```
>数据封装是面向对象编程的一个重要特点，它防止函数直接访问类类型的内部成员。类成员的访问限制是通过在类主体内部对各个区域标记 `public`、`private`、`protected` 来指定的。关键字 `public`、`private`、`protected` 称为访问修饰符。

>一个类可以有多个 `public`、`protected` 或 `private` 标记区域。每个标记区域在下一个标记区域开始之前或者在遇到类主体结束右括号之前都是有效的。成员和类的默认访问修饰符是 `private`。

3. 封装虚部和实部
```cpp
private:
	double real;  
	double imag;
```
> 将 `double` 型的 `real` 和 `imag` 封装到类的 `private` 区域是一种常见的做法，这样做可以提高类的封装性和安全性。通过将这些变量设置为私有成员，你可以防止它们被类外部的代码直接访问和修改，从而减少因误操作而导致的错误。

4. 构造函数
```cpp
public:
	Complex();
	Complex(double a, double b);
--snip--
```
>C++ 中的构造函数是一种特殊的成员函数，它在创建类的对象时自动调用，用于初始化对象的成员变量。构造函数的名称必须与类名相同，并且不能有返回类型。

>构造函数可以有参数，也可以没有参数。有参数的构造函数称为参数化构造函数，它可以用于初始化对象时提供特定的初始值。没有参数的构造函数称为默认构造函数，如果没有提供任何构造函数，编译器会自动生成一个默认的构造函数。

```cpp
Complex::Complex() {
	real = 0;
	imag = 0;
}
Complex::Complex(double a, double b) {
	real = a;
	imag = b;
}
```
> 在类外实现类内定义的成员函数时，需要使用类名和作用域解析运算符 `::` 来指定这个函数属于哪个类。这种方式可以清晰地表明函数的归属，同时也允许在类的定义和实现之间进行分离，使得代码更加模块化和易于维护。

5. 其他成员函数及其实现
```cpp
public:
	--snip--
	[[nodiscard]] double GetReal() const;  
	[[nodiscard]] double GetImag() const;  
	void Display() const;
```

>`[[nodiscard]]` 是 C++17 引入的属性，用于提示编译器该函数的返回值不应被忽略。

> 以`const` 声明的常量成员函数不会修改任何成员变量的值，提高了安全性

```cpp
double Complex::GetReal () const {  
    return real;  
}  
double Complex::GetImag() const {  
    return imag;  
}  
void Complex::Display() const {  
	if (real > 0) cout<<real;  
	else if (real < 0) cout<<"- "<<-real;
    if (imag < 0 && imag != -1) cout<<" - "<<-imag<<"i ";  
    else if (imag > 0 && imag != 1) cout << " + " << imag << "i ";  
    else if (imag == 1) cout << "+ i";  
    cout<<endl;  
}
```

6. 运算符重载
```cpp
Complex operator-(Complex a) const;
friend Complex operator+(Complex a, Complex b);
```
> 运算符重载允许我们为类实例定义运算符（如`+`, `-`等）的操作。这意味着我们可以指定当这些运算符用于我们的自定义类型时应该做什么。

> 在C++中，`friend`关键字用于声明友元函数。友元函数虽然不是类的成员函数，但它可以访问该类的所有私有（private）和保护（protected）成员。在这里，`operator+`被声明为`Complex`类的友元，这样它就可以访问`Complex`对象的私有或保护成员，如其实部和虚部，从而能够直接进行计算。

```cpp
Complex Complex::operator- (Complex a) const {  
    Complex c;  
    c.real = this -> real - a.real;  
    c.imag = this -> imag - a.imag;  
    return c;  
}
```
> 在C++中，类的成员函数有一个隐含的调用对象，通常通过指针`this`表示。当你调用一个对象的成员函数时，这个对象会自动成为该函数的隐含调用者，并且可以通过`this`指针在函数内部访问。

```cpp
Complex operator+ (Complex a, Complex b) {  
    Complex c;  
    c.real = a.real + b.real;  
    c.imag = a.imag + b.imag;  
    return c;  
}
```
 
> 加法运算符 `+` 被重载为一个友元函数。这是因为加法操作通常是对称的，即 `a + b` 和 `b + a` 应该得到相同的结果。将加法运算符重载为友元函数而不是成员函数，有助于强调这种对称性。作为友元函数，它可以直接访问两个操作数的私有成员，而不偏向于任何一方。这样，我们可以更自然地实现加法运算，使得 `operator+` 函数能够平等地处理两个操作数。

### complex.h
```cpp
#ifndef COMPLEX_H  
#define COMPLEX_H  
  
#include "iostream"  
using namespace std; //Basic IO service  
  
class Complex {  
  
private:  
    double real;  
    double imag;  
  
public:  
    Complex(); //Empty Cons  
    Complex(double a, double b); //Build from a number  
    [[nodiscard]] double GetReal() const;  
    [[nodiscard]] double GetImag() const;  
    void Display() const;  
    Complex operator-(Complex a) const; //Reload '-' when appears '- a'  
    friend Complex operator+(Complex a, Complex b); //'friend' allows '+' to use private members  
};  
  
Complex::Complex() {  
    real = 0;  
    imag = 0;  
}  
Complex::Complex(double a, double b) {  
    real = a;  
    imag = b;  
}  
double Complex::GetReal () const {  
    return real;  
}  
double Complex::GetImag() const {  
    return imag;  
}  
void Complex::Display() const {  
	if (real > 0) cout<<real;  
	else if (real < 0) cout<<"- "<<-real;
    if (imag < 0 && imag != -1) cout<<" - "<<-imag<<"i ";  
    else if (imag > 0 && imag != 1) cout << " + " << imag << "i ";  
    else if (imag == 1) cout << "+ i";  
    cout<<endl;  
}  
  
Complex Complex::operator- (Complex a) const {  
    Complex c;  
    c.real = this -> real - a.real;  
    c.imag = this -> imag - a.imag;  
    return c;  
}  
  
  
Complex operator+ (Complex a, Complex b) {  
    Complex c;  
    c.real = a.real + b.real;  
    c.imag = a.imag + b.imag;  
    return c;  
}  
#endif //COMPLEX_H
```
### 使用complex.h
```cpp
#include "iostream"  
#include "complex.h"  
using namespace std;  
int main() {  
    Complex a(2.3, 8.6), b(4, 9), c;  
    c = a + b;  
    cout<<"a + b = ";  
    c.Display();  
    c = a - b;  
    cout<<"a - b = ";  
    c.Display();  
    return 0;  
}
```
输出应该为
```shell
a + b = 6.3 + 17.6i 
a - b = -1.7 - 0.4i 
```
