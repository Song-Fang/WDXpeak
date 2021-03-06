#The Design and Evolution of C++ （C++语言的设计和演化）
---

##1.C++的史前时代

C++最初设计的准则

1.好的工具应该有对程序组织的支持：类，某种形式的类分层形式；对并发的某种形式的支持；对于基于类的类型系统的强检查

2.好的工具产生的代码能像BCPL运行的一样快，再把通过分别编译得到的程序单元组合成整个程序也应该像BCPL一样有效

3.好的工具应该允许高度可移植的实现，移植时不需要复杂的运行支持系统。

##2.C With Class

1.作为程序设计语言的设计者，并没有理由去强迫程序员使用某种特定的风格。但是有义务去鼓励和支持各种各样的被证明有效的风格与实践；还应当提供合适当的语言特性和工具，以帮助程序员避免公认的陷阱和圈套。

2.类的关键设计决策

- 让程序员去描述类型，通过这些类型建立对象，一个类就是一个类型。
- 把用户定义类型的对象的表示多作为类定义的一部分。
- 采用编译时的访问控制来限制对实际表示的访问。
- 对于函数成员，需要描述其完整的类型（返回类型和参数类型），编译时会做静态的类型检查。
- 函数定义和声明分开，使类声明更像一个接口描述，更容易进行分别编译
- `new`保证了构造函数一定被调用
- 同时提供指针和非指针类型
- 对象有3种分配方式：栈，静态，堆，为在堆上的成员分配和释放提供了`new`和`delete`两个操作

3.设计规则：用户定义类型和内部类型与各语言规则的关系应该是一样的，应该能从语言和工具方面得到同样程度的支持。

4.设计规则：只提供一个特征是不够的，还必须以一种实际上可以负担的起的形式来提供它。

5.`inline`机制:
- 程序员可以决定编译器把那些函数变成`inline`，不鼓励`inline`函数的过度使用。
- 关键字`inline`对于编译器来说是一种提示，编译器可以忽略。
- `inline`函数在整个程序中必须有唯一的定义。

6.连接模型
- 分别编译应该能使用传统的C/Fortran、UNIX/DOS风格的连接器。
- 连接应该具有类型安全性
- 连接过程中不应该要求有任何形式的数据库
- 应该很容易并且高效的与采用其他语言（C、汇编）写出的程序片段连接到一起

7.按名称等价是C++类型系统的基石，布局相容性规则保证了可以使用显示转换，以便能提供低级的转换服务。

8.最早形成的关于C with Class的设计目标中有一个要求：不应该使用比线性检索更复杂的算法。

9.决定只将无条件警告的功能用在那些“具有超过90%的可能性造成实际错误”的情况，确保忽略C++的警告将被看做是一种愚蠢的行为。

10.为什么是C？
- 灵活，高效，可用，可移植。

11.一个程序设计语言要服务于两种目的：
- 它为程序员提供了一种载体，使他们能描述需要执行的动作
- 提供了一组概念，供程序员借住他们思考什么东西是能做的

12.多数人过分关注语法的问题而损害了类型方面的问题。
- C++设计中，最关键的问题总与类型、歧义性、访问控制等有关。

13.C++的保护概念：
- 保护是通过编译时的机制提供的，目标是防止意外事件，而不是防止欺骗或者有意的侵犯
- 访问权是由类本身授权的，而不是单方面的取用
- 访问权控制是通过名称实行的，并不依赖于被命名事物的种类
- 保护的单位是类，而不是个别对象
- 收控制的是访问权，而不是可见性

14.操作系统的读/写保护的概念融入了C++的`const`概念

15.`new`将调用某种分配函数获得存储，而后调用一个构造函数去初始化这些存储；`delete`是被引进了作为`new`的对应物。

16.还考虑过直接支持并发的问题，但拒绝了，因为更喜欢基于库的方式。

##3.C++的诞生

1.虚函数是C++最显著的新特征，是对人们用这个语言进行程序设计风格影响最大的东西。

2.对象布局模型：实现中最关键的思想，就是把在一个类中定义的虚函数集合定义为一个指向函数的指针数组。这样对于虚函数的调用，变成了通过该数组而进行的简单的间接函数调用。

3.虚函数只能被派生类中的函数覆盖，该函数应该具有同样的名称、同样的参数和返回类型。

