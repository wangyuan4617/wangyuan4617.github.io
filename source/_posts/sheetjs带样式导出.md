---
title: sheetjs带样式导出
date: 2024-07-11 14:23:04
tags: [sheetjs, xlsx]
---

### 一、引入需要的包

在 sheetjs 中只能导出默认不带样式的表格，如果需要表格宽度、字体颜色、单元格背景颜色等，需要引入另外的包

```js
npm install xlsx-js-style
```

### 二、使用

使用 xlsx-js-style 和 sheetjs 的形式相差不大，需要从 xlsx 中导出 写文件的方法

```js
import { utils, writeFile } from "xlsx-js-style";
```

使用时需要把每个单元格都看一个对象，这个对象包含单元格的数据和样式

```js
{
    v: "数据",// v->value
    t: "s",// t->type 数据类型
    s: { //s->style
        font: { //字体相关设置
            name: "微软雅黑",//字体
            sz: 12,//字体大小
            bold: true,//是否粗体
        },
        border:{// 单元格边框相关
        top:{// 上边框
            style: "thin",// 细线
            color: '#000000'
        },
        bottom:{...},// 下边框
        left:{...},// 左边框
        right:{...}// 右边框
    },
    fill:{//单元格背景颜色，在excel中叫 填充所以叫fill(猜的)
    fgColor:{//背景颜色
    rgb:'#FFFFFF'
    }
}
}
}
```

更多参数可以查看 sheetjs 的[单元格类型文档](https://docs.sheetjs.com/docs/csf/cell)

xlsx-js-style 的 npm 地址[xlsx-js-style](https://www.npmjs.com/package/xlsx-js-style)
