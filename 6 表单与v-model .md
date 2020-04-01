表单类控件承载了 个网页数据的录入与交互，本章将介绍如何使用指令 v-model 完成表单的数据双向绑定。

## 1 基本用法
表单控件在实际业务较为常见，比如单选、多选、下拉选择、输入框等，用它们可以完成数据的录入、校验、提交等。 vue 提供了 v-model 指令，用于在表单类元素上双向绑定数据，例如在输入框上使用时，输入的内容会实时映射到绑定的数据上。例如下面的例子：

![图片](https://uploader.shimo.im/f/fEys8hxqbL8Ay7xc.png!thumbnail)

在输入框输入的同时，｛ { message } ｝也会实时将内容渲染在视图中，如图

![图片](https://uploader.shimo.im/f/KZZEqXiGMRYOPseU.png!thumbnail)

对于文本域 textarea 是同样的用法

![图片](https://uploader.shimo.im/f/xZnP3jgalgknrT1J.png!thumbnail)

* 使用 v-model 后，表单控件显示的值只依赖所绑定的数据，不再关心初始化时的 value属性，对于 textarea>< /textarea> 之间插入的值，也不会生效．
* 使用 v-model 时，如果是用中文输入法输入中文，一般在没有选定词组前，也就是在拼音阶段， Vue 是不会更新数据的，当敲下汉字时才会触发史新 如果希望总是实时更新，可以用＠input 来替代 v-model.  事实上， v-model 也是一个特殊的语法糖，只不过它会在不同的表单上智能处理
### 1.1 单选按钮
单选按钮在单独使用时，不需要 v-model ，直接使用 v-bind 绑定一 布尔类型, 为真选中, 为否时不选，例如

![图片](https://uploader.shimo.im/f/4fOrCeUZ4ig7IPKb.png!thumbnail)

如果是组合使用来实现互斥选择的效果，就需要 v-model 配合 value 来使用

![图片](https://uploader.shimo.im/f/2hGXGrPA17Q81ZlT.png!thumbnail)

![图片](https://uploader.shimo.im/f/1Xv18gU5AdQR7veh.png!thumbnail)

数据 picked 值与单选按钮的 value 值一致时，就会选中该项，所以当前状态下选中的是第二项 JavaScript ，

![图片](https://uploader.shimo.im/f/A5aV6Vza9DoRcIFW.png!thumbnail)

### 1.2 复选框
复选框也分单独使用和组合使用，不过用法稍与单选不同。复选框单独使用时，也是用 v-model 来绑定一个布尔值，例如

![图片](https://uploader.shimo.im/f/5eo4XwCZcOIgxAyr.png!thumbnail)

在句选时，数据 checked 值变为了 true, label 渲染的内容也会更新。

组合使用时，也是 v-model 和 value 一起，多个勾选框都绑定到同一个数组类型的数据， value的值在数组当中，就会选中这一项。这一过程也是双向的，在勾选时， value 的值也会自动 push这个数组中，示例代码如下

![图片](https://uploader.shimo.im/f/DkVzCSBdQbYgOP0E.png!thumbnail)

![图片](https://uploader.shimo.im/f/gm8mjLnIZ5AEiux3.png!thumbnail)

当前状态下的结果如图

![图片](https://uploader.shimo.im/f/iQDuLLUWJ4grJBLy.png!thumbnail)

### 1.3 选择列表
选择列表就是下拉选择器，也是常见的表单控件，同样也分为单选和多选两种方式。先看下单选的示例代码：

![图片](https://uploader.shimo.im/f/ICRf1ib7Sb4fY32V.png!thumbnail)

![图片](https://uploader.shimo.im/f/fryyzWkMRBAwy8Wn.png!thumbnail)

<option＞是备选项，如果含有 value 属性,  v-model 就会优先匹配 value. 如果没有, 就会直接匹配＜option＞的 text ，比如选中第二项时 selected 的值是 js 而不是JavaScript. 

给＜ select＞添加属性 multiple 就可 多选了， 此时 v-model 绑定的是一个数组, 与复选框用法类似, 代码如下

![图片](https://uploader.shimo.im/f/BWsq4ww5KDIFY7SN.png!thumbnail)

在业务中，＜option＞经常用 v-for 动态输出， value, text 也是用 v-bind 来动态输出的 例如：

![图片](https://uploader.shimo.im/f/K6UibatiaxkTGCUk.png!thumbnail)

![图片](https://uploader.shimo.im/f/molG5X1aU4QaH0Iz.png!thumbnail)

虽然用选择列表＜select>控件可以很简单地完成下拉选择的需求，但是在实际业务中反而不常用，因为它的样式依赖平台和浏览器，无法统一,不太美观, 功能也受限， 如不支持搜索，所以常见的解决方案是用 div 模拟一个类似的控件。后面会尝试编写个下拉选择器的通用组件。

## 2 绑定值
上面的单选按钮、复选框和选择列表在单独使用或单选的模式下, v-model 绑定的值是一个静态字符串或布尔值.  但在业务中，有时需要绑定动态数据,  这时可以用 v-bind 来实现。

### 2.1 单选按钮
![图片](https://uploader.shimo.im/f/Dn08nBXTDToV1bP0.png!thumbnail)

在选中时， app.picked === app.value, 都是123

### 2.2 复选框
![图片](https://uploader.shimo.im/f/JM2ZaimWvCEGpKg5.png!thumbnail)

勾选时 app.toggle === app.value1, 未勾选时， app.toggle === app .v alue2.

### 2.3 选择列表
![图片](https://uploader.shimo.im/f/wUML5MZfoZMdNqoN.png!thumbnail)

当选中时， app.selected 是一个 Object ，所以 app.selected.number == 123

## 3 修饰符
与事件的修饰符类似， v-model 也有修饰符，用于控制数据同步的时机。

### 3.1 .lazy
在输入框中， v-model 默认是在 input 事件中同步输入框的数据（除了提示中介绍的中文输入法情况外），使用修饰符 .lazy 会转变为在change 事件中同步，示例代码如下：

![图片](https://uploader.shimo.im/f/U334AZBgBQoc1cFp.png!thumbnail)

这时， message 并不是实时改变的，而是在失焦或按回车时才更新。

### 3.2 .number
使用修饰符.number 可以将输入转换为 Number 类型，否则虽然你输入的是数字，但它的类型其实是 String ，比如在数字输入框时会比较有用，示例代码如下：

![图片](https://uploader.shimo.im/f/Tfjq4URUbeg9395K.png!thumbnail)

### 3.3 .trim
修饰符 .trim 可以自动过滤输入的首尾空格，示例代码如下

![图片](https://uploader.shimo.im/f/IZWOIo8S9Okg2gFz.png!thumbnail)