4.派生类中的名称将遮蔽基类中具有相同名称的任何对象或函数。

5.一种特征能够怎样被用好，要比它可能怎样被用错更重要的多。

6.运算符重载是C++语言中最主要的一种财富。

7.凡是声明为`explict`的构造函数只能采用显示的对象构造，不能用于隐式转换。

8.应该白重载操作作为一种扩展语言的机制，而不是改变语言的机制。

9.引入引用机制主要也是为了支持运算符的重载。

10.所有的函数调用都在编译期进行检查，可以通过函数声明中的特殊描述形式抑制对最后一些参数的检查。
```
int printf(const char*, ...);
```

11.引进了具有BCPL风格的注释，同时允许C风格和BCPL风格注释
```
int a; /*C风格注释*/
int b; //BCPL风格注释
```

12.构造函数和析构函数都遵循与其他函数一样的访问控制规则。
- 带来有用的技术，通过把执行操作的函数隐藏起来，以达到控制操作的目的。

13.C++引入`::`来表示类的成员关系，而将`.`保留专用于对象的成员关系。

14.允许把声明写在需要它的任何地方：
- 对于引用和常量而言，这种风格是根本性的，因为这些东西不能赋值；
- 对那些采用默认初始化方式代价特别高的类型，从本质上说，这种机制的效率更高。
- 为了尽量减少由于未经初始化的变量带来的问题，利用构造函数来保证初始化。

15.在一个`for`语句的初始化部分引进的名称，其作用域到这个for语句的最后结束。

16.C++与ANSI C之间不应该存在无故的不兼容性，而确实应该有一些不兼容性，只要他们不是无故的。

17.从根本上说，静态类型检查的概念被看成是为程序提供一种尽可能强的保证，而不仅仅被作为一种取得运行效率的手段。

##4.C++语言设计规则

1.C++设计目标：

>C++应该使认真的程序员能够觉得编程变得更愉快了

>C++是一种通用的程序设计语言，它应该：
- 是一种更好的C
- 支持数据抽象
- 支持面向对象的程序设计

2.设计支持规则
- 支持健全的设计概念
- 为程序的组织提供各种机制
- 直接说出你的意思
- 所有特征都必须是能够负担的
- 允许一个有用的特征比防止各种错误使用更重要
- 支持从分别开发的部分出发进行软件的组合

3.允许用语言本身表达所有重要的东西，而不是在注释中或者通过宏这类黑客手段。

4.语言的技术性规则
- 不隐式的违反静态类型系统
- 为用户定义类型提供与内部类型同样好的支持
- 局部化是好事情
- 避免顺序依赖：如果交换两个声明会导致不同的意思，那么这就应该是个错误。
- 如果有疑问，就选择该特征的最容易说清除的形式
- 语法是重要的：保证类型系统的一致性，一般而言，保证语言的语义清晰、定义良好是最基本的东西。
- 清除使用预处理程序的必要性

5.低级程序设计支持规则
- 使用传统的（笨）连接器：容易移植，容易与其他语言写的软件相互操作
- 没有无故的与C的不兼容性
- 在C++下面不为更低级的语言留下空间（除汇编语言之外）
- 对不用的东西不需要付出代价（0开销规则）
- 遇到有疑问的地方就提供手工控制的手段

##6.标准化

1.通过两个规则去尽可能减少产生顺序依赖的可能性：
- 类型重新定义规则：如果一个类型已在某个类中使用过，就不能在那里重新定义
- 重写规则：对于直接写在类声明中的成员函数进行分析时，就像它们实在类声明之后重新定义的那样做

##8.库

1.对库的建设者和使用者的建议：不要去与类型系统作斗争；C++的基本结构鼓励一种强类型风格的程序设计。

2.对并行的支持永远是各种库和语言扩充的丰富源泉。

3.基础库
- 水平的基础库:提供了基本的类，以便在每个应用中对每个程序员都能有所帮助。
- 垂直的基础库：为么某个特定的环境提供一个完整的服务集合。

##9.展望

1.C++取得预期成功
- C++能生成运行时和空间特性极好的代码，这种代码能和C语言媲美；
- C++使人可以将用它写出的代码集成到常规系统中，并能在传统系统中成长；
- C++允许人民逐步转移到新的程序设计技术上。

2.始终把静态类型检测看做最根本的东西，无论是对于好的设计，还是对于运行时的效率。

