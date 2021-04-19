# Rails问题整理

#### redirect_to 重定向问题（Turbolinks）

[turbolinks github地址](https://github.com/turbolinks/turbolinks)

>  在rails的某个路由（命名为中转路由）中定义了`redirect_to _url`，在view中`link_to`绑定了该中转路由地址，点击`link_to`的时候跳转到路由然后重定向到`_url`。中转路由中包含鉴权和清楚token的操作。

**问题**：项目中使用了`turbolinks`，在点击跳转的时候，中转路由进行302重定向，结果重定向的结果是错的。是由于`turbolinks`导致中转路由被执行了两次，在view中绑定中转路由的地方禁用`turbolinks`即可。

```ruby
<a href="/" data-turbolinks="false">Disabled</a>

<div data-turbolinks="false">
  <a href="/">Disabled</a>
</div>
```

