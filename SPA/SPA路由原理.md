
### 引子
***

    目前前端领域多以SPA(Single Page Web Application)为主，我们会发现
    react-router、vue-router都会提供两种路由模式选择，即hash以及history模式。

### 传统页面及SPA
***
    传统页面，不同的视图写在不同的html文件里，一个web引用会有多个html文件，不同路径加载相应的html、js等文件。

    SPA，只存在一个html文件，一个顶级div容器，通过路由的改变切换视图,加载不同的js。

    两者最大的区别在于，访问的Url路径是否会带上.html后缀



### 路由原理
***
    在不刷新页面的情况下，改变页面内容，有三种方式可以实现。
    一是ajax，二是hash模式，三是H5的history


###  Hash模式
***

由于 hash 值的变化不会导致浏览器像服务器发送请求，而且 hash 的改变会触发 hashchange 事件，浏览器的前进后退也能对其进行控制，所以在 H5 的 history 模式出现之前，基本都是使用 hash 模式来实现前端路由。    

#### 关键Api

  ```js
  window.location.hash = 'hash字符串'; // 用于设置 hash 值

  let hash = window.location.hash; // 获取当前 hash 值

  // 监听hash变化，点击浏览器的前进后退会触发
  window.addEventListener('hashchange', function(event){ 
      let newURL = event.newURL; // hash 改变后的新 url
      let oldURL = event.oldURL; // hash 改变前的旧 url
  },false)
  ```

### History模式
***
    history api可以分为两大部分，切换和修改，切换历史状态包括back、forward、go三个方法，对应浏览器的前进，后退，跳转操作，有同学说了，(谷歌)浏览器只有前进和后退，没有跳转，在前进后退上长按鼠标，会出来所有当前窗口的历史记录，从而可以跳转(也许叫跳更合适)：


    切换包括history.pushState() 和 history.replaceState() 均接收三个参数（state, title, url）

    参数说明如下：

    state：合法的 Javascript 对象，可以用在 popstate 事件中
    title：现在大多浏览器忽略这个参数，可以直接用 null 代替
    url：任意有效的 URL，用于更新浏览器的地址栏


    history.pushState() 和 history.replaceState() 的区别在于：
    history.pushState() 在保留现有历史记录的同时，将 url 加入到历史记录中。
    history.replaceState() 会将历史记录中的当前页面历史替换为 url。

#### 关键Api
```js 
const { history } = window;

history.go(-2);//后退两次
history.go(2);//前进两次
history.back(); //后退
hsitory.forward(); //前进

window.addEventListener("popstate", (e) => {
  consol.log({
    location: location.href,
    state: e.state,
  })
})

```







      