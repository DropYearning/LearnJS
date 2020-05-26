#  JavaScript 基础

## 1 基本语法
- JavaScript并不强制要求在每个语句的结尾加`;`，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上`;`
  > 让JavaScript引擎自动加分号在某些情况下会改变程序的语义，导致运行结果与期望不一致。在本教程中，我们不会省略;，所有语句都会添加;。
- 注释 : 以`//`开头直到行末的字符被视为行注释
- 另一种块注释是用`/*...*/`把多行字符包裹起来，把一大“块”视为一个注释：
- JavaScript不区分整数和浮点数，统一用Number表示
  - `NaN;` // NaN表示Not a Number，当无法计算结果时用NaN表示
  - `Infinity;` // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
- 十六进制用0x前缀和0-9，a-f表示，例如：`0xff00`，`0xa5b4c3d2`
- 字符串是以**单引号**'或**双引号**"括起来的任意文本，比如`'abc'`，`"xyz"`等等
- JavaScript在设计时，有两种比较运算符：
  - 第一种是`==`比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
  - 第二种是`===`比较，它不会自动转换数据类型，如果数据类型不一致，返回`false`，如果一致，再比较。
- `NaN`这个特殊的Number与所有其他值都不相等，包括它自己：`NaN === NaN; // false`
  - 唯一能判断NaN的方法是通过`isNaN()`函数
- 最后要注意浮点数的相等比较：`1 / 3 === (1 - 2 / 3); // false `
  - 浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：`Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true`
- `null`表示一个“空”的值，它和`0`以及空字符串`''`不同，`0`是一个数值，`''`表示长度为0的字符串，而`null`表示“空”。
- 在JavaScript中，还有一个和`null`类似的`undefined`，它表示“未定义”。
    - JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。
- JavaScript把`null`、`undefined`、`0`、`NaN`和空字符串`''`视为`false`，其他值一概视为`true`


### 1.1 变量
- 变量在JavaScript中就是用一个变量名表示，变量名是大小写英文、数字、`$`和`_`的组合，且不能用数字开头。变量名也不能是JavaScript的关键字，如`if`、`while`等。申明一个变量用`var`语句
    - 在JavaScript中，使用等号`=`对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量，但是要注意只能用`var`申明一次
    - JS是一门动态语言，这种变量本身类型不固定
- 如果一个变量没有通过`var`申明就被使用，那么该变量就自动被申明为**全局变量**：`i = 10; // i现在是全局变量`
  > 为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过`var`申明变量，未使用`var`申明变量就使用的，将导致运行错误。启用strict模式的方法是在JavaScript代码的第一行写上：`'use strict';`
- 使用`var`申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内（函数的概念将稍后讲解），同名变量在不同的函数体内互不冲突。


### 1.2 数组
- JavaScript的数组可以包括任意数据类型。例如：`[1, 2, 3.14, 'Hello', null, true];`
- 数组用`[]`表示，元素之间用`,`分隔。
- 另一种创建数组的方法是通过`Array()`函数实现：`new Array(1, 2, 3); // 创建了数组[1, 2, 3]`
- 请注意，直接给Array的length赋一个新的值会导致Array大小的变化：
    ```js
    var arr = [1, 2, 3];
    arr.length; // 3
    arr.length = 6;
    arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
    arr.length = 2;
    arr; // arr变为[1, 2]
    ```
- Array可以通过索引把对应的元素修改为新的值，因此，对Array的索引进行赋值会直接修改这个Array：
  ```js
    var arr = ['A', 'B', 'C'];
    arr[1] = 99;
    arr; // arr现在变为['A', 99, 'C']
  ```
