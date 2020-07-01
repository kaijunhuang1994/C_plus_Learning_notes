# Markdown学习笔记

[TOC]

## 什么是Markdown？
**Markdown**是一种文本格式，你可以用它来控制文档的显示。
Markdown Preview Enhanced([MPE](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/))

## Markdown基本要素

### 标题
通过在文本前增加`#`来实现标题，最高六级标题

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

如果你想要给你的标题添加` id `或者` class`，请在标题最后添加 `{#id .class1 .class2}`
**这个暂时没有测试成功**

```markdown
# 这有一个标题{#my_id}
# 这个标题有两个classes{.class1 .class2}
```


### 强调
通过在文本首尾添加一个星号`*`或者下划线`_`来实现**斜体**

通过在文本首尾添加两个星号`*`或者下划线`_`来实现**粗体**

通过在文本首尾添加两个波浪线`~`来实现~~划掉字体~~


```markdown
*这是斜体*
_这是斜体_

**这是粗体**
__这是粗体__

~~这个文字会被划掉~~
```

### 列表
#### 无序列表
- Item 1
- Item 2
  - Item 2a
  - Item 2b

#### 有序列表
1. Item 1 
2. Item 2
3. Item 3
   1. Item 3a
   2. Item 3b 


###  添加图片
```markdown
![图片名称](图片路径)
```


### 链接
```markdown
[链接名称](链接路径)
```

### 引用
通过在段首添加`>`符号实现引用，且可添加多级引用

正如Kanye West所说：
> We're living the future so 
> the present is our past.
>> hahahaha

### 分割线

三个连字符

---

三个星号

***

三个下划线

___

### 行内代码
在行间代码块前后添加`即可：

代码块：`<addr>`

### 代码块
你可以在你的代码上面和下面添加` ``` `来表示代码块。


#### 语法高亮
通过在` ``` `后添加相应的语言实现代码高亮
```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```


#### 代码块 class（MPE 扩展的特性）
通过在` ``` `后添加`{.class1 .class}`实现代码块功能
```javascript {.class1 .class}
function add(x, y) {
  return x + y
}
```

#### 代码行数（MPE 扩展的特性）
通过在` ``` `后添加`{.line-numbers}`实现代码行数显示
```javascript {.line-numbers}
function add(x, y) {
  return x + y
}
```

#### 高亮代码行数（MPE 扩展的特性）
你可以通过添加`highlight`属性的方式来高亮代码行数：

高亮第2行：
```javascript {highlight=2}
function add(x, y) {
  return x + y
}
```

高亮第1到3行：
```javascript {highlight=1-3}
function add(x, y) {
  return x + y
}
```

高亮第1到3，5，7到9行：
```javascript {highlight=[1-3,5,7-9]}
function add(x, y) {
  return x + y
}
function add(x, y) {
  return x + y
}
function add(x, y) {
  return x + y
}
```
### 任务列表
`- []`代表未完成任务
`- [x]`代表已经完成的任务

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item

### 表格
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

## 拓展的语法（MPE 扩展的特性）

### 表格2
> 需要在VScode插件设置中打开 `enableExtendedTableSyntax` 选项来使其工作。

colspan `>` or `empty cell` :
| a | b |
| --- | ---|
| > | 1 |
| 2 ||

rowspan `^`:
| a | b |
| --- | ---|
| 1 | 2 |
| ^ | 4 |

### Emoji & Font-Awesome

> 只适用于 `markdown-it parser` 而不适用于` pandoc parser`。 缺省下是启用的。你可以在插件设置里禁用此功能。

:smile:
:fa-car:


### 上标
在需要上标的前后添加`^`:

30^th^

### 下标
在需要下标的前后添加`~`:

H~2~O

### 脚注

Content [^1]

[^1]: Hi! This is a footnote

### 缩略（暂时无用）

_[HTML]: Hyper Text Markup Language
_[W3C]: World Wide Web Consortium
The HTML specification
is maintained by the W3C.

### 标记
在需要下标的前后添加`=`:

==mark==

### CriticMarkup
> CriticMarkup 仅可用于 `markdown-it parser`，不与 `pandoc parser` 兼容。
> CriticMarkup默认缺省，需要在VScode插件设置中打开来使其工作

* Addition `{++ ++}`
* Deletion `{-- --}`
* Substitution `{~~ ~> ~~}`
* Comment `{>> <<}`
* Highlight `{== ==}{>> <<}`

Don't go around saying{-- to people that--} the world owes you a living. The world owes you nothing. It was here first. {~~One~>Only one~~} thing is impossible for God: To find {++any++} sense in any copyright law on the planet. {==Truth is stranger than fiction==}{>>strange but true<<}, but it is because Fiction is obliged to stick to possibilities; Truth isn’t.

### 在文档里运行代码

在vscode的设置里搜索`MPE`,打开`enableScriptExecution`选项才可以使用

```C++{cmd = true}
#include <iostream>
int main()
{
  int a = 3;
  int b = 4;
  std::cout<<a+b;
  return 0;
}
```
前提是vscode可以识别编译器路径（自并没有配置）
