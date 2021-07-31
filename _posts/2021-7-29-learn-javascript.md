---
layout: post
title: JavaScript学习笔记
categories: [JavaScript]
description: 廖雪峰JavaScript网站学习记录
keywords: JavaScript
---

本篇博客主要依据[廖雪峰官方javascript教程](https://www.liaoxuefeng.com/wiki/1022910821149312)学习并记录相关知识点。

JavaScript之前在做数据库课设时其实用到过，主要是前端设计那里，包括jQuery、Ajax也都涉及过，但也仅此而已，并没有系统地学习过这个语言。现在对于网页的脚本感兴趣，因此趁着有时间特地学习一下。

基本语法

- JavaScript的每个语句以 `;` 结束，但是不强制要求每个语句以 `;` 结尾，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上，在某些情况下会改变程序的语义，导致运行结果与期望不一致
    - `return` 语句
        ```js
        return
            {id: 1}
        ```

- JavaScript不区分整数和浮点数，统一用 `Number` 表示

- 比较运算符：
    - 第一种是 `==` 比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
    - 第二种是 `===` 比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。
    - 由于JavaScript这个设计缺陷，不要使用 `==` 比较，始终坚持使用 `===` 比较。
    - `NaN` 这个特殊的 `Number` 与所有其他值都不相等，包括它自己

- 启用strict模式的方法是在JavaScript代码的第一行写上 `'use strict';`

字符串

- 由于多行字符串用 `\n` 写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 \`...\` 表示

- 模板字符串 ```var message = `你好, ${name}, 你今年${age}岁了!`;```

- 字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果，调用常用字符串方法本身不会改变原有字符串的内容，而是返回一个新字符串

Array

- 直接给 `Array` 的 `length` 赋一个新的值会导致大小的变化，如果通过索引赋值时，索引超过了范围，同样会引起数组大小的变化，在编写代码时，不建议直接修改数组大小，访问索引时要确保索引不会越界
- 常用方法
    - `indexOf`
        - 搜索一个指定的元素的位置
    - `slice`
        - 对应 `String` 的 `substring()` 版本，它截取部分元素，返回一个新的 `Array`
        - 起止参数包括开始索引，不包括结束索引
        - 如果不给 `slice()` 传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个 `Array`
    - `push` 和 `pop`
        - `push()` 向末尾添加若干元素，`pop()` 则把 `Array` 的最后一个元素删除掉
    - `unshift` 和 `shift`
        - 开头添加与删除
    - `sort`
    - `reverse`
    - `splice`
        - 从指定的索引开始删除若干元素，然后再从该位置添加若干元素
    - `concat`
        - 把当前的 `Array` 和另一个 `Array` 连接起来，并返回一个新的 `Array`
        - 并没有修改当前 `Array`，可以接收任意个元素和 `Array` ，并且自动把 `Array` 拆开，然后全部添加到新的 `Array` 里
    - `join`
        - 把当前 `Array` 的每个元素都用指定的字符串连接起来，然后返回连接后的字符串，如果 `Array` 的元素不是字符串，将自动转换为字符串后再连接
- 高阶函数
    - `every`
        - 判断数组的所有元素是否满足测试条件
    - `find`
        - 查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回 `undefined`
    - `findIndex`
        - 查找符合条件的第一个元素，返回这个元素的索引，如果没有找到，返回 `-1`
    - `forEach`
        - 和 `map()` 类似，把每个元素依次作用于传入的函数，但不会返回新的数组。常用于遍历数组，因此传入的函数不需要返回值
- 由于 `Array` 也是对象，而它的每个元素的索引被视为对象的属性，因此， `for ... in` 循环循环出 `Array` 的索引（得到的是 `String` 而不是 `Number` ）

对象

- 用一个 `{...}` 表示一个对象，键值对以 `xxx: xxx` 形式申明，用 `,` 隔开。最后一个键值对不需要在末尾加 `,` ，如果加了，有的浏览器（如低版本的IE）将报错
- 如果属性名包含特殊字符，就必须用 `''` 括起来，访问这个属性也无法使用 `.` 操作符，必须用 `['xxx']` 来访问，编写代码时属性名尽量使用标准的变量名
- 对象的所有属性都是字符串，属性对应的值可以是任意数据类型，访问不存在的属性不报错，而是返回 `undefined`
- `delete` 删除属性
- `in` 检查是否存在属性
    - 如果 `in` 判断一个属性存在，这个属性不一定是当前对象的，可能是继承得到的（如 `toString` ），可以用 `hasOwnProperty()` 方法判断

Map

- 初始化 `Map` 需要一个二维数组，或者直接初始化一个空 `Map`
- `map.set()/get()/delete()`

Set

- 要创建一个 `Set` ，需要提供一个 `Array` 作为输入，或者直接创建一个空 `Set`
- `set.add()/delete()`

iterable

- 遍历 `Array` 可以采用下标循环，遍历 `Map` 和 `Set` 就无法使用下标。为了统一集合类型，ES6标准引入了新的 `iterable` 类型， `Array` 、 `Map` 和 `Set` 都属于 `iterable` 类型
- 具有 `iterable` 类型的集合可以通过新的 `for ... of` 循环来遍历
- `iterable` 内置的 `forEach` 方法接收一个函数，每次迭代就自动回调该函数
    ```js
    var a = ['A', 'B', 'C'];
    a.forEach(function (element, index, array) {
        // element: 指向当前元素的值
        // index: 指向当前索引
        // array: 指向Array对象本身
        console.log(element + ', index = ' + index);
    });

    var s = new Set(['A', 'B', 'C']);
    s.forEach(function (element, sameElement, set) {
        console.log(element);
    });

    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    m.forEach(function (value, key, map) {
        console.log(value);
    });

    var a = ['A', 'B', 'C'];
    a.forEach(function (element) {
        console.log(element);
    });
    ```

函数

- 由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数，传入的参数比定义的少也没有问题
- 关键字 `arguments`
    - 只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数，常用于判断传入参数的个数
- `rest` 参数
    - 只能写在最后，前面用 `...` 标识，不再需要 `arguments` 我们就获取了全部参数，如果传入的参数连正常定义的参数都没填满也不要紧， `rest` 参数会接收一个空数组（注意不是 `undefined` ）
- 变量作用域
    - 函数嵌套时内部函数可以访问外部函数定义的变量，反过来则不行。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量
- 变量提升
    - JavaScript的函数定义有个特点：它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部，但不会提升变量的赋值。
    - 在函数内部定义变量时，严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个var申明函数内部用到的所有变量
- 全局作用域
    - 不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象 `window` ，全局作用域的变量实际上被绑定到 `window` 的一个属性。
    - JavaScript实际上只有一个全局作用域，任何变量（函数也视为变量）如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报 `ReferenceError` 错误
- 命名空间
    - 全局变量会绑定到 `window` 上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。
    - 减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中
- 局部作用域
    - 由于JavaScript的变量作用域实际上是函数内部，我们在 `for` 循环等语句块中是无法定义具有局部作用域的变量的，为了解决块级作用域，ES6引入了新的关键字 `let` ，用 `let` 替代 `var` 可以申明一个块级作用域的变量。
- 常量
    - ES6标准引入了新的关键字 `const` 来定义常量， `const` 与 `let` 都具有块级作用域
- 解构赋值
    - 对数组元素进行解构赋值时，多个变量要用 `[...]` 括起来，解构赋值可以嵌套或忽略某些元素
        ```js
        // 嵌套
        let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
        // 忽略某些元素
        let [, , z] = ['hello', 'JavaScript', 'ES6'];
        ```
    - 如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性，使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为 `undefined`
        ```js
        // 赋值
        var {name, age, passport} = person;
        // 嵌套
        var {name, address: {city, zip}} = person;
        // 把 passport 属性赋值给变量 id :
        let {name, passport:id} = person;
        // 设置默认值
        var {name, single=true} = person;
        ```
    - 有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误。这是因为JavaScript引擎把 `{` 开头的语句当作了块处理，于是 `=` 不再合法。解决方法是用小括号括起来
        ```js
        var x, y;
        // 解构赋值:
        {x, y} = { name: '小明', x: 100, y: 200};
        // 语法错误: Uncaught SyntaxError: Unexpected token = 
        // 正确写法：
        ({x, y} = { name: '小明', x: 100, y: 200});
        ```
    - 如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中
- 方法（this关键字）
    - 在一个独立的函数调用中，根据是否是 `strict` 模式， `this` 指向 `undefined` 或 `window`
    - 要指定函数的 `this` 指向哪个对象，可以用函数本身的 `apply` 方法，它接收两个参数，第一个参数就是需要绑定的 `this` 变量，第二个参数是 `Array` ，表示函数本身的参数
    - 与`apply()` 类似的方法是 `call()`。唯一区别是：`apply()` 把参数打包成 `Array` 再传入；`call()` 把参数按顺序传入。
- `map/reduce`
    - 字符串转数字：`return s.split('').map((x) => x * 1).reduce((x, y) => x * 10 + y);`
- `sort`
    - 对于两个元素 `x` 和 `y` ，如果认为 `x < y` ，则返回 `-1` ，如果认为 `x == y` ，则返回 `0` ，如果认为 `x > y` ，则返回 `1`
    - `Array` 的 `sort()` 方法默认把所有元素先转换为 `String` 再排序
    - `sort()` 方法会直接对 `Array` 进行修改，它返回的结果仍是当前 `Array`

闭包（Closure）

- 返回函数不要引用任何循环变量，或者后续会发生变化的变量

generator

- `function*`

标准对象

- `null` 的类型是 `object` ，`Array` 的类型也是 `object` ，如果我们用 `typeof` 将无法区分出 `null` 、`Array` 和通常意义上的 `object` —— `{}`
- 不要使用 `new Number()` 、`new Boolean()` 、`new String()` 创建包装对象
- 用 `parseInt()` 或 `parseFloat()` 来转换任意类型到 `number`
- 用 `String()` 来转换任意类型到 `string` ，或者直接调用某个对象的 `toString()` 方法
- 通常不必把任意类型转换为 `boolean` 再判断，因为可以直接写 `if (myVar) {...}`
- `typeof` 操作符可以判断出 `number` 、`boolean` 、`string` 、`function` 和 `undefined`
- 判断 `Array` 要使用 `Array.isArray(arr)`
- 判断 `null` 请使用 `myVar === null`
- 判断某个全局变量是否存在用 `typeof window.myVar === 'undefined'`
- 函数内部判断某个变量是否存在用 `typeof myVar === 'undefined'`

Date

- 浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值
    ```js
    var now = new Date();
    now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
    now.getFullYear(); // 2015, 年份
    now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
    now.getDate(); // 24, 表示24号
    now.getDay(); // 3, 表示星期三
    now.getHours(); // 19, 24小时制
    now.getMinutes(); // 49, 分钟
    now.getSeconds(); // 22, 秒
    now.getMilliseconds(); // 875, 毫秒数
    now.getTime(); // 1435146562875, 以number形式表示的时间戳
    ```
- 创建一个指定日期和时间的 `Date` 对象
    - 月份范围用整数表示是 `0~11` ，`0` 表示一月，`1` 表示二月……
        ```js
        var d = new Date(2015, 5, 19, 20, 15, 30, 123);
        d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
        ```
    - 解析一个符合ISO 8601格式的字符串返回的不是 `Date` 对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个 `Date`
        ```js
        var d = Date.parse('2015-06-24T19:49:22.875+08:00');
        d; // 1435146562875

        var d = new Date(1435146562875);
        d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
        d.getMonth(); // 5
        ```
- 时区
    - 既可以显示本地时间，也可以显示调整后的UTC时间
        ```js
        var d = new Date(1435146562875);
        d.toLocaleString(); // '2015/6/24 下午7:49:22'，本地时间（北京时区+8:00），显示的字符串与操作系统设定的格式有关
        d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC时间，与本地时间相差8小时
        ```
    - 传递一个 `number` 类型的时间戳就不用关心时区转换。任何浏览器都可以把一个时间戳正确转换为本地时间。假设浏览器所在电脑的时间是准确的，那么世界上无论哪个时区的电脑，它们此刻产生的时间戳数字都是一样的，所以，时间戳可以精确地表示一个时刻，并且与时区无关。
        ```js
        if (Date.now) {
            console.log(Date.now()); // 老版本IE没有now()方法
        } else {
            console.log(new Date().getTime());
        }
        ```

正则表达式

- 正则表达式
    - `\d` 可以匹配一个数字
    - `\w` 可以匹配一个字母或数字
    - `\s` 可以匹配一个空格（也包括Tab等空白符）
    - `.` 可以匹配任意字符
    - `*` 表示任意个字符（包括 `0` 个）
    - `+` 表示至少一个字符
    - `?` 表示 `0` 个或 `1` 个字符
    - `{n}` 表示 `n` 个字符
    - `{n,m}` 表示 `n-m` 个字符
    - 可以用 `[]` 表示范围
        - `[0-9a-zA-Z\_]` 可以匹配一个数字、字母或者下划线
        - `[0-9a-zA-Z\_]+` 可以匹配至少由一个数字、字母或者下划线组成的字符串
        - `[a-zA-Z\_\$][0-9a-zA-Z\_\$]*` 可以匹配由字母或下划线、`$` 开头，后接任意个由一个数字、字母或者下划线、`$` 组成的字符串，也就是JavaScript允许的变量名
        - `[a-zA-Z\_\$][0-9a-zA-Z\_\$]{0, 19}` 更精确地限制了变量的长度是1-20个字符
    - `A|B` 可以匹配 `A` 或 `B`
    - `^` 表示行的开头，`^\d` 表示必须以数字开头
    - `$` 表示行的结束，`\d$` 表示必须以数字结束
- 创建一个正则表达式
    - 直接通过 `/正则表达式/` 写出来
    - 通过 `new RegExp('正则表达式')` 创建一个 `RegExp` 对象
- `RegExp` 对象的 `test()` 方法用于测试给定的字符串是否符合条件
- 正则表达式切分字符串
    ```js
    'a b   c'.split(' '); // ['a', 'b', '', '', 'c']
    'a b   c'.split(/\s+/); // ['a', 'b', 'c']
    'a,b, c  d'.split(/[\s\,]+/); // ['a', 'b', 'c', 'd']
    'a,b;; c  d'.split(/[\s\,\;]+/); // ['a', 'b', 'c', 'd']
    ```
- 分组
    - 用 `()` 表示的就是要提取的分组（Group）
    - 如果正则表达式中定义了组，就可以在 `RegExp` 对象上用 `exec()` 方法提取出子串来
    - `exec()` 方法在匹配成功后，会返回一个 `Array` ，第一个元素是正则表达式匹配到的整个字符串，后面的字符串表示匹配成功的子串。`exec()` 方法在匹配失败时返回 `null` 。
        ```js
        var re = /^(\d{3})-(\d{3,8})$/;
        re.exec('010-12345'); // ['010-12345', '010', '12345']
        re.exec('010 12345'); // null
        ```
- 正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符，加个?就可以采用非贪婪匹配
    ```js
    var re = /^(\d+)(0*)$/;
    re.exec('102300'); // ['102300', '102300', '']
    var re = /^(\d+?)(0*)$/;
    re.exec('102300'); // ['102300', '1023', '00']
    ```
- 全局匹配可以多次执行 `exec()` 方法来搜索一个匹配的字符串。当我们指定 `g` 标志后，每次运行 `exec()` ，正则表达式本身会更新 `lastIndex` 属性，表示上次匹配到的最后索引
    ```js
    var s = 'JavaScript, VBScript, JScript and ECMAScript';
    var re=/[a-zA-Z]+Script/g;
    re.exec(s); // ['JavaScript']
    re.lastIndex; // 10
    re.exec(s); // ['VBScript']
    re.lastIndex; // 20
    re.exec(s); // ['JScript']
    re.lastIndex; // 29
    re.exec(s); // ['ECMAScript']
    re.lastIndex; // 44
    re.exec(s); // null，直到结束仍没有匹配到
    ```
- 还可以指定 `i` 标志表示忽略大小写，`m` 标志表示执行多行匹配

JSON

- JSON的字符串规定必须用双引号 `""` ，`Object` 的键也必须用双引号 `""`
- 序列化 `JSON.stringify(obj, null, '  ')`
    - 要输出得好看一些，可以加上参数，按缩进输出
    - 第二个参数用于控制如何筛选对象的键值，如果我们只想输出指定的属性，可以传入 `Array`，还可以传入一个函数，这样对象的每个键值对都会被函数先处理
    - 对象内部定义`toJSON()` 方法，直接返回JSON应该序列化的数据
- 反序列化 `JSON.parse()`
    - 接收一个JSON格式的字符串
    - 还可以接收一个函数，用来转换解析出的属性

面向对象

- JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程
    - 不要直接用 `obj.__proto__` 去改变一个对象的原型，并且，低版本的IE也无法使用 `__proto__`
    - `Object.create()` 方法可以传入一个原型对象，并创建一个基于该原型的新对象
    - 除了直接用 `{ ... }` 创建一个对象外，还可以用一种构造函数的方法来创建对象，用关键字 `new` 来调用这个函数，并返回一个对象
        ```js
        function Student(props) {
            this.name = props.name || '匿名'; // 默认值为'匿名'
            this.grade = props.grade || 1; // 默认值为1
        }
        Student.prototype.hello = function () {
            alert('Hello, ' + this.name + '!');
        };
        function createStudent(props) {
            return new Student(props || {})
        }
        ```
    - `class` 关键字定义对象，`extends` 来实现继承
        ```js
        class PrimaryStudent extends Student {
            constructor(name, grade) {
                super(name); // 记得用super调用父类的构造方法!
                this.grade = grade;
            }

            myGrade() {
                alert('I am at grade ' + this.grade);
            }
        }
        ```

浏览器对象

- `window`
    - 浏览器窗口
    - `innerWidth` 和 `innerHeight` 属性，可以获取浏览器窗口的内部宽度和高度
- `navigator`
    - 浏览器的信息
    - `navigator.appName` ：浏览器名称；
    - `navigator.appVersion` ：浏览器版本；
    - `navigator.language` ：浏览器设置的语言；
    - `navigator.platform` ：操作系统类型；
    - `navigator.userAgent` ：浏览器设定的 `User-Agent` 字符串
    - `navigator` 的信息可以很容易地被用户修改，所以读取的值不一定是正确的
- `screen`
    - 屏幕的信息
    - `screen.width` ：屏幕宽度，以像素为单位；
    - `screen.height` ：屏幕高度，以像素为单位；
    - `screen.colorDepth` ：返回颜色位数，如8、16、24。
- `location`
    - 当前页面的URL信息
        ```js
        location.href; //http://www.example.com:8080/path/index.html?a=1&b=2#TOP
        location.protocol; // 'http'
        location.host; // 'www.example.com'
        location.port; // '8080'
        location.pathname; // '/path/index.html'
        location.search; // '?a=1&b=2'
        location.hash; // 'TOP'
        ```
    - 要加载一个新页面，可以调用 `location.assign()` 。如果要重新加载当前页面，调用 `location.reload()` 方法非常方便
- document
    - 当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document` 对象就是整个DOM树的根节点