- 如果数组的某个元素又是一个`Array`，则可以形成多维数组
- 数组的常用方法：
  - 与String类似，`Array`也可以通过`indexOf()`来搜索一个指定的元素的位置
  - `slice()`就是对应String的`substring()`版本，它截取`Array`的部分元素，然后返回一个新的`Array`
  - `push()`向`Array`的末尾添加若干元素，`pop()`则把`Array`的最后一个元素删除掉
  - 如果要往`Array`的头部添加若干元素，使用`unshift()`方法，`shift()`方法则把`Array`的第一个元素删掉
  - `sort()`可以对当前`Array`进行排序，它会直接修改当前`Array`的元素位置，直接调用时，按照默认顺序排序
  - `reverse()`把整个`Array`的元素给掉个个，也就是反转
  - `concat()`方法把当前的`Array`和另一个`Array`连接起来，并返回一个新的`Array`
  - `join()`方法是一个非常实用的方法，它把当前`Array`的每个元素都用指定的字符串连接起来，然后返回连接后的字符串
  

### 1.3 字符串
- 由于**多行字符**串用` `写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号
- 模板字符串, 如果有很多变量需要连接，用+号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：
  ```js
    var name = '小明';
    var age = 20;
    var message = `你好, ${name}, 你今年${age}岁了!`;
    alert(message);
  ```
- 要获取字符串某个指定位置的字符，使用类似Array的下标操作，索引号从0开始
- 需要特别注意的是，字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果：
  ```js
    var s = 'Test';
    s[0] = 'X';
    alert(s); // s仍然为'Test'
  ```  
- 字符串的常用方法：
  - `toUpperCase()`把一个字符串全部变为大写
  - `toLowerCase()`把一个字符串全部变为小写
  - `indexOf()`会搜索指定字符串出现的位置
  - `substring()`返回指定索引区间的子串


### 1.4 对象
- JavaScript的对象是一组**由键-值组成的无序集合**，例如：
    ```js
    var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
    };
    ```
- JavaScript对象的键都是字符串类型，值可以是任意数据类型。上述`person`对象一共定义了6个键值对，其中每个键又称为对象的属性，例如，`person`的`name`属性为`'Bob'`，`zipcode`属性为`null`。
- 要获取一个对象的属性，我们用`对象变量.属性名`的方式：
    ```js
    person.name; // 'Bob'
    person.zipcode; // null
    ```
- 也可以用`xiaohong['name']`来访问`xiaohong`的`name`属性，不过`xiaohong.name`的写法更简洁。我们在编写JavaScript代码的时候，属性名尽量使用标准的变量名，这样就可以直接通过`object.prop`的形式访问一个属性了。
- JavaScript用一个`{...}`表示一个对象，键值对以`xxx: xxx`形式申明，用`,`隔开。**注意，最后一个键值对不需要在末尾加`,`，如果加了，有的浏览器（如低版本的IE）将报错**。
- 如果属性名包含特殊字符，就必须用''括起来：
  ```js
    var xiaohong = {
        name: '小红',
        'middle-school': 'No.1 Middle School'// 
    };
  ```
  - `middle-school`不是一个有效的变量，就需要用`''`括起来。访问这个属性也无法使用`.`操作符，必须用`['xxx']`来访问
  - 由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性：
  ```js
    var xiaoming = {
        name: '小明'
    };
    xiaoming.age; // undefined
    xiaoming.age = 18; // 新增一个age属性
    xiaoming.age; // 18
    delete xiaoming.age; // 删除age属性
    xiaoming.age; // undefined
    delete xiaoming['name']; // 删除name属性
    xiaoming.name; // undefined
    delete xiaoming.school; // 删除一个不存在的school属性也不会报错
  ```
- 如果我们要检测`xiaoming`是否拥有某一属性，可以用`in`操作符：
    ```js
    var xiaoming = { name: '小明', birth: 1990, school: 'No.1 Middle School', height: 1.70, weight: 65, score: null }; 
    'name' in xiaoming; // true 'grade' in xiaoming; // false
    ```
  - 不过要小心，如果`in`判断一个属性存在，这个属性不一定是`xiaoming`的，它可能是`xiaoming`继承得到的
  - 要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法

