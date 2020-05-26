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

### 2.3 高阶函数
- JavaScript的函数其实都指向某个变量。既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为**高阶函数**。
- 编写高阶函数，就是让函数的参数能够接收别的函数。
- 一个最简单的高阶函数：
  ```js
    function add(x, y, f) {
        return f(x) + f(y);
    }
    var x = add(-5, 6, Math.abs); // 11
  ```
#### 2.3.1 Map
- `map()`方法定义在JavaScript的`Array`中，我们调用`Array`的`map()`方法，传入我们自己的函数，就得到了一个新的`Array`作为结果:
  ```js
    function pow(x) {
        return x * x;
    }
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
    var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
  ```
- 所以，`map()`作为高阶函数，事实上它把运算规则抽象了，因此，我们不但可以计算简单的f(x)=x2，还可以计算任意复杂的函数，比如，把`Array`的所有数字转为字符串

#### 2.3.2 Reduce
- Array的`reduce()`把一个函数作用在这个`Array`的`[x1, x2, x3...]`上，这个函数必须接收两个参数，`reduce()`把结果继续和序列的下一个元素做累积计算，其效果就是： `[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)`
- 比方说对一个`Array`求和，就可以用`reduce`实现：
  ```js
    var arr = [1, 3, 5, 7, 9];
    arr.reduce(function (x, y) {
        return x + y;
    }); // 25
  ```
- filter也是一个常用的操作，它用于把`Array`的某些元素过滤掉，然后返回剩下的元素。
- 和`map()`类似，`Array`的`filter()`也接收一个函数。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`true`还是`false`决定保留还是丢弃该元素。
- 例如，在一个`Array`中，删掉偶数，只保留奇数，可以这么写：
    ```js
    var arr = [1, 2, 4, 5, 6, 9, 10, 15];
    var r = arr.filter(function (x) {
        return x % 2 !== 0;
    });
    r; // [1, 5, 9, 15]
    ```
#### 2.3.3 sort
- 通常规定，对于两个元素`x`和`y`，如果认为`x < y`，则返回`-1`，如果认为`x == y`，则返回`0`，如果认为`x > y`，则返回`1`，这样，排序算法就不用关心具体的比较过程，而是根据比较结果直接排序。
- JavaScript的`Array`的`sort()`方法就是用于排序的
- **`Array`的`sort()`方法默认把所有元素先转换为String再排序**，结果`'10'`排在了`'2'`的前面，因为字符`'1'`比字符`'2'`的ASCII码小
- `sort()`方法也是一个高阶函数，它还可以接收一个比较函数来实现自定义的排序：
  ```js
    var arr = [10, 20, 1, 2];
    arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
    });
    console.log(arr); // [1, 2, 10, 20]
  ```

#### 2.3.4 其他高阶函数
- `every()`方法可以判断数组的所有元素是否满足测试条件。
- `find()`方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回`undefined`
- `findIndex()`和`find()`类似，也是查找符合条件的第一个元素，不同之处在于`findIndex()`会返回这个元素的索引，如果没有找到，返回`-1`
- `forEach()`和`map()`类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。`forEach()`常用于遍历数组，因此，传入的函数不需要返回值
    ```js
    var arr = ['Apple', 'pear', 'orange'];
    arr.forEach(console.log); // 依次打印每个元素
    ```

#### 2.3.5 箭头函数
- ES6标准新增了一种新的函数：**Arrow Function（箭头函数）**。
  ```js
    x => x * x
    // 相当于
    function (x) {
        return x * x;
    }
  ```
- 箭头函数相当于匿名函数，并且简化了函数定义。箭头函数有两种格式，一种像上面的，只包含一个表达式，连`{ ... }`和`return`都省略掉了。还有一种可以包含多条语句，这时候就不能省略`{ ... }`和`return`：
  ```js
    x => {
        if (x > 0) {
            return x * x;
        }
        else {
            return - x * x;
        }
    }
  ```

