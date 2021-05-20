# 其他小知识

## 跨域

1、什么是跨域？

浏览器的同源策略、是对JavaScript的安全限制；同源策略是指：协议、端口、域名必须相同、否则会跨越！如果缺少同源策略，容易受到 [XSS](https://baike.baidu.com/item/XSS%E6%94%BB%E5%87%BB/954065)、[CSFR](https://baike.baidu.com/item/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0/13777878?fromtitle=CSRF&fromid=2735433)等攻击


2、如何解决跨越？

- jsonp
- 后端开放 Access-Control-Allow-Origin 请求头
- nginx代理跨域
- [nodejs代理跨越](https://github.com/angxuejian/nodejs-http-proxy-template)


## 浏览器
1、浏览器输入url发生了什么？

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

2、回流、重绘？

1. 回流：**负责计算元素位置、尺寸。** 例：div的宽度设置为50%, 将50%计算为页面像素尺寸时。这一阶段就叫 回流。

2. 重绘：**负责获取页面像素信息、如 颜色、圆角、阴影等**

3. 最后：每次回流都会对浏览器进行额外的计算消耗。降低回流的方式：**要么减少次数，要么降低影响范围、要么使用硬件加速**

    1. 减少次数：减少dom操作、频繁修改文本内容、尺寸、位置、窗口大小等
    2. 降低影响范围：操作使用 absolute、fixed布局的元素。脱离文档流、避免牵一发而动全身
    3. 硬件加速：使用绝对布局只是降低影响，但还是使用同一图层。 而使用 translate 则是新建复合图层，与之前图片并无关联。注意哦！过度使用硬件加速也会消耗内存。


4. 写的不好。推荐[这个](https://www.cnblogs.com/dailc/p/8325991.html)

## JS对比？
```
// 示例一
const data = { age: 21 }
console.log(data === { age: 21 }) // false


// 示例二
const num = 9
console.log(num === 9) // true
```

1、为什么 示例一打印为false，而示例二打印为 true ？

1. 因为js 会将简单数据类型分配到 栈中， 而复杂类型会分配到 堆中，只会在 栈中留下指针
2. 示例一中创建了两次 `{ age: 21 }`， 但只会在栈中留下两次指针，而两次的数据的都会存在堆中，之后 `===` 只会对比指针指向是否相等。
3. 示例二中虽然创建了两次`9`，但是都会将数据存在 栈中，之后`===` 会在 栈中直接对比数据

## 百度地图 Marker点

例：需要在地图上展示以下图标。可以看到右上角图标是根据不同业务来切换的。但目前百度的Marker点 只能绘制一张 ICON 的图标，如何解决此难题？

![1](/img/1.png)

答：

1. 触发不同业务条件时，通过 canvas 绘制成一张图。

    缺点：效率慢、不可能在渲染marker点时 一张一张的画。只能在触发阶段时将图画好

    优点：渲染marker点时，不用任何操作，直接渲染即可

2. 将图标分为多层、在同一坐标点下覆盖多张图片。达到错位效果，使结果看起来为一个图标

    缺点：需要将图标分为多层、修改原数组长度、需要控制每层图片的顺序、点击marker时只会点到最上面的图片、图标宽度会比平常要大，需要通过偏移量来控制图标的点击区域

    优点：效率要高一点、直接渲染


## <span id='h5-key'>h5键盘问题？</span>

1、键盘顶起后 背景图片高度发生变化问题？

在 onload 触发后，获取当前window的高度、动态将高度赋值给根组件body

2、禁止 键盘顶起页面

将页面使用 `fixed布局`，这样在android机型下，键盘就不会顶起页面；但是ios机型下fixed布局会有问题，所以ios机型下使用 relative布局即可

使用 fixed布局 一定要是 top:0; 如bottom:0;页面还是会顶起的

> android 会顶起页面，使用 fixed布局才不会<br>
> ios 不会顶起页面，使用 fixed布局才会

3、键盘顶起页面但不改变页面大小

和 示例1 思路一样，在onload触发后修改body的高度。


4、底部有button按钮、键盘顶起button也被顶起

 - ios机型可以 监听 input的 焦点 和 失去焦点事件，android机型键盘下拉时不会触发失去焦点事件
 
 - android机型监听 窗口大小事件， ios机型无法监听窗口大小事件
  
> 触发事件时可以将button按钮 隐藏或显示

<br>

[查看h5键盘示例](https://angxuejian.github.io/works/keyboard-height/)

## h5 canvas画图

1、清晰度问题

**获取设备像素点 / canvas像素的 = 缩放比例**

**canvas的宽高 * 缩放比例 = 放大后的canvas画布**，清晰度就会上去

提示：先将canvas画成功后，在将宽高 * 缩放比例，要不然绘制位置时，都是放大过的，尺寸位置都是错的。

2、图片跨域问题

将 绘制代码放在 服务端就不存在跨域问题了。

[如何在前端开启本地服务端](https://github.com/angxuejian/http-cros-proxy-template)


3、键盘顶起、canvas画图比例错误

键盘问题可以先看看[h5键盘问题？](#h5-key)

主要问题是 键盘顶起页面，绘制时获取的当前被键盘顶起来的高度，并不是屏幕本身高度

可以在 onload后将页面高度 保存下来、绘制时使用保存的高度，而不是实时获取的高度

<br>

[查看h5 canvas画图示例](https://angxuejian.github.io/works/canvas-poster/)


## h5调用移动端 拍摄 + 相册

点击会调用手机系统弹窗、选择拍摄或相册
```
<input type="file" accept="image/*">
```

change事件：ios + android 都支持

input事件：ios部分机型不支持

## H5适应横屏 和 竖屏	

1、竖屏

正常开发即可、没什么可讲的；[示例](https://angxuejian.github.io/works/canvas-poster/public/)

2、横屏

横屏又可以分为`自动旋转横屏` 和 `竖屏状态下横屏`。前者是用户自动将屏幕横过来，后者是通过旋转根节点横屏。

> 缺点：
>> 自动旋转横屏：需要用户主动横屏<br><br>
>> 竖屏状态下横屏：此时用户开启横屏，页面将展示错乱，且输入法还是竖屏状态


H5能力有限不能调用手机横屏，只能监听窗口大小改变。想要出现横屏输入法，必须是手机横屏状态

权衡利弊之后还是选择使用后者`竖屏状态下横屏`

解决思路大致可以这样：

1. 一开始使用竖屏、通过旋转根节点实现横屏
2. 监听 `resize` 屏幕改变成横屏时、在将根节点恢复到为旋转之前

```
// 获取dom 节点的宽高

document.documentElement.clientWidth
document.documentElement.clientHeight
```

最后看这里 - [示例](https://angxuejian.github.io/works/horizontal-screen-animation/)



## 关于视频自动播放问题
PC端
```
<video
  // 设置后，音频会初始化为静音，注意浏览器只有设置静音，才能自动播放
  muted

  // 视频会马上自动开始播放，不会停下来等着数据载入结束。
  autoplay="autoplay"

  // 布尔属性；指定后，会在视频结尾的地方，自动返回视频开始的地方
  loop="true"

  // 一个布尔属性，标志视频将被“inline”播放，即在元素的播放区域内。
  x5-playsinline="true"
  playsinline="true"
  webkit-playsinline="true"

  // 一个布尔属性，用于禁用使用有线连接的设备(HDMI、DVI等)的远程播放功能。
  x-webkit-airplay="allow"

  // 这个视频优先加载
  preload="auto"

  // 启用同层H5播放器，就是在视频全屏的时候，div可以呈现在视频层上，也是WeChat安卓版特有的属性。同层播放别名也叫做沉浸式播放
  x5-video-player-type="h5"

  // :全屏设置。它又两个属性值，ture和false，true支持全屏播放
  x5-video-player-fullscreen="true"

  // 进度条控制
  controls
>
<source src="indexMove.mp4" type="video/mp4">
</video>
```

移动端
```
<video
  playsinline='true'
  preload='auto'
  webkit-playsinline='true'
  x-webkit-airplay="allow" 
  x5-video-player-type="h5"  
  x5-video-player-fullscreen="true" 
  x5-video-orientation="portraint"
  preload='auto'
  style='object-fit: cover;'
  controls
>
  <source  type="video/mp4" src="https://www.zgckarsa.top/b.mp4"></source>
</video>
```

安卓机型应该没什么问题、通过js可以播放视频，但是ios机型必须要点击后才可以播放(只要有交互，就可以用js播放)。

在微信H5中可以监听`WeixinJSBridgeReady`事件，在事件回调后就可以通过js播放视频；如果需要在特定页面才播放视频，**可以在监听事件中先播放，然后暂停；** 因为已经播放过一次了，之后就可以通过js播放视频了。只限微信浏览器

<br>
<br>

最后 - 视频播放进度(每次都从头播放)

```
video.addEventListener('pause', function() {
  video.currentTime = 0
})
```

> 也可以考虑一下这个[序列图片实现视频播放效果](https://www.zhangxinxu.com/study/201805/image-sequence-frame-play.html)


## background - 点击态

```
// index.html

<div id='back'></div>

// index.css

<!-- 背景图 必须是雪碧图 -->
#back {
   background: url('../img/new/back.png') no-repeat top/100% auto;
}

// index.js

$('#back').on('touchstart', start)
$('#back').on('touchend', end)

function start() {
  $(this).css('background-position-y', 'bottom')
}
function end() {
  $(this).css('background-position-y', 'top')
}

```

## 小程序打开文档
```
wx.downloadFile({
  url: url,
  filePath: wx.env.USER_DATA_PATH + `/${name}.${type}`,
  success: res => {
    wx.hideLoading()
    wx.openDocument({
      showMenu:true,
      filePath:res.filePath,
      }
    })
  }
})
```
