---
title: 纯css弹窗处理
date: 2024-07-11 14:38:46
tags: [css]
---

### 一、使用情况

最开始是要做一个高德地图展示，需要在多个 market 上展示弹窗，但是高德地图的 infoWindow 同时只能存在一个，所以需要自己写 market 的逻辑

```
可以直接使用react-amap这个包给market传入组件就不需要考虑这个问题
```

### 二、实现思路

1、利用 type 为 checkbox 的 input 标签的选中和非选中实现两种状态  
2、 css 中的选择器实现 style 的改变，通过切换 display 的状态来展示弹窗

```html
<!-- 省去了一些样式 -->
<div class="marker">
  <label for="inputId">
    <!--此svg是一个带波纹动画的圆圈-->
    <svg
      width="20"
      height="20"
      viewBox="0 0 40 40"
      xmlns="http://www.w3.org/2000/svg"
    >
      <circle cx="20" cy="20" r="10" fill="red" />
      <circle id="circle" cx="20" cy="20" r="0" fill="red">
        <animate
          attributeName="r"
          from="0"
          to="20"
          dur="1s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="opacity"
          from="1"
          to="0.1"
          dur="1s"
          repeatCount="indefinite"
        />
      </circle>
    </svg>
  </label>
  <input type="checkbox" class="check_input" id="inputId" />
  <!--此div必须要和input相邻-->
  <div class="infoWindow">
    <!--在这里写悬浮窗中的内容-->
  </div>
</div>
```

```css
.marker {
  position: relative;
  /*设置宽高颜色等 */
}
.infoWindow {
  /*设置窗口的样式*/
  position: absolute;
  display: none;
}

/**重点 通过选择器实现切换显示隐藏*/
.check_input[type="checkbox"]:checked + .infoWindow {
  display: block;
}
```

### 三、其他

原生 html 写起来挺麻烦的，但是是一种解题思路，想要简单的实现方式还是需要用第三方包，

[codePen 在线看效果](https://codepen.io/wangyuan4617/pen/rNgOWza)
