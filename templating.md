# 使用模板

模板提供了一种简便的方式，将展现逻辑从控制器和业务逻辑中分离出来。

一般来说，模板包含应用程序的 HTML 代码，但也可以使用其他的格式，例如 XML 。

模板通常也被称为「视图」, 而它是 [模型-视图-控制器](http://laravel-china.github.io/pages/Design-Patterns.html#model-view-controller) (MVC) 软件架构模式第二个元素的 **一部份** 。

## 好处

使用模板的主要好处是可以将呈现逻辑与应用程序的其他部分进行分离。模板的单一职责就是呈现格式化后的内容。它不负责数据的查询，保存或是其他复杂的任务。进一步促成了更干净、更具可读性的代码，在团队协作开发中尤其有用，开发者可以专注服务端的代码（控制器、模型），而设计师负责客户端代码 (网页) 。

模板同时也改善了前端代码的组织架构。一般来说，模板放置在「视图」文件夹中，每一个模板都放在独立的一个文件中。这种方式鼓励代码重用，它将大块的代码拆成较小的、可重用的片段，通常称为局部模板。举例来说，网站的头、尾区块可以各自定义为一个模板，之后将它们放在每一个页面模板的上、下位置。

最后，根据你选择的类库，模板可以通过自动转义用户的内容，从而带来更多的安全性。有些类库甚至提供沙箱机制，模板设计者只能使用在白名单中的变量和函数。

## 原生 PHP 模板

原生 PHP 模板就是指直接用 PHP 来写模板，这是很自然的选择，因为 PHP 本身其实是个模板语言。这代表你可以在其他的语言中结合 PHP 使用，比如 HTML 。这对 PHP 开发者相当有利，因为不需要额外学习新的语法，他们熟知可以使用的函数，并且使用的编辑器也已经内置了语法高亮和自动补全。此外，原生的 PHP 模板没有了编译阶段，速度会更快。

现今的 PHP 框架都会使用一些模板系统，这当中多数是使用原生的 PHP 语法。在框架之外，一些类库比如 [Plates][plates] 或 [Aura.View][aura]，提供了现代化模板的常见功能，比如继承、布局、扩展，让原生的 PHP 模板更容易使用。


### 原生 PHP 模板的简单示例

使用 [Plates][plates] 类库。

```
{% highlight php %}
<?php // user_profile.php ?>

<?php $this->insert('header', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->escape($name)?></p>

<?php $this->insert('footer') ?>
{% endhighlight %}
```

### 原生 PHP 模板使用继承的示例

使用 [Plates][plates] 类库。

```
{% highlight php %}
<?php // template.php ?>

<html>
<head>
    <title><?=$title?></title>
</head>
<body>

<main>
    <?=$this->section('content')?>
</main>

</body>
</html>
{% endhighlight %}

{% highlight php %}
<?php // user_profile.php ?>

<?php $this->layout('template', ['title' => 'User Profile']) ?>

<h1>User Profile</h1>
<p>Hello, <?=$this->escape($name)?></p>
{% endhighlight %}
```

[plates]: http://platesphp.com/
[aura]: https://github.com/auraphp/Aura.View

## 编译模板

尽管 PHP 不断升级为成熟的、面向对象的语言，但它作为模板语言 [没有改善多少][article_templating_engines]。编译模板，比如 [Twig] [Brainy] 或 [Smarty]* ，提供了模板专用的新语法，填补了这片空白。从自动转义到继承以及简化控制结构，编译模板设计地更容易编写，可读性更高，同时使用上也更加的安全。编译模板甚至可以在不同的语言中使用，[Mustache] 就是一个很好的例子。由于这些模板需要编译，在性能上会带来一些轻微的影响，不过如果适当的使用缓存，影响就变得非常小了。

**虽然 Smarty 提供了自动转义的功能, 不过这个功能默认是关闭的*

### 编译模板简单示例

使用 [Twig] 类库。

```
{% highlight html+jinja %}
{% raw %}
{% include 'header.html' with {'title': 'User Profile'} %}

<h1>User Profile</h1>
<p>Hello, {{ name }}</p>

{% include 'footer.html' %}
{% endraw %}
{% endhighlight %}
```

### 编译模板使用继承示例

使用 [Twig] 类库。

```
{% highlight html+jinja %}
{% raw %}
// template.html

<html>
<head>
    <title>{% block title %}{% endblock %}</title>
</head>
<body>

<main>
    {% block content %}{% endblock %}
</main>

</body>
</html>
{% endraw %}
{% endhighlight %}

{% highlight html+jinja %}
{% raw %}
// user_profile.html

{% extends "template.html" %}

{% block title %}User Profile{% endblock %}
{% block content %}
    <h1>User Profile</h1>
    <p>Hello, {{ name }}</p>
{% endblock %}
{% endraw %}
{% endhighlight %}
```

[article_templating_engines]: http://fabien.potencier.org/article/34/templating-engines-in-php
[Twig]: http://twig.sensiolabs.org/
[Brainy]: https://github.com/box/brainy
[Smarty]: http://www.smarty.net/
[Mustache]: http://mustache.github.io/

## 延伸阅读

### 文章与教程

* [Templating Engines in PHP](http://fabien.potencier.org/article/34/templating-engines-in-php)
* [An Introduction to Views & Templating in CodeIgniter](http://code.tutsplus.com/tutorials/an-introduction-to-views-templating-in-codeigniter--net-25648)
* [Getting Started With PHP Templating](http://www.smashingmagazine.com/2011/10/17/getting-started-with-php-templating/)
* [Roll Your Own Templating System in PHP](http://code.tutsplus.com/tutorials/roll-your-own-templating-system-in-php--net-16596)
* [Master Pages](https://laracasts.com/series/laravel-from-scratch/episodes/7)
* [Working With Templates in Symfony 2](http://code.tutsplus.com/tutorials/working-with-templates-in-symfony-2--cms-21172)
* [Writing Safer Templates](https://github.com/box/brainy/wiki/Writing-Safe-Templates)

### 类库

* [Aura.View](https://github.com/auraphp/Aura.View) *(native)*
* [Blade](http://laravel.com/docs/templates) *(compiled, framework specific)*
* [Brainy](https://github.com/box/brainy) *(compiled)*
* [Dwoo](http://dwoo.org/) *(compiled)*
* [Latte](https://github.com/nette/latte) *(compiled)*
* [Mustache](https://github.com/bobthecow/mustache.php) *(compiled)*
* [PHPTAL](http://phptal.org/) *(compiled)*
* [Plates](http://platesphp.com/) *(native)*
* [Smarty](http://www.smarty.net/) *(compiled)*
* [Twig](http://twig.sensiolabs.org/) *(compiled)*
* [Zend\View](http://framework.zend.com/manual/2.3/en/modules/zend.view.quick-start.html) *(native, framework specific)*