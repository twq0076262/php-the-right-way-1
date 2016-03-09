# 测试

为你的 PHP 程序编写自动化测试被认为是最佳实践，可以帮助你建立良好的应用程序。 自动化测试是非常棒的工具，它能确保你的应用程序在改变或增加新的功能时不会影响现有的功能，不应该忽视。

PHP 有一些不同种类的测试工具 (或框架) 可以使用，它们使用不同的方法 - 但他們都试图避免手动测试和大型 QA 团队的需求，确保最近的变更不会破坏既有功能。

## 测试驱动开发

[Wikipedia](http://en.wikipedia.org/wiki/Test-driven_development) 上的定义:
> 测试驱动开发 (TDD) 是一种以非常短的开发周期不断迭代的软件开发过程:首先开发者对将要实现的功能或者新的方法写一个失败的自动化测试用例，然后就去写代码来通过这个测试用例，最终通过重构代码让一其达到可接受的水准。**Kent Beck**， 这个技术创造者或者说重新发现者，在2003年声明TDD 鼓励简单的设计和激励信心。

目前你可以应用的几种不同类型的测试：

### 单元测试
单元测试是一种编程方法来确认函数，类和方法以我们预期的方式来工作，单元测试会贯穿整个项目的开发周期。通过检查各个函数和方法的输入输出，你就可以保证内部的逻辑已经正确执行。通过使用依赖注入和编写"mock" 类以及 stubs 来确认依赖被正确的使用，提高测试覆盖率。

当你创建一个类或者一个函数，你应该为它们的每一个行为创建一个单元测试。至少你应该确认当你输入一个错误参数会触发一个错误，你输入一个有效的参数会得到正确的结果。这会帮助你在开发周期后段对类或者函数做出修改后，确认已有的功能任然可以正常的工作。可替代的方法是在源码中使用 `var_dump()` ，但这种方法却不能去构建一个或大或小的应用。

单元测试的其他用处是在给开源项目贡献代码时。如果你写了一个测试证明代码有bug，然后修复它，并且展示测试的过程，这样补丁将会更容易被接受。如果你在维护一个项目，在处理 pull request 的时候可以将单元测试作为一个要求。

[PHPUnit](https://phpunit.de/) 是业界PHP应用开发单元测试框架的标准，但也有其他可选的框架：

- [atoum](https://github.com/atoum/atoum)
- [Enhance PHP](https://github.com/Enhance-PHP/Enhance-PHP)
- [PUnit](http://punit.smf.me.uk/)
- [SimpleTest](http://simpletest.org/)

###集成测试
[Wikipedia](http://en.wikipedia.org/wiki/Test-driven_development) 上的定义:
> 集成测试 (有时候称为集成和测试，缩写为 `I&T`)是把各个模块组合在一起进行整体测试的软件测试阶段。它处于单元测试之后，验收测试之前。集成测试将已经经过了单元测试的模块做为输入模块，组合成一个整体，然后运行集成测试用例，然后输出一个可以进行系统测试的系统。

许多相同的测试工具既可以运用到单元测试，也可以运用到集成测试。

###功能性测试
有时候也被称之为验收测试，功能测试是通过使用工具来生成自动化的测试用例，然后在真实的系统上运行。而不是单元测试中简单的验证单个模块的正确性和集成测试中验证各个模块间交互的正确性。这些工具会使用代表性的真实数据来模拟真实用户的行为来验证系统的正确性。

####功能测试的工具

- [Selenium](http://docs.seleniumhq.org/)
- [Mink](http://mink.behat.org/en/latest/)
- [Codeception](http://codeception.com/) 是一个全栈的测试框架包括验收性测试工具。
- [Storyplayer](http://datasift.github.io/storyplayer/) 是一个全栈的测试框架并且支持随时创建和销毁测试环境。

## 行为驱动开发

有两种不同的行为驱动开发 (BDD): SpecBDD 和 StoryBDD。 SpecBDD 专注于代码的技术行为，而 StoryBDD 专注于业务逻辑或功能的行为和互动。这两种 BDD 都有对应的 PHP 框架。

采用 StoryBDD 时, 你编写可读的故事来描述应用程序的行为。接着这些故事可以作为应用程序的实际测试案例执行。[Behat] 是使用在 PHP 应用程序中的 StoryBDD框架，它受到 Ruby 的 [Cucumber] 项目的启发并且实现了 Gherkin DSL 來描述功能的行为。

采用 SpecBDD 时, 你编写规格来描述实际的代码应该有什么行为。你应该描述函数或者方法应该有什么行为，而不是测试函数或者方法。PHP 提供了 [PHPSpec] 框架来达到这个目的，这个框架受到了 Ruby 的 [RSpec project][Rspec] 项目的启发。

### BDD 链接

* [Behat], PHP 的 StoryBDD 框架， 受到了 Ruby's [Cucumber] 项目的启发。
* [PHPSpec], PHP 的 SpecBDD 框架， 受到了 Ruby's [RSpec] 项目的启发。
* [Codeception] 是一个使用 BDD 准则的全栈测试框架。

[Behat]: http://behat.org/
[Cucumber]: http://cukes.info/
[PHPSpec]: http://www.phpspec.net/
[RSpec]: http://rspec.info/
[Codeception]: http://codeception.com/

## 其他测试工具

除了个别的测试驱动和行为驱动框架之外，还有一些通用的框架和辅助函数类库，对任何的测试方法都很有用。

### 工具地址

* [Selenium] 是一个浏览器自动化工具 [integrated with PHPUnit]
* [Mockery] 是一个可以跟 [PHPUnit] 或者 [PHPSpec] 整合的 Mock 对象框架  
* [Prophecy] 是个有自己的想法，且非常强大灵活的 PHP 对象 mocking 框架。它整合了 [PHPSpec] 并且可以和 [PHPUnit] 一起使用

[Selenium]: http://seleniumhq.org/
[integrated with PHPUnit]: http://phpunit.de/manual/current/en/selenium.html
[Mockery]: https://github.com/padraic/mockery
[PHPUnit]: http://phpunit.de/
[PHPSpec]: http://www.phpspec.net/
[Prophecy]: 1