#### 2.3.6 generator
- generator（生成器）是ES6标准引入的新的数据类型。一个generator看上去像一个函数，但可以返回多次。
- generator和函数不同的是，generator由`function*`定义（注意多出的`*`号），并且，除了`return`语句，还可以用`yield`返回多次:
  ```js
    function* foo(x) {
        yield x + 1;
        yield x + 2;
        return x + 3;
    }
  ```
- 例如要编写一个产生斐波那契数列的函数，可以这么写：
    ```js
    function fib(max) {
        var
            t,
            a = 0,
            b = 1,
            arr = [0, 1];
        while (arr.length < max) {
            [a, b] = [b, a + b];
            arr.push(b);
        }
        return arr;
    }

    // 测试:
    fib(5); // [0, 1, 1, 2, 3]
    fib(10); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
    ```
- 函数只能返回一次，所以必须返回一个Array。但是，如果换成generator，就可以一次返回一个数，不断返回多次。用generator改写如下：
  ```js
    function* fib(max) {
        var
            t,
            a = 0,
            b = 1,
            n = 0;
        while (n < max) {
            yield a;
            [a, b] = [b, a + b];
            n ++;
        }
        return;
    }
    fib(5); // fib {[[GeneratorStatus]]: "suspended", [[GeneratorReceiver]]: Window}
    // 直接调用一个generator和调用函数不一样，fib(5)仅仅是创建了一个generator对象，还没有去执行它。
    var f = fib(5);
    f.next(); // {value: 0, done: false}
    f.next(); // {value: 1, done: false}
    f.next(); // {value: 1, done: false}
    f.next(); // {value: 2, done: false}
    f.next(); // {value: 3, done: false}
    f.next(); // {value: undefined, done: true}
  ```
- `next()`方法会执行generator的代码，然后，每次遇到`yield x;`就返回一个对象`{value: x, done: true/false}`，然后“暂停”。返回的`value`就是`yield`的返回值，`done`表示这个generator是否已经执行结束了。如果`done`为`true`，则`value`就是`return`的返回值。
- 当执行到`done`为`true`时，这个generator对象就已经全部执行完毕，不要再继续调用`next()`了。
- 第二个方法是直接用`for ... of`循环迭代generator对象，这种方式不需要我们自己判断`done`：
  ```js
    for (var x of fib(10)) {
        console.log(x); // 依次输出0, 1, 1, 2, 3, ...
    }
  ```
- 因为generator可以在执行过程中多次返回，所以它看上去就像一个可以记住执行状态的函数，利用这一点，写一个generator就可以实现需要用面向对象才能实现的功能

## 3 标准对象
- 在JavaScript的世界里，一切都是对象。
- 为了区分对象的类型，我们用`typeof`操作符获取对象的类型，它总是返回一个字符串
- `number`、`string`、`boolean`、`function`和`undefined`有别于其他类型。特别注意`null`的类型是`object`，`Array`的类型也是`object`，如果我们用`typeof`将无法区分出`null`、`Array`和通常意义上的object——`{}`。
- `number`、`boolean`和`string`都有包装对象。没错，在JavaScript中，字符串也区分`string`类型和它的包装类型。包装对象用`new`创建:
  ```js
    typeof new Number(123); // 'object'
    new Number(123) === 123; // false

    typeof new Boolean(true); // 'object'
    new Boolean(true) === true; // false

    typeof new String('str'); // 'object'
    new String('str') === 'str'; // false
  ```

### 3.1 Date
- 在JavaScript中，Date对象用来表示日期和时间:
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

