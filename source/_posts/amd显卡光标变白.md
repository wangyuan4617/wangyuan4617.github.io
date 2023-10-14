---
title: amd显卡光标变白
date: 2023-04-09 11:21:43
tags: [AMD, openGl, win]
---

如果使用了 amd 的 GPU 包括 apu，独立显卡，会出现鼠标移动到浏览器地址栏光标变白色的问题，会有一个时刻找不到光标位置，但是在截图和录屏中是正常的 \
解决方案：

#### 一、更换 windows 光标样式

    通过win+i打开设置面板，在左侧找到设备-鼠标，进入到鼠标设置后在右侧蓝色文字 “其他鼠标选项”，弹出鼠标属性窗口，切换到指针tab页，更换文本选择的指针样式。

#### 二、在关闭硬件加速

    在浏览器、office中设置关闭硬件加速会解决此问题，但是失去硬件加速在吃gpu的任务中cpu会负载比较高且卡顿

#### 三、修改注册表（联想高级售后经理火锅酱提供）

在桌面创建空白文件后缀改为.reg 写入以下代码：

```
 Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Dwm]
"OverlayTestMode"=dword:00000005
```

如果出现问题要恢复注册表则写入以下代码：

```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Dwm]
"OverlayTestMode"=-
```

注册表修改后需要重启才能生效 \
相关参考 \
https://www.bilibili.com/read/cv20029000 \
https://www.reddit.com/r/chrome/comments/x4za4o/white_cursor_bug/ \
https://www.zhihu.com/question/308547822/answer/2511480647 \
