# Taboola CSD JavaScript Style Guide() {

*Based on the [Airbnb Style Guide](https://github.com/airbnb/javascript), but IE compatible.*


## Table of Contents

  1. [Types](#types)
  2. [References](#references)
  3. [Objects](#objects)
  4. [Arrays](#arrays)
  5. [Destructuring](#destructuring)
  6. [Strings](#strings)
  7. [Functions](#functions)
  8. [Iterators](#iterators)
  9. [Properties](#properties)
  10. [Variables](#variables)
  11. [Hoisting](#hoisting)
  12. [Comparison Operators & Equality](#comparison-operators--equality)
  13. [Blocks](#blocks)
  14. [Control Statements](#control-statements)
  15. [Comments](#comments)
  16. [Whitespace](#whitespace)
  17. [Commas](#commas)
  18. [Semicolons](#semicolons)
  19. [Type Casting & Coercion](#type-casting--coercion)
  20. [Naming Conventions](#naming-conventions)
  21. [The DOM](#the-dom)
  22. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)

## Types

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Primitives**: When you access a primitive type you work directly on its value.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - Symbols cannot be faithfully polyfilled, so they should not be used when targeting browsers/environments that don’t support them natively.

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **Complex**: When you access a complex type you work on a reference to its value.

    - `object`
    - `array`
    - `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  <a name="references--prefer-var"></a><a name="2.1"></a>
  - [2.1](#references--prefer-var) Use `var` for all of your references. We support older browsers and therefore cannot use ES6 syntax.

    ```javascript
    // bad
    const a = 1;
    let b = 2;

    // good
    var a = 1;
    var b = 2;
    ```

  <a name="references--function-scope"></a><a name="2.2"></a>
  - [2.2](#references--function-scope) Note that `var` is function-scoped, not block-scoped. Read [here](https://blog.usejournal.com/a-guide-to-const-let-and-var-for-those-of-us-who-still-might-be-confused-1fc68eb5ff32) for more detail.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError

    
    // var exists in its current execution context.
    (function () {
    {
        var hello = 'Hi there!'
    }

    console.log(hello); // 'Hi there!'
    })()

    console.log(hello); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  <a name="no-es6-computed-properties"></a><a name="3.2"></a>
  - [3.2](#no-es6-computed-properties) Do not use computed property names when creating objects with dynamic property names.

    > Why? These are not supported in Internet Explorer.

    ```javascript

    function getKey(k) {
      return 'a key named ' + k;
    }

    // bad
    var obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };

    // good
    var obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;
    ```

  <a name="no-es6-object-shorthand"></a><a name="3.3"></a>
  - [3.3](#no-es6-object-shorthand) Do not use object method shorthand. This is part of ES6 and is not supported in IE.

    ```javascript
    // bad
    var atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    // good
    var atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    ```

  <a name="no-es6-object-concise"></a><a name="3.4"></a>
  - [3.4](#no-es6-object-concise) Do not use property value shorthand. 

    > Why? ES6!

    ```javascript
    var lukeSkywalker = 'Luke Skywalker';

    // bad
    var obj = {
      lukeSkywalker,
    };

    // good
    var obj = {
      lukeSkywalker: lukeSkywalker,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.5"></a>
  - [3.5](#objects--quoted-props) Only quote properties that are invalid identifiers.

    > Why? In general it is subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

    ```javascript
    // bad
    var bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    var good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a><a name="3.6"></a>
  - [3.6](#objects--prototype-builtins) Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`.

    > Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`).

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    var has = Object.prototype.hasOwnProperty; // cache the lookup once, in function scope.
    console.log(has.call(object, key));
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Use the literal syntax for array creation.

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

    ```javascript
    var someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="no-es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#no-es6-array-spreads) Do not use array spreads `...` to copy arrays. Use Array.prototype.slice instead

    ```javascript
    // bad
    var itemsCopy = [...items];

    // also bad
    var len = items.length;
    var itemsCopy = [];
    var i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // good
    var itemsCopy = items.slice(0);
    ```

  <a name="arrays--callback-return"></a><a name="4.4"></a>
  - [4.4](#arrays--callback-return) Use return statements in array method callbacks. Do not use arrow functions.

    ```javascript
    // good
    [1, 2, 3].map(function (x) {
      var y = x + 1;
      return x * y;
    });

    // bad
    [1, 2, 3].map((x) => x + 1)

    // bad - no returned value means `acc` becomes undefined after the first iteration
    [[0, 1], [2, 3], [4, 5]].reduce(function (acc, item, index) {
      var flatten = acc.concat(item);
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce(function (acc, item, index) {
      var flatten = acc.concat(item);
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      var { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter(function(msg) {
      var subject = msg.subject;
      var author = msg.author;

      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a><a name="4.5"></a>
  - [4.5](#arrays--bracket-newline) Use line breaks after open and before close array brackets if an array has multiple lines

    ```javascript
    // bad
    var arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    var objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    var numberInArray = [
      1, 2,
    ];

    // good
    var arr = [[0, 1], [2, 3], [4, 5]];

    var objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    var numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  <a name="no-destructuring--object"></a><a name="5.1"></a>
  - [5.1](#no-destructuring--object) Do not use object destructuring when accessing and using properties of an object.

    > Why? Not supported in IE.

    ```javascript
    // bad
    function getFullName(user) {
      var { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // bad
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      var firstName = user.firstName;
      var lastName = user.lastName;

      return firstName + ' ' + lastName;
    }
    ```

  <a name="no-destructuring--array"></a><a name="5.2"></a>
  - [5.2](#no-destructuring--array) Do not use array destructuring.

    ```javascript
    var arr = [1, 2, 3, 4];

    // bad
    var [first, second] = arr;

    // good
    var first = arr[0];
    var second = arr[1];
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) Use single quotes `''` for strings. Only escape when necessary

    ```javascript
    // bad
    var name = "Capt. Janeway";

    // bad - template literals not supported in IE
    var name = `Capt. Janeway`;

    // good
    var name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

    > Why? Broken strings are painful to work with and make code less searchable.

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) When programmatically building up strings, use concatenation instead of template strings.

    > Why? Template strings are not supported in IE.

    ```javascript
    // bad
    function sayHi(name) {
        return `How are you, ${name}?`;
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) Never use `eval()` on a string, it opens too many vulnerabilities.

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) Do not unnecessarily escape characters in strings.

    > Why? Backslashes harm readability, thus they should only be present when necessary.

    ```javascript
    // bad
    var foo = '\'this\' \i\s \"quoted\"';

    // very bad (remember ALL of our javascript is a string)
    var foo = \"'this' is \"quoted\"\" 

    // good 
    var foo = '\'this\' is \"quoted\"';
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) Use named function expressions instead of function declarations.

    > Why? Function declarations inside of try-catch blocks can break in Safari. Also, function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. 

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // good
    var foo = function () {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) Wrap immediately invoked function expressions in parentheses.

    > Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) Never declare a function in a non-function block (`if`, `while`, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement.

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
      test = function() {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="functions--signature-spacing"></a><a name="7.6"></a>
  - [7.6](#functions--signature-spacing) Spacing in a function signature.

    > Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name.

    ```javascript
    // bad
    var f = function(){};
    var g = function (){};
    var h = function() {};

    // good
    var x = function () {};
    var y = function a() {};
    ```

<a name="functions--reassign-params"></a><a name="7.7"></a>
  - [7.7](#functions--reassign-params) Never reassign parameters.

    > Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

    ```javascript
    // bad
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // good
    function f3(a) {
      var b = a || 1;
      // ...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators

  <a name="iterators--nope"></a><a name="8.1"></a>
  - [8.1](#iterators--nope) Don’t use iterators if you don't have to. Prefer JavaScript’s higher-order functions instead of loops like `for-in` or `for-of`.

    > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects.

    > Use `forEach()` / `map()` / `every()` / `filter()` / `reduce()` / `some()` / ... to iterate over arrays, and `Object.keys()` to produce arrays so you can iterate over objects.

    ```javascript
    var numbers = [1, 2, 3, 4, 5];

    // bad
    var sum = 0;
    for (var num of numbers) {
      sum += num;
    }
    sum === 15;

    // good
    var sum = 0;
    numbers.forEach(function (num) {
      sum += num;
    });
    sum === 15;

    // best (use the functional force)
    var sum = numbers.reduce(function (total, num) {
        return total + num
    }, 0);
    sum === 15;

    // bad
    var increasedByOne = [];
    for (var i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // good
    var increasedByOne = [];
    numbers.forEach(function (num) {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    var increasedByOne = numbers.map(function (num) { 
        return num + 1 
    });
    ```


## Properties

  <a name="properties--dot"></a><a name="9.1"></a>
  - [9.1](#properties--dot) Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28,
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="9.2"></a>
  - [9.2](#properties--bracket) Use bracket notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```
  
  <a name="properties--nested"></a><a name="9.3"></a>
  - [9.3](#properties--nested) Use short-circuiting with the `&&` operator to check for undefined values.

    ```javascript
    // bad - assumes that each of those properties exists -> TypeError
    if (TRCImpl.feedsManager.configs['below-article-thumbs_ARC'].fti === 'tribunedigital-chicagotribune-feed-action-bucket-1569860912943') {
      // some code
    }

    // good - won't throw an error
    if (
        TRCImpl
        && TRCImpl.feedsManager 
        && TRCImpl.feedsManager.configs 
        && TRCImpl.feedsManager.configs['below-article-thumbs_ARC'] 
        && TRCImpl.feedsManager.configs['below-article-thumbs_ARC'].fti === 'tribunedigital-chicagotribune-feed-action-bucket-1569860912943'
    ) {
      // some code
    }
    ```

## Variables

  <a name="variables--var"></a><a name="10.1"></a>
  - [10.1](#variables--var) Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.
    
    > If you need to define a variable in one hook to use in other hooks, add it to the TRC object, which is global. Make sure your key name is unique!

    ```javascript
    // bad
    superPower = getExposedToRadiation();

    // good
    var superPower = getExposedToRadiation();

    // good if you need to access in another hook
    TRC = TRC || {}
    TRC._superPower = getExposedToRadiation()
    ```

  <a name="variables--one-var"></a><a name="10.2"></a>
  - [10.2](#variables--one-var) Use one `var` declaration per variable or assignment. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Why? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

    ```javascript
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
    ```

<a name="variables--no-chain-assignment"></a><a name="10.3"></a>
  - [10.3](#variables--no-chain-assignment) Don’t chain variable assignments. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > Why? Chaining variable assignments creates implicit global variables.

    ```javascript
    // bad
    (function example() {
      // JavaScript interprets this as
      // var a = ( b = ( c = 1 ) );
      // The var keyword only applies to variable a; variables b and c become
      // global variables.
      var a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
      var a = 1;
      var b = a;
      var c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="10.4"></a>
  - [10.4](#variables--unary-increment-decrement) Avoid using unary increments and decrements (`++`, `--`).  eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Why? Per the eslint documenation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

    ```javascript
    // bad

    var array = [1, 2, 3];
    var num = 1;
    num++;
    --num;

    var sum = 0;
    var truthyCount = 0;
    for (var i = 0; i < array.length; i++) {
      var value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // good

    var array = [1, 2, 3];
    var num = 1;
    num += 1;
    num -= 1;

    var sum = array.reduce(function (a, b) { 
        return a + b 
    }, 0);

    var truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a><a name="10.5"></a>
  - [10.5](#variables--linebreak) Avoid linebreaks before or after `=` in an assignment. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len.html), surround the value in parens. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > Why? Linebreaks surrounding `=` can obfuscate the value of an assignment.

    ```javascript
    // bad
    var foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // bad
    var foo
      = 'superLongLongLongLongLongLongLongLongString';

    // good
    var foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // good
    var foo = 'superLongLongLongLongLongLongLongLongString';
    ```

<a name="variables--no-unused-vars"></a><a name="10.6"></a>
  - [10.6](#variables--no-unused-vars) Disallow unused variables.

    > Why? Variables that are declared and not used anywhere in the code are most likely an error due to incomplete refactoring. Such variables take up space in the code and can lead to confusion by readers.

    ```javascript
    // bad

    var some_unused_var = 42;

    // Write-only variables are not considered as used.
    var y = 10;
    y = 5;

    // A read for a modification of itself is not considered as used.
    var z = 0;
    z = z + 1;

    // Unused function arguments.
    function getX(x, y) {
        return x;
    }

    // good

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  <a name="hoisting--anon-expressions"></a><a name="11.1"></a>
  - [11.1](#hoisting--anon-expressions) Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="11.2"></a>
  - [11.2](#hoisting--named-expressions) Named function expressions hoist the variable name, not the function name or the function body.

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
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="11.3"></a>
  - [11.3](#hoisting--declarations) Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```
  
  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

  **[⬆ back to top](#table-of-contents)**

## Comparison Operators & Equality

  <a name="comparison--eqeqeq"></a><a name="12.1"></a>
  - [12.1](#comparison--eqeqeq) Use `===` and `!==` over `==` and `!=`.
    
    > Why? `==` does not check for type equality and will attempt to coerce values compared into the same type. This can lead to unpredictable results (and is probably never what you actually want to do). It is also a very common typo to accidentally use `=` when you are defaulting to `==`. This can lead to lots of bugs and broken publisher websites.


  <a name="comparison--if"></a><a name="12.2"></a>
  - [12.2](#comparison--if) Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    - **Objects** evaluate to **true**
    - **Undefined** evaluates to **false**
    - **Null** evaluates to **false**
    - **Booleans** evaluate to **the value of the boolean**
    - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```


  <a name="comparison--shortcuts"></a><a name="12.3"></a>
  - [12.3](#comparison--shortcuts) Use shortcuts for booleans, but explicit comparisons for strings and numbers.

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="12.4"></a>
  - [12.4](#comparison--moreinfo) For more information see [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

  <a name="comparison--nested-ternaries"></a><a name="12.5"></a>
  - [12.5](#comparison--nested-ternaries) Ternaries should not be nested and generally be single line expressions.

    ```javascript
    // bad
    var foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    var maybeNull = value1 > value2 ? 'baz' : null;

    // better
    var foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    var foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="12.6"></a>
  - [12.6](#comparison--unneeded-ternary) Avoid unneeded ternary statements.

    ```javascript
    // bad
    var foo = a ? a : b;
    var bar = c ? true : false;
    var baz = c ? false : true;

    // good
    var foo = a || b;
    var bar = !!c;
    var baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a><a name="12.7"></a>
  - [12.7](#comparison--no-mixed-operators) When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: `+`, `-`, and `**` since their precedence is broadly understood. We recommend enclosing `/` and `*` in parentheses because their precedence can be ambiguous when they are mixed.

    > Why? This improves readability and clarifies the developer’s intention.

    ```javascript
    // bad
    var foo = a && b < 0 || c > 0 || d + 1 === 0;

    // bad
    var bar = a ** b - 5 % d;

    // bad
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d;
    }

    // bad
    var bar = a + b / c * d;

    // good
    var foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // good
    var bar = a ** b - (5 % d);

    // good
    if (a || (b && c)) {
      return d;
    }

    // good
    var bar = a + (b / c) * d;
    ```

**[⬆ back to top](#table-of-contents)**


## Blocks

  <a name="blocks--braces"></a><a name="13.1"></a>
  - [13.1](#blocks--braces) Use braces with all multi-line blocks.

    ```javascript
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
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="13.2"></a>
  - [13.2](#blocks--cuddled-elses) If you’re using multi-line blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace.

    ```javascript
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
    ```

  <a name="blocks--no-else-return"></a><a name="13.3"></a>
  - [13.3](#blocks--no-else-return) If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks.

    ```javascript
    // bad
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // bad
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // bad
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // good
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // good
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // good
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Control Statements

  <a name="control-statements"></a>
  - [14.1](#control-statements) In case your control statement (`if`, `while` etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.

    > Why? Requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [14.2](#control-statements--value-selection) Don't use selection operators in place of control statements.

    ```javascript
    // bad
    !isRunning && startRunning();

    // good
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments--multiline"></a><a name="15.1"></a>
  - [15.1](#comments--multiline) Use `/** ... */` for multi-line comments.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="15.2"></a>
  - [15.2](#comments--singleline) Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

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
      var type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a><a name="15.3"></a>
  - [15.3](#comments--spaces) Start all comments with a space to make it easier to read.

    ```javascript
    // bad
    //is current tab
    var active = true;

    // good
    // is current tab
    var active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```
**[⬆ back to top](#table-of-contents)**

## Whitespace

<a name="whitespace--before-blocks"></a><a name="16.1"></a>
  - [16.1](#whitespace--before-blocks) Place 1 space before the leading brace.

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
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="16.2"></a>
  - [16.2](#whitespace--around-keywords) Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations.

    ```javascript
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
    ```

  <a name="whitespace--infix-ops"></a><a name="16.3"></a>
  - [16.3](#whitespace--infix-ops) Set off operators with spaces.

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

<a name="whitespace--after-blocks"></a><a name="16.4"></a>
  - [16.4](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement.

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

    // bad
    var arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    var arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="16.5"></a>
  - [16.5](#whitespace--padded-blocks) Do not pad your blocks with blank lines.

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a><a name="16.6"></a>
  - [16.6](#whitespace--no-multiple-blanks) Do not use multiple blank lines to pad your code.

    ```javascript
    // bad
    if (isFeed) {

      var cards = feed.querySelectorAll('.videoCube');


      cards[0].style.margin = 'none';
    }

    // good
    if (isFeed) {
      var cards = feed.querySelectorAll('.videoCube');
      cards[0].style.margin = 'none';
    }
    ```

  <a name="whitespace--in-parens"></a><a name="16.7"></a>
  - [16.7](#whitespace--in-parens) Do not add spaces inside parentheses.

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="16.8"></a>
  - [16.8](#whitespace--in-brackets) Do not add spaces inside brackets.

    ```javascript
    // bad
    var foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    var foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="16.9"></a>
  - [16.9](#whitespace--in-braces) Add spaces inside curly braces.

    ```javascript
    // bad
    var foo = {clark: 'kent'};

    // good
    var foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="16.10"></a>
  - [16.10](#whitespace--max-len) Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up.

    > Why? This ensures readability and maintainability.

    ```javascript
    // bad
    var foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    var feedCard = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;
    ```

  <a name="whitespace--block-spacing"></a><a name="16.11"></a>
  - [16.11](#whitespace--block-spacing) Require consistent spacing inside an open block token and the next token on the same line. This rule also enforces consistent spacing inside a close block token and previous token on the same line.

    ```javascript
    // bad
    function foo() {return true;}
    if (foo) { bar = 0;}

    // good
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a><a name="16.12"></a>
  - [16.12](#whitespace--comma-spacing) Avoid spaces before commas and require a space after commas.

    ```javascript
    // bad
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // good
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a><a name="16.13"></a>
  - [16.13](#whitespace--computed-property-spacing) Enforce spacing inside of computed property brackets.

    ```javascript
    // bad
    obj[foo ]
    obj[ 'foo-bar']
    obj[foo[ bar ]]

    // good
    obj[foo]
    obj['foo-bar']
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a><a name="16.14"></a>
  - [16.14](#whitespace--func-call-spacing) Avoid spaces between functions and their invocations.

    ```javascript
    // bad
    func ();

    func
    ();

    // good
    func();
    ```

  <a name="whitespace--key-spacing"></a><a name="16.15"></a>
  - [16.15](#whitespace--key-spacing) Enforce spacing between keys and values in object literal properties.

    ```javascript
    // bad
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // good
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a><a name="16.16"></a>
  - [16.16](#whitespace--no-trailing-spaces) Avoid trailing spaces at the end of lines.

  <a name="whitespace--no-multiple-empty-lines"></a><a name="16.17"></a>
  - [16.17](#whitespace--no-multiple-empty-lines) Avoid multiple empty lines, only allow one newline at the end of files, and avoid a newline at the beginning of files.

    ```javascript
    // bad - multiple empty lines
    var x = 1;


    var y = 2;

    // bad - 2+ newlines at end of file
    var x = 1;
    var y = 2;


    // bad - 1+ newline(s) at beginning of file

    var x = 1;
    var y = 2;

    // good
    var x = 1;
    var y = 2;

    ```

**[⬆ back to top](#table-of-contents)**

## Commas

<a name="commas--leading-trailing"></a><a name="17.1"></a>
  - [17.1](#commas--leading-trailing) Leading commas: **Nope.**

    ```javascript
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
      aTime,
    ];

    // bad
    var hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    var hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="17.2"></a>
  - [17.2](#commas--dangling) Additional trailing comma: **Yup.** But only for objects, not for functions. Trailing commas in functions are not supported in IE.

    > Why? This leads to cleaner git diffs (if you are using git to track your hook changes). It also makes it easier to add properties to the object.

    ```diff
    // bad - git diff without trailing comma
    var hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - git diff with trailing comma
    var hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // bad
    var hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];

    // good
    var hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // bad
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // good
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  <a name="semicolons--required"></a><a name="18.1"></a>
  - [18.1](#semicolons--required) **Yup.**

    > Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether or not it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

    ```javascript
    // bad - raises exception
    var luke = {}
    var leia = {}
    [luke, leia].forEach(function (jedi) { jedi.father = 'vader' })

    // bad - raises exception
    var reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // good
    var luke = {};
    var leia = {};
    [luke, leia].forEach(function (jedi) {
      jedi.father = 'vader';
    });

    // good
    var reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // good
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  <a name="coercion--explicit"></a><a name="19.1"></a>
  - [19.1](#coercion--explicit) Perform type coercion at the beginning of the statement.

  <a name="coercion--strings"></a><a name="19.2"></a>
  - [19.2](#coercion--strings) Strings:

    ```javascript
    // => this.reviewScore = 9;

    // bad
    var totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // bad
    var totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    var totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // good
    var totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="19.3"></a>
  - [19.3](#coercion--numbers) Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings.

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

  <a name="coercion--comment-deviations"></a><a name="19.4"></a>
  - [19.4](#coercion--comment-deviations) If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](https://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you’re doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="19.5"></a>
  - [19.5](#coercion--bitwise) **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](https://es5.github.io/#x4.3.19), but bitshift operations always return a 32-bit integer ([source](https://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="19.6"></a>
  - [19.6](#coercion--booleans) Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // best
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--descriptive"></a><a name="20.1"></a>
  - [20.1](#naming--descriptive) Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }

    // bad
    var even = Array.prototype.slice.call(box.querySelectorAll('.videoCube')).filter(function (el, idx) { return idx % 2 === 0 })

    // good
    var isEven = function isEven(element, idx) {
        return idx % 2 === 0;
    }
    var widgetSlots = box.querySelectorAll('.videoCube')
    var evenSlots = Array.prototype.filter.call(widgetSlots, isEven)
    ```

  <a name="naming--camelCase"></a><a name="20.2"></a>
  - [20.2](#naming--camelCase) Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```
**[⬆ back to top](#table-of-contents)**

## The Dom
  <a name="the-dom--query-selector"></a><a name="21.1"></a>
  - [21.1](#the-dom--query-selector) Use `querySelector` and `querySelectorAll` over `getElementById` and `getElementsByClassName`

    ```javascript
    // bad
    var widgetSlots = box.getElementsByClassName('videoCube');

    // good
    var widgetSlots = box.querySelectorAll('.videoCube');
    ```
  
  <a name="the-dom--null-checking-elements"></a><a name="21.2"></a>
  - [21.2](#the-dom--null-checking-elements) Make sure elements exist before manipulating them.

    ```javascript
    // bad
    var widget = document.querySelector('.thumbnails-a');
    widget.style.background = 'transparent';

    // good
    var widget = document.querySelector('.thumbnails-a');

    if (!!widget) {
      widget.style.background = 'transparent';
    }
    ```
 
  <a name="the-dom--array-from-node-lists-and-html-element-collections"></a><a name="21.3"></a>
  - [21.3](#the-dom--array-from-node-lists-and-html-element-collections) Use `Function.prototype.apply` with a `null` `this` argument to get arrays from NodeLists and HTMLElementCollections.

    ```javascript
    // bad
    var widgetSlots = box.querySelectorAll('.videoCube');
    var slotArr = [];

    for (var i = 0; i < widgetSlots.length; i++) {
      slotArr.push(widgetSlots[i]);
    }

    // better
    var widgetSlots = box.querySelectorAll('.videoCube');
    var slotArr = Array.prototype.slice.call(widgetSlots);
    
    // best
    var widgetSlots = box.querySelectorAll('.videoCube');
    var slotArr = Array.apply(null, widgetSlots);
    ```
 
  <a name="the-dom--node-lists-and-html-element-collections"></a><a name="21.4"></a>
  - [21.4](#the-dom--node-lists-and-html-element-collections) Use `Function.prototype.call` to call `Array.prototype` methods on NodeLists and HTMLElementCollections.

    > Why? If you need to map, filter, reduce, or iterate over (forEach) an NodeList or HTMLElementCollection, this saves you from creating an intermediate array.

    ```javascript
    // bad
    var getText = function getTextFromTag(tag) {
      return tag.innerText;
    }
  
    var titleTags = box.querySelectorAll('.video-title');
    var titleTagArray = Array.apply(null, titleTags);
    var titles = titleTagArray.map(getText);

    // good
    var getText = function getTextFromTag(tag) {
      return tag.innerText;
    }

    var titleTags = box.querySelectorAll('.video-title');
    var titles = Array.prototype.map.call(titleTags, getTitle);
    ```

## ECMAScript 5 Compatibility

  <a name="es5-compat--kangax"></a><a name="22.1"></a>
  - [22.1](#es5-compat--kangax) Refer to [Kangax](https://twitter.com/kangax/)’s ES5 [compatibility table](https://kangax.github.io/es5-compat-table/).

  <a name="es5-compat--mdn"></a><a name="22.2"></a>
  - [22.3](#es5-compat--mdn) Refer to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) to check for supported API's.

**[⬆ back to top](#table-of-contents)**


# };


