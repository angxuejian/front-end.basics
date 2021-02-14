# front-end.basics
关于前端的小知识😁

## 一、跨域
### 1. 什么是跨域？
浏览器的同源策略、是对JavaScript的安全限制；

同源策略是指：协议、端口、域名必须相同、否则会跨越！

如果缺少同源策略，容易受到 [XSS](https://baike.baidu.com/item/XSS%E6%94%BB%E5%87%BB/954065)、[CSFR](https://baike.baidu.com/item/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0/13777878?fromtitle=CSRF&fromid=2735433)等攻击

### 2. 如何解决跨越
- jsonp
- 后端开放 Access-Control-Allow-Origin 请求头
- nginx代理跨域
- [nodejs代理跨越](https://github.com/angxuejian/nodejs-http-proxy-template)


## 二、浏览器
### 1. 浏览器输入url发生了什么？
1. 输入url 
2. 浏览器主进程接管、分配线程
3. 请求http(省略 dns、ip)
4. 等待响应、获取内容
5. 浏览器渲染

    1. 根据html生成dom树 + 根据css生成style树
    2. 将 dom树 和 style树 合并成 render树
    3. 根据render树进行 回流, 计算元素位置、尺寸 
    4. 根据render树进行 重绘, 获取页面像素信息
    5. 由浏览器将页面像素信息交给GPU、GPU将各层合成(composite)、展示页面。

### 2. 回流、重绘？
#### 回流(重排)
负责计算元素位置、尺寸。

例：
div的宽度设置为50%, 将50%计算为页面像素尺寸时。这一阶段就叫 回流。

#### 重绘
负责获取页面像素信息、如 颜色、圆角、阴影等

#### 最后
每次回流都会对浏览器进行额外的计算消耗。

降低回流的方式：要么减少次数，要么降低影响范围、要么使用硬件加速

  1. 减少次数：减少dom操作、频繁修改文本内容、尺寸、位置、窗口大小等
  2. 降低影响范围：操作使用 absolute、fixed布局的元素。脱离文档流、避免牵一发而动全身
  3. 硬件加速：使用绝对布局只是降低影响，但还是使用同一图层。 而使用 translate 则是新建复合图层，与之前图片并无关联。注意哦！过度使用硬件加速也会消耗内存。





### 写的不好。推荐[这个](https://www.cnblogs.com/dailc/p/8325991.html)