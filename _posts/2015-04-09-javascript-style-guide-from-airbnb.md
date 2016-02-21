---
layout: post
title: JavaScript编码规范 - From Airbnb
description: 
category: work
---
<link rel="stylesheet" type="text/css" href="/js/prettify/css/github.css" />
####类型
* 基本类型 Primitives
  * string
  * number
  * boolean
  * null
  * undefined
  <pre class="prettyprint">
    var foo = 1;
    var bar = foo;
    bar = 9;
    console.log(foo, bar); // => 1, 9
  </pre>
* 复合类型 Objects
  * 创建对象，使用literal syntax
  <pre class="prettyprint">
   // bad
   var item = new Object();
   // good
   var item = {};
  </pre>
  * 不要使用保留字reserved words
  <pre class="prettyprint">
  // bad
  var superman = {
    default: { clark: 'kent' },
    private: true
  };
  // good
  var superman = {
    defaults: { clark: 'kent' },
    hidden: true
  };
  </pre>
  * 使用可读性强的同义词代替保留字
  <pre class="prettyprint">
  // bad
  var superman = {
    class: 'alien'
  };
  // bad
  var superman = {
    klass: 'alien'
  };
  // good
  var superman = {
    type: 'alien'
  };
  </pre>
 
####数组 Arrays
* 创建对象，使用literal syntax
  <pre class="prettyprint">
  // bad
  var items = new Array();
  // good
  var items = [];
  </pre>
* 使用push方法
  <pre class="prettyprint">
  var someStack = [];
  // bad
  someStack[someStack.length] = 'abracadabra';
  // good
  someStack.push('abracadabra');
  </pre>
* 需要复制数组时，使用slice
  <pre class="prettyprint">
  var len = items.length;
  var itemsCopy = [];
  var i;
  // bad
  for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
  }
  // good
  itemsCopy = items.slice();
  </pre>
* 使用slice将对象转换为数组
<pre class="prettyprint">
function trigger() {
    var args = Array.prototype.slice.call(arguments);
    ...
}
</pre> 

####字符串 Strings
* 对字符串使用单引号
<pre class="prettyprint">
// bad
var name = "Bob Parr";
// good
var name = 'Bob Parr';
// bad
var fullName = "Bob " + this.lastName;
// good
var fullName = 'Bob ' + this.lastName;
</pre>
* 超过80个字符的字符串应该使用字符串连接符进行跨行
  Notice：对长字符串过度使用连接符将会影响性能
<pre class="prettyprint">
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
// bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';
// good
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
</pre>
* 以编程方式创建字符串的时应该使用Array的join方法而不是通过连接符，尤其是在IE中
<pre class="prettyprint">
var items;
var messages;
var length;
var i;
messages = [{
      state: 'success',
      message: 'This one worked.'
}, {
      state: 'success',
      message: 'This one worked as well.'
}, {
      state: 'error',
      message: 'This one did not work.'
}];
length = messages.length;
// bad
function inbox(messages) {
      items = 'ul';
      for (i = 0; i < length; i++) {
        items += 'li' + messages[i].message + 'li';
      }
      return items + 'ul';
}
// good
function inbox(messages) {
      items = [];
      for (i = 0; i < length; i++) {
        // use direct assignment in this case because we're micro-optimizing.
        items[i] = 'li' + messages[i].message + 'li';
      }
      return 'ul' + items.join('') + 'ul';
}
</pre>

####函数 Functions
* 函数表达式
<pre class="prettyprint">
// anonymous function expression
var anonymous = function() {
      return true;
};
// named function expression
var named = function named() {
      return true;
};
// immediately-invoked function expression (IIFE)
(function() {
      console.log('Welcome to the Internet. Please follow me.');
})();
</pre>
* 不要再非函数块(if,while)中声明函数
<pre class="prettyprint">
// bad
if (currentUser) {
      function test() {
        console.log('Nope.');
      }
}
// good
var test;
if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
}
</pre>
* 不要命名一个参数为arguments，否则它将优先于传递给每个函数作用域中的arguments对象
<pre class="prettyprint">
// bad
function nope(name, options, arguments) {
    // ...stuff...
}
// good
function yup(name, options, args) {
    // ...stuff...
}
</pre>

