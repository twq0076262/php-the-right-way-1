# 依赖管理

PHP 有很多可供使用的库、框架和组件。通常你的项目都会使用到其中的若干项 - 这些就是项目的依赖。直到最近，PHP 也没有一个很好的方式来管理这些项目依赖。即使你通过手动的方式去管理，你依然会为自动加载器而担心。但现在这已经不再是问题了。

目前 PHP 有两个使用较多的包管理系统 － [Composer] 和 [PEAR]。Composer 是 PHP 所使用的主要的包管理器，然而在很长的一段时间里，PEAR 曾经扮演着这个角色。你应该了解 PEAR 是什么，因为即使你从来没有使用过它，你依然有可能会碰到对它的引用。

[Composer]: http://laravel-china.github.io/#composer_and_packagist
[PEAR]: http://laravel-china.github.io/#pear

## Composer 与 Packagist

Composer 是一个**杰出** 的依赖管理器。在 `composer.json` 文件中列出你项目所需的依赖包，加上一点简单的命令，Composer 将会自动帮你下载并设置你的项目依赖。

现在已经有许多 PHP 第三方包已兼容 Composer，随时可以在你的项目中使用。这些「packages(包)」都已列在 [Packagist]，这是一个官方的 Composer 兼容包仓库。

### 如何安装 Composer

你可以安装 Composer 到局部 (在你当前工作目录;这里不是很推荐)或是全局(e.g. /usr/local/bin)。我们假设你想安装 Composer 到局部。在你的项目根目录输入:

    curl -s https://getcomposer.org/installer | php

这条命令将会下载 `composer.phar` (一个 PHP 二进制档)。你可以使用 `php` 执行这个文件用来管理你的项目依赖。
**请注意：** 假如你是直接下载代码来编译，请先在线阅读代码确保它是安全的。

#### Windows环境下安装

对于Windows 的用户而言最简单的获取及执行方法就是使用 [ComposerSetup] 安装程序，它会执行一个全局安装并设置你的 `$PATH`，所以你在任意目录下在命令行中使用 `composer`。

### 如何手动安装 Composer

手动安装 Compose r是一个高端的技术;仅管如此还是有许多开发者有各种原因喜欢使用这种交互式的应用程序安装 Composer。在安装前请先确认你的PHP安装项目如下：

- 正在使用一个满足条件的 PHP 版本
- `.phar` 文件可以正确的被执行
- 相关的目录有足够的权限
- 相关有问题的扩展没有被载入
- 相关的 `php.ini` 设置已完成

由于手动安装没有执行这些检查，你必须自已衡量决定是否值得做这些事，以下是如何手动安装 Composer ：

    curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
    chmod +x $HOME/local/bin/composer

路径 `$HOME/local/bin` (或是你选择的路径) 应该在你的 `$PATH` 环境变量中。这将会影响 `composer` 这个命令是否可用.

当你遇到文档指出执行 Composer 的命令是 `php composer.phar install`时，你可以使用下面命令替代:

    composer install

本章节会假设你已经安装了全局的 Composer。

### 如何设置及安装依赖

Composer 会通过一个 `composer.json` 文件持续的追踪你的项目依赖。 如果你喜欢，你可以手动管理这个文件，或是使用 Composer 自己管理。`composer require` 这个指令会增加一个项目依赖，如果你还没有 `composer.json` 文件, 将会创建一个。这里有个例子为你的项目加入 [Twig] 依赖。

    composer require twig/twig:~1.8

另外 `composer init` 命令将会引导你创建一个完整的 `composer.json` 文件到你的项目之中。无论你使用哪种方式，一旦你创建了 `composer.json` 文件，你可以告诉 Composer 去下载及安装你的依赖到 `vendors/` 目录中。这命令也适用于你已经下载并已经提供了一个 `composer.json` 的项目：

    composer install

接下来，添加这一行到你应用的主要 PHP 文件中，这将会告诉 PHP 为你的项目依赖使用 Composer 的自动加载器。

```
{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}
```

现在你可以使用你项目中的依赖，且它们会在需要时自动完成加载。

### 更新你的依赖

Composer 会建立一个 `composer.lock` 文件，在你第一次执行 `php composer.phar install` 时，存放下载的每个依赖包精确的版本编号。假如你要分享你的项目给其他开发者，并且 `composer.lock` 文件也在你分享的文件之中的话。 当他们执行 `php composer.phar install` 这个命令时，他们将会得到与你一样的依赖版本。 当你要更新你的依赖时请执行 `php composer.phar update`。

