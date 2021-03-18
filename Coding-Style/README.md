# Coding Style

## JavaScript 开发规范

- 尽量使用 `let` 或 `const` 来声明变量，避免使用 `var`：

    `var` 声明的作用域时函数作用域，而 `let` 是块作用域。针对常量需要使用 `const` 来声明，可以防止常量被修改。

- 声明对象或数组时最好使用字面量语法去创建：

    ``` javascript
    // Create a object
    const obj = {};

    // Create an array
    const arr = [];
    ```

- 获取对象属性值时，最好使用点语法：

    在获取对象属性值时优先使用 `object.key` 的方式去获取，但是当使用变量作为对象的 `key` 时，需要使用 `object[key]` 的方式。

- 在对数组尾部新增数据时，尽量使用数组对象原生的方法：

    ```javascript
    const arr = [];

    // bad
    arr[arr.length] = 'val';

    // good
    arr.push('val');
    ```

- 进行数组或对象操作时，尽量使用扩展运算符：

    扩展运算符是 ES6 新增的内容，其可以在函数调用或数组构造时，将数组在语法层面展开，并在构造字面量对象的时候将对象按照 `key-value` 的方式展开。其常见用途如下：

    ```javascript
    // 数组的复制
    const arr1 = [1,2,3,4];
    const arr2 = [...arr1]; // [1,2,3,4]

    // 数组的合并
    const arr1 = [1,2,3];
    const arr2 = [4,5,6];
    const merge = [...arr1,...arr2]; // [1,2,3,4,5,6]

    // 字符串转数组
    let arr = [..."hello"]; // ['h','e','l','l','o']
    ```

- 访问和使用对象的多个属性时使用对象分解：

    当需要访问一个对象里的多个属性时，我们可以使用 `let {key1, key2} = obj`，此时 `key1` 和 `key2` 的值分别为 `obj.key1` 和 `obj.key2`。

    ```javascript
    // bad
    function getFullName(user) {
      let firstName = user.firstName;
      let lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

    数组也可以使用类似的方法，如 `let [first, second] = arr`，此时 `first` 和 `second` 的值为 `arr[0]` 和 `arr[1]`。

- 在函数需要返回多个数据时，使用对象进行包裹而不是数组：

    JavaScript 的函数中只能返回一个参数，当有多条参数需要返回时，我们可以使用一个对象将参数值包含起来返回，如下所示：

    ``` javascript
    function multipleOutput(input) {
      // .....
      return {x,y,z};
    }

    const {x,z} = multipleOutput(input);
    ```

    返回对象相较于返回数组来说，它可以让函数调用者按照需求来获取返回值。

- 声明字符串时最好使用单引号：

    ```javascript
    // bad
    const str = "hello world";

    // good
    const str = 'hello world';
    ```

- 在合并字符串是尽量使用模板字符串的插值操作：

    在进行字符串合并的时候尽量不要使用 `+` 运算符，尽量使用模板字符串的插值操作，如下所示：

    ```javascript
    const name = 'lasia';

    // bad
    let greeting = 'my name is' + name + '!';

    // good
    let greeting = `my name is ${name}!`;
    ```

- 不要将函数的参数命名为 `arguments`：

    在命名函数参数时，不能将其参数名命名为 `arguments`，这将会与函数中的 `arguments` 对象产生歧义。

    ```javascript
    // bad
    function foo(name, options, argument){
      // ...
    }

    // good
    function foo(name, options, args){
      // ...
    }
    ```

- 将默认参数放在最后：

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

- 当需要使用匿名函数时，尽量使用箭头函数表达式：

    ```javascript
    let arr = [1,2,3];

    // bad
    arr.map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    arr.map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

- 在使用箭头函数表达式的时候，函数体内只有一个表达式时，可以直接省略 `return` 来进行返回值的操作：

    ```javascript
    let arr = [1,2,3];

    // bad
    arr.map((x) => {
      return x * x;
    });

    // good
    arr.map((x) => x * x);
    ```

- 在使用箭头函数的时候，无论有几个参数都需要使用小括号将参数包裹起来：

    ```javascript
    let arr = [1,2,3];

    // bad
    arr.map(x => x * x);

    // good
    arr.map((x) => x * x);
    ```

- 避免嵌套赋值变量：

    在对变量进行赋值时，需要对变量分开赋值，即使多个变量数值相同。

    ```javascript
    // bad
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1 (b and c is global valuable)
    console.log(c); // 1

    // good
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError
    ```

- 尽量避免使用一元增量或减量：

    在执行一个变量自增或自减操作时，尽量避免使用 `i++` 操作，尽量用 `i += 1` 进行替代。

    ```javascript
    let i = 1;
    // bad
    i++;
    i--;

    // good
    i += 1;
    i -= 1;
    ```

- 使用全等于而不是等于：

    在比较两个变量是否相等的时候避免使用双等号操作符，尽量使用全等号，如下所示：

    ```javascript
    const a = 1;
    const b = 1;

    // bad
    a == b;
    a != b;

    // good
    a === b;
    a !== b;
    ```

- 在 `if` 语句中的条件表达式中，不同类型的变量有不同的表示方法：

    ```javascript
    // 布尔值
    if (bool) {
      // .. .
    }

    // 字符串
    if (str !== '') {
      // ....
    }

    // 数组
    if (arr.length > 0) {
      // ...
    }
    ```

- 在使用 `switch` 时使用时中括号表明代码块：

    JavaScript 中，`switch` 语句中的 `case` 决定了每个不同条件所执行的代码，默认情况下 `case` 后不需要中括号来限定所执行代码区域，而是用缩进来区分。所以在使用 `switch` 时需要在 `case` 后加上中括号用以限定。如下：

    ```javascript
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        let x = 2;
        break;
      }
      default: {
        let x = 10;
      }
    }
    ```