####属性 Properties
* 使用点表示法访问属性
<pre class="prettyprint">
var luke = {
      jedi: true,
      age: 28
};
// bad
var isJedi = luke['jedi'];
// good
var isJedi = luke.jedi;
</pre>
* 用变量访问属性时要使用下标表示法([])
<pre class="prettyprint">
var luke = {
      jedi: true,
      age: 28
};
function getProp(prop) {
      return luke[prop];
}
var isJedi = getProp('jedi');
</pre>

####变量 Variables
* 总是使用var声明变量，不然其将变为全局变量。我们要想办法避免全局空间污染
<pre class="prettyprint">
// bad
superPower = new SuperPower();
// good
var superPower = new SuperPower();
</pre>
* 使用var声明每个变量，这样很容易添加新的变量声明
<pre class="prettyprint">
// bad
var items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';
// bad
// (compare to above, and try to spot the mistake)
var items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';
// good
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';
</pre>
* 最后声明未赋值的变量，这对于你需要根据之前已经赋值的变量对其他变量进行赋值时是很有帮助的
<pre class="prettyprint">
// bad
var i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;
// bad
var i;
var items = getItems();
var dragonball;
var goSportsTeam = true;
var len;
// good
var items = getItems();
var goSportsTeam = true;
var dragonball;
var length;
var i;
</pre>
* 在作用域顶端对变量赋值
<pre class="prettyprint">
// bad
function() {
      test();
      console.log('doing stuff..');
      //..other stuff..
      var name = getName();
      if (name === 'test') {
        return false;
      }
      return name;
}
// good
function() {
      var name = getName();
      test();
      console.log('doing stuff..');
      //..other stuff..
      if (name === 'test') {
        return false;
      }
      return name;
}
// bad - unnessary function call
function() {
      var name = getName();
      if (!arguments.length) {
        return false;
      }
      this.setFirstName(name);
      return true;
}
// good
function() {
      var name;
      if (!arguments.length) {
        return false;
      }
      name = getName();
      this.setFirstName(name);
      return true;
}
</pre>
 
####声明 Hoisting
* 变量声明是在作用域顶端，但并未赋值
Variable declarations get hoisted to the top of their scope, but their assignment does not.
<pre class="prettyprint">
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {
      console.log(notDefined); // => throws a ReferenceError
}
// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
}
// The interpreter is hoisting the variable
// declaration to the top of the scope,
// which means our example could be rewritten as:
function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
}
</pre>
* 匿名表达式可以提升变量，但不能提升函数
Anonymous function expressions hoist their variable name, but not the function assignment.
* 命名表达式会提升变量，而不是函数名或者函数体
<pre class="prettyprint">
function example() {
      console.log(anonymous); // => undefined
      anonymous(); // => TypeError anonymous is not a function
      var anonymous = function() {
        console.log('anonymous function expression');
      };
}
</pre>
* 函数声明会提升变量和函数
Named function expressions hoist the variable name, not the function name or the function body.Function declarations hoist their name and the function body.
<pre class="prettyprint">
function example() {
      console.log(named); // => undefined
      named(); // => TypeError named is not a function
      superPower(); // => ReferenceError superPower is not defined
      var named = function superPower() {
        console.log('Flying');
      };
}
// the same is true when the function name
// is the same as the variable name.
function example() {
      console.log(named); // => undefined
      named(); // => TypeError named is not a function
      var named = function named() {
        console.log('named');
      }
}
</pre>