3.环境与语言的隔离是最根本的东西。

4.我构造C++是想作为一座桥梁，程序员能够借助它，从传统的程序设计过渡到基于数据抽象和面向对象的程序设计。

5.C++最有实力的地方并不是他的某个独到之处特别伟大，而在于它在事物的大范围变化中都表现的不错。

##10.存储管理

1.运算符`new`的工作是保证互相分离的存储分配和初始化能正确的一起使用。从逻辑上看`new`是在构造函数之前调用的，因此它必须返回一个`void *`。
```
class X
{
	void * operator new(size_t);
    void operator delete(void *);
    
    void * operator new[](size_t);
    void operator delete[](void *);
};
```

2.放置
> 我们需要一种机制把对象安放到某个特定的地址

> 我们需要一种机制在某个特定分配区里分配对象

解决方案是允许重载`operator new()`，以便new运算符提供一种允许附加参数的语法形式。

3.在`operator new()`和`operator delete()`之间有一种明显的、经过深思熟虑的不对称性：前者能重载而后者不能。

4.存储器耗尽：
- 在任何情况下，当一个库调用因为存储耗尽而失败时，用户都应该能够取得控制。
- 不能要求用户在每次分配操作之后去测试存储耗尽的情况。

##11.重载

1.C++重载时区分了5种匹配：
- 匹配中不转换或者只使用不可避免的转换：数组名到指针，函数名到函数指针，`T`到`const T`
- 使用了整数提升的匹配以及`float`到`double`
- 使用了标准的类型转换：`int`到`double`，`derived *`到`base *`，`unsigned int`到`int`
- 使用了用户定义转换的匹配
- 使用了在函数声明里的省略号`...`的匹配

2.对于只有一个参数的函数，选择在上表中最好的匹配，如果存在两个最好的匹配，这个调用就是有歧义的，会产生一个编译错误。

3.对涉及多于一个参数的调用，一个函数想要被选中，就要求它至少在某个参数上比其他任何函数都匹配的更好，而对于每个参数都至少与其他函数匹配的同样好。

4.C++连接的一种实现
- 首先，需要对每个C++函数编码，在其名称后面附加上它的参数类型。
- 为表达具有C连接的情况，C++引入了一个连接描述扩展
```
extern "C"
{
	doubel sqrt(double); //告诉编译程序，在目标代码中对于sqrt()采用C的命名习惯
}
```

5.如果你希望禁止某些东西，就应该把完成它的操作定义为一个私用的成员函数。
- 为了禁止对类X对象的复制，只需将其复制构造函数和赋值运算符都定义为私有的
- 将析构函数定义为私有的，可以避免栈分配和全局分配，防止随便使用delete
- 只允许全局和堆栈变量，禁止自由空间分配，声明一个`operator new()`
```
class No_free_store
{
	class Dummy{};
    void * operator new(size_t, Dummy);
};
```
- 利用私有的析构函数，禁止派生

6.对象的复制被定义为对所有非静态的成员和基类成员的按成员复制。

7.增量和减量的重载
```
class ptr_to_X
{
	X& operator++();  //前置
    X operator++(int); //后置
};
```

8.`bool`已经是C++里面的一个特别的整类型，带有字面量`true`和`false`。
- 非零值可以隐式的转换到`true`，`true`可以隐式转换到1
- 零值可以隐式的转换到`false`，`false`可以隐式的转到到0

##13.类概念的精炼
1.一个抽象类表示了一个接口：
- 有助于捕捉由于混淆了类作为接口的角色和他们表示对象角色而引起的错误。
- 支持一种基于将接口的描述和实现分开来的设计风格

2.静态类型检测的一个目的就是在程序运行之前检查错误和不一致性。

3.抽象类只能用作其他类的基类，从一个抽象类派生的类必须或者给基类的纯虚函数提供定义，或者将其重新定义为纯虚函数。

4.`const`成员函数保证不会修改对象的值。`const`成员函数可以用于`const`对象和非`const`对象，而非`const`成员函数只能用于非`const`对象。

5.通过引进`const_cast`，使程序员能够区分有意的强制去掉const与有意做其他类型操作的强制。

6.将一个对象声明为const，就认为它具有从其构造函数完成到析构函数开始之间的不变性。

7.一个`static`成员声明仅仅是一个声明，它所声明的对象或者函数必须在程序里的某个地方有唯一的定义。