当你需要灵活的定义你所需要的依赖版本时，这是最有用。 举例来说需要一个版本为 ~1.8 时，意味着 “任何大于 1.8.0 ，但小于 2.0.x-dev 的版本”。你也可以使用通配符 `*` 在 `1.8.*` 之中。现在Composer在`composer update` 时将升级你的所有依赖到你限制的最新版本。

### 更新通知

要接收关于新版本的更新通知。你可以注册 [VersionEye]，这个 web 服务可以监控你的 Github 及 BitBucket 帐号中的 `composer.json` 文件，并且当包有新更新时会发送邮件给你。

### 检查你的依赖安全问题

[Security Advisories Checker] 是一个 web 服务和一个命令行工具，二者都会仔细检查你的 `composer.lock` 文件，并且告诉你任何你需要更新的依赖。

### 处理 Composer 全局依赖

Composer 也可以处理全局依赖和他们的二进制文件。用法很直接，你所要做的就是在命令前加上`global`前缀。如果你想安装 PHPUnit 并使它全局可用，你可以运行下面的命令：

```
{% highlight console %}
composer global require phpunit/phpunit
{% endhighlight %}
```

这将会创建一个 `~/.composer` 目录存放全局依赖，要让已安装依赖的二进制命令随处可用，你需要添加 `~/.composer/vendor/bin` 目录到你的 `$PATH` 变量。

* [其他学习 Composer 相关资源][Learn about Composer]

[Packagist]: http://packagist.org/
[Twig]: http://twig.sensiolabs.org
[VersionEye]: https://www.versioneye.com/
[Security Advisories Checker]: https://security.sensiolabs.org/
[Learn about Composer]: http://getcomposer.org/doc/00-intro.md
[ComposerSetup]: https://getcomposer.org/Composer-Setup.exe

## PEAR 介绍

[PEAR][1] 是另一个常用的依赖包管理器, 它跟 Composer 很类似，但是也有一些显著的区别。

PEAR 需要扩展包有专属的结构, 开发者在开发扩展包的时候要提前考虑为 PEAR 定制, 否则后面将无法使用 PEAR.

PEAR 安装扩展包的时候, 是全局安装的, 意味着一旦安装了某个扩展包, 同一台服务器上的所有项目都能用上, 当然, 好处是当多个项目共同使用同一个扩展包的同一个版本, 坏处是如果你需要使用不同版本的话, 就会产生冲突.

### 如何安装 PEAR

你可以通过下载 `.phar` 文件来安装 PEAR. [官方文档安装部分][2] 里面有不同系统中安装 PEAR 的详细信息.

如果你是使用 Linux, 你可以尝试找下系统应用管理器, 举个栗子, Debian 和 Ubuntu 有个 `php-pear` 的 apt 安装包.

### 如何安装扩展包

如果扩展包是在 [PEAR packages list][3] 这个列表里面的, 你可以使用以下命令安装:

```
{% highlight console %}
pear install foo
{% endhighlight %}
```

如果扩展包是托管到别的渠道上, 你需要 `发现 (discover)` 渠道先, 请见文档 [使用渠道][4].

* [Learn about PEAR][1]

### 使用 Composer 来安装 PEAR 扩展包

如果你正在使用 [Composer][5], 并且你想使用一些 PEAR 的代码, 你可以通过 Composer 来安装 PEAR 扩展包.

下面是从 `pear2.php.net` 安装代码依赖的示例:

```
{% highlight json %}
{
    "repositories": [
        {
            "type": "pear",
            "url": "http://pear2.php.net"
        }
    ],
    "require": {
        "pear-pear2/PEAR2_Text_Markdown": "*",
        "pear-pear2/PEAR2_HTTP_Request": "*"
    }
}
{% endhighlight %}
```

第一部分 `"repositories"` 是让 Composer 从哪个渠道去获取扩展包, 然后, `"repositories"` 部分使用下面的命名规范:

> pear-channel/Package

前缀 "pear" 是为了避免冲突写死的.

成功安装扩展包以后, 代码会放到项目的 `vendor` 文件夹中, 并且可以通过加载 Composer 的自动加载器进行加载:

> vendor/pear-pear2.php.net/PEAR2_HTTP_Request/pear2/HTTP/Request.php

在代码里面可以这样使用:

```
{% highlight php %}
<?php
$request = new pear2\HTTP\Request();
{% endhighlight %}
```

* [学习更多 PEAR 和 Composer 的使用][6]


[1]: http://pear.php.net/
[2]: http://pear.php.net/manual/en/installation.getting.php
[3]: http://pear.php.net/packages.php
[4]: http://pear.php.net/manual/en/guide.users.commandline.channels.php
[5]: /#composer_and_packagist
[6]: http://getcomposer.org/doc/05-repositories.md#pear