# CSS

## 超出文本显示省略号

1、单行超出
```
// index.html
<div class='only'>单行超出文本显示省略号</div>


// index.css
.only {
  width: 100px; // 必须设置宽度！
  border: 1px solid #000;
  height: 100px;

  <!-- 要点 -->
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
  width: 100px;
  height: 45px; // 设置 高度 必须要于 2 行高度相等
  border: 1px solid #999;

  <!-- 要点 -->
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2; // 超出 2 行显示省略号
  overflow: hidden;
}
```