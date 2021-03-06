



[参考文档](https://www.dazhuanlan.com/2019/12/25/5e024967ccdf8/)
[参考文档](https://www.jianshu.com/p/722a51ee4d55?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

###  h5和native互调

1. 异步通信
通过 JSBridge，JS 可使用原生层的资源、网络访问能力、接口处理能力等，这些大部分情况下都是异步的（尽管部分情况 JSBridge 原生层与 JS 的通信技术可同步取得结果），同时也为做到 Android 与 iOS 两端统一，接口最小化，双方的所有的通信都设计为异步的。
2. 双方对等
JSBridge的通信是全双工通信，原生层与JS层都即可作为请求调用方，也可作为响应回复方。JS 可调用原生层的接口函数，同时原生层也可调用 JS 的接口函数，只要一方注册过处理函数，对方调用时即可找到对应的处理函数。
3. 处理函数注册与调用
### 兼容性

1. 平台区分
navigator.userAgent 做平台区分
2.
### 版本升级
### 消息规范（方法名、状态码、回调、调用方式等）
### 原生功能API(版本号、手机设备信息)和业务API(token、相机调用等)



## 可讨论的问题
- 加载H5页面的方式

1. 把所有的H5、JS、CSS等文件打成zip包下载到本地
优点：离线有缓存，加载速度快，用户体验好。
缺点：保存和更新的逻辑比较复杂，如果没处理好可能会出问题。
2. 加载URL
优点：逻辑简单。
缺点：加载速度依赖于网络速度，没有访问过的网页就没有缓存。


- 前端的职责（网络请求）

1. 完全由前端自己处理。
优点：降低对Native的依赖，有利于设备业务的更新迭代。
缺点：前端工作量较大，同时接口的具体细节要暴露出来。
2. 完全由Native处理（Native将每一个接口，都封装成相应的方法，供前端调用）
优点：简单直接，避免向前端暴露接口的具体细节。
缺点：过于依赖Native端，业务变化可能还是会导致App必须升级，违背了混合开发的初衷。
3. Native端抽象出主要的方法，前端将请求和参数透传给Native（京东微联类似这种方式）
优点：只需要提供几个统一的方法，便能包含各种操作，比如控制设备（controlDevice），获取设备状态（getDevice）、定时（timming）等。
缺点：设计困难，可能还会牵扯到后台。



[如何设计](https://www.jianshu.com/p/722a51ee4d55?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)
