### iframe

主要的属性：height、width、src、scrolling、name

### 父子交互

```js
// 获取元素
let sonIframe = document.getElementById("soniframe");
// 获取子iframe的window
let sonWindow = sonIframe.contentWindow;
// 向子页面传递发送消息
sonWindow.postMessage(JSON.stringify(data), iframeUrl);
// 监听子iframe传来的数据
window.addEventListener("message", receiveMsg, false);
function receiveMsg(event) {
  console.log(e.data); //e.data为传递过来的数据
  console.log(e.origin); //e.origin为调用 postMessage 时消息发送方窗口的 origin（域名、协议和端口）
  console.log(e.source); //e.source为对发送消息的窗口对象的引用，可以使用此来在具有不同origin的两个窗口之间建立双向通信
  // 这里不准确，chrome没有这个属性
  // var origin = event.origin || event.originalEvent.origin;
  var origin = event.origin;
  if (origin !== "http://example.org:8080") return;

  // ...
  console.log("event", event.data);
}

// iframe子页面调用父页面的变量、js方法、元素(非跨域)：
window.parent.varName; //获取父页面js全局变量
window.parent.fnName; //获取父页面js全局方法
window.parent.document.getElementById(id); //获取父页面元素

// 父页面调用iframe页面中的变量、js方法、元素(非跨域)：
window.frames[frameName]; //
window.frames[frameName][0] = document.getElementById(frameId).contentWindow;
window.frames[frameName][0].varName; //获取子页面js全局变量
window.frames[frameName][0].fnName; //获取子页面js全局方法
window.frames[frameName][0].document.getElementById(id); //获取子页面元素
```

### 安全性

#### 前端防止页面嵌套

```js
// 判断当前页面的访问地址和浏览器窗口顶部的是否一致
if (window.top != window.self) {
  top.location.href = "你的url";
}

// 限定在同域下引用
if (window.top.location.host != window.location.host) {
  window.top.location.href = window.location.href;
}
```

#### 后端 X-Frame-Options 设置

- X-Frame-Options:DENY。当前页面不能被嵌套 iframe 里，即便是在相同域名的页面中嵌套也不允许,也不允许网页中有嵌套 iframe
- X-Frame-Options:SOMEORIGIN。页面的地址只能为同源域名下的页面。如：X-Frame-Options:foo.com。 则 foo.com 下的页面可以嵌入此页面。
- X-Frame-Options:ALLOW-FROM。只允许指定网页的 iframe 请求，兼容性差。

#### 后端 CSP 之页面防护

### 跨域

- document.domain

```js
//b.html是以iframe的形式嵌套在a.html中
//www.foo.com上的a.html
document.domain = "foo.com";
var ifr = document.createElement("iframe");
ifr.src = "http://script.foo.com/b.html";
ifr.style.display = "none";
document.body.appendChild(ifr);
ifr.onload = function () {
  var doc = ifr.contentDocument || ifr.contentWindow.document;
  // 在这里操纵b.html
  alert(doc.getElementsByTagName("h1")[0].childNodes[0].nodeValue);
};
//script.foo.com上的b.html
document.domain = "foo.com";
```

- postMessage

[postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

[详解](https://www.cnblogs.com/hq233/p/9849939.html)


#### iframe的优点：
1.iframe能够原封不动的把嵌入的网页展现出来。

2.如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。

3.网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用iframe来嵌套，可以增加代码的可重用。

4.如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由iframe来解决。

---   
#### iframe的缺点：
1.会产生很多页面，不容易管理。

2.iframe框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。

3.代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理iframe中的内容，所以使用iframe会不利于搜索引擎优化。

4.很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。

5.iframe框架页面会增加服务器的http请求，对于大型网站是不可取的。