### 1.5 循环
- `for`循环，通过**初始条件**、**结束条件**和**递增条件**来循环执行语句块
- for循环的一个变体是for ... in循环，它可以把一个对象的所有属性依次循环出来：
    ```js
    var o = {
        name: 'Jack',
        age: 20,
        city: 'Beijing'
    };
    for (var key in o) {
        console.log(key); // 'name', 'age', 'city'
    }
    ```
- `for`循环在已知循环的初始和结束条件时非常有用。而上述忽略了条件的`for`循环容易让人看不清循环的逻辑，此时用`while`循环更佳。
- `while`循环只有一个判断条件，条件满足，就不断循环，条件不满足时则退出循环
- 最后一种循环是`do { ... } while()`循环，它和`while`循环的唯一区别在于，不是在每次循环开始的时候判断条件，而是在每次循环完成的时候判断条件

### 1.6 Map和Set
- JavaScript的默认对象表示方式`{}`可以视为其他语言中的`Map`或`Dictionary`的数据结构，即一组键值对。
- 最新的ES6规范引入了新的数据类型`Map`:
  ```js
    var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);// 使用二维数组初始化Map
    m.get('Michael'); // 95
  ```
- 也可以直接初始化一个空的Map:
  ```js
    var m = new Map(); // 空Map
    m.set('Adam', 67); // 添加新的key-value
    m.set('Bob', 59);
    m.has('Adam'); // 是否存在key 'Adam': true
    m.get('Adam'); // 67
    m.delete('Adam'); // 删除key 'Adam'
    m.get('Adam'); // undefined
  ``` 
- `Set`和`Map`类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在`Set`中，没有重复的key
- 要创建一个`Set`，需要提供一个`Array`作为输入，或者直接创建一个空`Set`：
  ```js
    var s1 = new Set(); // 空Set
    var s2 = new Set([1, 2, 3]); // 含1, 2, 3
    var s = new Set([1, 2, 3, 3, '3']); // 重复元素在Set中自动被过滤
    s; // Set {1, 2, 3, "3"}
  ```
- 通过`add(key)`方法可以添加元素到`Set`中，可以重复添加，但不会有效果
- 通过`delete(key)`方法可以删除元素

### 1.7 iterable
- 遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。
- 具有`iterable`类型的集合可以通过新的`for ... of`循环来遍历。
- 用`for ... of`循环遍历集合，用法如下：
    ```js
    var a = ['A', 'B', 'C'];
    var s = new Set(['A', 'B', 'C']);
    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    for (var x of a) { // 遍历Array
        console.log(x);
    }
    for (var x of s) { // 遍历Set
        console.log(x);
    }
    for (var x of m) { // 遍历Map
        console.log(x[0] + '=' + x[1]);
    }
    ```
- `for ... of`循环和`for ... in`循环有何区别？——`for ... in`循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个`Array`数组实际上也是一个对象，它的每个元素的索引被视为一个属性。
  - 当我们手动给Array对象添加了额外的属性后，for ... in循环将带来意想不到的意外效果：
    ```js
    var a = ['A', 'B', 'C'];
    a.name = 'Hello';
    for (var x in a) {
        console.log(x); // '0', '1', '2', 'name'
    }
    ```
  - `for ... in`循环将把`name`包括在内，但`Array`的`length`属性却不包括在内。
  - `for ... of`循环则完全修复了这些问题，它只循环集合本身的元素
- 更好的方式是直接使用`iterable`内置的`forEach`方法，它接收一个函数，每次迭代就自动回调该函数。以`Array`为例:
  ```js
  var a = ['A', 'B', 'C'];
  a.forEach(function (element, index, array) {
        // element: 指向当前元素的值
        // index: 指向当前索引
        // array: 指向Array对象本身
        console.log(element + ', index = ' + index);
    });
  ```

