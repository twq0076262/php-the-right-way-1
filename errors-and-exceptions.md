# 错误与异常

## 错误

在许多「重异常」(exception-heavy) 的编程语言中，一旦发生错误，就会抛出异常。这确实是一个可行的方式。不过 PHP 却是一个 「轻异常」(exception-light) 的语言。当然它确实有异常机制，在处理对象时，核心也开始采用这个机制来处理，只是 PHP 会尽可能的执行而无视发生的事情，除非是一个严重错误。

举例来说:

```
{% highlight console %}
$ php -a
php > echo $foo;
Notice: Undefined variable: foo in php shell code on line 1
{% endhighlight %}
```

这里只是一个 notice 级别的错误，PHP 仍然会愉快的继续执行。这对有「重异常」编程经验的人来说会带来困惑，例如在 Python 中，引用一个不存在的变量会抛出异常：

{% highlight console %}
$ python
>>> print foo
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'foo' is not defined
{% endhighlight %}

本质上的差异在于 Python 会对任何小错误进行抛错，因此开发人员可以确信任何潜在的问题或者边缘的案例都可以被捕捉到，与此同时 PHP 仍然会保持执行，除非极端的问题发生才会抛出异常。

### 错误严重性

PHP 有几个错误严重性等级。三个最常见的的信息类型是错误（error）、通知（notice）和警告（warning）。它们有不同的严重性: `E_ERROR` 、`E_NOTICE`和 `E_WARNING`。错误是运行期间的严重问题，通常是因为代码出错而造成，必须要修正它，否则会使 PHP 停止执行。通知是建议性质的信息，是因为程序代码在执行期有可能造成问题，但程序不会停止。
警告是非致命错误，程序执行也不会因此而中止。


另一个在编译期间会报错的信息类型是「E_STRICT」。这个信息用來建议修改程序代码以维持最佳的互通性并能与今后的 PHP 版本兼容。

### 更改 PHP 错误报告行为

错误报告可以由 PHP 配置及函数调用改变。使用 PHP 内置的函数 `error_reporting()`，可以设定程序执行期间的错误等级，方法是传入预定义的错误等级常量，意味着如果你只想看到警告和错误（而非通知），你可以这样设定：

```
{% highlight php %}
<?php
error_reporting(E_ERROR | E_WARNING);
{% endhighlight %}
```

你也可以控制错误是否在屏幕上显示 （开发时比较有用）或隐藏后记录日志 （适用于正式环境）。如果想知道更多细节，可以查看 [错误报告](errorreport) 章节。

### 行内错误抑制

你可以让 PHP 利用错误控制操作符 `@` 来抑制特定的错误。将这个操作符放置在表达式之前，其后的任何错误都不会出现。

```
{% highlight php %}
<?php
echo @$foo['bar'];
{% endhighlight %}
```

如果 `$foo['bar']` 存在，程序会将结果输出，如果变量 `$foo` 或是 `'bar'` 键值不存在，则会返回 null 并且不输出任何东西。如果不使用错误控制操作符，这个表达式会产生一个错误信息 `PHP Notice: Undefined
variable: foo` 或 `PHP Notice: Undefined index: bar` 。

这看起来像是个好主意，不过也有一些讨厌的代价。PHP 处理使用 `@` 的表达式比起不用时效率会低一些。过早的性能优化在所有程序语言中也许都是争论点，不过如果性能在你的应用程序 / 类库中占有重要地位，那么了解错误控制操作符的性能影响就比较重要。

其次，错误控制操作符会 **完全** 吃掉错误。不但没有显示，而且也不会记录在错误日志中。此外，在正式环境中 PHP 也没有办法关闭错误控制操作符。也许你认为那些错误时无害的，不过那些较具伤害性的错误同时也会被隐藏。

如果有方法可以避免错误抑制符，你应该考虑使用，举例来说，上面的程序代码可以这样重写：

```
{% highlight php %}
<?php
echo isset($foo['bar']) ? $foo['bar'] : '';
{% endhighlight %}
```

当 `fopen()` 载入文件失败时，也许是一个使用错误抑制符的合理例子。你可以在尝试载入文件前检查是否存在，但是如果这个文件在检查后才被删除，而此时 `fopen()` 还未执行 （听起来有点不太可能，但是确实会发生），这时 `fopen()` 会返回 false _并且_ 抛出操作。这也许应该由 PHP 本身来解决，但这时一个错误抑制符才能有效解决的例子。

前面我们提到在正式的 PHP 环境中没有办法关闭错误控制操作符。但是 [Xdebug] 有一个 `xdebug.scream` 的 ini 配置项，可以关闭错误控制操作符。你可以按照下面的方式修改 `php.ini`。

```
{% highlight ini %}
xdebug.scream = On
{% endhighlight %}
```

你也可以在执行期间通过 `ini_set` 函数来设置这个值：

```
{% highlight php %}
<?php
ini_set('xdebug.scream', '1')
{% endhighlight %}
```

