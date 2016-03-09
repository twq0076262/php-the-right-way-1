# 数据库

绝大多数时候你的 PHP 程序都需要使用数据库来长久地保存数据。这时你有一些不同的选择可以来连接并与数据库进行交互。在 **PHP 5.1.0 之前**，我们推荐的方式是使用例如 [mysqli]，[pgsql]，[mssql] 等原生驱动。

原生驱动是在只使用 _一个_ 数据库的情况下的不错的方式，但如果，举个例子来说，你同时使用了 MySQL 和一点点 MSSQL，或者你需要使用 Oracle 的数据库，那你就不能够只使用一个数据库驱动了。你需要为每一个数据库去学习各自不同的 API &mdash; 这样做显然不科学。


[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[mssql]: http://php.net/mssql

## MySQL 扩展

PHP 中的 [mysql] 扩展已经不再进行新的开发了，并且已经[在 PHP 5.5.0 版本中正式被废弃][mysql_deprecated]，这意味着它将会在接下来的更新中被移除。如果你使用 `mysql_*` 开头的函数，例如 `mysql_connect()` 和 `mysql_query()` 的话，它们将会在后续的 PHP 版本无法使用。因此在以后的某个时间你需要重写你的代码。最好的办法是在你的开发计划中使用 [mysqli] 或 [PDO] 来取代 mysql 扩展，这样你才不会在后面手忙脚乱。

**如果你正从零开始，请一定避免使用 [mysql] 扩展：请选择 [MySQLi 扩展][mysqli],或者使用 [PDO]。**

* [PHP: MySQL增强版扩展][mysql_api]
* [MySQL 开发者 PDO 使用教程][pdo4mysql_devs]


[mysql]: http://php.net/mysql
[mysql_deprecated]: http://php.net/migration55.deprecated
[mysqli]: http://php.net/mysqli
[pdo]: http://php.net/pdo
[mysql_api]: http://php.net/mysqlinfo.api.choosing
[pdo4mysql_devs]: http://wiki.hashphp.org/PDO_Tutorial_for_MySQL_Developers

## PDO 扩展

[PDO] 是一个数据库连接抽象类库 &mdash; 自 5.1.0 版本起内置于 PHP 当中 &mdash; 它提供了一个通用的接口来与不同的数据库进行交互。比如你可以使用相同的简单代码来连接 MySQL 或是 SQLite：

```
{% highlight php %}
<?php
// PDO + MySQL
$pdo = new PDO('mysql:host=example.com;dbname=database', 'user', 'password');
$statement = $pdo->query("SELECT some_field FROM some_table");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['some_field']);

// PDO + SQLite
$pdo = new PDO('sqlite:/path/db/foo.sqlite');
$statement = $pdo->query("SELECT some_field FROM some_table");
$row = $statement->fetch(PDO::FETCH_ASSOC);
echo htmlentities($row['some_field']);
{% endhighlight %}
```

PDO 并不会对 SQL 请求进行转换或者模拟实现并不存在的功能特性；它只是单纯地使用相同的 API 连接不同种类的数据库。

更重要的是，`PDO` 使你能够安全的插入外部输入(例如 ID)到你的 SQL 请求中而不必担心 SQL 注入的问题。这可以通过使用 PDO 语句和限定参数来实现。

我们来假设一个 PHP 脚本接收一个数字 ID 作为一个请求参数。这个 ID 应该被用来从数据库中取出一条用户记录。下面是一个`错误`的做法：

```
{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/users.db');
$pdo->query("SELECT name FROM users WHERE id = " . $_GET['id']); // <-- NO!
{% endhighlight %}
```

这是一段糟糕的代码。你正在插入一个原始的请求参数到 SQL 请求中。这将让被黑客轻松地利用[SQL 注入]方式进行攻击。想一下如果黑客将一个构造的 `id` 参数通过像 `http://domain.com/?id=1%3BDELETE+FROM+users` 这样的 URL 传入。这将会使 `$_GET['id']` 变量的值被设为 `1;DELETE
FROM users` 然后被执行从而删除所有的 user 记录！因此，你应该使用 PDO 限制参数来过滤 ID 输入。

```
{% highlight php %}
<?php
$pdo = new PDO('sqlite:/path/db/users.db');
$stmt = $pdo->prepare('SELECT name FROM users WHERE id = :id');
$id = filter_input(INPUT_GET, 'id', FILTER_SANITIZE_NUMBER_INT); // <-- filter your data first (see [Data Filtering](http://laravel-china.github.io/php-the-right-way/#data_filtering)), especially important for INSERT, UPDATE, etc.
$stmt->bindParam(':id', $id, PDO::PARAM_INT); // <-- Automatically sanitized for SQL by PDO
$stmt->execute();
{% endhighlight %}
```

这是正确的代码。它在一条 PDO 语句中使用了一个限制参数。这将对外部 ID 输入在发送给数据库之前进行转义来防止潜在的 SQL 注入攻击。

对于写入操作，例如 INSERT 或者 UPDATE，进行[数据过滤](http://laravel-china.github.io/php-the-right-way/#data_filtering)并对其他内容进行清理（去除 HTML 标签，Javascript 等等）是尤其重要的。PDO 只会为 SQL 进行清理，并不会为你的应用做任何处理。

* [了解 PDO]

你也应该知道数据库连接有时会耗尽全部资源，如果连接没有被隐式地关闭的话，有可能会造成可用资源枯竭的情况。不过这通常在其他语言中更为常见一些。使用 PDO 你可以通过销毁（destroy）对象，也就是将值设为 NULL，来隐式地关闭这些连接，确保所有剩余的引用对象的连接都被删除。如果你没有亲自做这件事情，PHP 会在你的脚本结束的时候自动为你完成 —— 除非你使用的是持久链接。

* [了解 PDO 连接]


[pdo]: http://php.net/pdo
[SQL Injection]: http://wiki.hashphp.org/Validation
[了解 PDO]: http://php.net/book.pdo
[了解 PDO 连接]: http://php.net/pdo.connections

## 数据库交互

当开发者第一次接触 PHP 时，通常会使用类似下面的代码来将数据库的交互与表示层逻辑混在一起：

```
{% highlight php %}
<ul>
<?php
foreach ($db->query('SELECT * FROM table') as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>";
}
?>
</ul>
{% endhighlight %}
```

这从很多方面来看都是错误的做法，主要是由于它不易阅读又难以测试和调试。而且如果你不加以限制的话，它会输出非常多的字段。

其实还有许多不同的解决方案来完成这项工作 — 取决于你倾向于 [面向对象编程（OOP）](http://laravel-china.github.io/#object-oriented-programming)还是[函数式编程](http://laravel-china.github.io/#functional-programming) — 但必须有一些分离的元素。

来看一下最基本的做法：

```
{% highlight php %}
<?php
function getAllFoos($db) {
    return $db->query('SELECT * FROM table');
}

foreach (getAllFoos($db) as $row) {
    echo "<li>".$row['field1']." - ".$row['field1']."</li>"; // BAD!!
}
{% endhighlight %}
```

这是一个不错的开头。将这两个元素放入了两个不同的文件于是你得到了一些干净的分离。

创建一个类来放置上面的函数，你就得到了一个「Model」。创建一个简单的`.php`文件来存放表示逻辑，你就得到了一个「View」。这已经很接近 [MVC] — 一个大多数[框架](http://laravel-china.github.io/#frameworks)常用的面向对象的架构。

**foo.php**

```
{% highlight php %}
<?php
$db = new PDO('mysql:host=localhost;dbname=testdb;charset=utf8', 'username', 'password');

// Make your model available
include 'models/FooModel.php';

// Create an instance
$fooModel = new FooModel($db);
// Get the list of Foos
$fooList = $fooModel->getAllFoos();

// Show the view
include 'views/foo-list.php';
{% endhighlight %}
```

**models/FooModel.php**

```
{% highlight php %}
<?php
class FooModel
{
    protected $db;

    public function __construct(PDO $db)
    {
        $this->db = $db;
    }

    public function getAllFoos() {
        return $this->db->query('SELECT * FROM table');
    }
}
{% endhighlight %}
```

**views/foo-list.php**

```
{% highlight php %}
<?php foreach ($fooList as $row): ?>
    <?= $row['field1'] ?> - <?= $row['field1'] ?>
<?php endforeach ?>
{% endhighlight %}
```

向大多数现代框架的做法学习是很有必要的，尽管多了一些手动的工作。你可以并不需要每一次都完全这么做，但将太多的表示逻辑层代码和数据库交互掺杂在一些将会为你在想要对程序进行[单元测试](http://laravel-china.github.io/#unit-testing)时带来真正的麻烦。

[PHPBridge] 具有一项非常棒的资源叫做[创建一个数据类]。它包含了非常相似的逻辑而且非常适合刚刚习惯数据库交互概念的开发者使用。


[MVC]: http://code.tutsplus.com/tutorials/mvc-for-noobs--net-10488
[PHPBridge]: http://phpbridge.org/
[创建一个数据类]: http://phpbridge.org/intro-to-php/creating_a_data_class

## 数据库抽象层

许多框架都提供了自己的数据库抽象层，其中一些是设计在 [PDO][1] 的上层的。这些抽象层通常将你的请求在 PHP 方法中包装起来，通过模拟的方式来使你的数据库拥有一些之前不支持的功能。这种抽象是真正的数据库抽象，而不单单只是 PDO 提供的数据库连接抽象。这类抽象的确会增加一定程度的性能开销，但如果你正在设计的应用程序需要同时使用 MySQL，PostgreSQL 和 SQLite 时，一点点的额外性能开销对于代码整洁度的提高来说还是很值得的。

有一些抽象层使用的是[PSR-0][psr0] 或 [PSR-4][psr4] 命名空间标准，所以他们可以安装在任何你需要的应用程序中。

* [Aura SQL][6]
* [Doctrine2 DBAL][2]
* [Propel][7]
* [ZF2 Db][4]


[1]: http://php.net/book.pdo
[2]: http://www.doctrine-project.org/projects/dbal.html
[4]: http://packages.zendframework.com/docs/latest/manual/en/index.html#zend-db
[6]: https://github.com/auraphp/Aura.Sql
[7]: http://propelorm.org/
[psr0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[psr4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md
