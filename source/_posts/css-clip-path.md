---
title: css_clip-path
date: 2022-12-12 21:31:07
tags: css
---

### clip-path 
 “clip-path的作用是使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏。”

```
 直接写在选择器中：
div{
    width:50px;
    height:50px;
    clip-path:xxxxx;
}
```
### 基本语法
clip-source | basic-shape | geometry-box | none \
 #### 分别表示:
 ```
clip-source = url //可以通过url指定一个元素 \
basic-shape = inset | circle | ellipse | polygon  // 可以指定图形分别对应 矩形 | 圆 | 椭圆 | 多边形\
geometry-box = shape-box | fill-box | stroke-box | view-box\
```

### basic-shape举例
  * #### inset()
  ```
  // inset可以接收5个参数，分别为top,right,bottom,left, round radius(可选值，表示圆角)
  clip-path: inset(10em 20em 10em 20em round 5em) // round字符为固定字符
  
  ```
 * #### cirle()
  ```
  // cirle可以接收2个参数，分别为圆的半径，圆的中心点(默认为元素中心点)
  clip-path: cirle(50% at 16px 16px) //at 字符为固定字符
  ```
* #### ellipse()
  ```
  // ellipse可以接收3个参数，分别为椭圆的X轴半径，椭圆的Y轴半径，圆的中心点(默认为元素中心点)
  clip-path: ellipse(50% 60% at 50% 50%) //at 字符为固定字符
  ```

* #### polygon()
  ```
  // polygon可以接收多组数据，每组数据为拐点坐标(x y),每组数据通过逗号相隔
  clip-path: polygon(50% 0,100% 50%,0 100%) 
  ```
    1. polygon菱形：
    ```
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    ```
    2. polygon梯形：
    ```
    clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);
    ```
    3. polygon平行四边形形：
    ```
    clip-path: polygon(25% 0%, 100% 0%, 75% 100%, 0% 100%);
    ```
    4. polygon五边形：
    ```
    clip-path: polygon(50% 0%, 100% 38%, 82% 100%, 18% 100%, 0% 38%);
    ```
    5. polygon右箭头：
    ```
    clip-path: polygon(0% 20%, 60% 20%, 60% 0%, 100% 50%, 60% 100%, 60% 80%, 0% 80%);
    ```
    6. polygon左箭头
    ```
    clip-path: polygon(40% 0%, 40% 20%, 100% 20%, 100% 80%, 40% 80%, 40% 100%, 0% 50%);
    ```