####运算符 Comparison Operators & Equality
* 使用===和!==代替==和!=
* 比较运算符进行计算时会利用ToBoolean方法进行强制转换数据类型，并遵从以下规则
  * Objects的计算值是true
  * Undefined的计算值是false
  * Boolean的计算值是本身
  * Numbers如果是-0,+0或者NaN，则计算值是false反之是true
  * Strings如果是空，则计算值是false，反之是true
<pre class="prettyprint">
if ([0]) {
    // true
    // An array is an object, objects evaluate to true
}
</pre>
  * Tips
<pre class="prettyprint">
// bad
if (name !== '') {
    // ...stuff...
}
// good
if (name) {
    // ...stuff...
}
// bad
if (collection.length > 0) {
    // ...stuff...
}
// good
if (collection.length) {
    // ...stuff...
}
</pre>

####语句块 Blocks
* 对多行语句块使用大括号brace
<pre class="prettyprint">
// bad
if (test)
      return false;
// good
if (test) return false;
// good
if (test) {
      return false;
}
// bad
function() { return false; }
// good
function() {
      return false;
}
</pre>
* 对于if-else语句块，把if的右括号和else的左括号放在同一行
<pre class="prettyprint">
// bad
if (test) {
      thing1();
      thing2();
}
else {
      thing3();
}
// good
if (test) {
      thing1();
      thing2();
} else {
      thing3();
}
</pre>

####注释 Comments
* 多行注释使用/**...*/，需包含一个描述，所有参数的具体类型，值和返回值
<pre class="prettyprint">
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {
    // ...stuff...
      return element;
}
// good
/**
 \* make() returns a new element
 \* based on the passed in tag name
 \*
 \* @param {String} tag
 \* @return {Element} element
 */
function make(tag) {
      // ...stuff...
      return element;
}
</pre>
* 单行注释使用//，注释放在语句的上一行，并在注释之前留空行
<pre class="prettyprint">
// bad
var active = true;  // is current tab
// good
// is current tab
var active = true;
// bad
function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';
      return type;
}
// good
function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';
      return type;
}
</pre>
* 如果你指出的问题需要重新定位或者提出一个待解决的问题需要实现，给注释添加 _FIXME or TODO_ 前缀有利于其他开发者快速理解。这些注释不同于通常的注释，因为它们是可实施的。这些实施措施就是 _FIXME -- need to figure this out or TODO -- need to implement_
<pre class="prettyprint">
function Calculator() {
      // FIXME: shouldn't use a global here
      total = 0;
      return this;
}
</pre>
* 使用 _//TODO:_ 给问题解决方案作注释
<pre class="prettyprint">
function Calculator() {
      // TODO: total should be configurable by an options param
      this.total = 0;
      return this;
}
</pre>

####空白 Whitespace
* 设置制表符为两个空格soft tabs
* 在左大括号前留一个空格
<pre class="prettyprint">
// bad
function test(){
      console.log('test');
}
// good
function test() {
      console.log('test');
}
// bad
dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
});
// good
dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
});
</pre>
* 在控制语句中（if, while etc），左括号之前留一个空格。函数的参数列表之前不要有空格
<pre class="prettyprint">
// bad
if(isJedi) {
      fight ();
}
// good
if (isJedi) {
      fight();
}
// bad
function fight () {
      console.log ('Swooosh!');
}
// good
function fight() {
      console.log('Swooosh!');
}
</pre>
* 使用空格分隔运算符
<pre class="prettyprint">
// bad
var x=y+5;
// good
var x = y + 5;
</pre>
* 当调用很长的方法链时使用缩进，可以使用点来强调这行是方法调用，不是新的语句
<pre class="prettyprint">
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();
// bad
$('#items').
      find('.selected').
        highlight().
      end().
      find('.open').
        updateCount();
