# 开发实践

## 基础知识

PHP 是一门庞大的语言，各个水平层次的开发者都可以利用它进行迅捷高效的开发。然而在对语言逐渐深入的学习过程中，我们往往会因为走捷径和/或不良习惯而忘记（或忽视掉）基础的知识。为了帮助彻底解决这个问题，这一章的目的就是提醒开发人员注意有关 PHP 的基础编程实践。

* 学习更多[基础知识](http://laravel-china.github.io/php-the-right-way/pages/The-Basics.html)

## 日期和时间

PHP 中 DateTime 类的作用是在你读、写、比较或者计算日期和时间时提供帮助。除了 DateTime 类之外，PHP 还有很多与日期和时间相关的函数，但 DateTime 类为大多数常规使用提供了优秀的面向对象接口。它还可以处理时区，不过这并不在这篇简短的介绍之内。

在使用 DateTime 之前，通过 `createFromFormat()` 工厂方法将原始的日期与时间字符串转换为对象或使用 `new DateTime` 来取得当前的日期和时间。使用 `format()` 将 DateTime 转换回字符串用于输出。

```
{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTime::createFromFormat('d. m. Y', $raw);

echo 'Start date: ' . $start->format('Y-m-d') . "\n";
{% endhighlight %}
```

对 DateTime 进行计算时可以使用 DateInterval 类。DateTime 类具有例如 `add()` 和 `sub()` 等将 DateInterval 当作参数的方法。编写代码时注意不要认为每一天都是由相同的秒数构成的，不论是夏令时（DST）还是时区转换，使用时间戳计算都会遇到问题，应当选择日期间隔。使用 `diff()` 方法来计算日期之间的间隔，它会返回新的 DateInterval，非常容易进行展示。

```
{% highlight php %}
<?php
// create a copy of $start and add one month and 6 days
$end = clone $start;
$end->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Difference: ' . $diff->format('%m month, %d days (total: %a days)') . "\n";
// Difference: 1 month, 6 days (total: 37 days)
{% endhighlight %}
```

DateTime 对象之间可以直接进行比较：

```
{% highlight php %}
<?php
if ($start < $end) {
    echo "Start is before end!\n";
}
{% endhighlight %}
```

最后一个例子来演示 DatePeriod 类。它用来对循环的事件进行迭代。向它传入开始时间、结束时间和间隔区间，会得到这其中所有的事件。

```
{% highlight php %}
<?php
// output all thursdays between $start and $end
$periodInterval = DateInterval::createFromDateString('first thursday');
$periodIterator = new DatePeriod($start, $periodInterval, $end, DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // output each date in the period
    echo $date->format('Y-m-d') . ' ';
}
{% endhighlight %}
```

* [阅读 DateTime][datetime]
* [阅读日期格式][dateformat] (支持的日期字符串格式)

[datetime]: http://php.net/book.datetime
[dateformat]: http://php.net/function.date

## 设计模式 {#design_patterns_title}

当你在编写自己的应用程序时，最好在项目的代码和整体架构中使用通用的设计模式，这将帮助你更轻松地对程序进行维护，也能够让其他的开发者更快地理解你的代码。

当你使用框架进行开发时，绝大部分的上层代码以及项目结构都会基于所使用的框架，因此很多关于设计模式的决定已经由框架帮你做好了。当然，你还是可以挑选你最喜欢的模式并在你的代码中进行应用。但如果你并没有使用框架的话，你就需要自己去寻找适合你的应用的最佳模式了。

* 学习更多[设计模式](http://laravel-china.github.io/php-the-right-way/pages/Design-Patterns.html)

## 使用 UTF-8 编码

_本章是由 [Alex Cabal](https://alexcabal.com/) 最初撰写在 [PHP Best Practices](https://phpbestpractices.org/#utf-8) 中的，我们使用它作为进行建议的基础_。

### 这不是在开玩笑。请小心、仔细并且前后一致地处理它。

目前，PHP 仍未在底层实现对 Unicode 的支持。虽然有很多途径可以确保 UTF-8 字符串能够被正确地处理，但这并不是很简单的事情，通常需要对 Web 应用进行全方面的检查，从 HTML 到 SQL 再到 PHP。我们将争取进行一个简洁实用的总结。

### PHP 层面的 UTF-8

最基本的字符串操作，像是连结两个字符串或将字符串赋值给变量，并不需要对 UTF-8 做特别的处理。然而大多数字符串的函数，像 `strpos()` 和 `strlen()`，确实需要特别的处理。这些函数名中通常包含 `mb_*`：比如，`mb_strpos()` 和 `mb_strlen()`。这些 `mb_*` 字符串是由 [Multibyte String Extension] 提供支持的，它专门为操作 Unicode 字符串而特别进行了设计。

在操作 Unicode 字符串时，请你务必使用 `mb_*` 函数。例如，如果你对一个 UTF-8 字符串使用 `substr()`，那返回的结果中有很大可能会包含一些乱码。正确的方式是使用 `mb_substr()`。

最难的地方在于每次都要记得使用 `mb_*` 函数。如果你哪怕只有一次忘记了使用，你的 Unicode 字符串就有在接下来的过程中变成乱码的风险。

不是所有的字符串函数都有一个对应的 `mb_*` 函数。如果你想要的功能没有对应的 `mb_*` 函数的话，那只能说你运气不佳了。

你应该在你所有的 PHP 脚本（或全局包含的脚本）的开头使用 `mb_internal_encoding()` 函数，然后紧接着在会对浏览器进行输出的脚本中使用 `mb_http_output()`。在每一个脚本当中明确声明字符串的编码可以免去很多日后的烦恼。

另外，许多对字符串进行操作的函数都有一个可选的参数用来指定字符串编码。当可以设定这类参数时，你应该始终明确指定使用 UTF-8。例如，`htmlentities()` 有一个字符编码的选项，你应该始终将其设为 UTF-8。从 PHP 5.4.0 开始, `htmlentities()` 和 `htmlspecialchars()` 的编码都已经被默认设为了 UTF-8。

最后，如果你所编写的是分布式的应用程序并且不能确定 `mbstring` 扩展一定开启的话，可以考虑使用 [patchwork/utf8] Composer 包。它会在 `mbstring` 可用时自动使用，否则自动切换回非 UTF-8 函数。

[Multibyte String Extension]: http://php.net/book.mbstring
[patchwork/utf8]: https://packagist.org/packages/patchwork/utf8

### 数据库层面的 UTF-8

如果你使用 PHP 来操作到 MySQL，有些时候即使你做到了上面的每一点，你的字符串仍可能面临在数据库中以非 UTF-8 的格式进行存储的问题。

为了确保你的字符串从 PHP 到 MySQL都使用 UTF-8，请检查确认你的数据库和数据表都设定为 `utf8mb4` 字符集和整理，并且确保你的 PDO 连接请求也使用了 `utf8mb4` 字符集。请看下方的示例代码，这是 _非常重要_ 的。

请注意为了完整的 UTF-8 支持，你必须使用 `utf8mb4` 而不是  `utf8`！你会在进一步阅读中找到原因。

### 浏览器层面的 UTF-8

使用 `mb_http_output()` 函数来确保 PHP 向浏览器输出 UTF-8 格式的字符串。

随后浏览器需要接收 HTTP 应答来指定页面是由 UTF-8 进行编码的。以前这一步是通过在页面 `<head>` 标签下包含[字符集 `<meta>` 标签](http://htmlpurifier.org/docs/enduser-utf8.html)实现的，这是一种可行的方式。但更好的做法是在 `Content-Type` 响应头中进行设置，因为这样做的速度会[更快](https://developers.google.com/speed/docs/best-practices/rendering#SpecifyCharsetEarly)。

```
{% highlight php %}
<?php
// Tell PHP that we're using UTF-8 strings until the end of the script
mb_internal_encoding('UTF-8');

// Tell PHP that we'll be outputting UTF-8 to the browser
mb_http_output('UTF-8');

// Our UTF-8 test string
$string = 'Êl síla erin lû e-govaned vîn.';

// Transform the string in some way with a multibyte function
// Note how we cut the string at a non-Ascii character for demonstration purposes
$string = mb_substr($string, 0, 15);

// Connect to a database to store the transformed string
// See the PDO example in this document for more information
// Note the `charset=utf8mb4` in the Data Source Name (DSN)
$link = new PDO(
    'mysql:host=your-hostname;dbname=your-db;charset=utf8mb4',
    'your-username',
    'your-password',
    array(
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_PERSISTENT => false
    )
);

// Store our transformed string as UTF-8 in our database
// Your DB and tables are in the utf8mb4 character set and collation, right?
$handle = $link->prepare('insert into ElvishSentences (Id, Body) values (?, ?)');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->bindValue(2, $string);
$handle->execute();

// Retrieve the string we just stored to prove it was stored correctly
$handle = $link->prepare('select * from ElvishSentences where Id = ?');
$handle->bindValue(1, 1, PDO::PARAM_INT);
$handle->execute();

// Store the result into an object that we'll output later in our HTML
$result = $handle->fetchAll(\PDO::FETCH_OBJ);

header('Content-Type: text/html; charset=UTF-8');
?><!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>UTF-8 test page</title>
    </head>
    <body>
        <?php
        foreach($result as $row){
            print($row->Body);  // This should correctly output our transformed UTF-8 string to the browser
        }
        ?>
    </body>
</html>
{% endhighlight %}
```

### 延伸阅读

* [PHP 手册：字符串运算符](http://php.net/language.operators.string)
* [PHP 手册：字符串函数](http://php.net/ref.strings)
    * [`strpos()`](http://php.net/function.strpos)
    * [`strlen()`](http://php.net/function.strlen)
    * [`substr()`](http://php.net/function.substr)
* [PHP 手册：多字节字符串函数](http://php.net/ref.mbstring)
    * [`mb_strpos()`](http://php.net/function.mb-strpos)
    * [`mb_strlen()`](http://php.net/function.mb-strlen)
    * [`mb_substr()`](http://php.net/function.mb-substr)
    * [`mb_internal_encoding()`](http://php.net/function.mb-internal-encoding)
    * [`mb_http_output()`](http://php.net/function.mb-http-output)
    * [`htmlentities()`](http://php.net/function.htmlentities)
    * [`htmlspecialchars()`](http://php.net/function.htmlspecialchars)
* [PHP UTF-8 Cheatsheet](http://blog.loftdigital.com/blog/php-utf-8-cheatsheet)
* [Handling UTF-8 with PHP](http://www.phpwact.org/php/i18n/utf-8)
* [Stack Overflow: What factors make PHP Unicode-incompatible?](http://stackoverflow.com/questions/571694/what-factors-make-php-unicode-incompatible)
* [Stack Overflow: Best practices in PHP and MySQL with international strings](http://stackoverflow.com/questions/140728/best-practices-in-php-and-mysql-with-international-strings)
* [How to support full Unicode in MySQL databases](http://mathiasbynens.be/notes/mysql-utf8mb4)
* [Bringing Unicode to PHP with Portable UTF-8](http://www.sitepoint.com/bringing-unicode-to-php-with-portable-utf8/)
* [Stack Overflow: DOMDocument loadHTML does not encode UTF-8 correctly](http://stackoverflow.com/questions/8218230/php-domdocument-loadhtml-not-encoding-utf-8-correctly)