「Scream」这个 PHP 扩展提供了和 xDebug 类似的功能，只是 Scream 的 ini 设置项叫做 `scream.enabled` 。

当你在调试代码而错误信息被隐藏时，这是最有用的方法。请务必小心使用 scream ，而是把它当时暂时性的调试工具。有许多的 PHP 函数类库代码也许无法在错误抑制操作符停用时正常使用。

* [Error Control Operators]
* [SitePoint]
* [Xdebug]
* [Scream]


### 错误异常类

PHP 可以完美化身为「重异常」的程序语言，只需要几行代码就能切换过去。基本上你可以利用 `ErrorException` 类抛出「错误」来当做「异常」，这个类是继承自 `Exception` 类。

这在大量的现代框架中是一个常见的做法，比如 Symfony 和 Laravel。Laravel 默认使用 [Whoops!] 扩展包来处理错误，如果 `app.debug` 启动的话，会将错误当成异常显示出来，而关闭则会隐藏。

在开发过程中将错误当作异常抛出可以更好的处理它，如果在开发时发生异常，你可以将它包在一个 catch 语句中具体说明这种情况如何处理。每捕捉一个异常，都会使你的应用程序越来越健壮。

更多关于如何使用 `ErrorException` 来处理错误的细节，可以参考 [ErrorException Class](errorexception)。

* [Error Control Operators]
* [Predefined Constants for Error Handling]
* [`error_reporting()`][error_reporting]
* [Reporting][errorreport]


[errorreport]: /#error_reporting
[Xdebug]: http://xdebug.org/docs/basic
[Scream]: http://php.net/book.scream
[Error Control Operators]: http://php.net/language.operators.errorcontrol
[SitePoint]: http://www.sitepoint.com/
[Whoops!]: http://filp.github.io/whoops/
[errorexception]: http://php.net/class.errorexception
[Predefined Constants for Error Handling]: http://php.net/errorfunc.constants
[error_reporting]: http://php.net/function.error-reporting

## 异常

异常是许多流行编程语言的标配，但它们往往被 PHP 开发人员所忽视。像 Ruby 就是一个极度重视异常的语言，无论有什么错误发生，像是 HTTP 请求失败，或者数据库查询有问题，甚至找不到一个图片资源，Ruby （或是所使用的 gems），将会抛出异常，你可以通过屏幕立刻知道所发生的问题。

PHP 处理这个问题则比较随意，调用 `file_get_contents()` 函数通常只会给出 `FALSE` 值和警告。许多较早的 PHP 框架比如 CodeIgniter 只是返回 false，将信息写入专有的日志，或者让你使用类似 `$this->upload->get_error()` 的方法来查看错误原因。这里的问题在于你必须找出错误所在，并且通过翻阅文档来查看这个类使用了什么样的错误的方法，而不是明确的暴露错误。

另一个问题发生在当类自动抛出错误到屏幕时会结束程序。这样做会阻挡其他开发者动态处理错误的机会。应该抛出异常让开发人员意识到错误的存在，让他们可以选择处理的方式，例如：

```
{% highlight php %}
<?php
$email = new Fuel\Email;
$email->subject('My Subject');
$email->body('How the heck are you?');
$email->to('guy@example.com', 'Some Guy');

try
{
    $email->send();
}
catch(Fuel\Email\ValidationFailedException $e)
{
    // 验证失败
}
catch(Fuel\Email\SendingFailedException $e)
{
    // 这个驱动无法发送 email
}
finally
{
    // 无论抛出什么样的异常都会执行，并且在正常程序继续之前执行
}
{% endhighlight %}
```

### SPL 异常

原生的 `Exception` 类并没有提供太多的调试情境给开发人员，不过可以通过建立一个特殊的 `Exception` 来弥补它，方式就是建立一个继承自原生 `Exception` 类的一个子类：

```
{% highlight php %}
<?php
class ValidationException extends Exception {}
{% endhighlight %}
```

如此一来，可以加入多个 catch 区块，并且根据不同的异常分别处理。通过这样可以建立 <em>许多</em>自定义异常，其中有些已经在 [SPL 扩展][splext] 提供的 SPL 异常中定义了。

举例来说，如果你使用了 `__call()` 魔术方法去调用一个无效的方法，而不是抛出一个模糊的标准 Exception 或是建立自定义的异常处理，你可以直接抛出 `throw new BadMethodCallException;`。

* [Read about Exceptions][exceptions]
* [Read about SPL Exceptions][splexe]
* [Nesting Exceptions In PHP][nesting-exceptions-in-php]
* [Exception Best Practices in PHP 5.3][exception-best-practices53]


[splext]: /#standard_php_library
[exceptions]: http://php.net/language.exceptions
[splexe]: http://php.net/spl.exceptions
[nesting-exceptions-in-php]: http://www.brandonsavage.net/exceptional-php-nesting-exceptions-in-php/
[exception-best-practices53]: http://ralphschindler.com/2010/09/15/exception-best-practices-in-php-5-3