- 在使用三元表达式时避免多重嵌套：

    三元表达式可以简写特定的 `if` 语句，但是不可以嵌套编写。如下所示：

    ```javascript
    // bad
    const foo = maybe1 > maybe2 ? "bar" : val1 > val2 ? val2 : val1;

    // good
    const min = val1 > vla2 ? val2 : val2;

    const foo = maybe1 > maybe2 ? "bar" : min;
    ```

    在遇到需要嵌套的三元表达式时，可以将其从后向前进行拆分。

- 避免非必须的三元表达式：

    三元表达式可以很方便的进行部分 `if` 语句判断，但是在一些非必须的情况下，尽量避免使用三元表达式，如下：

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? false : true;

    // good
    const foo = a || b;
    const bar = !c;
    ```

- 进行幂次方运算时，避免使用 `Math.pow()` 方法：

    JavaScript中的 `Math` 库提供了多种的运算方法，但是，在进行简单的幂次运算，如取平方时，可以使用 `let y = x ** 2` 的方式去代替 `let y = Math.pow(x,2)`。

- 在使用 `if else` 语句时，需要注意将 `else` 语句放在 `if` 语句的结尾括号的同一行：

    ```javascript
    // bad
    if (condition) {
      // ...
    }
    else {
      // ....
    }

    // good
    if (condition) {
      // ...
    } else {
      // ..
    }

- 若 `if` 语句代码块中有 `return` 操作时， `else` 关键字可以省略：

    ```javascript
    // bad
    function test() {
      if (condition) {
        return x;
      } else {
        return y;
      }
    }


    // good
    function test() {
      if (condition) {
        return x;
      }
        return y;
      }
    ```

- 多行注释使用 `/** ... */` 进行注释：

    ```javascript
    // bad
    // make() return a element
    // based on class name

    // good
    /**
     * make() return a element
     * based on class name
     */
    ```

- 代码缩进使用 2 个空格：

    在编写代码时，代码的缩进空格数应该为 2 个，而不是 4 个，同时使用空格进行缩进，不能使用 `tab`。

- `if` 后的括号需要左右空格，同时运算操作符左右需要空格，函数的大括号需要空格：

    ```javascript
    // if statement
    if (condition) {
      // ...
    }

    // operator
    let x = 1 + 2 * 3;

    // function
    function test() {
      // ...
    }
    ```

- 在代码块之后，下一个语句之前保留一个空行：

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;
    ```

- 函数名称和括号不要有空格：

    ```javascript
    // bad
    function test () {
      // ...
    }

    // good
    function test() {
      // ...
    }
    ```

- 声明对象时也需要注意空格的使用：

    ``` javascript
    // bad
    var obj = { foo : 12 };

    // good
    var obj = { foo: 12 };
    ```

- 数组以及对象的初始化时，需要注意逗号的标注：

    ```javascript
    // bad
    const obj = {
      firstName: 'Dana',
      lastName: 'Lucy'
    };

    const arr = [
      'Dana',
      'Lucy'
    ];

    // good
    const obj = {
      firstName: 'Dana',
      lastName: 'Lucy',
    };

    const arr = [
      'Dana',
      'Lucy',
    ];
    ```

- 将字符转换为数字时，需注意 `Number` 和 `parseInt` 方法的使用格式：

    ```javascript
    const input = '2';

    // bad
    const val = new Number(input);

    // bad
    const val = parseInt(input);

    // good
    const val = Number(input);

    // good
    const val = parseInt(input, 10);
    ```

- 定义变量时，需要一一定义，但是需要注意代码的有序性，不能随便定义：

    ```javascript
    // bad
    const obj = {};
    let val = 2;
    let name = 'lasia';
    var id = 1;
    const arr = [];
    var age = 20;

    // good
    const obj = {};
    const arr = [];
    let val = 2;
    let name = 'lasia';
    var id = 1;
    var age = 20;
    ```

- 尽量不要使用如 `var that = this` 的方式去延长作用域：

    在函数内部涉及到另一个函数时，往往会出现 `this` 指向的问题，尽量不要使用 `var that = this` 的写法，而应该用箭头函数去替代，因为箭头函数的 `this` 指向调用它的对象。

    ```javascript
    // bad
    function foo() {
      var that = this;
      return function() {
        console.log(that);
      }
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      }
    }
    ```

## 名命规范

- 避免使用单字符进行名命，名命需要有描述性：

    ```javascript
    // bad
    function q() {
      // query something
    }

    // good
    function query() {
      // ...
    }
    ```

- 使用驼峰名命的方式进行名命：

    ```javascript
    // bad
    const OBJect = {};
    const person_name_obj = {};

    // good
    const object = {};
    const personNameObj = {};
    ```

- 只在为类或构造函数名命时才使用大写字母：

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name:'lasia',
    });
    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }
    const good = new User({
      name: 'lasia',
    });
    ```

- 在给变量名命时避免使用下划线进行名命：

    在名命变量时尽量使用全字母名命，如 `firstName`，而不要使用 `_firstName_` 等方法。

- 总结：

    在对变量或函数名命时，我们需要注意这个名称是否能够准确的描述这个变量或函数的用途，如对函数名命时使用动词加名词，使用形容词加名词的方式去名命类，常量名全部大写。

    在命名时不用太过担心变量名的长度问题，我们应该追求的是变量名能够精准的表达出变量及方法的含义。同时，我们也需要在变量名中注意正确的语法时态，比如 `pushMessagesCount` 就不如写成 `pushedMessagesCount`。

    在表述用途的同时也要注意防止一些不必要的表达出现，造成变量名冗长。
