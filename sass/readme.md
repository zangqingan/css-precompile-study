# 一、SASS的学习
1.1 sass概述
sass是世界上最成熟、最稳定、最强大的专业级CSS扩展语言！sass是基于Ruby语言开发而成，因此安装sass前需要安装Ruby。
它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能，这些拓展令 CSS 更加强大与优雅。
Sass 有两种语法格式。
首先是 SCSS (Sassy CSS) —— 也是本文示例所使用的格式 —— 这种格式仅在 CSS3 语法的基础上进行拓展，所有 CSS3 语法在 SCSS 中都是通用的，同时加入 Sass 的特色功能。此外，SCSS 也支持大多数 CSS hacks 写法以及浏览器前缀写法 (vendor-specific syntax)，以及早期的 IE 滤镜写法。这种格式以 .scss 作为拓展名。使用这个。

另一种也是最早的 Sass 语法格式，被称为缩进格式 (Indented Sass) 通常简称 "Sass"，是一种简化格式。它使用 “缩进” 代替 “花括号” 表示属性属于某个选择器，用 “换行” 代替 “分号” 分隔属性，很多人认为这样做比 SCSS 更容易阅读，书写也更快速。缩进格式也可以使用 Sass 的全部功能，只是与 SCSS 相比个别地方采取了不同的表达方式，具体请查看 the indented syntax reference。这种格式以 .sass 作为拓展名。
与 CSS 相同，使用 @charset 可以声明特定的编码格式。在样式文件的起始位置（前面没有任何空白与注释）插入 @charset "encoding-name"， Sass 将会按照给出的编码格式编译文件。注意所使用的编码格式必须可转换为 Unicode 字符集。
# 二、sass变量(variables)
2.1 sass变量声明
我们知道再css中，样式是由属性名和属性值组成的。而在sass中引入了编程语言中的变量，把一直重复使用的属性值用变量代替即可。
一般的我们会声明一个 _variables.scss 的文件来专门存放声明的变量。
可以在变量中存储颜色、字体 或任何 CSS 值，并在将来重复利用。
Sass 使用 $ 符号 作为变量的标志，变量的声明和css属性的声明很像。css生成时，变量最终会被它们的值所替代。
例如：$变量名:变量值(也就是css属性值)
$primary-color: #333;
body {
  color: $primary-color;
}

2.2 sass变量数据类型 (Data Types)
sass支持 6 种主要的数据类型：
    数字，1, 2, 13, 10px。如：$base-font-size: 1rem;
    字符串，有引号字符串与无引号字符串，"foo", 'bar', baz
    颜色，blue, #04a3f9, rgba(255,0,0,0.5)
    布尔型，true, false
    空值，null
    数组 (list)，用空格或逗号作分隔符，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
    maps (映射), 相当于 JavaScript 的 object，(key1: value1, key2: value2)可视为键值对的集合类似js中的对象object，映射必须始终由括号包围，并且必须始终以逗号分隔。通过map-get函数查找映射中的值。

语法：
$变量名:('具体的变量名1':属性值1,'具体的变量名2':属性值2,);
然后通过 map-get($变量名, '具体的变量名1');取得属性值1。
$colors: (
  'primary': #db9e3f,
  'info': #4b67af,
  'danger': #791a15,
  'blue-1': #1f3695,
  'blue': #4394e4,
  'white': #fff,
  'white-1': #fcfcfc,
  'white-2': #eceef0,
  'light': #f9f9f9,
  'light-1': #d4d9de,
  'grey': #999,
  'grey-1': #666,
  'dark-1': #343440,
  'dark': #222,
  'black': #000,
  
);
$border-color: map-get($colors, 'light-1');

# 三、sass嵌套(nested rules)
在css中重复写选择器是非常恼人的。如果要写一大串指向页面中同一块的样式时，往往需要 一遍又一遍地写同一个ID：如下示例
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }

像这种情况，sass可以让你只写一遍，且使样式可读性更高。在Sass中，你可以像俄罗斯套娃那样在规则块中嵌套规则块。
sass在输出css时会帮你把这些嵌套规则处理好，避免你的重复书写。就是在一个花括号{}内写所以叫嵌套。

#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
最终编译后还是如下的样式。
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }

这就是sass嵌套的魅力
父选择器的标识符&，编译后的 CSS 文件中 & 将被替换成嵌套外层的父选择器，& 必须作为选择器的第一个字符，其后可以跟随后缀生成复合的选择器
#main {
  color: black;
  &-sidebar { border: 1px solid; }
}