## 2 函数
### 2.1 函数的定义与调用
- 在JavaScript中，定义函数的方式如下：
  ```js
  function abs(x) {
        if (x >= 0) {
            return x;
        } else {
            return -x;
        }
    }
  ```
  * `function`指出这是一个函数定义；
  * `abs`是函数的名称；
  * `(x)`括号内列出函数的参数，多个参数以`,`分隔；
  * `{ ... }`之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句。
- 由于**JavaScript的函数也是一个对象**，上述定义的`abs()`函数实际上是一个函数对象，而函数名`abs`可以视为指向该函数的变量。
- 第二种定义函数的方式如下：
    ```js
    var abs = function (x) {
        if (x >= 0) {
            return x;
        } else {
            return -x;
        }
    };
    ```
  - 在这种方式下，`function (x) { ... }`是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量`abs`，所以，通过变量`abs`就可以调用该函数。
- 上述两种定义**完全等价**，注意**第二种方式按照完整语法需要在函数体末尾加一个`;`**，表示赋值语句结束。
- 调用函数时，按顺序传入参数即可：`abs(10); // 返回10`
  - 由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：`abs(10, 'blablabla'); // 返回10`
  - 传入的参数比定义的少也没有问题：`abs(); // 返回NaN ,此时abs(x)函数的参数x将收到undefined，计算结果为NaN`
- 如果想要避免函数收到undefined的参数，可以对参数进行检查：
  ```js
  function abs(x) {
        if (typeof x !== 'number') {
            throw 'Not a number';
        }
        if (x >= 0) {
            return x;
        } else {
            return -x;
        }
    }
  ```
- JavaScript还有一个免费赠送的关键字`arguments`，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。`arguments`类似`Array`但它不是一个`Array`：
    ```js
    function foo(x) {
        console.log('x = ' + x); // 10
        for (var i=0; i<arguments.length; i++) {
            console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
        }
    }
    foo(10, 20, 30);
    /**
     * x = 10
     * arg 0 = 10
     * arg 1 = 20
     * arg 2 = 30
     * /
    ```
  - 实际上arguments最常用于判断传入参数的个数。` if (arguments.length === 2) {...}`
- ES6标准引入了rest参数:
    ```js
    function foo(a, b, ...rest) {
        console.log('a = ' + a);
        console.log('b = ' + b);
        console.log(rest);
    }

    foo(1, 2, 3, 4, 5);
    // 结果:
    // a = 1
    // b = 2
    // Array [ 3, 4, 5 ]

    foo(1);
    // 结果:
    // a = 1
    // b = undefined
    // Array []
    ```
  - rest参数只能写在最后，前面用`...`标识，从运行结果可知，传入的参数先绑定`a`、`b`，多余的参数以数组形式交给变量`rest`，所以，不再需要`arguments`我们就获取了全部参数。
  - 如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是`undefined`）。

### 2.2 变量作用域与解构赋值
#### 2.2.1 局部变量
- 如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量【局部变量】
  - JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。
- JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：
  ```js
    function foo() {
        var x = 'Hello, ' + y;
        console.log(x);
        var y = 'Bob';
    }
    foo();
  ```
  - 对于上述`foo()`函数，JavaScript引擎看到的代码相当于：
    ```js
    function foo() {
        var y; // 提升变量y的申明，此时y为undefined
        var x = 'Hello, ' + y;
        console.log(x);
        y = 'Bob';
    }
    ```
    - 语句`var x = 'Hello, ' + y;`并**不报错**，原因是变量`y`在稍后申明了。但是`console.log`显示`Hello, undefined`，说明变量`y`的值为`undefined`。这正是因为**JavaScript引擎自动提升了变量`y`的声明，但不会提升变量`y`的赋值**。
    - 由于JavaScript的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个`var`申明函数内部用到的所有变量
