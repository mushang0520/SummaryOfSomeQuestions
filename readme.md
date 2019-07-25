# 问题列表
## 纯CSS实现滚动指示器
	1.在<body>标签内插入指示器元素：
	```
	<div class="indicator"></div>
	```
	2.粘贴如下所示的CSS代码：
	```
	body {
	    position: relative;
	}
	.indicator {
	    position: absolute;
	    top: 0; right: 0; left: 0; bottom: 0;
	    background: linear-gradient(to right top, teal 50%, transparent 50%) no-repeat;
	    background-size: 100% calc(100% - 100vh);
	    z-index: 1;
	    pointer-events: none;
	    mix-blend-mode: darken;
	}
	.indicator::after {
	    content: '';
	    position: fixed;
	    top: 5px; bottom: 0; right: 0; left: 0;
	    background: #fff;
	    z-index: 1;
	}
	```
## mui使用中的坑
	```
	# 引入mui.js,不要引入mui.min.js,否则会出现各种问题，目前mui.min.js是阉割版的，希望以后官方能说明，或者使用统一的版本
	# mui.openWindow()打开新页面，a链接中href="javascript:;"来阻止默认跳转,不能用#代替，否则会出现跳转之后，立马返回当前页的bug。
	```
## ios滑动不流畅
```
// 设置overflow: auto的滑动元素加上：
-webkit-overflow-scrolling:touch
// 如果元素的内容是动态撑开的，会造成不能滑动的bug，
//需要手动触发滚动条加上下面的代码：
//方案1：
height: calc(100% + 1px)
//方案二：
height: 101%
//方案三：
    .wrap:before {
      content:"";
      width: 1px;
      float: left;
      height: calc(100% + 1px);
      margin-left: -1px;
      display: block;

    }
    .wrap:after {
      content: "";
      width: 100%;
      clear: both;
      display: block;
    }
```
## 安卓手机系统键盘挤压页面，使固定定位或者flex布局错乱
    原因是安卓的系统键盘会挤压页面，使其失去原有的高度，解决办法是固定页面body的高度
```
    var bodyHeight = window.document.body.clientHeight
      window.onresize = function () {
        window.document.body.style.height = window.document.body.clientHeigh = bodyHeight
      }
 ```
 ## mint-ui的date-picker在部分安卓手机上不显示选择列表的情况
    原因是没能继承父容器宽度
    ```
    解决方案：设置.picker-items{width:100%}
    ```
## 彩色图片转为黑白
```
.gray { 
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    -ms-filter: grayscale(100%);
    -o-filter: grayscale(100%);
    
    filter: grayscale(100%);
	
    filter: gray;
}
```
* 我们还可以直接在CSS,像图片一样引用SVG文件
```
.gray{
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
  filter: gray;
  filter: url("data:image/svg+xml;utf8,<svg version='1.1' xmlns='http://www.w3.org/2000/svg' height='0'><filter id='greyscale'><feColorMatrix type='matrix' values='0.3333 0.3333 0.3333 0 0 0.3333 0.3333 0.3333 0 0 0.3333 0.3333 0.3333 0 0 0 0 0 1 0' /></filter></svg>#greyscale");
} 
```
### ES6动态的加载机制
```
// 1. 动态的加载机制

// a.js
export let a = 10;
export let b = 10;
export function add() {
  a = 15;
  b = 20;
  return a+b;
};

// b.js
import {a, b, add} from './a.js';
a+b;    // 20
add();  // 35
a+b;    // 35
```
### 伪类 :active 生效

要CSS伪类 `:active` 生效，只需要给 document 绑定 `touchstart` 或 `touchend` 事件

    <style>
    a {
      color: #000;
    }
    a:active {
      color: #fff;
    }
    </style>
    <a herf=foo >bar</a>
    <script>
      document.addEventListener('touchstart',function(){},false);
    </script>

### 消除 transition 闪屏

两个方法

    -webkit-transform-style: preserve-3d;
    /*设置内嵌的元素在 3D 空间如何呈现：保留 3D*/
    -webkit-backface-visibility: hidden;
    /*（设置进行转换的元素的背面在面对用户时是否可见：隐藏）*/
    
### 消除 IE10 里面的那个叉号

    input:-ms-clear{display:none;}
    
