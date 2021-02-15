# CSS动画


## X Y Z 轴旋转动画
```
// index.css

.rotate {
  animation: rotate 3s linear infinite; // 动画名称 时长 动画速度 动画播放次数
}
@keyframes rotate {
  from {
    // transform: rotateX(0deg); // 定义沿着 X 轴的 3D 旋转。
    // transform: rotateY(0deg); // 定义沿着 Y 轴的 3D 旋转。
    transform: rotateZ(0deg); // 定义沿着 Z 轴的 3D 旋转。
  }
  to {
    // transform: rotateX(360deg);
    // transform: rotateY(360deg);
    transform: rotateZ(360deg);
  }
}

.stop {
  animation-play-state: paused; // 暂停播放动画
}

.play {
  animation-play-state: running; // 继续播放动画
}

```


