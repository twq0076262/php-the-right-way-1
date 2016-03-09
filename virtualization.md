# 虚拟化技术

在开发和线上阶段使用不同的系统运行环境的话, 经常会遇到各种各样的 BUG, 并且在团队开发的时候, 让所有成员都保持使用最新版本的软件和类库, 也是一件很让人头痛的事情. 

如果你是在 Windows 下开发, 线上环境是 Linux (或者别的非 Windows 系统) 的话, 或者团队协同开发的时候, 建议使用虚拟机. 

除了大家熟知的 VMware 和 VirtualBox 外, 还有很多工具可以让你快速, 轻松的用上虚拟环境.

## Vagrant 简介

[Vagrant] 可以让你使用单一的配置信息来部署一套虚拟环境, 最后打包为一个所谓的 box (就是已经部署好环境的虚拟机器). 你可以手动来安装和配置 box, 也可以使用自动部署工具, 如 [Puppet] 或者 [Chef] .

自动部署工具可以让你快速部署一套一模一样的环境, 避免了一大堆的手动的命令输入, 并且允许你随时删除和重建一个全新的 box, 虚拟机的管理变得更加简单. 

Vagrant 还可以在虚拟机和主机上分享文件夹, 意味着你可以在主机里面编辑代码, 然后在虚拟机里面运行.

### 需要更多的帮助?

下面是一些其他的软件, 可以帮助你更好的使用 Vagrant: 

- [Rove][Rove]: 使用 Chef 自动化安装一些常用的软件, PHP 包含在内.
- [Puphpet][Puphpet]: 简单的 Web 图形界面用来生成部署 PHP 环境的 Puppet 脚本, 此项目不仅可以用在开发上, 也可以在生产环境中使用.
- [Protobox][Protobox]: 是一个基于 vagrant 的一个层, 还有 Web 图形界面, 允许你使用一个 YAML 文件来安装和配置虚拟机里面的软件.
- [Phansible][Phansible]: 提供了一个简单的 Web 图形界面, 用来创建 Ansible 自动化部署脚本, 专门为 PHP 项目定制.

[Vagrant]: http://vagrantup.com/
[Puppet]: http://www.puppetlabs.com/
[Chef]: http://www.opscode.com/
[Rove]: http://rove.io/
[Puphpet]: https://puphpet.com/
[Protobox]: http://getprotobox.com/
[Phansible]: http://phansible.com/

## Docker 简介

除了 Vagrant, Docker 是另一个实现生产和开发环境统一的非常棒的方案. 

Docker 为各种应用程序提供了 Linux 容器. 

你可以安装 Docker 镜像, 如 MySQL 和 PostgreSQL 等, 并且不会污染到你的本地机器, 可以看下 [Docker Hub Registry][docker-hub], 在这里你可以找到你想要的, 提前配置好的, 允许你简单几部就能运行起来的 Linux 容器. 

### 例子: 在 Docker 里面运行 PHP 应用

在你成功 [安装 Docker][docker-install] 后, 你只需要一步就可以安装 Apache + PHP.

下面的命令, 会下载一个功能齐全的 Apache 和 最新版本的 PHP, 并会设置 WEB 目录 `/path/to/your/php/files` 运行在 `http://localhost:8080`:

```
{% highlight console %}
docker run -d --name my-php-webserver -p 8080:80 -v /path/to/your/php/files:/var/www/html/ php:apache
{% endhighlight %}
```

在使用 `docker run` 命令以后, 如果你想停止, 或者再次开启容器的话, 只需要执行以下命令: 

```
{% highlight console %}
docker stop my-php-webserver
{% endhighlight %}

{% highlight console %}
docker start my-php-webserver
{% endhighlight %}
```

### 了解更多关于 Docker 的信息

The commands mentioned above only show a quick way to run an Apache web server with PHP support but there are a lot
more things that you can do with Docker. 

上面的命令能让你轻松使用 Apache + PHP 环境, 然而, Docker 还提供了好多别的命令, 例如, 作为 PHP 程序员, 一个最重要的事情, 是让你的 Web Server 和数据库链接起来, 怎么实现可以仔细看下 [Docker User Guide][docker-doc].

* [Docker Website][Docker]
* [Docker Installation][docker-install]
* [Docker Images at the Docker Hub Registry][docker-hub]
* [Docker User Guide][docker-doc]


[Docker]: http://docker.com/
[docker-hub]: https://registry.hub.docker.com/
[docker-install]: https://docs.docker.com/installation/
[docker-doc]: https://docs.docker.com/userguide/