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
