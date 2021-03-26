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