来源出处：[http://msdn.microsoft.com/en-us/library/windows/apps/hh767361.aspx](http://msdn.microsoft.com/en-us/library/windows/apps/hh767361.aspx "article4")
    
###关于 iOS 与 OS X 端字体的优化(横竖屏会出现字体加粗不一致等)
iOS 浏览器横屏时会重置字体大小，设置 `text-size-adjust` 为 `none` 可以解决 iOS 上的问题，但桌面版 Safari 的字体缩放功能会失效，因此最佳方案是将 `text-size-adjust` 为 `100%` 。

    -webkit-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
	text-size-adjust: 100%;
    
### JS 事件相关
click 事件普遍 300ms 的延迟
在手机上绑定 click 事件，会使得操作有 300ms 的延迟，体验并不是很好。
开发者大多数会使用封装的 tap 事件来代替 click 事件，所谓的 tap 事件由 touchstart 事件 + touchmove 判断 + touchend 事件封装组成

### iOS 点击会慢 300ms 问题

 [https://developers.google.com/mobile/articles/fast_buttons?hl=de-DE](https://developers.google.com/mobile/articles/fast_buttons?hl=de-DE "article5")
 [http://stackoverflow.com/questions/12238587/eliminate-300ms-delay-on-click-events-in-mobile-safari](http://stackoverflow.com/questions/12238587/eliminate-300ms-delay-on-click-events-in-mobile-safari "article5")

使用 CSS3 动画的时尽量利用3D加速，从而使得动画变得流畅。动画过程中的动画闪白可以通过 backface-visibility 隐藏。

    -webkit-transform-style: preserve-3d;
    -webkit-backface-visibility: hidden;
 
    
### IE10 的特殊鼠标事件

[http://www.mansonchor.com/blog/blog_detail_73.html](http://www.mansonchor.com/blog/blog_detail_73.html "article5")

### 不让 Android 手机识别邮箱

    <meta content="email=no" name="format-detection" />
    
### 禁止 iOS 识别长串数字为电话

    <meta content="telephone=no" name="format-detection" />
    
### 禁止 iOS 弹出各种操作窗口

    -webkit-touch-callout:none

### 禁止用户选中文字

    -webkit-user-select:none
    
### 动画效果中，使用 translate 比使用定位性能高

<http://paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/>

### 获取滚动条

    window.scrollY
    window.scrollX
 
 比如要绑定一个 touchmove 的事件，正常的情况下类似这样(来自呼吸二氧化碳)
 
    $('div').on('touchmove', function(){
       //.….code
    });
    
而如果中间的 code 需要处理的东西多的话，FPS 就会下降影响程序顺滑度，而如果改成这样

    $('div').on('touchmove', function(){
       setTimeout(function(){
         //.….code
       },0);
    });
    
把代码放在 setTimeout 中，会发现程序变快.
<https://developers.google.cn/web/fundamentals/design-and-ux/input/touch/#_7>

### 关于 iOS 系统中，WebAPP 启动图片在不同设备上的适应性设置

<http://stackoverflow.com/questions/4687698/mulitple-apple-touch-startup-image-resolutions-for-ios-web-app-esp-for-ipad/10011893#10011893>

### 关于 iOS 系统中，中文输入法输入英文时，字母之间可能会出现一个六分之一空格(焦点科技葛亮)
可以通过正则去掉 

    this.value = this.value.replace(/\u2006/g, '');

### 关于 Android WebView中，input 元素输入时出现的怪异情况
见图
![怪异图](https://user-gold-cdn.xitu.io/2019/4/25/16a53dad3b2e4a02?w=360&h=640&f=png&s=46277)

Android web视图，例如在 HTC EVO 和三星的 Galaxy Nexus 中，文本输入框在输入时表现的就像占位符。情况为一个类似水印的东西在用户输入区域，一旦用户开始输入便会消失(见图片)。

在 Android 的默认样式下当输入框获得焦点后，若存在一个绝对定位或者fixed的元素，布局会被破坏，其他元素与系统输入字段会发生重叠(如搜索图标将消失为搜索字段)，可以观察到布局与原始输入字段有偏差(见截图)。
这是一个相当复杂的问题，以下简单布局可以重现这个问题:

    <label for="phone">Phone: *</label>
    <input type="tel" name="phone" id="phone" minlength="10" maxlength="10" inputmode="latin digits" required="required" />
    
解决方法

    -webkit-user-modify: read-write-plaintext-only
    
详细参考：<http://www.bielousov.com/2012/android-label-text-appears-in-input-field-as-a-placeholder/>
注意，该属性会导致中文不能输入词组，只能单个字。感谢鬼哥与飞（游勇飞）贡献此问题与解决方案


### JS 动态生成的 select 下拉菜单在 Android2.x 版本的默认浏览器里不起作用

解决方法删除了 `overflow-x:hidden;` 然后在JS生成下来菜单之后 focus 聚焦，这两步操作之后解决了问题。(来自岛都-小Qi)

参考：<http://stackoverflow.com/questions/4697908/html-select-control-disabled-in-android-webview-in-emulator>

###移动端 HTML5 audio autoplay 失效问题

这个不是 BUG，由于自动播放网页中的音频或视频，会给用户带来一些困扰或者不必要的流量消耗，所以苹果系统和安卓系统通常都会禁止自动播放和使用 JS 的触发播放，必须由用户来触发才可以播放。

解决方法思路：先通过用户 touchstart 触碰，触发播放并暂停（音频开始加载，后面用 JS 再操作就没问题了）。

解决代码：

```
document.addEventListener('touchstart', function () {
    document.getElementsByTagName('audio')[0].play();
    document.getElementsByTagName('audio')[0].pause();
});
```
方案出处：<http://stackoverflow.com/questions/17350924/iphone-html5-audio-tag-not-working>

扩展阅读：<http://yujiangshui.com/recent-projects-review/#toc-7>

###移动端 HTML5 input date 不支持 placeholder 问题

input type date 的 placeholder 支持性有一定问题，因为浏览器会针对此类型 input 增加 datepicker 模块，看上去没那么必要支持 placeholder。

对 input type date 使用 placeholder 的目的是为了让用户更准确的输入日期格式，iOS 上会有 datepicker 不会显示 placeholder 文字，但是为了统一表单外观，往往需要显示。Android 部分机型没有 datepicker 也不会显示 placeholder 文字。

简单的进行了测试：

桌面端（Mac）

- Safari 不支持 datepicker，placeholder 正常显示。
- Firefox 不支持 datepicker，placeholder 正常显示。
- Chrome 支持 datepicker，显示 年、月、日 格式，忽略 placeholder。

移动端

- iPhone5 iOS7 有 datepicker 功能，但是不显示 placeholder。
- Andorid 4.0.4 无 datepicker 功能，不显示 placeholder

问题解决方法：

先使其 type 为 text，此时支持 placeholder，当触摸或者聚焦的时候，使用 JS 切换使其触发 datepicker 功能。

	<input placeholder="Date" class="textbox-n" type="text" onfocus="(this.type='date')"  id="date"> 

方案出处：<http://stackoverflow.com/questions/20321202/not-showing-place-holder-for-input-type-date-field-ios-phonegap-app>

###IOS Safari  支持localstorage但是setItem异常（QUOTA_EXCEEDED_ERR:DOM Exception 22）

      平台：IOS8.1
      browser：Safari600.1.4
 
 问题源自于项目需要在浏览器中遮罩提示,点击关闭状态存储在localstorage中。Safari浏览器关闭后刷新页面层依旧存在
 <a href="http://stackoverflow.com/questions/14555347/html5-localstorage-error-with-safari-quota-exceeded-err-dom-exception-22-an">bug issue</a>
简单的存储状态可以使用cookie的方式替代。
###Chrome 地址栏自动隐藏交互行为对于fixed 顶部的元素遮挡

	系统：IOS8.1
	浏览器：Chrome 26.0.1410.53

  描述信息：页面包含fixed顶部的tip element，当页面向下滑动的时候Chrome地址栏自动隐藏，当向上滑动的时候地址栏自动出现。这种交互行为本身的好处会增大用户可视、交互区域。但是在Chrome 26这个版本这个浏览器UI布局使用adjustPan的方式，以至于向上滑动以后fixed的元素没有被自动向下移动（没有重绘）。
```
<img src="./thumb/chrome-autohidden-influnce-fixed-element.png" width="320px" height="480px" alt="Chrome自动隐藏地址栏影响fixed元素显示"/>

<a href="https://code.google.com/p/chromium/issues/detail?id=288747">bug fixed</a>
<a href="http://stackoverflow.com/questions/11258877/fixed-element-disappears-in-chrome">解决办法在这里</a>
```
###Android平台遮罩层下的input、select、a等元素可以被点击和focus(点击穿透)
   
  问题发现于三星手机，这个在特定需求下才会有，因此如果没有类似问题的可以不看。首先需求是浮层操作，在三星上被遮罩的元素依然可以获取focus、click、change. <a href="https://code.google.com/p/android/issues/detail?id=6721">bug issue</a> ，在查看bug报告list以后，找到了两种解决方案，第一是通过层显示以后加入对应的class名控制，第二是通过将可获取焦点元素加入的disabled属性，也可以利用属性加dom锁定的方式（disabled的一种变换方式）

###部分机型存在type为search的input，自带close按钮样式修改方法

  有些机型的搜索input控件会自带close按钮（一个伪元素），而通常为了兼容所有浏览器，我们会自己实现一个，此时去掉原生close按钮的方法为

	#Search::-webkit-search-cancel-button{
    	display: none;    
	}

  如果想使用原生close按钮，又想使其符合设计风格，可以对这个伪元素的样式进行修改。

###唤起select的option展开
zepto方式:

```
$(sltElement).trigger("mousedown");
```
原生js方式:
```
function showDropdown(sltElement) {
    var event;
    event = document.createEvent('MouseEvents');
    event.initMouseEvent('mousedown', true, true, window);
    sltElement.dispatchEvent(event);
};
```
# Andorid Issues
  1. 三星I9100 （Android 4.0.4）不支持display:-webkit-flex这种写法的弹性布局，
         但支持display:-webkit-box这种写法的布局, 
         相关的属性也要相应修改，如-webkit-box-pack: center</p>
         移动端采用弹性布局时，建议直接写display:-webkit-box系列的布局
  2. touchmove事件在Android部分机型(如LG Nexus 5 android4.4，小米2 android 4.1.1)上只会触发一次
         p>解决方案是在触发函数里面加上e.preventDefault(); 记得将e也传进去。
# iOS Issues

* [Position fixed & scrolling on iOS](http://remysharp.com/2012/05/24/issues-with-position-fixed-scrolling-on-ios/)
* `iOS 5.0-` 的`Date`构造函数不支持规范标准中定义的`YYYY-MM-DD`格式，如 `new Date('2013-11-11')` 是 `Invalid Date`, 但支持`YYYY/MM/DD`格式，可用 `new Date('2013/11/11')` 
* IOS（7.1.1版本）使用webapp模式时滚动元素无法滚动到头，解决办法是设置 `<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />` 让APP占用整个屏幕空间布局。参考[官方文档](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)
* IOS（7.1.1版本）滚动元素中设定其中某个元素的 `innerHTML` 属性一定机率导致画面闪动（估计是触发了重绘），解决办法是设置文字时使用`textContent`属性。
* IOS（7.1.1版本）动态改变滚动元素中某个元素 `top` 属性一定机率导致画面闪动，解决办法是使用 `translateY` 替代
* IOS（7.1.1版本）通过`-webkit-overflow-scrolling: touch`方式设定的滚动元素时，如果滚到头的时候拖动，会出现页面的整体滚动，解决办法见<https://github.com/chemzqm/scrollfix/blob/master/index.js>
* IOS8一个页面内播放超过15个video之后触发解码器错误，将不能继续播放其他video。

### 常见的浏览器兼容问题
* HTML中的兼容问题
    * 不同浏览器的标签默认的外补丁和内补丁不同；
    
        1. 场景：随便写几个标签，不加样式控制的情况下，各自的margin 和padding差异较大。
        解决方法：上来先消除默认样式*{margin:0;padding:0;}；
        块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大（即双倍边距bug）；

        2. 场景：常见症状是IE6中后面的一块被顶到下一行；
        解决方法：在float的标签样式控制中加入 display:inline;将其转化为行内属性

* IE6中 z-index失效

            * 场景：元素的父级元素设置的z-index为1，那么其子级元素再设置z-index时会失效，其层级会继承父级元素的设置，造成某些层级调整上    的BUG；
        原因：z-index起作用有个小小前提，就是元素的position属性要 是relative，absolute或是fixed。
        解决方案：1.position:relative改为position:absolute；2.去除浮动；3.浮动元素添加position属性（如relative，absolute等）。


* 在写a标签的样式，写的样式没有效果，其实只是写的样式被覆盖了

        正确的a标签顺序应该：link/ visited/hover/active


* 24位的png图片，ie6中不兼容透明底儿

        解决方式：使用png透明图片呗，但是需要注意的是24位的PNG图片在IE6是不支持的，解决方案有两种：
        
        使用8位的PNG图片；
        为IE6准备一套特殊的图片





* js在不同浏览器中的兼容问题

事件监听的兼容；

* IE不支持addEventListener；
解决：给IE使用attachEvent
```
var addHandler = function(el, type, handler, args) {
    if (el.addEventListener) {
        el.addEventListener(type, handler, false);
    } else if (el.attachEvent) {
        el.attachEvent('on' + type, handler);
    } else {
        el['on' + type] = handler;
    }
};
var removeHandler = function(el, type, handler, args) {
    if (el.removeEventListener) {
        el.removeEventListener(type, handler, false);
    } else if (el.detachEvent) {
        el.detachEvent('on' + type, handler);
    } else {
        el['on' + type] = null;
    }
};
```
* event.target的兼容，引发事件的DOM元素。
    * IE6789不支持event.target；
    * 解决方法：event.srcElement;
    ```
    // 以下为兼容写法
    target = event.target || event.srcElement;
    ```
* 阻止事件冒泡的兼容
    * IE6789不支持event.stopPropagation；
    ```
    // 以下为兼容写法
    event.stopPropagation ? event.stopPropagation() : (event.cancelBubble = false);
    ```

#eslint配置规则
```
	## "rules": {

	// 定义对象的set存取器属性时，强制定义get
	"accessor-pairs": 2,
	// 指定数组的元素之间要以空格隔开(,后面)， never参数：[ 之前和 ] 之后不能带空格，always参数：[ 之前和 ] 之后必须带空格
	"array-bracket-spacing": [2, "never"],
	// 在块级作用域外访问块内定义的变量是否报错提示
	"block-scoped-var": 0,
	// if while function 后面的{必须与if在同一行，java风格。
	"brace-style": [2, "1tbs", { "allowSingleLine": true }],
	// 双峰驼命名格式
	"camelcase": 2,
	// 数组和对象键值对最后一个逗号， never参数：不能带末尾的逗号, always参数：必须带末尾的逗号，
	// always-multiline：多行模式必须带逗号，单行模式不能带逗号
	"comma-dangle": [2, "never"],
	// 控制逗号前后的空格
	"comma-spacing": [2, { "before": false, "after": true }],
	// 控制逗号在行尾出现还是在行首出现
	// http://eslint.org/docs/rules/comma-style
	"comma-style": [2, "last"],
	// 圈复杂度
	"complexity": [2,9],
	// 以方括号取对象属性时，[ 后面和 ] 前面是否需要空格, 可选参数 never, always
	"computed-property-spacing": [2,"never"],
	// 强制方法必须返回值，TypeScript强类型，不配置
	"consistent-return": 0,
	// 用于指统一在回调函数中指向this的变量名，箭头函数中的this已经可以指向外层调用者，应该没卵用了
	// e.g [0,"that"] 指定只能 var that = this. that不能指向其他任何值，this也不能赋值给that以外的其他值
	"consistent-this": 0,
	// 强制在子类构造函数中用super()调用父类构造函数，TypeScrip的编译器也会提示
	"constructor-super": 0,
	// if else while for do后面的代码块是否需要{ }包围，参数：
	// multi 只有块中有多行语句时才需要{ }包围
	// multi-line 只有块中有多行语句时才需要{ }包围, 但是块中的执行语句只有一行时，
	// 块中的语句只能跟和if语句在同一行。if (foo) foo++; else doSomething();
	// multi-or-nest 只有块中有多行语句时才需要{ }包围, 如果块中的执行语句只有一行，执行语句可以零另起一行也可以跟在if语句后面
	// [2, "multi", "consistent"] 保持前后语句的{ }一致
	// default: [2, "all"] 全都需要{ }包围
	"curly": [2, "all"],
	// switch语句强制default分支，也可添加 // no default 注释取消此次警告
	"default-case": 2,
	// 强制object.key 中 . 的位置，参数:
	// property，'.'号应与属性在同一行
	// object, '.' 号应与对象名在同一行
	"dot-location": [2, "property"],
	// 强制使用.号取属性
	// 参数： allowKeywords：true 使用保留字做属性名时，只能使用.方式取属性
	// false 使用保留字做属性名时, 只能使用[]方式取属性 e.g [2, {"allowKeywords": false}]
	// allowPattern: 当属性名匹配提供的正则表达式时，允许使用[]方式取值,否则只能用.号取值 e.g [2, {"allowPattern": "^[a-z]+(_[a-z]+)+$"}]
	"dot-notation": [2, {"allowKeywords": true}],
	// 文件末尾强制换行
	"eol-last": 2,
	// 使用 === 替代 ==
	"eqeqeq": [2, "allow-null"],
	// 方法表达式是否需要命名
	"func-names": 0,
	// 方法定义风格，参数：
	// declaration: 强制使用方法声明的方式，function f(){} e.g [2, "declaration"]
	// expression：强制使用方法表达式的方式，var f = function() {} e.g [2, "expression"]
	// allowArrowFunctions: declaration风格中允许箭头函数。 e.g [2, "declaration", { "allowArrowFunctions": true }]
	"func-style": 0,
	"no-alert": 0,//禁止使用alert confirm prompt
	"no-array-constructor": 2,//禁止使用数组构造器
	"no-bitwise": 0,//禁止使用按位运算符
	"no-caller": 1,//禁止使用arguments.caller或arguments.callee
	"no-catch-shadow": 2,//禁止catch子句参数与外部作用域变量同名
	"no-class-assign": 2,//禁止给类赋值
	"no-cond-assign": 2,//禁止在条件表达式中使用赋值语句
	"no-console": 2,//禁止使用console
	"no-const-assign": 2,//禁止修改const声明的变量
	"no-constant-condition": 2,//禁止在条件中使用常量表达式 if(true) if(1)
	"no-continue": 0,//禁止使用continue
	"no-control-regex": 2,//禁止在正则表达式中使用控制字符
	"no-debugger": 2,//禁止使用debugger
	"no-delete-var": 2,//不能对var声明的变量使用delete操作符
	"no-div-regex": 1,//不能使用看起来像除法的正则表达式/=foo/
	"no-dupe-keys": 2,//在创建对象字面量时不允许键重复 {a:1,a:1}
	"no-dupe-args": 2,//函数参数不能重复
	"no-duplicate-case": 2,//switch中的case标签不能重复
	"no-else-return": 2,//如果if语句里面有return,后面不能跟else语句
	"no-empty": 2,//块语句中的内容不能为空
	"no-empty-character-class": 2,//正则表达式中的[]内容不能为空
	"no-empty-label": 2,//禁止使用空label
	"no-eq-null": 2,//禁止对null使用==或!=运算符
	"no-eval": 1,//禁止使用eval
	"no-ex-assign": 2,//禁止给catch语句中的异常参数赋值
	"no-extend-native": 2,//禁止扩展native对象
	"no-extra-bind": 2,//禁止不必要的函数绑定
	"no-extra-boolean-cast": 2,//禁止不必要的bool转换
	"no-extra-parens": 2,//禁止非必要的括号
	"no-extra-semi": 2,//禁止多余的冒号
	"no-fallthrough": 1,//禁止switch穿透
	"no-floating-decimal": 2,//禁止省略浮点数中的0 .5 3.
	"no-func-assign": 2,//禁止重复的函数声明
	"no-implicit-coercion": 1,//禁止隐式转换
	"no-implied-eval": 2,//禁止使用隐式eval
	"no-inline-comments": 0,//禁止行内备注
	"no-inner-declarations": [2, "functions"],//禁止在块语句中使用声明（变量或函数）
	"no-invalid-regexp": 2,//禁止无效的正则表达式
	"no-invalid-this": 2,//禁止无效的this，只能用在构造器，类，对象字面量
	"no-irregular-whitespace": 2,//不能有不规则的空格
	"no-iterator": 2,//禁止使用iterator 属性
	"no-label-var": 2,//label名不能与var声明的变量名相同
	"no-labels": 2,//禁止标签声明
	"no-lone-blocks": 2,//禁止不必要的嵌套块
	"no-lonely-if": 2,//禁止else语句内只有if语句
	"no-loop-func": 1,//禁止在循环中使用函数（如果没有引用外部变量不形成闭包就可以）
	"no-mixed-requires": [0, false],//声明时不能混用声明类型
	"no-mixed-spaces-and-tabs": [2, false],//禁止混用tab和空格
	"linebreak-style": [0, "windows"],//换行风格
	"no-multi-spaces": 1,//不能用多余的空格
	"no-multi-str": 2,//字符串不能用\换行
	"no-multiple-empty-lines": [1, {"max": 2}],//空行最多不能超过2行
	"no-native-reassign": 2,//不能重写native对象
	"no-negated-in-lhs": 2,//in 操作符的左边不能有!
	"no-nested-ternary": 0,//禁止使用嵌套的三目运算
	"no-new": 1,//禁止在使用new构造一个实例后不赋值
	"no-new-func": 1,//禁止使用new Function
	"no-new-object": 2,//禁止使用new Object()
	"no-new-require": 2,//禁止使用new require
	"no-new-wrappers": 2,//禁止使用new创建包装实例，new String new Boolean new Number
	"no-obj-calls": 2,//不能调用内置的全局对象，比如Math() JSON()
	"no-octal": 2,//禁止使用八进制数字
	"no-octal-escape": 2,//禁止使用八进制转义序列
	"no-param-reassign": 2,//禁止给参数重新赋值
	"no-path-concat": 0,//node中不能使用dirname或filename做路径拼接
	"no-plusplus": 0,//禁止使用++，--
	"no-process-env": 0,//禁止使用process.env
	"no-process-exit": 0,//禁止使用process.exit()
	"no-proto": 2,//禁止使用proto属性
	"no-redeclare": 2,//禁止重复声明变量
	"no-regex-spaces": 2,//禁止在正则表达式字面量中使用多个空格 /foo bar/
	"no-restricted-modules": 0,//如果禁用了指定模块，使用就会报错
	"no-return-assign": 1,//return 语句中不能有赋值表达式
	"no-script-url": 0,//禁止使用javascript:void(0)
	"no-self-compare": 2,//不能比较自身
	"no-sequences": 0,//禁止使用逗号运算符
	"no-shadow": 2,//外部作用域中的变量不能与它所包含的作用域中的变量或参数同名
	"no-shadow-restricted-names": 2,//严格模式中规定的限制标识符不能作为声明时的变量名使用
	"no-spaced-func": 2,//函数调用时 函数名与()之间不能有空格
	"no-sparse-arrays": 2,//禁止稀疏数组， [1,,2]
	"no-sync": 0,//nodejs 禁止同步方法
	"no-ternary": 0,//禁止使用三目运算符
	"no-trailing-spaces": 1,//一行结束后面不要有空格
	"no-this-before-super": 0,//在调用super()之前不能使用this或super
	"no-throw-literal": 2,//禁止抛出字面量错误 throw "error";
	"no-undef": 1,//不能有未定义的变量
	"no-undef-init": 2,//变量初始化时不能直接给它赋值为undefined
	"no-undefined": 2,//不能使用undefined
	"no-unexpected-multiline": 2,//避免多行表达式
	"no-underscore-dangle": 1,//标识符不能以_开头或结尾
	"no-unneeded-ternary": 2,//禁止不必要的嵌套 var isYes = answer === 1 ? true : false;
	"no-unreachable": 2,//不能有无法执行的代码
	"no-unused-expressions": 2,//禁止无用的表达式
	"no-unused-vars": [2, {"vars": "all", "args": "after-used"}],//不能有声明后未被使用的变量或参数
	"no-use-before-define": 2,//未定义前不能使用
	"no-useless-call": 2,//禁止不必要的call和apply
	"no-void": 2,//禁用void操作符
	"no-var": 0,//禁用var，用let和const代替
	"no-warning-comments": [1, { "terms": ["todo", "fixme", "xxx"], "location": "start" }],//不能有警告备注
	"no-with": 2,//禁用with
	"array-bracket-spacing": [2, "never"],//是否允许非空数组里面有多余的空格
	"arrow-parens": 0,//箭头函数用小括号括起来
	"arrow-spacing": 0,//=>的前/后括号
	"accessor-pairs": 0,//在对象中使用getter/setter
	"block-scoped-var": 0,//块语句中使用var
	"brace-style": [1, "1tbs"],//大括号风格
	"callback-return": 1,//避免多次调用回调什么的
	"camelcase": 2,//强制驼峰法命名
	"comma-dangle": [2, "never"],//对象字面量项尾不能有逗号
	"comma-spacing": 0,//逗号前后的空格
	"comma-style": [2, "last"],//逗号风格，换行时在行首还是行尾
	"complexity": [0, 11],//循环复杂度
	"computed-property-spacing": [0, "never"],//是否允许计算后的键名什么的
	"consistent-return": 0,//return 后面是否允许省略
	"consistent-this": [2, "that"],//this别名
	"constructor-super": 0,//非派生类不能调用super，派生类必须调用super
	"curly": [2, "all"],//必须使用 if(){} 中的{}
	"default-case": 2,//switch语句最后必须有default
	"dot-location": 0,//对象访问符的位置，换行的时候在行首还是行尾
	"dot-notation": [0, { "allowKeywords": true }],//避免不必要的方括号
	"eol-last": 0,//文件以单一的换行符结束
	"eqeqeq": 2,//必须使用全等
	"func-names": 0,//函数表达式必须有名字
	"func-style": [0, "declaration"],//函数风格，规定只能使用函数声明/函数表达式
	"generator-star-spacing": 0,//生成器函数*的前后空格
	"guard-for-in": 0,//for in循环要用if语句过滤
	"handle-callback-err": 0,//nodejs 处理错误
	"id-length": 0,//变量名长度
	"indent": [2, 4],//缩进风格
	"init-declarations": 0,//声明时必须赋初值
	"key-spacing": [0, { "beforeColon": false, "afterColon": true }],//对象字面量中冒号的前后空格
	"lines-around-comment": 0,//行前/行后备注
	"max-depth": [0, 4],//嵌套块深度
	"max-len": [0, 80, 4],//字符串最大长度
	"max-nested-callbacks": [0, 2],//回调嵌套深度
	"max-params": [0, 3],//函数最多只能有3个参数
	"max-statements": [0, 10],//函数内最多有几个声明
	"new-cap": 2,//函数名首行大写必须使用new方式调用，首行小写必须用不带new方式调用
	"new-parens": 2,//new时必须加小括号
	"newline-after-var": 2,//变量声明后是否需要空一行
	"object-curly-spacing": [0, "never"],//大括号内是否允许不必要的空格
	"object-shorthand": 0,//强制对象字面量缩写语法
	"one-var": 1,//连续声明
	"operator-assignment": [0, "always"],//赋值运算符 += -=什么的
	"operator-linebreak": [2, "after"],//换行时运算符在行尾还是行首
	"padded-blocks": 0,//块语句内行首行尾是否要空行
	"prefer-const": 0,//首选const
	"prefer-spread": 0,//首选展开运算
	"prefer-reflect": 0,//首选Reflect的方法
	"quotes": [1, "single"],//引号类型 `` "" ''
	"quote-props":[2, "always"],//对象字面量中的属性名是否强制双引号
	"radix": 2,//parseInt必须指定第二个参数
	"id-match": 0,//命名检测
	"require-yield": 0,//生成器函数必须有yield
	"semi": [2, "always"],//语句强制分号结尾
	"semi-spacing": [0, {"before": false, "after": true}],//分号前后空格
	"sort-vars": 0,//变量声明时排序
	"space-after-keywords": [0, "always"],//关键字后面是否要空一格
	"space-before-blocks": [0, "always"],//不以新行开始的块{前面要不要有空格
	"space-before-function-paren": [0, "always"],//函数定义时括号前面要不要有空格
	"space-in-parens": [0, "never"],//小括号里面要不要有空格
	"space-infix-ops": 0,//中缀操作符周围要不要有空格
	"space-return-throw-case": 2,//return throw case后面要不要加空格
	"space-unary-ops": [0, { "words": true, "nonwords": false }],//一元运算符的前/后要不要加空格
	"spaced-comment": 0,//注释风格不要有空格什么的
	"strict": 2,//使用严格模式
	"use-isnan": 2,//禁止比较时使用NaN，只能用isNaN()
	"valid-jsdoc": 0,//jsdoc规则
	"valid-typeof": 2,//必须使用合法的typeof的值
	"vars-on-top": 2,//var必须放在作用域顶部
	"wrap-iife": [2, "inside"],//立即执行函数表达式的小括号风格
	"wrap-regex": 0,//正则表达式字面量用小括号包起来
	"yoda": [2, "never"]//禁止尤达条件
	}
	}
```
# 如何在 Web 关闭页面时发送 Ajax 请求
	```
	// blob 发送
	blob = new Blob([`room_id=123`], {type : 'application/x-www-form-urlencoded'});
	 navigator.sendBeacon("/cgi-bin/leave_room", blob)
	// FormData 发送var fd = new FormData();
	fd.append('room_id', 123);
	navigator.sendBeacon("/cgi-bin/leave_room", fd);
	// URLSearchParams 发送
	var params = new URLSearchParams({ room_id: 123 })
	navigator.sendBeacon("/cgi-bin/leave_room", params);
	```
	# mui使用中的坑...
	```
	引入mui.js,不要引入mui.min.js,否则会出现各种问题，目前mui.min.js是阉割版的，希望以后官方能说明，或者使用统一的版本
	mui.openWindow()打开新页面，a链接中href="javascript:;"来阻止默认跳转,不能用#代替，否则会出现跳转之后，立马返回当前页的bug。
	```
