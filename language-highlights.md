# 语言亮点

## 编程范式

PHP 是一个灵活的动态语言，支持多种编程技巧。这几年一直不断的发展，重要的里程碑包含 PHP 5.0 (2004) 增加了完善的面向对象模型，PHP 5.3 (2009) 增加了匿名函数与命名空间以及 PHP 5.4 (2012) 增加的 traits。

### 面向对象编程

PHP 拥有完整的面向对象编程的特性，包括类，抽象类，接口，继承，构造函数，克隆和异常等。

* [阅读 PHP 面向对象编程][oop]
* [阅读 Traits][traits]

### 函数式编程 Functional Programming

PHP 支持函数是"第一等公民"，即函数可以被赋值给一个变量，包括用户自定义的或者是内置函数，然后动态调用它。函数可以作为参数传递给其他函数（称为高阶函数），也可以作为函数返回值返回。

PHP 支持递归，也就是函数自己调用自己，但多数 PHP 代码使用迭代。

自从 PHP 5.3 (2009) 之后开始引入对闭包以及匿名函数的支持。

PHP 5.4 增加了将闭包绑定到对象作用域中的特性，并改善其可调用性，如此即可在大部分情况下使用匿名函数取代一般的函数。

* 学习更多 [PHP 函数式编程](http://laravel-china.github.io/php-the-right-way/pages/Functional-Programming.html)
* [阅读匿名函数][anonymous-functions]
* [阅读闭包类][closure-class]
* [更多关于 Closures RFC][closures-rfc]
* [阅读 Callables][callables]
* [阅读动态调用函数 `call_user_func_array()`][call-user-func-array]

### 元编程

PHP 通过反射 API 和魔术方法，可以实现多种方式的元编程。开发者通过魔术方法，如 `__get()`, `__set()`, `__clone()`, `__toString()`, `__invoke()`，等等，可以改变类的行为。Ruby 开发者常说 PHP 没有 `method_missing` 方法，实际上通过 `__call()` 和 `__callStatic()` 就可以完成相同的功能。

* [阅读魔术方法][magic-methods]
* [阅读反射][reflection]
* [阅读重载][overloading]


[oop]: http://php.net/language.oop5
[traits]: http://php.net/language.oop5.traits
[anonymous-functions]: http://php.net/functions.anonymous
[closure-class]: http://php.net/class.closure
[closures-rfc]: https://wiki.php.net/rfc/closures
[callables]: http://php.net/language.types.callable
[call-user-func-array]: http://php.net/function.call-user-func-array
[magic-methods]: http://php.net/language.oop5.magic
[reflection]: http://php.net/intro.reflection
[overloading]: http://php.net/language.oop5.overloading

## 命名空间

如前所述，PHP 社区已经有许多开发者开发了大量的代码。这意味着一个类库的 PHP 代码可能使用了另外一个类库中相同的类名。如果他们使用同一个命名空间，那将会产生冲突导致异常。

_命名空间_ 解决了这个问题。如 PHP 手册里所描述，命名空间好比操作系统中的目录，两个同名的文件可以共存在不同的目录下。同理两个同名的 PHP 类可以在不同的 PHP 命名空间下共存，就这么简单。

因此把你的代码放在你的命名空间下就非常重要，避免其他开发者担心与第三方类库冲突。

[PSR-4][psr4] 提供了一种命名空间的推荐使用方式，它提供一个标准的文件、类和命名空间的使用惯例，进而让代码做到随插即用。

2014 年 10 月，PHP-FIG 废弃了上一个自动加载标准： [PSR-0][psr0]，而采用新的自动加载标准 [PSR-4][psr4]。但 PSR-4 要求 PHP 5.3 以上的版本，而许多项目都还是使用 PHP 5.2，所以目前两者都能使用。如果你在新应用或扩展包中使用自动加载标准，应优先考虑使用 PSR-4。

* [阅读命名空间][namespaces]
* [阅读 PSR-0][psr0]
* [阅读 PSR-4][psr4]


[namespaces]: http://php.net/language.namespaces
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md

## PHP 标准库

PHP 标准库 (SPL) 随着 PHP 一起发布，提供了一组类和接口。包含了常用的数据结构类 (堆栈，队列，堆等等)，以及遍历这些数据结构的迭代器，或者你可以自己实现 SPL 接口。

* [阅读 SPL][spl]
* [Lynda.com 上的 SPL 视频教程(付费)][spllynda]

[spl]: http://php.net/book.spl
[spllynda]: http://www.lynda.com/PHP-tutorials/Up-Running-Standard-PHP-Library/175038-2.html

## 命令行接口

PHP 是为开发 Web 应用而创建，不过它的命令行脚本接口(CLI)也非常有用。PHP 命令行编程可以帮你完成自动化的任务，如测试，部署和应用管理。

CLI PHP 编程非常强大，可以直接调用你自己的程序代码而无需创建 Web 图形界面，需要注意的是**不要**把 CLI PHP 脚本放在公开的 web 目录下！

在命令行下运行 PHP :

```
{% highlight console %}
> php -i
{% endhighlight %}
```

选项 `-i` 将会打印 PHP 配置，类似于 [`phpinfo()`][phpinfo] 函数。

选项 `-a` 提供交互式 shell，和 Ruby 的 IRB 或 python 的交互式 shell 相似，此外还有很多其他有用的[命令行选项][cli-options]。

接下来写一个简单的 "Hello, $name" CLI 程序，先创建名为 `hello.php` 的脚本：

```
{% highlight php %}
<?php
if($argc != 2) {
    echo "Usage: php hello.php [name].\n";
    exit(1);
}
$name = $argv[1];
echo "Hello, $name\n";
{% endhighlight %}
```

PHP 会在脚本运行时根据参数设置两个特殊的变量，[`$argc`][argc] 是一个整数，表示参数*个数*，[`$argv`][argv] 是一个数组变量，包含每个参数的*值*，
它的第一个元素一直是 PHP 脚本的名称，如本例中为 `hello.php`。

命令运行失败时，可以通过 `exit()` 表达式返回一个非 0 整数来通知 shell，常用的 exit 返回码可以查看[列表][exit-codes].

运行上面的脚本，在命令行输入：

```
{% highlight console %}
> php hello.php
Usage: php hello.php [name]
> php hello.php world
Hello, world
{% endhighlight %}
```

 * [学习如何在命令行运行 PHP][php-cli]
 * [学习如何在 Windows 环境下运行 PHP 命令行程序][php-cli-windows]

[phpinfo]: http://php.net/function.phpinfo
[cli-options]: http://php.net/features.commandline.options
[argc]: http://php.net/reserved.variables.argc
[argv]: http://php.net/reserved.variables.argv
[exit-codes]: http://www.gsp.com/cgi-bin/man.cgi?section=3&amp;topic=sysexits
[php-cli]: http://php.net/features.commandline
[php-cli-windows]: http://php.net/install.windows.commandline

## Xdebug

合适的调试器是软件开发中最有用的工具之一，它使你可以跟踪程序执行结果并监视程序堆栈中的信息。
Xdebug 是一个 php 的调试器，它可以被用来在很多 IDE(集成开发环境) 中做断点调试以及堆栈检查。它还可以像 PHPUnit 和 KCacheGrind 一样，做代码覆盖检查或者程序性能跟踪。

如果你仍在使用 `var_dump()`/`print_r()` 调错，经常会发现自己处于困境，并且仍然找不到解决办法。这时，你该使用调试器了。

[安装 Xdebug][xdebug-install] 可能很费事，但其中一个最重要的「远程调试」特性 —— 如果你在本地开发，并在虚拟机或者其他服务器上测试，远程调试可能是你想要的一种方式。

通常，你需要修改你的 Apache VHost 或者 .htaccess 文件的这些值:

```
{% highlight ini %}
php_value xdebug.remote_host=192.168.?.?
php_value xdebug.remote_port=9000
{% endhighlight %}
```

「remote host」 和 「remote port」 这两项对应和你本地开发机监听的地址和端口。然后将你的 IDE 设置成「listen for connections」模式，并访问网址：

    http://your-website.example.com/index.php?XDEBUG_SESSION_START=1

你的 IDE 将会拦截当前执行的脚本状态，运行你设置的断点并查看内存中的值。

图形化的调试器可以让你非常容易的逐步的查看代码、变量，以及运行时的 evel 代码。许多 IDE 已经内置或提供了插件支持 XDebug 图形化调试器。比如 MacGDBp 是 Mac 上的一个免费，开源的单机调试器。

 * [学习更多 Xdebug][xdebug-docs]
 * [学习更多 MacGDBp][macgdbp-install]


[xdebug-install]: http://xdebug.org/docs/install
[xdebug-docs]: http://xdebug.org/docs/
[macgdbp-install]: http://www.bluestatic.org/software/macgdbp/