### 3.2 JSON
- **JSON是JavaScript Object Notation的缩写**，它是一种数据交换格式。
- 在JSON出现之前，大家一直用XML来传递数据。因为XML是一种纯文本格式，所以它适合在网络上交换数据。XML本身不算复杂，但是，加上DTD、XSD、XPath、XSLT等一大堆复杂的规范以后，任何正常的软件开发人员碰到XML都会感觉头大了，最后大家发现，即使你努力钻研几个月，也未必搞得清楚XML的规范。
- 终于，在2002年的一天，道格拉斯·克罗克福特（Douglas Crockford）同学为了拯救深陷水深火热同时又被某几个巨型软件企业长期愚弄的软件工程师，发明了JSON这种超轻量级的数据交换格式。
- 在JSON中，一共就这么几种数据类型：
  * `number`：和JavaScript的`number`完全一致；
  * `boolean`：就是JavaScript的`true`或`false`；
  * `string`：就是JavaScript的`string`；
  * `null`：就是JavaScript的`null`；
  * `array`：就是JavaScript的`Array`表示方式——`[]`；
  * `object`：就是JavaScript的`{ ... }`表示方式。
  * 以及上面的任意组合。
- JSON还定死了字符集必须是UTF-8
- JSON的字符串规定必须用双引号""，Object的键也必须用双引号""
- 对象**序列化**成JSON格式的字符串：
  ```js
    var xiaoming = {
        name: '小明',
        age: 14,
        gender: true,
        height: 1.65,
        grade: null,
        'middle-school': '\"W3C\" Middle School',
        skills: ['JavaScript', 'Java', 'Python', 'Lisp']
    };
    var s = JSON.stringify(xiaoming);
    console.log(s);
    // {"name":"小明","age":14,"gender":true,"height":1.65,"grade":null,"middle-school":"\"W3C\" Middle School","skills":["JavaScript","Java","Python","Lisp"]}
  ```
- 如果我们还想要精确控制如何序列化小明，可以给`xiaoming`定义一个`toJSON()`的方法，直接返回JSON应该序列化的数据：
    ```js
        var xiaoming = {
        name: '小明',
        age: 14,
        gender: true,
        height: 1.65,
        grade: null,
        'middle-school': '\"W3C\" Middle School',
        skills: ['JavaScript', 'Java', 'Python', 'Lisp'],
        toJSON: function () {
            return { // 只输出name和age，并且改变了key：
                'Name': this.name,
                'Age': this.age
            };
        }
    };
    JSON.stringify(xiaoming); // '{"Name":"小明","Age":14}'
    ```
- **反序列化**：拿到一个JSON格式的字符串，我们直接用JSON.parse()把它变成一个JavaScript对象
  ```js
    JSON.parse('[1,2,3,true]'); // [1, 2, 3, true]
    JSON.parse('{"name":"小明","age":14}'); // Object {name: '小明', age: 14}
    JSON.parse('true'); // true
    JSON.parse('123.45'); // 123.45
  ```

