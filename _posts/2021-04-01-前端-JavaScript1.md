# JavaScript



## 在Rails中使用JavaScript



### Web浏览器常规工作原理

在浏览器输入地址 http://localhost:3000 后，浏览器（客户端）会向服务器发起一个请求。然后浏览器处理响应，获取相关的静态资源文件，比如JavaScript、样式表和图像，然后显示页面内容。点击链接后发生的事情也是如此：获取页面，获取静态资源，把全部内容放在一起，显示最终网页。这个过程叫做**”请求响应循环“**。

### 非侵入式JavaScript

Rails使用一种叫做**”非侵入式JavaScript“（Unobtrusive JavaScript）**的技术把JavaScript依附到DOM上。

**”行间JavaScript“**实例：

```html
<a href="#" onclick="this.style.backgroundColor='#990000'">Paint it red</a>
```

**”非侵入式JavaScript“**实例：

```html
<a href="#" data-background-color="#990000">Paint it red</a>
<a href="#" data-background-color="#009900" data-text-color="#FFFFFF">Paint it green</a>
<a href="#" data-background-color="#000099" data-text-color="#FFFFFF">Paint it blue</a>
```

```coffeescript
@paintIt = (element, backgroundColor, textColor) ->
  element.style.backgroundColor = backgroundColor
  if textColor?
    element.style.color = textColor
 
$ ->
  $("a[data-background-color]").click (e) ->
    e.preventDefault()
 
    backgroundColor = $(this).data("background-color")
    textColor = $(this).data("text-color")
    paintIt(this, backgroundColor, textColor)
```

这种方法称为**”非侵入式JavaScript“**，因为JavaScript代码不再和HTML混合在一起。这样做正确分离了关注点，易于修改功能。我们可以轻易把这种效果添加到其他链接上，只要添加相应的data属性即可。