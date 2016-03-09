# 安全

## Web 应用程序安全

攻击者无时无刻不在准备对你的 Web 应用程序进行攻击，因此提高你的 Web 应用程序的安全性是非常有必要的。幸运的是，来自[开放式 Web 应用程序安全项目][1] (OWASP) 的有心人已经整理了一份包含了已知安全问题和防御方式的全面的清单。这份清单对于具有安全意识的开发者来说是必读的。

* [阅读 OWASP 安全指南][2]


[1]: https://www.owasp.org/
[2]: https://www.owasp.org/index.php/Guide_Table_of_Contents

## 密码哈希

每个人在建构 PHP 应用时终究都会加入用户登录的模块。用户的帐号及密码会被储存在数据库中，在登录时用来验证用户。

在存储密码前正确的哈希密码是非常重要的。哈希密码是单向不可逆的，该哈希值是一段固定长度的字符串且无法逆向推算出原始密码。这就代表你可以哈希另一串密码，来比较两者是否是同一个密码，但又无需知道原始的密码。如果你不将密码哈希，那么当未授权的第三者进入你的数据库时，所有用户的帐号资料将会一览无遗。有些用户可能（很不幸的）在别的网站也使用相同的密码。所以务必要重视数据安全的问题。

**使用 `password_hash` 来哈希密码**

`password_hash` 函数在 PHP 5.5 时被引入。 此函数现在使用的是目前 PHP 所支持的最强大的加密算法 BCrypt 。 当然，此函数未来会支持更多的加密算法。 `password_compat` 库的出现是为了提供对 PHP >= 5.3.7 版本的支持。

在下面例子中，我们哈希一个字符串，然后和新的哈希值对比。因为我们使用的两个字符串是不同的（'secret-password' 与 'bad-password'），所以登录失败。

```
{% highlight php %}
<?php
require 'password.php';

$passwordHash = password_hash('secret-password', PASSWORD_DEFAULT);

if (password_verify('bad-password', $passwordHash)) {
    // Correct Password
} else {
    // Wrong password
}
{% endhighlight %}
```

* [了解 `password_hash()`] [1]
* [PHP >= 5.3.7 && < 5.5 的 `password_compat`] [2]
* [了解密码学中的哈希] [3]
* [PHP `password_hash()` RFC] [4]


[1]: http://php.net/function.password-hash
[2]: https://github.com/ircmaxell/password_compat
[3]: http://en.wikipedia.org/wiki/Cryptographic_hash_function
[4]: https://wiki.php.net/rfc/password_hash

## 数据过滤

永远不要信任外部输入。请在使用外部输入前进行过滤和验证。`filter_var()` 和 `filter_input()` 函数可以过滤文本并对格式进行校验（例如 email 地址）。

外部输入可以是任何东西：`$_GET` 和 `$_POST` 等表单输入数据，`$_SERVER` 超全局变量中的某些值，还有通过 `fopen('php://input', 'r')` 得到的 HTTP 请求体。记住，外部输入的定义并不局限于用户通过表单提交的数据。上传和下载的文档，session 值，cookie 数据，还有来自第三方 web 服务的数据，这些都是外服输入。

虽然外部输入可以被存储、组合并在以后继续使用，但它依旧是外部输入。每次你处理、输出、连结或在代码中包含时，请提醒自己检查数据是否已经安全地完成了过滤。

数据可以根据不同的目的进行不同的 _过滤_ 。比如，当原始的外部输入被传入到了 HTML 页面的输出当中，它可以在你的站点上执行 HTML 和 JavaScript 脚本！这属于跨站脚本攻击（XSS），是一种很有杀伤力的攻击方式。一种避免 XSS 攻击的方法是在输出到页面前对所有用户生成的数据进行清理，使用 `strip_tags()` 函数来去除 HTML 标签或者使用 `htmlentities()` 或是 `htmlspecialchars()` 函数来对特殊字符分别进行转义从而得到各自的 HTML 实体。

另一个例子是传入能够在命令行中执行的选项。这是非常危险的（同时也是一个不好的做法），但是你可以使用自带的 `escapeshellarg()` 函数来过滤执行命令的参数。

最后的一个例子是接受外部输入来从文件系统中加载文件。这可以通过将文件名修改为文件路径来进行利用。你需要过滤掉`"/"`, `"../"`, [null 字符][6]或者其他文件路径的字符来确保不会去加载隐藏、私有或者敏感的文件。

* [学习更多数据过滤][1]
* [学习更多 `filter_var`][4]
* [学习更多 `filter_input`][5]
* [学习更多 null 字符问题][6]

### 数据清理

数据清理是指删除（或转义）外部输入中的非法和不安全的字符。

