# 网页性能调优技巧

在(至少大部分)编程中, 过早优化是万恶之源 -- 高德纳`克努斯

在不存在性能瓶颈的前提下进行性能的提升, 是技术能量的浪费. 或者在软件构建的初始阶段, 就已经开始考虑怎么对其进行优化.  
从而导致代码过于繁琐, 过于未卜先知, 这就尴尬了.

## 移动端性能优化

主要涉及7类, 分别也有网络加载类, 缓存类, 页面渲染类, CSS优化类, JS执行类, 图片类以及架构协议类.  

### 网络加载类

1. 首屏数据请求提前,避免JavaScript文件加载后才请求数据, 模块化资源并行下载, 资源预加载.
2. 首屏加载和按需加载,非首屏内容滚屏加载,保证首屏内容最小化, inline首屏必备的CSS和JavaScript.
3. head标签中假如`rel="dns-prefetch"`以及`href="http://example.com"`属性的`<link>`标签设置DNS预解析. 类似的还有preconnect预链接与prefetch预获取以及prerender预渲染
4. 合理利用MTU策略(Maximum Transmission Unit), 通信协议某一层上可通过的最大数据包大小.

### 缓存类

1. 合理利用浏览器缓存
2. 尝试使用AMP HTML(Accelerate Mobile Page Project), PWA(Progressive Web App)模式(渐进式Web应用模式)

### 架构协议类

1. 尝试使用SPDY协议与HTTP2协议
2. 使用后端数据渲染
3. 使用NativeView(React)代替DOM的性能劣势

### 

1. 使用ViewPort固定屏幕渲染(设置),可以加速页面渲染内容(也即限制移动端的缩放, 避免页面重排, 设置user-scalable=no)
2. 避免各种形式重排重绘(reflow与repaint), 包括合理使用Canvas和requestAnimationFrame, 不滥用float, 不滥用web字体或过多font-size声明
3. 使用CSS3动画,开启GPU加速, 做好脚本容错(??????)

```css
<!-- CSS3动画或者过渡尽量使用transform和opacity来实现动画，不要使用left和top。 -->
<!-- 开启这个GPU加速会导致耗电量增加 -->
.translate3d {
    -webkit-transform: translate3d(0,0,0),
    -moz-transform: translate3d(0,0,0),
    -ms-tranform: translate3d(0,0,0),
    transform: translate3d(0,0,0),
}
```

### 图片类

1. 使用较小的图片, 定义图片大小限制, 或使用更高压缩比格式的图片, 并对图片进行压缩处理
2. 合理使用base64内嵌图片, 以及使用SVG代替图片, 或使使用iconfont代替图片
3. 设置图片懒加载
4. 设置强缓存

## 电脑端性能优化

主要分为网页加载类, 以及页面渲染类. 网页加载主要致力于`减少HTTP的请求数`以及`减小HTTP响应的大小`, 而页面渲染类则是致力于减少页面的`reflow(重排)`行为以及DOM相关的性能优化

### 网页加载类 => 减少HTTP请求数与减小HTTP响应的大小

主要分为缓存管理以及静态的CDN资源存储, 以及一些小技巧.

#### 缓存管理部分

1. 为HTML指定`Cache-Control`或`Expires`
2. 合理设置`Etag`和`Last-Modified`
3. 使用GET来完成Ajax请求, 使用可缓存的AJAX(加上一个expire的设置)
4. 减少页面重定向(301永久重定向, SEO方面会被搜索引擎惩罚, 但同时现代浏览器会对其做一定的优化, 访问一次后再访问会直接跳转到对应网址, 不同于302临时重定向)
5. 缩小Favicon.ico并缓存

#### 静态CDN资源存储部分

1. 使用静态资源分域, 利用CDN来存储文件
2. CDN Combo下载传输内容(前端资源的动态渲染, 除了Combo模式, 还有Seed模式)
3. 将图片，js，css的请求划分到单独的域名下，请求中将不包括业务中的cookie(因为cookie有域的限制, 则使用非主要域名时发送请求中不会带有cookie数据)，这样可以降低传输延时，提升整体响应速度。 从而做到cookie隔离。

#### CSS, JS小技巧

1. 将CSS或JavaScript放到外部文件中,避免使用`<style>`或`<script>`标签直接引入. (css在head中使用link标签引入, script使用src属性或动态载入)
2. (使用defer与async), 消除阻塞渲染的CSS与JavaScript
3. 避免页面中空的href和src, 避免使用CSS import引用加载CSS
4. 避免使用CSS import引用加载CSS
5. 消除阻塞渲染的CSS与JavaScript, 避免运行耗时的JavaScript

### 页面渲染类 => 减少页面重排(reflow), 以及DOM性能优化

1. CSS资源引用放到HTML文件顶部, JavaScript资源引用放到HTML底部
2. 尽量预先设定图片等大小, 不要在HTML中直接缩放图片
3. 减少DOM元素数量与深度，尽量避免使用`<table>`, `<iframe>`.
4. CSS动画使用translate\scale代替top\height, 开启GPU加速, 以及尽量减少使用JS动画(只要不reflow, 在safari上css3动画与js动画都挺流畅的, repaint
5. 避免使用CSS表达式或CSS滤镜, 