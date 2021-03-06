当你第一次学习move语义和完美转发时，它们看起来很直截了当：
 
* __Move语义__使编译器能够把昂贵的拷贝操作替换为代价较小的move操作。 和拷贝构造函数以及拷贝赋值运算符能赋予你控制拷贝对象的能力一样，move构造函数以及move赋值运算符提供给你对move语义的控制。Move语义使得move-only类型的创建成为可能，比如说`std::unique_ptr`，`std::future`以及`std::thread`.
* __完美转发__让我们可以写出接受任意参数的函数模板，并且将之转发到其他的函数，使得target函数接受的参数和forwarding函数接受的参数相同。

右值引用对于这两个看起来毫无关系的概念来说，就像是粘合两者的胶水。它作为潜在的语言机制，为move语义和完美转发的实现提供支持。

你对这些特性越有经验,你就越发现你对它们的第一印象就像是刚刚发现了冰山一角。move语义，完美转发以及右值引用跟它们看起来比有细微差别，比如说，move语义并不move任何东西，完美转发是不完美的。move操作的代价并不总是比拷贝低；就算当它们确实代价底时，也没有达到你想象的低的程度；它也并不总是在move有效的上下文中被调用。结构`type&&`并不一定总是代表一个右值引用。

不管你怎么去探索这些特性，看起来它们总是还有一些你还没注意到的地方。幸运的是，它们的知识不是永无止境的。本章会带你直达基础。看完本章节，C++ 11的这部分内容对你来说就变得栩栩如生。比如说，你就会知道std::move和std::forward的常见用法，带有迷惑性的`type&&`用法对你来说变得很平常。你也会理解move操作的各种让人感到奇怪的表现的原因.这些都会水到渠成。到那时，你又会回到了起点，因为move语义，完美转发，以及右值引用又一次看起来是那么的直截了当。但这次，它们(直截了当)的状态会一直保持下去。

在本章的所有Item中，你必须要牢记一点，作为函数的参数，永远是一个左值，即使它(在函数的参数列表中)的类型是一个右值引用。例如：

```cpp
void f(Widget&& w);
```
参数w是一个左值，即使它的类型是一个对Widget的右值引用。(如果你对此感到不理解，请重新回顾一下第2页(原文的页码 --不负责任的译者说)所讲的左值与右值的概览内容)