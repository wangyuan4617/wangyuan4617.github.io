---
title: BEM命名管理样式
date: 2023-02-25 10:19:32
tags: [css,element]
---

### 前言
 做VUE开发最常使用的就是elementUI/elementPlus，使用中发现element的元素类名起的非常规整，结合自己开发中的困难(指不知道怎么起类名)，看一下如何优雅的使用BEM命名
 ### BEM介绍
 BEM是一种针对CSS的命名规范的简称，B/E/M分别代表，块(Block)，元素(Element)，修饰符(Modifier)的简写。
 以elementPlus举例：
 - 块(Block)：可以理解为一个模块或者一个组件，块是规范中最大的单位，块之间是相互独立的，块之间不能相互影响。比如组件el-table,el-dialog,el-form等等。
 - 元素(Element)：可以理解为一个块(Block)中的元素，它是依赖上下文的，它被块(Block)包裹，但是每个元素(Element)之间也是相互独立的，它主要用来描述当前元素是什么。比如单选框el-radio中的input的那个点和单选框中的文字部分，el-dialog中的header,footer等等。
 - 修饰符(Modifier)：可以理解为对元素或者块的修饰,所以它不能单独出现，必须要跟随块(Block)或者元素(Element)使用。比如按钮的success状态el-button--success，alert组件success状态的el-alert--success<br/>
  
块(Block)元素(Element)修饰符(Modifier)之间的书写规范为
```
.block{} 
.block__element{}
.block--modifier{}
.block__element--modifier{}
```
#### 示例
```
<!-- 按钮 -->
<button class="el-button el-button--primary"></button>
<!-- dialog -->
<div class="el-dialog">
    <header class="el-dialog__header"></header>
    <div class="el-dialog__body"></div>
    <footer class="el-dialog__footer"></footer>
</div>
```
<font color='red'>！注意在使用中一个项目中块(Block)的命名应该是唯一的</font>

### 什么时候使用BEM
当我们使用BEM方法命名时，我们要知道哪些东西是应该使用BEM格式的。只有当需要明确关联性的模块关系时，才需要使用 BEM 格式。

### 使用混合拆分样式
在BEM中，位置和布局样式通过父级块来进行设置。这就需要通过混合组合块与元素，组合多个实体（块、元素、修饰符都被称作 BEM实体）的表现与样式，同时不耦合代码。

示例：
```
<!-- top 块 -->
<div class="top">
    <!-- search-form块混合top块的search-form元素 -->
    <form class="search-form top__search-form">搜索</form>
</div>
```

这样就通过混合的方式把位置样式从块中剥离了，可以在.top__search-form中设置表单的位置或浮动等样式，保持了 search-form块的样式独立，对其完整样式代码进行了解耦。在传统的命名方式中，我们经常通过嵌套的方式，如.top .search-form来对局部样式进行调整。但是这样做会改变选择器的权重。<font color='red'>在 BEM 的思想中，保持选择器扁平和低权重是一个准则。</font>

因此在使用 BEM 命名时需要格外注意遵循它的工作方式：

+ 不在块里设置位置、布局相关的样式，只设置基本样式。
+ 通过混合的方式，在作为父级块的元素时设置布局样式。
+ 适时拆分元素为独立的块，解耦样式并形成新的命名空间。

### 开放封闭原则
开放封闭原则是所有面向对象原则的核心，是说软件实体应该是可扩展，而不可修改的。即对扩展是开放的，而对修改是封闭的。如果将这个原则应用到BEM的使用上，就是说我们应该使用modifier去拓展block或element的样式，而不应该去修改block或element的基础样式。

