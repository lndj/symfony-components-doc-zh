# Asset 组件

> Asset 组件管理 Web 资产的 URL 生成和版本控制，例如 CSS 样式表，JavaScript 文件和图像文件。

在以往，web 应用通常会将资源文件的 URLs 写死，例如：

```html
<link rel="stylesheet" type="text/css" href="/css/main.css">

<!-- ... -->

<a href="/"><img src="/images/logo.png"></a>
```