8.嵌套类使得作用域规则更加规范，并改进了信息局部化的能力。

9.指向数据成员的指针是表达C++的类布局方式的一种与实现无关的方式。

##14.强制转换
1.运行时类型信息机制包括3个主要部分：
- 一个运算符`danamic_cast`，给定一个指向某对象的基类指针，它能得到一个指向这个对象的派生类指针，只有在被指对象确实属于所指明的派生类时才会给出这个指针，否则返回0。
- 一个运算符`typeid`，它对一个给定的基类指针识别出被指对象的确切类型
- 一个结构`type_info`，作为与有关类型的更对运行时类型信息的hook点

2.找出一个对象的确切类型有时被称为类型标识，这个运算符被起名为`typeid`

3.尽量减少RTTI的使用：
- 最可取的是根本就不用运行时类型信息机制，完全依靠静态的类型检查
- 如果这样做不行，我们只好使用动态强制
- 如果必须的话，我们可以做`typeid`的比较
- 如果确实需要知道关于一个类型的更多的信息，那么我们需要使用定义在`typeid`上的操作，以获取更详细的信息。

4.无论从语法还是从语义上看，强制都是C和C++中最难看的特征之一。

5.`reinterpret_cast`运算符允许将任意的指针转换到其他类型指针，也允许做任意整数类型和任意指针类型之间的转换。本质上这些转换都是不安全的，而且是不可移植的。

6.`const_cast<T>e`用于通过转换获得对描述为`const`和`volatile`的数据的访问权。

7.对于所有情况，最好是能删除掉所有强制，无论是新的老的。

##15.模板
1.模板的概念根植于对描述参数化容器类的愿望；异常来自于渴望为运行时错误的处理提供一种标准化方式。

2.模板机制通过扩展静态类型检测所能处理问题的范围，能够减少运行时错误出现的数量；而异常就是为了处理这些错误专门提供的一种机制。

3.C++允许非类型的模板参数，这种机制基本上被看做是为容器类提供给大小和界限所需的信息。
```
template <typename T, int i> class Buffer
{
	T v[i];
    int sz;
public:
	Buffer():sz(i){}
};
```

4.对模板参数没有提出任何限制，所有的类型检查都被推迟到模板实例化的时刻进行。

5.避免由于实例化太多而造成不必要的空间开销，这件事一直被当做最要紧的事情。

6.在一个显示的模板参数表里，只有最后的参数是可缺省的。

7.具有同样名称的模版函数与其他函数的重载解析可以分为下面3个步骤：
- 在函数中查找准确的匹配，如果找到，就调用它
- 查找这样的函数模板，从它出发能够生成可以通过准确匹配进行调用的函数，如果找到，就调用它
- 试着去做函数重载的解析，如果能找到这样的函数，就调用它
- 如果无法找到一个能够调用的函数，那么调用就是错误的，如果在某一步找到多个，调用是有歧义的，也是错误的。

8.类型或者算法的任何一个性质都可以用一个类型表示。

9.语言并不要求显示的实例化，显式实例化只不过是一种对编译和连接过程进行手工优化的机制。

##16.异常处理
1.在异常处理的设计中做了如下假定：
- 异常基本上是为了处理错误
- 与函数定义相比，异常处理器是很少的
- 与函数调用相比，异常出现的频率低得多
- 异常是语言层的概念：不仅有实现问题，还不是错误处理策略

2.C++异常处理思想：
- 允许从抛出点将任意数量的信息以类型安全的方式传递到异常处理器
- 对于不抛异常的代码没有任何额外的代价
- 保证引发的每个异常都能被适当的处理器捕获
- 提供一种将异常组合的方式，可以编写出一组异常的处理器
- 是一种按默认方式就能对多线程系统正确工作的机制
- 是一种能够与其他语言合作的机制，特别是与C语言的合作
- 容易使用
- 容易实现

3.C++的异常处理机制能保证部分构造起来的对象也能被正确的销毁。

##17.名称空间
1.局部声明的名称，都将遮蔽名称相同的非局部声明，名称的任何非法重载都将在声明点被检查出来。

##18.C语言预处理
1.C++为`#define`的主要应用提供了如下的替代方式
- `const`用于常量
- `inline`用于开子程序
- `template`用于以类型为参数的函数
- `template`用于参数化类型
- `namespace`用于更一般的命名问题