+ 例如有两个按钮，如下所示：
    ```
    <div>
        <button class="btn">正确</button>
        <button class="btn">错误</button>
    </div>

    .btn{
        font-size:16px;
        color:white;
        border:none;
        padding:10px 8px;
        display: inline-block;
        background-color:green;
    }
    ```

    如果要将名为错误的按钮的背景颜色改为红色，我们需要给.btn加一个modifier，而不是直接去修改.btn的样式：

    ```
    <div>
        <button class="btn">正确</button>
        <button class="btn btn--error">错误</button>
    </div>

    .btn{
        font-size:16px;
        color:white;
        border:none;
        padding:10px 8px;
        display: inline-block;
        background-color:green;
    }
    .btn--error{
        background-color:red;
    }
    ```

### 具体使用方法
 #### useNameSpace 
 element中使用了useNameSpace的工具方法，每个块(Block)也就是el-table/el-form这些是一个命名空间
  + 先准备需要的参数
    ```
    // 定义一个组件的命名前缀，element中是‘el’
    const defaultNamespace = 'el'
    // 再定义一个描述组件状态的变量，例如：is-active
    const statePrefix = 'is-'
    ```
 + 再定义一个bem方法返回符合规范的类名
    ```
    const _bem = (
     namespace: string,
     block: string,
     blockSuffix: string,
     element: string,
     modifier: string,
    ) => {
     let cls = `${namespace}-${block}`
     if (blockSuffix)
       cls += `-${blockSuffix}`
    ​
     if (element)
       cls += `__${element}`
    ​
     if (modifier)
       cls += `--${modifier}`
    ​
     return cls
    }

    ```

 + 最后定义一个useNameSpace方法，返回BEM方法，用来判断补全
  
    ```
    export const  useNamespace = (block: string) => {
     const namespace = computed(() => defaultNamespace)
     const b = (blockSuffix = '') =>
       _bem(unref(namespace), block, blockSuffix, '', '')
     const e = (element?: string) =>
       element ? _bem(unref(namespace), block, '', element, '') : ''
     const m = (modifier?: string) =>
       modifier ? _bem(unref(namespace), block, '', '', modifier) : ''
     const be = (blockSuffix?: string, element?: string) =>
       blockSuffix && element
         ? _bem(unref(namespace), block, blockSuffix, element, '')
         : ''
     const em = (element?: string, modifier?: string) =>
       element && modifier
         ? _bem(unref(namespace), block, '', element, modifier)
         : ''
     const bm = (blockSuffix?: string, modifier?: string) =>
       blockSuffix && modifier
         ? _bem(unref(namespace), block, blockSuffix, '', modifier)
         : ''
     const bem = (blockSuffix?: string, element?: string, modifier?: string) =>
       blockSuffix && element && modifier
         ? _bem(unref(namespace), block, blockSuffix, element, modifier)
         : ''
    // 这个是is-active这种状态描述的方法
     const is: {
       (name: string, state: boolean | undefined): string
       (name: string): string
    } = (name: string, ...args: [boolean | undefined] | []) => {
       const state = args.length >= 1 ? args[0]! : true
       return name && state ? `${statePrefix}${name}` : ''
    }
     return {
       namespace,
       b,
       e,
       m,
       be,
       em,
       bm,
       bem,
       is,
    }
    }
    ```

    #### 使用举例
    ```
    // 例如开发到了dialog组件，在script中先定义命名空间，在html中直接使用即可
    import { useNamespace } from './compo/useNamespace'
    const bs = useNamespace('dialog')
    ns.b() // el-dialog
    ns.b('overlay') // el-dialog-overlay
    ns.e('header') // el-dialog__header
    ns.m('theme-dark') // el-dialog--theme-dark
    ns.be('header','close') // el-dialog-header__close
    ns.em('footer','small') // el-dialog__footer--small
    ns.bm('footer','small') // el-dialog-footer--small
    ns.bem('footer','btn','primary') // el-dialog-footer__btn--primary
    ns.is('closeable') // is-closeable

    ```

### 其他
另外element还使用了sass的 mixins 功能为css样式提供了具体使用，/element-plus/packages/theme-chalk/src/mixins