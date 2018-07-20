---
title: CSS3 clip rect应用
comments: true
date: 2018-07-17 11:31:34
tags: [CSS3,clip,rect]
categories: CSS
---

## 定义和用法
clip 属性剪裁绝对定位元素。属性允许您规定一个元素的可见尺寸，这样此元素就会被修剪并显示为这个形状。定义一个剪裁矩形。对于一个绝对定义元素，在这个矩形内的内容才可见。出了这个剪裁区域的内容会根据overflow的值来处理。剪裁区域可能比元素的内容区大，也可能比内容区小。

---

## 可能的值
值 | 描叙
---|---
shape | 设置元素的形状。唯一合法的形状值是：rect (top, right, bottom, left)
auto | 默认值。不应用任何剪裁。
inherit | 规定应该从父元素继承 clip 属性的值。

---
<!--more-->
## 示例

HTML：
```
<div class="rect"></div>
```
CSS：

```
 .rect {
        width: 200px;
        height: 200px;
        background-color: pink;
        position: relative;
        margin: 100px;
    }

    .rect::before,
    .rect::after {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        content: '';
        z-index: -1;
        margin: -5%;
        box-shadow: inset 0 0 0 2px;
        animation: clipMe 8s linear infinite;
    }

    .rect::after {
        animation-delay: -4s;
    }

    @keyframes clipMe {
        0%,
        100% {
            clip: rect(0, 220px, 2px, 0)
        }
        25% {
            clip: rect(0, 2px, 220px, 0)
        }
        50% {
            clip: rect(218px, 220px, 220px, 0)
        }
        75% {
            clip: rect(0, 220px, 220px, 218px)
        }
    }
```

## 效果

<div style="text-align:center">![rect](/image/rect.jpg)</div>