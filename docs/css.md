# CSS

## 超出文本显示省略号

1、单行超出
```
// index.html
<div class='only'>单行超出文本显示省略号</div>


// index.css
.only {
  width: 150px;
  border: 1px solid #333;  /* 必须设置宽度！ */
  height: 50px;
  box-sizing: border-box;

  /* 要点 */
  word-break:break-all
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
```

2、多行超出
```
// index.html
<div class='more'>多行超出文本显示省略号、多行超出文本显示省略号</div>


// index.css
.more {
  width: 150px;
  height: 45px; /* 设置 高度 必须要于 2 行高度相等 */
  border: 1px solid #333;
  box-sizing: border-box;

  /* 要点 */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2; /* 超出 2 行显示省略号 */
  overflow: hidden;
}
```

## 无所不能

1、画对号 ✔️

```
// index.html
<div class='check'></div>


// index.css
.check {
  width: 10px;
  height: 22px;
  border-width: 0 4px 4px 0;
  border-style: solid;
  border-color: #16C60C;
  transform: rotate(45deg);
}

// tip: 将div设置为长方形、只显示 右边框 + 下边框、旋转45度即可。
```

## 指定弧度的圆弧
```
// index.html
<div>
  <div class="arc"></div>
  <div class="circle"></div>
</div>


// index.css
.circle {
  width: 200px;
  height: 200px;
  border: 3px solid #666;
  background-color: blanchedalmond;
  border-radius: 50%;
  box-sizing: border-box;
}
.arc {
  width: 200px;
  height: 200px;
  position: absolute;
  clip: rect(0 200px 200px 100px);
  animation: myarc 3s linear infinite;
}
.arc::after {
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  clip: rect(0 100px 200px 0);
  background-color: #666;
  transform: rotate(30deg);
  border-radius: 50%;
  box-sizing: border-box;
  border: 3px solid red;
}
@keyframes myarc {
  from { transform: rotateZ(0);}
  to   { transform: rotateZ(360deg);}
}

/* tips:
* 将圆看成正方形。分别裁剪一左一右的长方形、旋转左边长方形 指定角度即可
* 将 400*400的正方形 裁剪为 200*200的长方形。实际宽高还是400*400，只是显示200*200的宽高。
* 主要是利用这点来实现的！可能说的比较模糊...  快去动手试一试、一试便知！
**/
```
