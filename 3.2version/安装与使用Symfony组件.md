如果你有一个新的项目（或者已经有一个项目）要使用一个或多个 Symfony 的组件，最简单的方式就是使用 [Composer](https://getcomposer.org/) 。Composer 可以非常智能地帮你下载和安装你需要的组件，而且还帮你做好组件的自动加载，让你可以立即使用他们。

本篇将通过[ The Finder Component ]() 来说明安装和使用方法，这适用于其他任何组件。

## 使用 Finder 组件

1. 如果你是创建一个全新的项目，请为它创建一个目录。

2. 打开终端程序，使用 Composer 来安装这个库。

  ```shell
  composer require symfony/finder
  ```

  其中 `symfony/finder` 就是你想要安装的组件的名称。

  > 如果你还没有[安装 Composer ](https://getcomposer.org/download/)，取决于你使用哪种安装方式，你可能需要将 `composer.phar` 文件放置于当前目录中。在这种情况下，不必担心，你只需运行 `php composer.phar require symfony/finder` 命令即可。

3. 码起来！

  一旦 Composer 下载了组件，您所需要做的就是 `require` 由 Composer 生成的 `vendor/autoload.php` 文件。此文件负责自动加载所有库，以便您可以立即使用它们：

  ```php
  // File example: src/script.php

  // 将此处的路径更改为 "vendor/"
  // 目录的相对地址
  require_once __DIR__.'/../vendor/autoload.php';

  use Symfony\Component\Finder\Finder;

  $finder = new Finder();
  $finder->in('../data/');

  // ...
  ```
  
## 使用所有组件

如果你想使用所有的 Symfony 组件，那么不要逐个添加它们，你可以安装 `symfony/symfony` 包：

```sh
composer require symfony/symfony
```

这也将包括 Bundle 和 Bridge 库，你可能需要或可能不需要。

## 然后接下来？

现在组件已安装并自动加载，请阅读特定组件的文档以了解有关如何使用它的更多信息。

And have fun!

---
[下一篇：Asset 组件](https://github.com/lndj/symfony-components-doc-zh/blob/master/3.1version/Asset/the-asset-component.md)