// good
$('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();
// bad
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
      .attr('width', (radius + margin) * 2).append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
// good
var leds = stage.selectAll('.led')
      .data(data)
      .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
      .append('svg:g')
      .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
      .call(tron.led);
</pre>
* 在语句块和下一个语句之前留一个空行

####逗号 Commas
* 不要在语句前留逗号
<pre class="prettyprint">
// bad
var story = [
      once
      , upon
      , aTime
];
// good
var story = [
      once,
      upon,
      aTime
];
// bad
var hero = {
      firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
};
// good
var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
};
</pre>
* 不要有多余的逗号

####分号 Semicolons
* Yup
<pre class="prettyprint">
// bad
(function() {
      var name = 'Skywalker'
      return name
})()
// good
(function() {
      var name = 'Skywalker';
      return name;
})();
// good (guards against the function becoming an argument when two files with IIFEs are concatenated)
;(function() {
      var name = 'Skywalker';
      return name;
})();
</pre>

####类型分配和强制转换  Type casting &　Coercion
* 在声明时进行类型转换
* Strings
<pre class="prettyprint">
//  => this.reviewScore = 9;
// bad
var totalScore = this.reviewScore + '';
// good
var totalScore = '' + this.reviewScore;
// bad
var totalScore = '' + this.reviewScore + ' total score';
// good
var totalScore = this.reviewScore + ' total score';
</pre>
* 使用parseInt对Numbers进行转换，不要省略进制参数
<pre class="prettyprint">
var inputValue = '4';
// bad
var val = new Number(inputValue);
// bad
var val = +inputValue;
// bad
var val = inputValue >> 0;
// bad
var val = parseInt(inputValue);
// good
var val = Number(inputValue);
// good
var val = parseInt(inputValue, 10);
</pre>
* 无论出于什么原因，或许你做了一些”粗野”的事；或许parseInt成了你的瓶颈；或许考虑到性能，需要使用位运算，都要用注释说明你为什么这么做
<pre class="prettyprint">
// good
/**
 \* parseInt was the reason my code was slow.
 \* Bitshifting the String to coerce it to a
 \* Number made it a lot faster.
 */
var val = inputValue >> 0;
</pre>
* 注意：当使用位运算bitshift时，Numbers被视为64位的值，但是位运算总是返回32位整型。对于整型值大于32位的进行位运算将导致不可预见的行为。最大的有符号32位整数是2,147,483,647
<pre class="prettyprint">
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
</pre>
* Booleans
<pre class="prettyprint">
var age = 0;
// bad
var hasAge = new Boolean(age);
// good
var hasAge = Boolean(age);
// good
var hasAge = !!age;
</pre>

####命名规范
* 避免单字母名称，使其具有描述性
<pre class="prettyprint">
// bad
function q() {
      // ...stuff...
}
// good
function query() {
      // ..stuff..
}
</pre>
* 使用驼峰法命名变量、函数和类名
<pre class="prettyprint">
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
var o = {};
function c() {}
// good
var thisIsMyObject = {};
function thisIsMyFunction() {}
</pre>
* 命名私有属性时使用前置下划线underscore 
<pre class="prettyprint">
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
// good
this._firstName = 'Panda';
</pre>
* this引用使用_this变量
<pre class="prettyprint">
// bad
function() {
      var self = this;
      return function() {
        console.log(self);
      };
}
// bad
function() {
      var that = this;
      return function() {
        console.log(that);
      };
}
// good
function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
}
</pre>
* 命名函数时，下面方式有利于堆栈跟踪
<pre class="prettyprint">
// bad
var log = function(msg) {
      console.log(msg);
};
// good
var log = function log(msg) {
      console.log(msg);
};
</pre>
* 如果文件单独包含一个类，文件名应该和类名保持一致
<pre class="prettyprint">
// file contents
class CheckBox {
      // ...
}
module.exports = CheckBox;
// in some other file
// bad
var CheckBox = require('./checkBox');
// bad
var CheckBox = require('./check_box');
// good
var CheckBox = require('./CheckBox');
</pre>