## 4 面向对象编程
![wDRfHk](https://gitee.com/pxqp9W/testmarkdown/raw/master/imgs/2020/05/wDRfHk.jpg)
- **JavaScript不区分类和实例的概念**，而是通过**原型（prototype）**来实现面向对象编程。
  ```js
    var Student = {
        name: 'Robot',
        height: 1.2,
        run: function () {
            console.log(this.name + ' is running...');
        }
    };

    var xiaoming = {
        name: '小明'
    };

    xiaoming.__proto__ = Student;

    xiaoming.name; // '小明'
    xiaoming.run(); // 小明 is running...
    ```
    - 注意最后一行代码把xiaoming的原型指向了对象Student，看上去xiaoming仿佛是从Student继承下来的：
    - xiaoming有自己的name属性，但并没有定义run()方法。不过，由于小明是从Student继承而来，只要Student有run()方法，xiaoming也可以调用
- **JavaScript的原型链和Java的Class区别就在，它没有“Class”的概念**，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已。
- `Object.create()`方法可以传入一个原型对象，并创建一个基于该原型的新对象，但是新对象什么属性都没有，因此，我们可以编写一个函数来创建`xiaoming`：
    ```js
    // 原型对象:
    var Student = {
        name: 'Robot',
        height: 1.2,
        run: function () {
            console.log(this.name + ' is running...');
        }
    };

    function createStudent(name) {
        // 基于Student原型创建一个新对象:
        var s = Object.create(Student);
        // 初始化新对象:
        s.name = name;
        return s;
    }

    var xiaoming = createStudent('小明');
    xiaoming.run(); // 小明 is running...
    xiaoming.__proto__ === Student; // true
    ```

### 4.1 创建对象
- JavaScript对每个创建的对象都会设置一个原型，指向它的原型对象。
- 当我们用`obj.xxx`访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到`Object.prototype`对象，最后，如果还没有找到，就只能返回`undefined`。
    - 例如，Arrays的原型链是：`arr ----> Array.prototype ----> Object.prototype ----> null``Array.prototype`定义了`indexOf()`、`shift()`等方法，因此你可以在所有的`Array`对象上直接调用这些方法。
    - 函数也是一个对象，它的原型链是：`foo ----> Function.prototype ----> Object.prototype ----> null`
- 除了直接用`{ ... }`创建一个对象外，JavaScript还可以用一种**构造函数**的方法来创建对象。它的用法是，先定义一个构造函数：
    ```js
    function Student(name) {
        this.name = name;
        this.hello = function () {
            alert('Hello, ' + this.name + '!');
        }
    }

    var xiaoming = new Student('小明');
    xiaoming.name; // '小明'
    xiaoming.hello(); // Hello, 小明!
    ```
    - 这确实是一个普通函数，但是在JavaScript中，可以用关键字`new`来调用这个函数，并返回一个对象
    - 如果不写`new`，这就是一个普通函数，它返回`undefined`。但是，如果写了`new`，它就变成了一个构造函数，它绑定的`this`指向新创建的对象，并默认返回`this`，也就是说，不需要在最后写`return this;`。
    - 用`new Student()`创建的对象还从原型上获得了一个`constructor`属性，它指向函数`Student`本身：![https://static.liaoxuefeng.com/files/attachments/1024698721053600/l](https://static.liaoxuefeng.com/files/attachments/1024698721053600/l)
        ```js
        xiaoming.constructor === Student.prototype.constructor; // true
        Student.prototype.constructor === Student; // true
        Object.getPrototypeOf(xiaoming) === Student.prototype; // true
        xiaoming instanceof Student; // true
        ```
    - 为了区分普通函数和构造函数，**按照约定，构造函数首字母应当大写，而普通函数首字母应当小写**，这样，一些语法检查工具如jslint将可以帮你检测到漏写的new。

### 4.2 原型继承
- 在传统的基于Class的语言如Java、C++中，继承的本质是扩展一个已有的Class，并生成新的Subclass。由于这类语言严格区分类和实例，继承实际上是类型的扩展。
- 但是，JavaScript由于采用原型继承，我们无法直接扩展一个Class，因为根本不存在Class这种类型。
- 例如：我们要基于Student扩展出PrimaryStudent，可以先定义出PrimaryStudent：
    ```js
    function PrimaryStudent(props) {
        // 调用Student构造函数，绑定this变量:
        Student.call(this, props);
        this.grade = props.grade || 1;
    }
    ```
- 必须想办法把原型链修改为：`new PrimaryStudent() ----> PrimaryStudent.prototype ----> Student.prototype ----> Object.prototype ----> null`
- 为了实现这一点，参考道爷（就是发明JSON的那个道格拉斯）的代码，中间对象可以用一个空函数F来实现：![https://static.liaoxuefeng.com/files/attachments/1034288859918112/l](https://static.liaoxuefeng.com/files/attachments/1034288859918112/l)
    ```js
    // PrimaryStudent构造函数:
    function PrimaryStudent(props) {
        Student.call(this, props);
        this.grade = props.grade || 1;
    }

    // 空函数F:
    function F() {
    }

    // 把F的原型指向Student.prototype:
    F.prototype = Student.prototype;

    // 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
    PrimaryStudent.prototype = new F();

    // 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
    PrimaryStudent.prototype.constructor = PrimaryStudent;

    // 继续在PrimaryStudent原型（就是new F()对象）上定义方法：
    PrimaryStudent.prototype.getGrade = function () {
        return this.grade;
    };

    // 创建xiaoming:
    var xiaoming = new PrimaryStudent({
        name: '小明',
        grade: 2
    });
    xiaoming.name; // '小明'
    xiaoming.grade; // 2

    // 验证原型:
    xiaoming.__proto__ === PrimaryStudent.prototype; // true
    xiaoming.__proto__.__proto__ === Student.prototype; // true

    // 验证继承关系:
    xiaoming instanceof PrimaryStudent; // true
    xiaoming instanceof Student; // true
    ```
- JavaScript的原型继承实现方式就是：

  1. 定义新的构造函数，并在内部用`call()`调用希望“继承”的构造函数，并绑定`this`；

  2. 借助中间函数`F`实现原型链继承，最好通过封装的`inherits`函数完成；

  3. 继续在新的构造函数的原型上定义新方法。



### 4.3 class继承
- 新的关键字`class`从ES6开始正式被引入到JavaScript中。`class`的目的就是让定义类更简单。
- 如果用新的`class`关键字来编写`Student`，可以这样写：
    ```js
    class Student {
        constructor(name) {
            this.name = name;
        }

        hello() {
            alert('Hello, ' + this.name + '!');
        }
    }
    var xiaoming = new Student('小明');
    xiaoming.hello();
    ```
    - 比较一下就可以发现，`class`的定义包含了构造函数`constructor`和定义在原型对象上的函数`hello()`（注意没有`function`关键字），这样就避免了`Student.prototype.hello = function () {...}`这样分散的代码。
- 用`class`定义对象的另一个巨大的好处是继承更方便了。想一想我们从`Student`派生一个`PrimaryStudent`需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需要考虑了，直接通过`extends`来实现：
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
- 不是所有的主流浏览器都支持ES6的class。如果一定要现在就用上，就需要一个工具把`class`代码转换为传统的`prototype`代码，可以试试[Babel](https://babeljs.io/)这个工具。

## 5 浏览器

### 5.1 浏览器对象
- `window`对象不但充当全局作用域，而且表示浏览器窗口。
    - `window`对象有`innerWidth`和`innerHeight`属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。
- `navigator`对象表示浏览器的信息，最常用的属性包括：
  * `navigator.appName`：浏览器名称；
  * `navigator.appVersion`：浏览器版本；
  * `navigator.language`：浏览器设置的语言；
  * `navigator.platform`：操作系统类型；
  * `navigator.userAgent`：浏览器设定的`User-Agent`字符串。
- `screen`对象表示屏幕的信息，常用的属性有：
  * `screen.width`：屏幕宽度，以像素为单位；
  * `screen.height`：屏幕高度，以像素为单位；
  * `screen.colorDepth`：返回颜色位数，如8、16、24。
- `location`对象表示当前页面的URL信息。例如，一个完整的URL：`http://www.example.com:8080/path/index.html?a=1&b=2#TOP`
- `document`对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document`对象就是整个DOM树的根节点。
    - 用`document`对象提供的`getElementById()`和`getElementsByTagName()`可以按ID获得一个DOM节点和按Tag名称获得一组DOM节
    - `document`对象还有一个`cookie`属性，可以获取当前页面的Cookie。
- `history`对象保存了浏览器的历史记录，JavaScript可以调用`history`对象的`back()`或`forward ()`，相当于用户点击了浏览器的“后退”或“前进”按钮。
    - 这个对象属于历史遗留对象，对于现代Web页面来说，由于大量使用AJAX和页面交互，简单粗暴地调用`history.back()`可能会让用户感到非常愤怒。新手开始设计Web页面时喜欢在登录页登录成功时调用`history.back()`，试图回到登录前的页面。这是一种错误的方法。
    - 任何情况，你都不应该使用`history`这个对象了。





## 参考资料
- [JavaScript教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1022910821149312)

