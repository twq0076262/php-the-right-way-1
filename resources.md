# 资源

## PHP 官方 {#from_the_source_title}

* [PHP 官方网站](http://php.net/)
* [PHP 官方文档](http://php.net/docs.php)

## 值得关注的大牛

* [Rasmus Lerdorf](http://twitter.com/rasmus)
* [Fabien Potencier](http://twitter.com/fabpot)
* [Derick Rethans](http://twitter.com/derickr)
* [Chris Shiflett](http://twitter.com/shiflett)
* [Sebastian Bergmann](http://twitter.com/s_bergmann)
* [Matthew Weier O'Phinney](http://twitter.com/mwop)
* [Pádraic Brady](http://twitter.com/padraicb)
* [Anthony Ferrara](http://twitter.com/ircmaxell)
* [Nikita Popov](http://twitter.com/nikita_ppv)

## 指导

* [phpmentoring.org](http://phpmentoring.org/) - PHP 社区中的一对一指导。

## PHP 的 Paas 提供商

* [PagodaBox](https://pagodabox.com/)
* [AppFog](https://appfog.com/)
* [Heroku](https://devcenter.heroku.com/categories/php)
* [fortrabbit](http://fortrabbit.com/)
* [Engine Yard Cloud](https://www.engineyard.com/products/cloud)
* [Red Hat OpenShift Platform](http://openshift.com)
* [dotCloud](http://docs.dotcloud.com/services/php/)
* [AWS Elastic Beanstalk](http://aws.amazon.com/elasticbeanstalk/)
* [cloudControl](https://www.cloudcontrol.com/)
* [Windows Azure](http://www.windowsazure.com/)
* [Google App Engine](https://developers.google.com/appengine/docs/php/gettingstarted/)
* [Jelastic](http://jelastic.com/)

## 框架

许多的 PHP 开发者都使用框架，而不是重新造轮子来构建 Web 应用。框架抽象了许多底层常用的逻辑，并提供了有益又简便的方法來完成常见的任务。

你并不一定要在每个项目中都使用框架。有时候原生的 PHP 才是正确的选择，但如果你需要一个框架，那么有如下三种主要类型：

* 微型框架
* 全栈框架
* 组件框架

微型框架基本上是一个封装的路由，用来转发 HTTP 请求至一个闭包，控制器，或方法等等，尽可能地加快开发的速度，有时还会使用一些类库来帮助开发，例如一个基本的数据库封装等等。他們用来构建 HTTP 的服务卓有成效。

许多的框架会在微型框架上加入相当多的功能，我们则称之为全栈框架。这些框架通常会提供 ORMs ，身份认证扩展包等等。

组件框架是多个独立的类库所结合起来的。不同的组件框架可以一起使用在微型或是全栈框架上。

* [热门的 PHP 框架](https://github.com/codeguy/php-the-right-way/wiki/Frameworks)

## 组件


正如标题提到的，「组件」是另一种建立，发布及推动开源的方式。现在存在的各种的组件库，其中最主要的两个为：

* [Packagist]
* [PEAR]

这两个组件库都有用來安装及升级的命令行工具，这部分已经在這部分已經在[依赖管理]中解释过。

此外，还有基于组件的构成的框架的提供商提供不包含框架的组件。这些项目通常和其他的组件或者特定的框架没有依赖关系。

例如，你可以使用 [FuelPHP 验证类库]，而不使用 FuelPHP 整个框架。

* [Aura]
* [FuelPHP]
* [Hoa Project]
* [Orno]
* [Symfony Components]
* [The League of Extraordinary Packages]
* Laravel's Illuminate Components
    * [Eloquent ORM]
    * [Queue]

_Laravel 的 [Illuminate 组件] 和 Laravel 框架将变得更加解耦。 现在我们只列出和 Laravel 框架最没有依赖关系的组件。_

[Packagist]: /#composer_and_packagist
[PEAR]: /#pear
[Dependency Management]: /#dependency_management
[FuelPHP Validation package]: https://github.com/fuelphp/validation
[Aura]: http://auraphp.com/packages/v2
[FuelPHP]: https://github.com/fuelphp
[Hoa Project]: https://github.com/hoaproject
[Orno]: https://github.com/orno
[Symfony Components]: http://symfony.com/doc/current/components/index.html
[The League of Extraordinary Packages]: http://thephpleague.com/
[Eloquent ORM]: https://github.com/illuminate/database
[Queue]: https://github.com/illuminate/queue
[Illuminate components]: https://github.com/illuminate

## 其他有用的资源

### Cheatsheets

* [PHP Cheatsheets](http://phpcheatsheets.com/) - for variable comparisons, arithmetics and variable testing in various
PHP versions
* [PHP Security Cheatsheet](https://www.owasp.org/index.php/PHP_Security_Cheat_Sheet)

### 更多最佳实践

* [PHP Best Practices](https://phpbestpractices.org/)
* [Best practices for Modern PHP Development](https://www.airpair.com/php/posts/best-practices-for-modern-php-development)

### PHP 世界

* [PHP Developer blog](http://blog.phpdeveloper.org/)

## Video Tutorials

### Youtube 视频
* [PHP Academy](https://www.youtube.com/user/phpacademy)
* [The New Boston](https://www.youtube.com/user/thenewboston)
* [Sherif Ramadan](https://www.youtube.com/user/businessgeek)
* [Level Up Tuts](https://www.youtube.com/user/LevelUpTuts)

### 付费视频

* [Standards and Best practices](http://teamtreehouse.com/library/standards-and-best-practices)
* [PHP Training on Pluralsight](http://www.pluralsight.com/search/?searchTerm=php)
* [PHP Training on Lynda.com](http://www.lynda.com/search?q=php)
* [PHP Training on Tutsplus](http://code.tutsplus.com/categories/php/courses)
* [Laracasts](https://laracasts.com/)

## 书籍

市面上有很多关于 PHP 的书，但遗憾的是很多都已经非常陈旧而且不正确的资料。甚至还有出版商发布「 PHP 6 」，这是不存在的书，而且永远不会出现。因为那些书，所以 PHP 的下一个版本为「 PHP 7 」。


这个章节的目录主要是针对 PHP 开发，并且会随着最新的技术趋势而更新。如果你想在这里加入你的书，请发送一个 PR ，我们将会审查你提供的内容是否有相关性。


### 免费书籍

* [PHP The Right Way](https://leanpub.com/phptherightway/) - This website is available as a book completely for free.

### 付费书籍

* [Build APIs You Won't Hate](https://leanpub.com/build-apis-you-wont-hate) - Everyone and their dog wants an API,
so you should probably learn how to build them.
* [Building Secure PHP Apps](https://leanpub.com/buildingsecurephpapps) - Learn the security basics that a senior
developer usually acquires over years of experience, all condensed down into one quick and easy handbook
* [Modernizing Legacy Applications In PHP](https://leanpub.com/mlaphp) - Get your code under control in a series of
small, specific steps
* [Securing PHP: Core Concepts](https://leanpub.com/securingphp-coreconcepts) - A guide to some of the most common
security terms and provides some examples of them in every day PHP
* [Scaling PHP]( https://leanpub.com/scalingphp) - Stop playing sysadmin and get back to coding
* [Signaling PHP]( https://leanpub.com/signalingphp) - PCNLT signals are a great help when writing PHP scripts that
run from the command line.
* [The Grumpy Programmer's Guide To Building Testable PHP Applications](https://leanpub.com/grumpy-testing) - Learning
to write testable code doesn't have to suck