#### 2.2.2 全局变量
- **不在任何函数内定义的变量就具有全局作用域**。实际上，JavaScript默认有一个全局对象`window`，全局作用域的变量实际上被绑定到`window`的一个属性:
  ```js
    var course = 'Learn JavaScript';
    alert(course); // 'Learn JavaScript'
    alert(window.course); // 'Learn JavaScript'
  ```
  - JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报`ReferenceError`错误。
- 全局变量会绑定到`window`上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：
  ```js
    // 唯一的全局变量MYAPP:
    var MYAPP = {};

    // 其他变量:
    MYAPP.name = 'myapp';
    MYAPP.version = 1.0;

    // 其他函数:
    MYAPP.foo = function () {
        return 'foo';
    };
  ```
    - 把自己的代码全部放入唯一的名字空间MYAPP中，会大大减少全局变量冲突的可能。许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。
#### 2.2.3 块级作用域与let关键字
- 由于JavaScript的变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量的：
    ```js
    function foo() {
        for (var i=0; i<100; i++) {
            //
        }
        i += 100; // 仍然可以引用变量i
    }
    ```
    - 为了解决块级作用域，ES6引入了新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量：
    ```js
    function foo() { 
        var sum = 0; 
        for (let i=0; i<100; i++) 
        {
             sum += i; 
        } // SyntaxError: i += 1; }
    ```
#### 2.2.4 常量
- ES6标准引入了新的关键字`const`来定义常量，`const`与`let`都具有块级作用域：
    ```js
    const PI = 3.14;
    PI = 3; // 某些浏览器不报错，但是无效果！
    PI; // 3.14
    ```
#### 2.2.5 解构赋值
- 从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。
- 传统的做法，把一个数组的元素分别赋值给几个变量：
  ```js
    var array = ['hello', 'JavaScript', 'ES6'];
    var x = array[0];
    var y = array[1];
    var z = array[2];
  ```
- 现在，在ES6中，可以使用解构赋值，直接对多个变量同时赋值：
  ```js
    var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
    let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
    let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
    var person = {
        name: '小明',
        age: 20,
        gender: 'male',
        passport: 'G-12345678',
        school: 'No.4 middle school'
    };
    var {name, age, passport} = person; // name, age, passport分别被赋值为对应属性
    var {name, single=true} = person; // 如果person对象没有single属性，默认赋值为true

  ```
  - 注意，对数组元素进行解构赋值时，多个变量要用[...]括起来。
- 解构赋值在很多时候可以大大简化代码。例如，交换两个变量x和y的值，可以这么写，不再需要临时变量：
  ```js
    var x=1, y=2;
    [x, y] = [y, x]
  ```
### 2.3 方法
- 在一个对象中绑定函数，称为这个对象的方法。
  ```js
    var xiaoming = {
        name: '小明',
        birth: 1990,
        age: function () {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
    };
    xiaoming.age; // function xiaoming.age()
    xiaoming.age(); // 今年调用是25,明年调用就变成26了
  ```
  - 在一个方法内部，`this`是一个特殊变量，它始终指向当前对象，也就是`xiaoming`这个变量。所以，`this.birth`可以拿到`xiaoming`的`birth`属性。
- 如果拆开写：
  ```js
    function getAge() {
        var y = new Date().getFullYear();
        return y - this.birth; // 该函数的this指向全局对象，也就是window
    }

    var xiaoming = {
        name: '小明',
        birth: 1990,
        age: getAge
    };

    xiaoming.age(); // 25, 正常结果
    getAge(); // NaN
  ```
  - 单独调用函数getAge()怎么返回了NaN
  - 如果以对象的方法形式调用，比如xiaoming.age()，该函数的this指向被调用的对象，也就是xiaoming，这是符合我们预期的。
  - 如果单独调用函数，比如getAge()，此时，该函数的this指向全局对象，也就是window。
- 要保证this指向正确，必须用obj.xxx()的形式调用:



## 参考资料
- [JavaScript教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1022910821149312)

