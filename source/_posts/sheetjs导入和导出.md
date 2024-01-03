---
title: sheetjs导入和导出
date: 2023-10-12 22:06:13
tags: [sheetjs, xlsx]
---

### 一、引入 xlsx 包

```js
import { utils, writeFile, read } from "xlsx";
```

### 二、读取一个文件

#### 1、通过传统的方式读取文件

兼容性比较好的方式，创建一个 type 为 file 的元素然后点击它

```js
const inputDom = document.createElement("input");
inputDom.setAttribute("type", "file");
inputDom.onchange = async () => {
  //TODO 操作文件
  const file = await inputDom.files[0].arrayBuffer();
  const worksheet = workbook.Sheets[workbook.SheetNames[0]];
  const raw_data = utils.sheet_to_json(worksheet, { header: 1 });
};
inputDom.click();
```

#### 2、通过比较新的 api

```js
const fileList = await window.showOpenFilePicker(); //默认会返回一个list
const file = await files[0].getFile(); //单选文件所以选择第一个
const arrB = await file.ArrayBuffer(); //sheetjs支持arrayBuffer格式的数据
const workbook = read(arrB); //调用sheetjs的read方法，读取数据
const worksheet = workbook.Sheets[workbook.SheetNames[0]]; //获取到第一张表的数据
const data = utils.sheet_to_json(worksheet, { header: 1 }); //通过utils中的sheet_to_json方法转换成json
```

[[Window：showOpenFilePicker() 方法 - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/showOpenFilePicker)](url)

### 三、导出数据

#### 1、可以通过页面中 table 元素导出

```js
const wb = XLSX.utils.table_to_book(document.querySelector("#el-table-result"));
const wbout = XLSX.write(wb, {
  bookType: "xlsx",
  bookSST: true,
  type: "array",
});
saveAs(new Blob([wbout], { type: "application/octet-stream" }), "文件名.xlsx");
```

这种方式一些缺点:
1、如果是表格中使用了输入框，则导出的表格中没有输入框中的数据  
2、如果表格中使用了按钮，则导出的表格中会夹带按钮上的文字

#### 2、通过 json 数组导出

通常情况下，表格的数据格式类似这种

```js
[
  {
    name: "张三",
    age: 10,
    sex: "男",
    address: "翻斗花园二号楼1001室",
  },
  {
    name: "李四",
    age: 11,
    sex: "女",
    address: "翻斗花园二号楼1002室",
  },
];
```

导出时需要转化为这种样式

```js
[
  {
    姓名: "张三",
    年龄: 10,
    性别: "男",
    地址: "翻斗花园二号楼1001室",
  },
  {
    姓名: "李四",
    年龄: 11,
    性别: "女",
    地址: "翻斗花园二号楼1002室",
  },
];
```

在 sheet 中默认会使用对象的 key 做表格的表头，整理成这种数据可以更值观

```js
const data = [{ xxxx }];
const ws = utils.json_to_sheet(data);
const wb = utils.book_new();
utils.book_append_sheet(wb, ws, "第三个参数是工作簿的名称，可选");
writeFile(wb, "文件的名字需要带后缀.xlsx"); //运行到这一行代码浏览器即可提示下载
```

#### 四、其他

导出教程：[https://docs.sheetjs.com/docs/getting-started/examples/export](url)

导入教程：[https://docs.sheetjs.com/docs/getting-started/examples/import](url)

api 参考：[https://docs.sheetjs.com/docs/api/](url)
