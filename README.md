
###文章来源
内容翻译源于[RisingStack/node-style-guide](https://github.com/RisingStack/node-style-guide)

### 警告通知

我们正在持续地将项目移植到 [Standard](https://github.com/feross/standard)的标准之下——你可以去看看!

# [RisingStack](http://risingstack.com) Node.js Style Guide() {

### 大多数内容源自于 [Airbnb styleguide](https://github.com/airbnb/javascript)

并且深受以下内容的影响:
- @caolan's [Node.js styleguide](http://caolanmcmahon.com/posts/nodejs_style_and_structure)
- @felixge's [Node.js styleguide](https://github.com/felixge/node-style-guide)

## Table of Contents

  1. [类型](#类型)
  1. [对象](#对象)
  1. [数组](#数组)
  1. [字符串](#字符串)
  1. [函数](#函数)
  1. [属性(Properties)](#属性properties)
  1. [变量(Variables)](#变量variables)
  1. [引用(Requires)](#引用requires)
  1. [回调函数(callback)](#回调函数callback)
  1. [捕获异常(Try catch)](#捕获异常try-catch)
  1. [变量声明提升(Hoisting)](#变量声明提升hoisting)
  1. [条件表达式 & 等号](#条件表达式--等号)
  1. [块(Blocks)](#块blocks)
  1. [注释](#注释)
  1. [空白字符(Whitespace)](#空白字符whitespace)
  1. [逗号](#逗号)
  1. [分号](#分号)
  1. [类型分配 & 强制转换](#类型分配--强制转换)
  1. [命名规范](#命名规范)
  1. [存取器](#存取器)
  1. [构造函数](#构造函数)
  1. [贡献者](#贡献者)
  1. [许可](#许可)

## 类型

  - **基础类型**: 当你访问一个基本类型的变量时你可以直接操作它的值.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **复合数据类型**: 当你访问一个复合数据类型的变量时你操作的是它的值的一个引用(reference).

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 对象

  - 字面量形式创建对象.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - 使用可读的同义词代替保留字.

    ```javascript
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
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 数组

  - 字面量形式创建数组.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 如果你不知道数组的长度请使用`push`

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - 如果你像复制数组请使用`slice`. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - 如果要把一个类似数组的元素转成数组请使用`slice`.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 字符串

  - 操作字符串的时候使用单引号 `''` 

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - 字符串长度超过80时应该用连接符让字符串多行表示.
  - 注意:如果过度使用,长字符串的连接会影响性能.[jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
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
    ```

  - 如果用代码生成字符串,请使用`join`而不是字符串连接符.

    ```javascript
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
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 函数

  - 函数写法:

    ```javascript
    // 匿名函数写法(anonymous function expression)
    var anonymous = function() {
      return true;
    };

    // 普通函数写法(named function expression)
    var named = function named() {
      return true;
    };

    //立即执行函数写法 (immediately-invoked function expression (IIFE))
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - 不要在没有函数的块(block)(如if,while等等)内部定义函数.应该将函数分配给一个变量.

    ```javascript
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
    ```

  - 不要将参数命名为`arguments`,因为这样的参数会优先于给每个函数作用域的`arguments`.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**



## 属性(Properties)

  - 访问属性时使用`.`符号

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - 当用变量访问属性时使用`[]`符号.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 变量(Variables)

  - 永远使用`var`来定义变量.不这样做的话它会存在于全局空间中.我们希望尽量避免污染全局空间.这是地球超人警告我们的.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - 每一行定义一个变量,并且每一行前面都要有`var`.

    ```javascript
    // bad
     var items = getItems(),
          goSportsTeam = true,
          dragonball = 'z';

    // good
     var items = getItems();
     var goSportsTeam = true;
     var dragonball = 'z';
    ```

  - 最后才定义没有赋初始值的变量.当这些变量可能取决于之前的某个值时会非常有用.

    ```javascript
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
    ```

  - 避免冗余的变量名,可以用`object`来做到同样的事情.

    ```javascript

    // bad
    var kaleidoscopeName = '..';
    var kaleidoscopeLens = [];
    var kaleidoscopeColors = [];

    // good
    var kaleidoscope = {
      name: '..',
      lens: [],
      colors: []
    };
    ```

  - 在作用域的最一开始就定义变量.这会避免一些由于变量提升导致的问题.

    ```javascript
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

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 引用(Requires)

  - 像这样的顺序引入你的node的requires:
      - 核心模块
      - npm模块
      - 其他

    ```javascript
    // bad
    var Car = require('./models/Car');
    var async = require('async');
    var http = require('http');

    // good
    var http = require('http');
    var fs = require('fs');

    var async = require('async');
    var mongoose = require('mongoose');

    var Car = require('./models/Car');
    ```

  - Do not use the `.js` when requiring modules

  ```javascript
    // bad
    var Batmobil = require('./models/Car.js');

    // good
    var Batmobil = require('./models/Car');

  ```


**[⬆ 回到顶部](#table-of-contents)**

## 回调函数(Callback)

  - 回调函数中永远先检查是否返回了错误.

  ```javascript
  //bad
  database.get('pokemons', function(err, pokemons) {
    console.log(pokemons);
  });

  //good
  database.get('drabonballs', function(err, drabonballs) {
    if (err) {
      // handle the error somehow, maybe return with a callback
      return console.log(err);
    }
    console.log(drabonballs);
  });
  ```

  - Return on callbacks

  ```javascript
  //bad
  database.get('drabonballs', function(err, drabonballs) {
    if (err) {
      // if not return here
      console.log(err);
    }
    // this line will be executed as well
    console.log(drabonballs);
  });

  //good
  database.get('drabonballs', function(err, drabonballs) {
    if (err) {
      // handle the error somehow, maybe return with a callback
      return console.log(err);
    }
    console.log(drabonballs);
  });
  ```

  -当你的回调函数是其它部分的"接口”时,在回调函数中使用描述性的参数.这会让你的代码具有可读性.

  ```javascript
  // bad
  function getAnimals(done) {
    Animal.get(done);
  }

  // good
  function getAnimals(done) {
    Animal.get(function(err, animals) {
      if(err) {
        return done(err);
      }

      return done(null, {
        dogs: animals.dogs,
        cats: animals.cats
      })
    });
  }
  ```

**[⬆ 回到顶部](#table-of-contents)**


## 捕获异常(Try catch)

- 只在同步函数内抛异常.

  Try-catch 块不能用来包裹异步代码.它们会冒泡到顶层,并且让这个进程崩溃.

  ```javascript
  //bad
  function readPackageJson (callback) {
    fs.readFile('package.json', function(err, file) {
      if (err) {
        throw err;
      }
      ...
    });
  }
  //good
  function readPackageJson (callback) {
    fs.readFile('package.json', function(err, file) {
      if (err) {
        return  callback(err);
      }
      ...
    });
  }
  ```

- 在同步代码中捕获异常.

  ```javascript
  //bad
  var data = JSON.parse(jsonAsAString);

  //good
  var data;
  try {
    data = JSON.parse(jsonAsAString);
  } catch (e) {
    //handle error - hopefully not with a console.log ;)
    console.log(e);
  }
  ```

**[⬆ 回到顶部](#table-of-contents)**

## 变量声明提升(Hoisting)

  - 变量的声明会提升到作用域的顶部,但是他们的赋值并不是.

    ```javascript
    // 我们知道这事不会工作的. (这里假设
    // notDefined不是全局变量)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 在你引用这个变量之后对变量声明,会导致变量声明提升.
    // 注意赋的值`true`并没有提升.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器将变量声明提升到作用域的头部.
    //这意味着我们的例子可以被改写成以下内容:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 匿名函数将它们的变量声明提升,但是函数赋值并没有提升.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - 命名函数提升他们的变量声明,但是函数声明和函数体并没有提升.

    ```javascript
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
    ```

  - 函数声明会提升他们的名称和函数体.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 详细信息请看 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ 回到顶部](#table-of-contents)**



## 条件表达式 & 等号

  - 使用 `===` 和 `!==` 来取代 `==` 和 `!=`.
  - 条件表达式是使用强制使用`ToBoolean`方式来计算并且永远遵循以下简单规则:

    + **Objects** 当作 **true**
    + **Undefined** 当作 **false**
    + **Null** 当作 **false**
    + **Booleans** 当作 **the value of the boolean**
    + **Numbers**  当 **+0, -0, or NaN**时当作 **false**, 否则 **true**
    + **Strings** 如果是空字符串 `''`,当作 **false** , 否则 **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - 使用简便写法.

    ```javascript
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
    ```

  - 详细信息请看 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ 回到顶部](#table-of-contents)**


## 块(Blocks)

  - 每个多行块都要使用花括号.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
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
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 注释

  - 多行注释使用 `/** ... */` .其中包含说明,表明每个参数和返回值的类型和值.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 单行注释使用 `//` . 在注释的上一行留一空行.

    ```javascript
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
    ```

  - 给你的注释加上`FIXME`或`TODO`前缀,帮助其他开发者快速理解你之前指定的那些需要回顾的代码,或者你需要一个实现去解决某个问题.这个和其他的注释不同因为他们是有行为的(actionable). 这些行为是`FIXME -- need to figure this out` 或 `TODO -- need to implement`.

  - 用`// FIXME:` 注释一个问题

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - 用 `// TODO:` 注释一个解决办法

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ 回到顶部](#table-of-contents)**


## 空白字符(Whitespace)

  - 使用软制表符设置2个空格.

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```

  - 前花括号之前留一个空格.

    ```javascript
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
    ```

  - 在操作符之间添加空格.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - 文件末尾添加一个新空行.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - 很长的方法链(long method chains)时使用缩进.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 逗号

  - **不要在最前面写逗号**

    ```javascript
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
    ```

  - 不要有多余逗号：这会在IE6、IE7和IE9的怪异模式中导致一些问题；同时，在ES3的一些实现中，多余的逗号会增加数组的长度。在ES5中已经澄清.[source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 分号

  - **这是需要的.**

    ```javascript
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

    // good
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 类型分配 & 强制转换

  - 执行强制类型转换的语句。.
  - 字符串:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - 使用parseInt对Numbers进行转换，并带一个进制作为参数.

    ```javascript
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
    ```

  - 无论出于什么原因，或许你做了一些”粗野”的事；或许parseInt成了你的瓶颈；或许需要使用位运算来提高[性能](http://jsperf.com/coercion-vs-casting/3)，都要用注释说明你为什么这么做.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **注意:** 注意：当使用位运算时，Numbers被视为[64位值](http://es5.github.io/#x4.3.19)，但是位运算总是返回[32位整型](http://es5.github.io/#x11.7)。对于整型值大于32位的进行位运算将导致不可预见的行为。[Discussion](https://github.com/airbnb/javascript/issues/109).最大的有符号32位整数是2,147,483,647.

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - 布尔值(Booleans):

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 命名规范

  - 避免单个字符的字面量.请描述性地命名.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - 当命名对象、函数和实例时使用骆驼拼写法.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - 命名类或者构造函数时使用帕斯卡拼写法.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - 命名私有属性时使用`_`前缀.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - 保存`this`引用时使用`_this`.

    ```javascript
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
    ```

  - 对函数命名有利于堆栈跟踪.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 存取器

  - 对于属性,访问器函数不是必要的.
  - 如果定义了存取器函数，应使用`getVal()` 和 `setVal(‘hello’)`的格式.

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - 如果属性是布尔值(boolean),使用格式应该是`Val()` 和 `hasVal()`.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - 创建`get()`和`set()`函数是可以的，但是要保持一致.

    ```javascript
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
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 构造函数

  - 在原型对象上定义方法,而不是用新对象重写它.重写使继承变为不可能:重置原型将重写整个基类.

    ```javascript
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
    ```

  - 方法应该返回this，有利于构成方法链.

    ```javascript
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
    luke.setHeight(20) // => undefined

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
    ```


  - 写一个自定义的`toString()`方法是可以的，只要确保它能正常运行并且不会产生副作用.

    ```javascript
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
    ```

**[⬆ 回到顶部](#table-of-contents)**

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**[⬆ 回到顶部](#table-of-contents)**

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## 贡献者

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## 许可

(The MIT License)

Copyright (c) 2014 RisingStack

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 回到顶部](#table-of-contents)**

# };