####存取器 Accessors
* 对于属性，访问器函数不是必须的
* 如果定义了存取器函数，应参照 getVal() 和 setVal('hello') 格式
<pre class="prettyprint">
// bad
dragon.age();
// good
dragon.getAge();
// bad
dragon.age(25);
// good
dragon.setAge(25);
</pre>
* 如果属性是boolean，格式应为 isVal() 和 hasVal()
<pre class="prettyprint">
// bad
if (!dragon.age()) {
      return false;
}
// good
if (!dragon.hasAge()) {
      return false;
}
</pre>
* 创建形式一致的 get() 和 set() 方法
<pre class="prettyprint">
function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
}
Jedi.prototype.set = function(key, val) {
      this[key] = val;
};
Jedi.prototype.get = function(key) {
      return this[key];
};
</pre>

####构造函数 Constructors
* 在原型对象上定义assign方法，而不要重写overwrite。重写使继承inheritance不可用，因为重写原型导致重写整个基类
<pre class="prettyprint">
function Jedi() {
      console.log('new jedi');
}
// bad
Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },
      block: function block() {
        console.log('blocking');
      }
};
// good
Jedi.prototype.fight = function fight() {
      console.log('fighting');
};
Jedi.prototype.block = function block() {
      console.log('blocking');
};
</pre>
* 返回this指针可以构建方法链
<pre class="prettyprint">
// bad
Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
};
Jedi.prototype.setHeight = function(height) {
      this.height = height;
};
var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined
// good
Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
};
Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
};
var luke = new Jedi();
luke.jump()
      .setHeight(20);
</pre>
* 写一个自定义的toString()方法是可以的，只要确保它能正常运行并且不会产生副作用
<pre class="prettyprint">
function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
}
Jedi.prototype.getName = function getName() {
      return this.name;
};
Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
};
</pre>

####事件 Events
* 当在事件对象上附加数据时（无论是DOM事件还是如Backbone一样拥有的私有事件），应传递hash对象而不是原始值，这可以让随后的贡献者给事件对象添加更多的数据，而不必去查找或者更新每一个事件处理程序。
<pre class="prettyprint">
// bad
$(this).trigger('listingUpdated', listing.id);
...
$(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
});
// good
$(this).trigger('listingUpdated', { listingId : listing.id });
...
$(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
});
</pre>

####模块 Modules
* 模块应该以 ! 开始，这能确保当脚本连接时，如果畸形模块忘记导入，包括最后一个分号，不会产生错误。
* 文件应该以驼峰式命名，放在同名的文件夹中，和唯一出口的名称相匹配
* 定义一个noConflict()方法来设置导出模块之前的版本,并返回当前版本。
* 在模块的顶部申明’use strict';
<pre class="prettyprint">
// fancyInput/fancyInput.js
!function(global) {
      'use strict';
      var previousFancyInput = global.FancyInput;
      function FancyInput(options) {
        this.options = options || {};
      }
      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };
      global.FancyInput = FancyInput;
}(this);
</pre>

####JQuery
* jQuery对象变量使用前缀$
<pre class="prettyprint">
// bad
var sidebar = $('.sidebar');
// good
var $sidebar = $('.sidebar');
</pre>
* 缓存jQuery对象
<pre class="prettyprint">
// bad
function setSidebar() {
      $('.sidebar').hide();
      // ...stuff...
      $('.sidebar').css({
        'background-color': 'pink'
      });
}
// good
function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();
      // ...stuff...
      $sidebar.css({
        'background-color': 'pink'
      });
}
</pre>
* 使用级联cascading $('.sidebar ul')或父子$('.sidebar > ul')选择器进行DOM查询
* 在范围内使用find进行jQuery对象查询
<pre class="prettyprint">
// bad
$('ul', '.sidebar').hide();
// bad
$('.sidebar').find('ul').hide();
// good
$('.sidebar ul').hide();
// good
$('.sidebar > ul').hide();
// good
$sidebar.find('ul').hide();
</pre>


[原文链接](https://github.com/airbnb/javascript)