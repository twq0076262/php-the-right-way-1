# 依赖注入

出自维基百科 [Wikipedia](http://en.wikipedia.org/wiki/Dependency_injection):

> 依赖注入是一种允许我们从硬编码的依赖中解耦出来，从而在运行时或者编译时能够修改的软件设计模式。

这句解释让依赖注入的概念听起来比它实际要复杂很多。依赖注入通过构造注入，函数调用或者属性的设置来提供组件的依赖关系。就是这么简单。

## 基本概念

我们可以用一个简单的例子来说明依赖注入的概念

下面的代码中有一个 `Database` 的类，它需要一个适配器来与数据库交互。我们在构造函数里实例化了适配器，从而产生了耦合。这会使测试变得很困难，而且 `Database` 类和适配器耦合的很紧密。

```
{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}
```

这段代码可以用依赖注入重构，从而解耦

```
{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}
```

现在我们通过外界给予 `Database` 类的依赖，而不是让它自己产生依赖的对象。我们甚至能用可以接受依赖对象参数的成员函数来设置，或者如果 `$adapter` 属性本身是 `public`的，我们可以直接给它赋值。

## 复杂的问题

如果你曾经了解过依赖注入，那么你可能见过 *"控制反转"(Inversion of Control)* 或者 *"依赖反转准则"(Dependency Inversion Principle)*这种说法。这些是依赖注入能解决的更复杂的问题。

### 控制反转

顾名思义，一个系统通过组织控制和对象的完全分离来实现"控制反转"。对于依赖注入，这就意味着通过在系统的其他地方控制和实例化依赖对象，从而实现了解耦。

一些 PHP 框架很早以前就已经实现控制反转了，但是问题是，应该反转哪部分以及到什么程度？比如， MVC 框架通常会提供超类或者基本的控制器类以便其他控制器可以通过继承来获得相应的依赖。这就是控制反转的例子，但是这种方法是直接移除了依赖而不是减轻了依赖。

依赖注入允许我们通过按需注入的方式更加优雅地解决这个问题，完全不需要任何耦合。

### 依赖反转准则

依赖反转准则是面向对象设计准则 S.O.L.I.D 中的 "D" ,倡导 *"依赖于抽象而不是具体"*。简单来说就是依赖应该是接口/约定或者抽象类，而不是具体的实现。我们能很容易重构前面的例子，使之遵循这个准则

```
{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
{% endhighlight %}
```

现在 `Database` 类依赖于接口，相比依赖于具体实现有更多的优势。

假设你工作的团队中，一位同事负责设计适配器。在第一个例子中，我们需要等待适配器设计完之后才能单元测试。现在由于依赖是一个接口/约定，我们能轻松地模拟接口测试，因为我们知道同事会基于约定实现那个适配器

这种方法的一个更大的好处是代码扩展性变得更高。如果一年之后我们决定要迁移到一种不同的数据库，我们只需要写一个实现相应接口的适配器并且注入进去，由于适配器遵循接口的约定，我们不需要额外的重构。

## 容器

你应该明白的第一件事是依赖注入容器和依赖注入不是相同的概念。容器是帮助我们更方便地实现依赖注入的工具，但是他们通常被误用来实现反模式设计 Service Location 。把一个依赖注入容器作为 Service Locator 注入进类中隐式地建立了对于容器的依赖，而不是真正需要替换的依赖，而且还会让你的代码更不透明，最终变得更难测试。

大多数现代的框架都有自己的依赖注入容器，允许你通过配置将依赖绑定在一起。这实际上意味着你能写出和框架层同样干净、解耦的应用层代码。

## 延伸阅读

* [Learning about Dependency Injection and PHP](http://ralphschindler.com/2011/05/18/learning-about-dependency-injection-and-php)
* [What is Dependency Injection?](http://fabien.potencier.org/article/11/what-is-dependency-injection)
* [Dependency Injection: An analogy](https://mwop.net/blog/260-Dependency-Injection-An-analogy.html)
* [Dependency Injection: Huh?](http://net.tutsplus.com/tutorials/php/dependency-injection-huh/)
* [Dependency Injection as a tool for testing](http://philipobenito.github.io/dependency-injection-as-a-tool-for-testing/)