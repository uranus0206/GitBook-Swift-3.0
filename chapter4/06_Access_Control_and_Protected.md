# 访问控制和protected
---

很多其他编程语言都有一种”protected“设定，可以限制某些类方法只能被它的子类所使用。

Swift支持了访问控制后，大家给我们的反馈都很不错。而有的开发者问我们：“为什么Swift没有类似protected的选项？” 



**当我们在设计Swift访问控制的不同等级时，我们认为有两种主要场景：**

* 在一个APP里：隐藏某个类的私密细节。
* 在一个开源框架里：不让导入这个框架的APP，随便接触框架的内部实现细节。

上面的两种常见情况，对应着private和internal这两个等级。

而protected相当于把访问控制和继承特性混在一起，把访问控制的等级设定增加了一个维度，使之复杂化。即使设定了protected，子类还是可以通过新的公开方法、新的属性来接触到所谓“protected”了的API。另一方面，我们可以在各种地方重写一个方法，所谓的保护却没有提供优化机制。这种设定往往在做不必要的限制  一 protected允许了子类，但又禁止所有其他别的类（包括那些帮助子类实现某些功能的类）接触父类的成员。

有的开发者指出，apple的框架有时候也会把给子类用的API分隔出来。这时候protected不就有用了吗？我们研究后发现，这些方法一般属于下面两种情况：一是这些方法对子类以外的类没啥用，所以不需要严格保护（例如上面说的协助实现某些功能的类）。二是这些方法就是设计出来被重写，而不是直接用的。举个例子，`drawRect(_:) `就是在UIKit基础上使用的方法，但它不能在UIKit以外应用。

除此之外，如果有了protected，它要怎么样和extension相互作用呢？一个类的extension能接触它的protected成员吗？一个子类的extension可以接触父类的protected成员吗？extension声明的位置对访问控制等级有没有影响呢？（复杂到要哭了是不是？）

对访问控制的设计，也依循了Objective－C开发者（包括apple内外的）的常规做法。Objective－C方法和属性一般在.h头文件里声明，但也可以写在.m实现文件里。假如有一个公开的类，想把里面某些部分设为只有框架内可以获取时，开发者一般会创建另一个头文件给内部使用。以上三种访问级别，就对应了Swift里面的public，private和internal。

Swift的访问控制等级和继承无关，是单维度、非常清楚明了的。我们认为这样的模式更简洁，同时满足了最主要的需求：将一个类、或一个框架的实现细节隔离保护起来。这可能和你以前用过的不同，但我们鼓励你试试看。  

本文由翻译自 Apple Swift Blog: [Access Control and Protected](https://developer.apple.com/swift/blog/?id=11)