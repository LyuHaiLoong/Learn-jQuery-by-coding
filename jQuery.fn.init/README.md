### jQuery.fn.init
练习jQuery.fn.init方法，实现无new操作，并根据自己的想法试写链式调用

### 关于无new操作
jQuery源码中，通过以下形式实现了无new化操作

```
jQuery = function( selector, context ) {
  //返回init的实例
  return new jQuery.fn.init( selector, context );
};
//在jQuery下添加fn属性，并让fn指向jQuery原型
jQuery.fn = jQuery.prototype = {
  constructor = jQuery,
  原型方法……
};
//创建jQuery.fn.init方法
init = jQuery.fn.init = function(){
  ...
};
//
init.prototype = jQuery.fn;

window.jQuery = window.$ = jQuery;
```
因为源码我刚刚开了个开头，所以这里边到底牵扯了多少逻辑也不太清楚。但是我自己写demo的时候，逻辑并不复杂，那这里边其实只需要认清一点——那就是jQuery.fn.init === jQuery.prototype.init。
所以我在自己写的时候，return实例这一步，直接写成了return Try.prototype.init(selector)。

这边的逻辑顺序，我目前初步理解为：
  1. 为了防止直接return new jQuery()形成死循环，所以需要返回另一个构造函数，这个构造函数必须继承jQuery所有方法;
  2. 实际上如果没有特殊意义的话，直接return jQuery.fn()这种形式也是可行的，那就要让jQuery.fn.prototype = jQuery.prototype。jQuery源码这里肯定是有用途，所以才写成这样的。但是像我这种Try.prototype.init写法，直接将init写到prototype原型下，是不能省去prototype的，因为prototype是对象，不能new操作;
  3. 整个操作的核心点只有一个，继承jQuery的prototype原型;
