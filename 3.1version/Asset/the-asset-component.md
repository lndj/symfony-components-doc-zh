# Asset 组件

> Asset 组件管理 Web 资产的 URL 生成和版本控制，例如 CSS 样式表，JavaScript 文件和图像文件。

在以往，web 应用通常会将资源文件的 URLs 写死，例如：

```html
<link rel="stylesheet" type="text/css" href="/css/main.css">

<!-- ... -->

<a href="/"><img src="/images/logo.png"></a>
```
除非你的应用非常简单，否则，我们不推荐这种做法。

写死 URLs 有些缺点，如下：

+ **模版累赘**：你不得不为每一项静态资源都写上全路径。若使用 Asset 组件，你就可以将静态资源依包的形式分组，以免重复书写通用部分的路径。

+ **版本控制苦难**： 必须为每个应用进行自定义的管理。对静态资源的 URLs 添加版本（例如：`main.css?v=5`），这对很多网站应用非常重要，因为这可以使你控制静态资源如何缓存。Asset 组件允许您为每个包定义不同的版本控制策略。

+ **移动静态资源的位置** 非常麻烦并且容易出错，它需要你仔细更新所有模板中包含的所有静态资源的路径。Asset 组件允许仅通过改变与静态资源包相关联的基本路径值来轻松地移动静态资源位置。

+ **几乎不可能使用多个CDN** ：这项技术需要你为每个请求随机更改静态资源的地址。Asset 组件对多 CDN 提供开箱即用的支持，包括常规的 `http://` 和安全的 `https://` 。

## 安装

你可以通过一下两种方式进行安装：
+ [通过 Composer 安装](https://github.com/lndj/symfony-components-doc-zh/blob/master/3.1version/%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8Symfony%E7%BB%84%E4%BB%B6.md) (`symfony/asset` on [Packagist](https://packagist.org/packages/symfony/asset))
+ 使用官方的Git仓库： [https://github.com/symfony/asset](https://github.com/symfony/asset)

## 使用

### 静态资源包

Asset组件通过包的形式来管理静态资源。包将所有共享相同属性的静态资源分组，例如：版本控制策略，基本路径，CDN 主机等。在以下基本示例中，创建一个包来管理静态资源，而不进行任何版本控制：

```php
use Symfony\Component\Asset\Package;
use Symfony\Component\Asset\VersionStrategy\EmptyVersionStrategy;

$package = new Package(new EmptyVersionStrategy());

echo $package->getUrl('/image.png');
// result: /image.png
```

`Packages` 类实现了 `PackageInterface` 接口，它定义了一下两个方法：

  `getVersion()`

  返回静态资源的版本

  `getUrl()`   

  返回绝对或相对与根公共目录相对的路径。

在使用一个包时，你可以：
<!-- 此处有锚点 -->
  1. 静态资源的版本
  2. 为静态资源设置一个通用的基本路径，（例如 `/css`）
  3. 为静态资源配置 CDN

### 静态资源版本控制

Asset 组件一个主要的特性就是管理应用程序静态资源的版本的能力。静态资源的版本通常被用来控制静态资源的缓存方式。

Asset 组件不是依赖于简单的版本机制，而是通过PHP类来定义高级版本控制策略。两个内置的策略分别是 `EmptyVersionStrategy` 和 `StaticVersionStrategy` ，前者不添加任何的版本给静态资源，后者允许你使用格式化的字符串来设置版本。

在下面的例子中，`StaticVersionStrategy` 添加 `v1` 后缀到任意一个静态资源路径上：

```php
use Symfony\Component\Asset\Package;
use Symfony\Component\Asset\VersionStrategy\StaticVersionStrategy;

$package = new Package(new StaticVersionStrategy('v1'));

echo $package->getUrl('/image.png');
// result: /image.png?v1
```

如果要修改版本格式，请传递一个 `sprintf` 兼容的格式字符串作为 `StaticVersionStrategy` 构造函数的第二个参数：

```php
// 将 'version' 放置在版本号之前
$package = new Package(new StaticVersionStrategy('v1', '%s?version=%s'));

echo $package->getUrl('/image.png');
// result: /image.png?version=v1

// 将版本号置于路径之前
$package = new Package(new StaticVersionStrategy('v1', '%2$s/%1$s'));

echo $package->getUrl('/image.png');
// result: /v1/image.png
```

### 自定义版本策略

使用 `VersionStrategyInterface` 接口来定义你自己的版本策略。例如，您的应用程序可能需要将当前日期追加到其所有网络资源，以便每天破坏缓存：

```php
use Symfony\Component\Asset\VersionStrategy\VersionStrategyInterface;

class DateVersionStrategy implements VersionStrategyInterface
{
    private $version;

    public function __construct()
    {
        $this->version = date('Ymd');
    }

    public function getVersion($path)
    {
        return $this->version;
    }

    public function applyVersion($path)
    {
        return sprintf('%s?v=%s', $path, $this->getVersion($path));
    }
}
```
### 静态资源分组

通常情况下，许多静态资源放置于同一路径下（例如： `/static/images` ），如果是这种情况，请使用 `PathPackage` 替换默认的 `Package` 类，以避免重复该路径：

```php
use Symfony\Component\Asset\PathPackage;
// ...

$package = new PathPackage('/static/images', new StaticVersionStrategy('v1'));

echo $package->getUrl('/logo.png');
// result: /static/images/logo.png?v1
```
