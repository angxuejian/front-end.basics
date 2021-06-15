# CSS动画


## X Y Z 轴旋转动画

rotateX：定义沿着 X 轴的 3D 旋转。

rotateY：定义沿着 Y 轴的 3D 旋转。

rotateZ：定义沿着 Z 轴的 3D 旋转。

```
.rotate {
  animation: rotate 3s linear infinite; /* 动画名称 时长 动画速度 动画播放次数 */
}
@keyframes rotate {
  from {
    /* transform: rotateX(0deg); */
    /* transform: rotateY(0deg); */

    transform: rotateZ(0deg);
  }
  to {
    /* transform: rotateX(360deg); */
    /* transform: rotateY(360deg); */

    transform: rotateZ(360deg);
  }
}

/* 暂停播放动画 */
.stop {
  animation-play-state: paused; 
}

/* 继续播放动画 */
.play {
  animation-play-state: running;
}

```

## <span id='fead'>淡入/淡出/闪烁</span>

1、淡入

使用淡入时、需要先设置dom元素为隐藏状态

```
.fead-in {
  animation: feadin 0.5s forwards; /* 动画名称 时长 保持最后一帧的状态*/
}

@keyframes feadin {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
```

2、淡出

```
.fead-out {
  animation: feadout 0.5s forwards;
}

@keyframes feadout {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
```

3、闪烁

```
.flash {
  animation: flash 1s linear infinite;
}

@keyframes flash {
  0% {
    opacity: 0.3;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0.3;
  }
}
```

## <span id='slide'>上/下/左/右 滑动</span>

translateX：为 左右方向、从左到右 0% - 100%

translateY：为 上下方向、从上到下 0% - 100%
```
.slide {
  animation: slide 1s forwards;
}

@keyframes slide {
  from {
    /* transform: translateX(-100%); */

    transform: translateY(-100%);
    opacity: 0;
  }
  to {
    /* transform: translateX(0%); */

    transform: translateY(0%);
    opacity: 1;
  }
}

```


## 进入/退出 缩放

1、进入
```
.zoom-in {
  animation: zoomin 1s forwards;
}

@keyframes zoomin {
  from {
    transform: scale(0.1);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
```

2、退出
```
.zoom-out {
  animation: zoomout 1s forwards;
}

@keyframes zoomout {
  from {
    transform: scale(1);
    opacity: 1;
  }
  to {
    transform: scale(0.1);
    opacity: 0;
  }
}
```

## 字幕效果

[滑动](#slide) + [淡入](#fead)

## 弹幕效果
```
.barrage {
  animation: bar-scale 0.5s ease forwards;
  opacity: 0;
  transform-origin: bottom left;
  transform: scale(0.1);
}

@keyframes bar-scale {
  50% {
    opacity: 0.5;
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}
```