例如，你需要在将外部输入包含在 HTML 中或者插入到原始的 SQL 请求之前对它进行过滤。当你使用 [PDO](http://laravel-china.github.io/php-the-right-way/#databases) 中的限制参数功能时，它会自动为你完成过滤的工作。

有些时候你可能需要允许一些安全的 HTML 标签输入进来并被包含在输出的 HTML 页面中，但这实现起来并不容易。尽管有一些像 [HTML Purifier][html-purifier] 的白名单类库为了这个原因而出现，实际上更多的人通过使用其他更加严格的格式限制方式例如使用 Markdown 或 BBCode 来避免出现问题。

[查看 Sanitization Filters][2]

### 有效性验证

验证是来确保外部输入的是你所想要的内容。比如，你也许需要在处理注册申请时验证 email 地址、手机号码或者年龄等信息的有效性。

[查看 Validation Filters][3]


[1]: http://php.net/book.filter
[2]: http://php.net/filter.filters.sanitize
[3]: http://php.net/filter.filters.validate
[4]: http://php.net/function.filter-var
[5]: http://php.net/function.filter-input
[6]: http://php.net/security.filesystem.nullbytes
[html-purifier]: http://htmlpurifier.org/

## 配置文件

当你在为你的应用程序创建配置文件时，最好的选择时参照以下的做法：

- 推荐你将你的配置信息存储在无法被直接读取和上传的位置上。
- 如果你一定要存储配置文件在根目录下，那么请使用 `.php` 的扩展名来进行命名。这将可以确保即使脚本被直接访问到，它也不会被以明文的形式输出出来。
- 配置文件中的信息需要被针对性的保护起来，对其进行加密或者设置访问权限。

## 注册全局变量

**注意：** 自 PHP 5.4.0 开始，`register_globals` 选项已经被移除并不再使用。这是在提醒你如果你正在升级旧的应用程序的话，你需要注意这一点。

当 `register_globals` 选项被开启时，它会使许多类型的变量（包括 `$_POST`, `$_GET` 和 `$_REQUEST`）被注册为全局变量。这将很容易使你的程序无法有效地判断数据的来源并导致安全问题。

例如：`$_GET['foo']` 可以通过 `$foo` 被访问到，也就是可以对未声明的变量进行覆盖。如果你使用低于 5.4.0 版本的 PHP 的话，请 __确保__ `register_globals` 是被设为 __off__ 的。

* [在 PHP 手册中了解 Register_globals](http://php.net/security.globals)

## 错误报告

错误日志对于发现程序中的错误是非常有帮助的，但是有些时候它也会将应用程序的结构暴露给外部。为了有效的保护你的应用程序不受到由此而引发的问题。你需要将在你的服务器上使用开发和生产（线上）两套不同的配置。

### 开发环境

为了在<strong>开发</strong>环境中显示所有可能的错误，将你的 `php.ini` 进行如下配置：

```
{% highlight ini %}
display_errors = On
display_startup_errors = On
error_reporting = -1
log_errors = On
{% endhighlight %}
```

> 将值设为 `-1` 将会显示出所有的错误，甚至包括在未来的 PHP 版本中新增加的类型和参数。
> 和 PHP 5.4 起开始使用的 `E_ALL` 是相同的。-
> [php.net](http://php.net/function.error-reporting)

`E_STRICT` 类型的错误是在 5.3.0 中被引入的，并没有被包含在 `E_ALL` 中。然而从 5.4.0 开始，它被包含在了 `E_ALL` 中。这意味着什么？这表示如果你想要在 5.3 中显示所有的错误信息，你需要使用 `-1` 或者 `E_ALL | E_STRICT`。

**不同 PHP 版本下开启全部错误显示**

* &lt; 5.3 `-1` 或 `E_ALL`
* &nbsp; 5.3 `-1` 或 `E_ALL | E_STRICT`
* &gt; 5.3 `-1` 或 `E_ALL`

### 生产环境

为了在<strong>生产</strong>环境中隐藏错误显示，将你的 `php.ini` 进行如下配置：

```
{% highlight ini %}
display_errors = Off
display_startup_errors = Off
error_reporting = E_ALL
log_errors = On
{% endhighlight %}
```

当在生产环境中使用这个配置时，错误信息依旧会被照常存储在 web 服务器的错误日志中，唯一不同的是将不再显示给用户。更多关于设置的信息，请参考 PHP 手册：

* [错误报告](http://php.net/errorfunc.configuration#ini.error-reporting)
* [显示错误](http://php.net/errorfunc.configuration#ini.display-errors)
* [显示启动错误](http://php.net/errorfunc.configuration#ini.display-startup-errors)
* [记录错误](http://php.net/errorfunc.configuration#ini.log-errors)