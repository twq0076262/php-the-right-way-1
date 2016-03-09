# 入门指南

## 使用当前稳定版本 (5.6)

如果你刚开始学习 PHP，请使用最新的稳定版本 [PHP 5.6][php-release]。PHP 近年来有了巨大的改进，增加了许多强大的 [新特性](http://laravel-china.github.io/php-the-right-way/#language_highlights)。虽然 5.2 和 5.6 之间增加的版本号似乎很小， 但它代表了 _重大的_ 改进。如果你想查找一个函数及其用法，可以去官方手册 [php.net][php-docs] 中查找。

[php-release]: http://php.net/downloads.php
[php-docs]: http://php.net/manual/

## 内置的 web 服务器

PHP 5.4 之后, 你可以不用安装和配置功能齐全的 Web 服务器，就可以开始学习 PHP。
要启动内置的 Web 服务器，需要从你的命令行终端进入项目的 Web 根目录，执行下面的命令:

```
> php -S localhost:8000
```

* [了解更多内置的命令行服务器][cli-server]


[cli-server]: http://php.net/features.commandline.webserver

## Mac 安装

OSX 系统会预装 PHP， 只是一般情况下版本会比最新稳定版低一些。目前 Lion 是  5.3.10，
Mavericks 是 5.4.17， Yosemite 则是 5.5.9， 但在 PHP 5.6 出来之后， 这些往往是不够的。

这里有许多方式在 OS X 上安装 PHP 。

### 通过 Homebrew 安装 PHP

[Homebrew] 是一个强大的 OS X 专用包管理器， 它可以帮助你轻松的安装 PHP 和各种扩展。
[Homebrew PHP] 是一个包含与 PHP 相关的 Formulae，能让你通过 homebrew 安装 PHP 的仓库。

也就是说, 你可以通过 `brew install` 命令安装 `php53`, `php54`, `php55` 或者 `php56` ，并且通过修改 `PATH` 变量来切换各个版本。或者你也可以使用 [brew-php-switcher][brew-php-switcher] 来自动切换。

### 通过 Macports 安装 PHP

[MacPorts] 是一个开源的，社区发起的项目，它的目的在于设计一个易于使用的系统，方便编译，安装以及升级 OS X 系统上的 command-line, X11 或者基于 Aqua 的开源软件。

MacPorts 支持预编译的二进制文件，因此你不必每次都重新从源码压缩包编译，如果你的系统没有安装这些包，它会节省你很多时间。

此时，你可以通过 `port install` 命名来安装 `php53`，`php54`，`php55` 或者 `php56`，比如：

    sudo port install php54
    sudo port install php55

你也可以执行 `select` 命令来切换当前的 php 版本：

    sudo port select --set php php55


### 通过 phpbrew 安装 PHP

[phpbrew] 是一个安装与管理多个 PHP 版本的工具。它在应用程序或者项目需要不同版本的 PHP 时非常有用，让你不再需要使用虚拟机来处理这些情况。

### 通过 Liip's binary installer 安装 PHP
[php-osx.liip.ch] 是另一种流行的选择，它提供了从5.3到5.6版本的单行安装功能。
它并不会覆盖Apple集成的PHP文件，而是将其安装在了一个独立的目录中(/usr/local/php5)。

### 源码编译

另一个让你控制安装 PHP 版本的选择就是 [自行编译][mac-compile]。
如果使用这种方法， 你必须先确认是否已经通过 「Apple's Mac Developer Center」 下载、安装 [Xcode][xcode-gcc-substitution] 或者 ["Command Line Tools for XCode"]。

### 集成包 (All-in-One Installers)

上面列出的解决方案主要是针对 PHP 本身， 并不包含：比如 Apache，Nginx 或者 SQL 服务器。
集成包比如 [MAMP][mamp-downloads] 和 [XAMPP][xampp] 会安装这些软件并且将他们绑在一起，不过易于安装的背后也牺牲了一定的弹性。


[Homebrew]: http://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[php-osx.liip.ch]: http://php-osx.liip.ch/
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[mac-compile]: http://php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
["Command Line Tools for XCode"]: https://developer.apple.com/downloads
[mamp-downloads]: http://www.mamp.info/en/downloads/
[xampp]: http://www.apachefriends.org/en/xampp.html
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher

## Windows 安装

你可以从 [windows.php.net/download][php-downloads] 下载二进制包。 解压后, 最好为你的 PHP 所在的根目录（php.exe 所在的文件夹）设置 [PATH][windows-path]，这样就可以从命令行中直接执行 PHP。

Windows 下有多种安装 PHP 的方式，你可以 [下载二进制安装包][php-downloads] 并使用 `.msi` 安装程序。从 PHP 5.3.0 之后，这个安装程序将不再提供下载支持。

如果只是学习或者本地开发，可以直接使用 PHP 5.4+ 内置的 Web 服务器， 还能省去配置服务器的麻烦。如果你想要包含有网页服务器以及 MySql 的集成包，那么像是[Web Platform Installer][wpi], [XAMPP][xampp], [EasyPHP][easyphp] 和 [WAMP][wamp] 这类工具将会帮助你快速建立 Windows 开发环境。不过这些工具将会与线上环境有些许差别，如果你是在 Windows 下开发，而生产环境则部署至 Linux ，请小心。

如果你需要将生产环境部署在 Windows 上，那 IIS7 将会提供最稳定和最佳的性能。你可以使用 [phpmanager][phpmanager] (IIS7 的图形化插件) 让你简单的设置并管理 PHP。IIS7 也有内置的 FastCGI ，你只需要将 PHP 配置为它的处理器即可。更多详情请见[dedicated area on iis.net][php-iis]。


[php-downloads]: http://windows.php.net/download/
[windows-path]: http://www.windows-commandline.com/set-path-command-line/
[wpi]: http://www.microsoft.com/web/downloads/platform.aspx
[xampp]: http://www.apachefriends.org/en/xampp.html
[easyphp]: http://www.easyphp.org/
[wamp]: http://www.wampserver.com/en/
[phpmanager]: http://phpmanager.codeplex.com/
[php-iis]: http://php.iis.net/