编译为

#main {color: black; }
#main-sidebar {border: 1px solid; }

# 四、sass指令和 @-Rules (Directives and @-Rules) 
Sass 支持所有的 CSS3 @-Rules，以及 Sass 自己特有的 “指令”（directives）主要是：控制指令 (control directives) 与 混合指令 (mixin directives) 两个部分。

## 5.1 CSS3 @-Rules
@import --- Sass 拓展了 @import 的功能，允许其导入 SCSS 或 Sass 文件。被导入的文件将合并编译到同一个 CSS 文件中，另外，被导入的文件中所包含的变量或者混合指令 (mixin) 都可以在导入的文件中使用。
如果需要导入 SCSS 或者 Sass 文件，但又不希望将其编译为 CSS，只需要在文件名前添加下划线，这样会告诉 Sass 不要编译这些文件，但导入语句中却不需要添加下划线。
例如，将文件命名为 _colors.scss，便不会编译 _colours.css 文件。
在其它scss文件中导入 @import "colors";


@media --- Sass 中 @media 指令与 CSS 中用法一样，只是增加了一点额外的功能：允许其在 CSS 规则中嵌套。
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}

编译为

.sidebar { width: 300px; }
@media screen and (orientation: landscape) {
    .sidebar {
        width: 500px; 
    } 
}





## 5.2 控制指令 (Control Directives)
控制指令是一种高级功能，日常编写过程中并不常用到，主要与混合指令 (mixin) 配合使用，尤其是用在 Compass 等样式库中。
### 5.2.1 @each 指令
@each 指令的格式是 $var in <list>, $var 可以是任何变量名，比如 $length 或者 $name，而 <list> 是一连串的值，也就是值列表。
语法1：list
@each $变量名 in 属性名1,属性名2,属性名3{
    //css属性
    
}
@each $变量名 in (属性名1,属性名2,属性名3){
    //css属性

}
语法2：maps
@each $key, $value in (key1: value1, key2: value2, key3: value3) {
  #{$key} {
    font-size: $value;
  }
}
$f-size:(key1: value1, key2: value2, key3: value3);
@each $key, $value in  $f-size{
  #{$key} {
    font-size: $value;
  }
}

具体例子如下：
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
编译为：
.puma-icon { background-image: url('/images/puma.png'); }
.sea-slug-icon { background-image: url('/images/sea-slug.png'); }
.egret-icon { background-image: url('/images/egret.png'); }
.salamander-icon { background-image: url('/images/salamander.png'); }

// text align
@each $var in (left, center, right) {
  .text-#{$var} {
    text-align: $var !important;
  }
}
编译为：
.text-left {text-align:left !inportant}
.text-center {text-align:center !inportant}
.text-right {text-align:right !inportant}

@each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
  #{$header} {
    font-size: $size;
  }
}
编译为：
h1 { font-size: 2em; }
h2 { font-size: 1.5em; }
h3 { font-size: 1.2em; }

$colors: (
  'primary': #db9e3f,
  'danger': #791a15,
  'black': #000,
);
@each $colorKey, $color in $colors {
  .bg-#{$colorKey} {
    background-color: $color;
  }
}
编译为：
.bg-primary { background-color:#db9e3f}
.bg-danger { background-color:#791a15}
.bg-black { background-color:#000}

### 5.2.2 @for 指令
@for 指令可以在限制的范围内循环重复输出格式，每次按要求（变量的值）对输出结果做出变动。
语法：注意$变量名 可以是任何变量，start和end必须是整数值
@for $变量名 from start through end {
    $变量名
}

@for $变量名 from start to end {
    $变量名
}
区别在于through包括start，end的值，而to只包括start的值而不包括 end的值
例子：
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
编译为
.item-1 { width: 2em; }
.item-2 { width: 4em; }
.item-3 { width: 6em; }

## 5.3 混合指令 (mixin directives)
通过sass的混合器实现大段样式的重用,混合器使用指令 @mixin 标识符定义，在样式表中通过 @include 来使用这个混合器，放在你希望的任何地方最终会将引入混合器的那行代码替换成混合器里边的内容。
语法：
@mixin 混合器名 {
    样式1,
    样式2,
    样式3,
    ....
}
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
如：添加跨浏览器的圆角边框
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
使用：
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

//sass最终生成：
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
给混合器传参;
混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。当@include混合器时，参数其实就是可以赋值给css属性值的变量。这种方式跟JavaScript的function很像
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }


