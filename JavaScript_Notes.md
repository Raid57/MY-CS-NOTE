# Basic

## 数据类型和变量

### **Number**

JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型：

```JS
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```



### **字符串**

字符串是以单引号'或双引号"括起来的任意文本，比如`'abc'`，`"xyz"`等等



### **布尔值**



### **比较运算符**

要特别注意相等运算符`==`。JavaScript在设计时，有两种比较运算符：

第一种是`==`比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

第二种是`===`比较，它不会自动转换数据类型，如果数据类型不一致，返回`false`，如果一致，再比较。

由于JavaScript这个设计缺陷，*不要*使用`==`比较，始终坚持使用`===`比较。

另一个例外是`NaN`这个特殊的Number与所有其他值都不相等，包括它自己：

```JS
NaN === NaN; // false
```

唯一能判断`NaN`的方法是通过`isNaN()`函数：

```JS
isNaN(NaN); // true
```

最后要注意浮点数的相等比较：

```js
1 / 3 === (1 - 2 / 3); // false
```



### **BigInt**

1.BigInt创建：整数后面加`n`，整数最大值为`9007199254740991`, 当我们想要表示整数`9223372036854775807`，可用末尾加n变成BigInt`9223372036854775807n`来存储；

2.BigInt四则运算，计算结果仍为BigInt类型，注意BigInt不能和Number一起运算；



要精确表示比253还大的整数，可以使用内置的BigInt类型，它的表示方法是在整数后加一个`n`，例如`9223372036854775808n`，也可以使用`BigInt()`把Number和字符串转换成BigInt：

```JS
// 使用BigInt:
var bi1 = 9223372036854775807n;
var bi2 = BigInt(12345);
var bi3 = BigInt("0x7fffffffffffffff");
console.log(bi1 === bi2); // false
console.log(bi1 === bi3); // true
console.log(bi1 + bi2);
```

使用BigInt可以正常进行加减乘除等运算，结果仍然是一个BigInt，但不能把一个BigInt和一个Number放在一起运算：

```js
console.log(1234567n + 3456789n); // OK
console.log(1234567n / 789n); // 1564, 除法运算结果仍然是BigInt
console.log(1234567n % 789n); // 571, 求余
console.log(1234567n + 3456789); // Uncaught TypeError: Cannot mix BigInt and other types
```



### null和undefined

`null`表示一个“空”的值，它和`0`以及空字符串`''`不同，`0`是一个数值，`''`表示长度为0的字符串，而`null`表示“空”。

在其他语言中，也有类似JavaScript的`null`的表示，例如Java也用`null`，Swift用`nil`，Python用`None`表示。但是，在JavaScript中，还有一个和`null`类似的`undefined`，它表示“未定义”。

JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。



### 数组

数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。例如：

```JS
[1, 2, 3.14, 'Hello', null, true];
```



### 对象

JavaScript的对象是一组由键-值组成的**无序集合**，例如：

```JS
var person = {
    name: 'Bob',
    age: 20,
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
};
```

JavaScript对象的键都是字符串类型，值可以是任意数据类型。上述`person`对象一共定义了6个键值对，其中每个键又称为对象的属性，例如，`person`的`name`属性为`'Bob'`，`zipcode`属性为`null`。



**访问属性**

```JS
let xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
};
```

`xiaohong`的属性名`middle-school`不是一个有效的变量，就需要用`''`括起来。访问这个属性也无法使用`.`操作符，必须用`['xxx']`来访问：

```JS
xiaohong['middle-school']; // 'No.1 Middle School'
xiaohong['name']; // '小红'
xiaohong.name; // '小红'
```



**添加/删除属性**

```JS
let xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
```



**校验属性**

如果我们要检测`xiaoming`对象是否拥有某一属性，可以用`in`操作符：

```JS
let xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

不过要小心，如果`in`判断一个属性存在，这个属性不一定是`xiaoming`的，它可能是`xiaoming`继承得到的：

```JS
'toString' in xiaoming; // true
```

因为`toString`定义在`object`对象中，而所有对象最终都会在原型链上指向`object`，所以`xiaoming`也拥有`toString`属性。

要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

```JS
let xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```



## Map和Set

**Map**

```JS
// 初始化方式1
let m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95

// 初始化方式2
let m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```



**Set**

```JS
let s1 = new Set(); // 空Set
let s2 = new Set([1, 2, 3]); // 含1, 2, 3
s.add(4);// 含1, 2, 3，4
s.delete(3);// 含1, 2, 4
```



## iterable

`for in` & `for of` 

* `for in`遍历的是数组的索引（`index`），更适合遍历对象

* `for of`遍历的是数组元素值（`value`），适用遍历数/数组对象/字符串/`map`/`set`等拥有迭代器对象（`iterator`）的集合，但是不能遍历对象，因为没有迭代器对象，但如果想遍历对象的属性，你可以用`for in`循环（这也是它的本职工作）或用内建的`Object.keys()`方法

```JS
var myObject={
　　a:1,
　　b:2,
　　c:3
}
for (var key of Object.keys(myObject)) {
  console.log(key + ": " + myObject[key]);
}
//a:1 b:2 c:3
```



### 小结

`for in`遍历的是数组的索引（即键名），而`for of`遍历的是数组元素值

`for in`总是得到对象的`key`或数组、字符串的下标

`for of`总是得到对象的`value`或数组、字符串的值





## 函数

### 函数定义和调用

```JS
function abs(x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

上述`abs()`函数的定义如下：

- `function`指出这是一个函数定义；
- `abs`是函数的名称；
- `(x)`括号内列出函数的参数，多个参数以`,`分隔；
- `{ ... }`之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句。

请注意，函数体内部的语句在执行时，一旦执行到`return`时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。

如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`undefined`。

由于JavaScript的函数也是一个对象，上述定义的`abs()`函数实际上是一个函数对象，而函数名`abs`可以视为指向该函数的变量。

因此，第二种定义函数的方式如下：

```JS
let abs = function (x) {
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
};
```



### 方法

#### basic

```JS
let xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        let y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是25,明年调用就变成26了
```

在一个方法内部，`this`是一个特殊变量，它始终指向当前对象，也就是`xiaoming`这个变量。所以，`this.birth`可以拿到`xiaoming`的`birth`属性。



```JS
function getAge() {
    let y = new Date().getFullYear();
    return y - this.birth;
}

let xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25, 正常结果
getAge(); // NaN 
```

单独调用函数`getAge()`怎么返回了`NaN`？

JavaScript的函数内部如果调用了`this`，如果以对象的方法形式调用，比如`xiaoming.age()`，该函数的`this`指向被调用的对象，也就是`xiaoming`，这是符合我们预期的。

如果单独调用函数，比如`getAge()`，此时，该函数的`this`指向全局对象，也就是`window`。

```JS
let fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN

// 等效于
let fn = getAge() {
    let y = new Date().getFullYear();
    return y - this.birth;
}
fn(); // NaN fn() 等效于 getAge()，fn拿到的方法签名
```



#### apply

虽然在一个独立的函数调用中，根据是否是strict模式，`this`指向`undefined`或`window`，不过，我们还是可以控制`this`的指向的！

要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。

用`apply`修复`getAge()`调用：

```javascript
function getAge() {
    let y = new Date().getFullYear();
    return y - this.birth;
}

let xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

另一个与`apply()`类似的方法是`call()`，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

比如调用`Math.max(3, 5, 4)`，分别用`apply()`和`call()`实现如下：

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，我们通常把`this`绑定为`null`。



#### 装饰器

利用`apply()`，我们还可以动态改变函数的行为。

JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数。

现在假定我们想统计一下代码一共调用了多少次`parseInt()`，可以把所有的调用都找出来，然后手动加上`count += 1`，不过这样做太傻了。最佳方案是用我们自己的函数替换掉默认的`parseInt()`：



### 高阶函数

#### map

```JSON
function pow(x) {
    return x * x;
}

let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
let results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);

```

注意：`map()`传入的参数是`pow`，即函数对象本身。



#### reduce

```JS
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)
let arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25
```



如果数组元素只有1个，那么还需要提供一个额外的初始参数以便至少凑够两：

```JS
let arr = [123];
arr.reduce(function (x, y) {
    return x + y;
}, 0); // 123
```



#### filter

##### Basic

例如，在一个`Array`中，删掉偶数，只保留奇数，可以这么写：

```JS
let arr = [1, 2, 4, 5, 6, 9, 10, 15];
let r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```



把一个`Array`中的空字符串删掉，可以这么写：

```JS
let arr = ['A', '', 'B', null, undefined, 'C', '  '];
let r = arr.filter(function (s) {
    return s && s.trim(); // 注意：IE9以下的版本没有trim()方法
}); // 第一个条件去除null和undefined，第二个条件去除''和'  ';
r; // ['A', 'B', 'C']
```



##### 回调函数

`filter()`接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示`Array`的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：

```JS
let arr = ['A', 'B', 'C'];
let r = arr.filter(function (element, index, self) {
    console.log(element); // 依次打印'A', 'B', 'C'
    console.log(index); // 依次打印0, 1, 2
    console.log(self); // self就是变量arr
    return true;
});
```

利用`filter`，可以巧妙地去除`Array`的重复元素：

```JS
let
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];

r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});

console.log(r);
```



#### sort

```JS
let arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    return y - x;
}); // [20, 10, 2, 1]
```



#### Array

##### every()

`every()`方法可以判断数组的所有元素是否满足测试条件。

```JS
let arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0

console.log(arr.every(function (s) {
    return s.toLowerCase() === s;
})); // false, 因为不是每个元素都全部是小写
```



##### find()

`find()`方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回`undefined`：

```JS
let arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写

console.log(arr.find(function (s) {
    return s.toUpperCase() === s;
})); // undefined, 因为没有全部是大写的元素
```



##### findIndex()

`findindex` 丢进去的是一个函数，找满足函数关系的元素。

`indexof` 丢进去的是要找的元素，直接找元素。

```js
let arr = ['Apple', 'pear', 'orange'];
console.log(arr.findIndex(function (s) {
    return s.toLowerCase() === s;
})); // 1, 因为'pear'的索引是1

console.log(arr.findIndex(function (s) {
    return s.toUpperCase() === s;
})); // -1
```



##### forEach()

`forEach()`和`map()`类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。`forEach()`常用于遍历数组，因此，传入的函数不需要返回值：

```JS
let arr = ['Apple', 'pear', 'orange'];
arr.forEach(x=>console.log(x)); // 依次打印每个元素
```



#### 闭包

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回；



## 标准对象

## Promise异步操作

### 基本使用

[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 是一个对象，它代表了一个异步操作的最终完成或者失败；[Doc Link](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)



假设现在有一个名为 `createAudioFileAsync()` 的函数，它接收一些配置和两个回调函数，然后异步地生成音频文件。一个回调函数在文件成功创建时被调用，另一个则在出现异常时被调用。

```JS
// 成功的回调函数
function successCallback(result) {
  console.log("音频文件创建成功：" + result);
}

// 失败的回调函数
function failureCallback(error) {
  console.log("音频文件创建失败：" + error);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback);
```



如果重写 `createAudioFileAsync()` 为返回 Promise 的形式，你可以把回调函数附加到它上面：

```JS
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
```



**推荐的调用方式**

```JS
doSomething()
  .then(function (result) {
    return doSomethingElse(result);
  })
  .then(function (newResult) {
    return doThirdThing(newResult);
  })
  .then(function (finalResult) {
    console.log(`得到最终结果：${finalResult}`);
  })
  .catch(failureCallback);
```



**Or，更简洁的`箭头函数`方式**

```JS
doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => {
    console.log(`得到最终结果：${finalResult}`);
  })
  .catch(failureCallback);
```

**! 备注：**箭头函数表达式可以有[隐式返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions#函数体)；所以，`() => x` 是 `() => { return x; }` 的简写。





### 错误处理

#### 基本使用

```JS
doSomething()
  .then((result) => doSomethingElse(result))
  .then((newResult) => doThirdThing(newResult))
  .then((finalResult) => console.log(`得到最终结果：${finalResult}`))
  .catch(failureCallback);
```

==通常，一遇到异常抛出，浏览器就会顺着 Promise 链寻找下一个 `onRejected` 失败回调函数或者由 `.catch()` 指定的回调函数。这和以下同步代码的工作原理（执行过程）非常相似。==

```JS
try {
  let result = syncDoSomething();
  let newResult = syncDoSomethingElse(result);
  let finalResult = syncDoThirdThing(newResult);
  console.log(`得到最终结果：${finalResult}`);
} catch (error) {
  failureCallback(error);
}
```

这种异步代码的对称性在 [`async`/`await`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 语法中达到了极致：

```JS
async function foo() {
  try {
    const result = await doSomething();
    const newResult = await doSomethingElse(result);
    const finalResult = await doThirdThing(newResult);
    console.log(`得到最终结果：${finalResult}`);
  } catch (error) {
    failureCallback(error);
  }
}
```



```JS
// MY TEST CASE
promiseMain(){
  this.promiseTest()
      .then(this.promiseTest2, function(){console.log('error')})
      .then(this.promiseTest, function(){console.log('error')});
},
promiseTest() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(this.myResolve());
    }, 1000);
  });
},
promiseTest2() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(this.myResolve2());
    }, 1000);
  });
},
myResolve(){
  console.log('业务处理逻辑-' + count++)
  return 'success'; // 返回一个对象(res) or value(res.data.data)
},
myResolve2(){
  throw new Error('resolve2 error!');
}

// 另一个例子：这里的.then(回调函数)里面传参是一个回调函数，回调函数里用到的值res是前一个函数返回的Promise里的值，.then会在前一个Promise 成功or异常Error的时候 再去执行相应的回调函数；
// 在Promise构造器里，只传一个参数的时候是 执行当前一个Promise执行顺利的时候的继续执行的回调函数
// 在Promise构造器里，传两个参数的时候，前一个是在顺利执行的回调，后一个是在抛出Error的时候执行的回调
getSubscriptionsOnAzureElseProviderApi({provider: this.provider}).then((res) => {
    if (res.data.code === 0) {
      console.log("response result", res.data.data);
	}
})
```



#### **嵌套**

嵌套是一种可以限制 `catch` 语句的作用域的控制结构写法。明确来说，嵌套的 `catch` 只会捕获其作用域及以下的错误，而不会捕获链中更高层的错误。如果使用正确，可以实现细粒度的错误恢复。

```JS
// 如果doSomethingCritical()异常，则找到外部的catch里的内容
// 如过doSomethingOptional异常，则找到其所在then作用域里promise链下一个catch指定的内容
// 如果doSomethingExtraNice异常，则找到下一个catch
doSomethingCritical()
  .then((result) =>
    doSomethingOptional()
      .then((optionalResult) => doSomethingExtraNice(optionalResult))
      .catch((e) => {}),
  ) // 即便可选操作失败了，也会继续执行
  .then(() => moreCriticalStuff())
  .catch((e) => console.log(`严重失败：${e.message}`));
```

Promise链条解释

主链条：**doSomethingCritical** -> **moreCriticalStuff**

详细链条：**doSomethingCritical**[doSomethingOptional -> doSomethingExtraNice -> 捕捉此子链的catch]

-> **moreCriticalStuff**[捕捉主链条的catch]



这个内部的 `catch` 语句仅能捕获到 `doSomethingOptional()` 和 `doSomethingExtraNice()` 的失败，并将该错误与外界屏蔽，之后就恢复到 `moreCriticalStuff()` 继续执行。值得注意的是，如果 `doSomethingCritical()` 失败，这个错误仅会被最后的（外部）`catch` 语句捕获到，并不会被内部 `catch` 吞掉。



在 `async`/`await` 中，这段代码看起来像这样：

```JS
async function main() {
  try {
    const result = await doSomethingCritical();
    try {
      const optionalResult = await doSomethingOptional(result);
      await doSomethingExtraNice(optionalResult);
    } catch (e) {
      // 忽略可选步骤的失败并继续执行。
    }
    await moreCriticalStuff();
  } catch (e) {
    console.error(`严重失败：${e.message}`);
  }
}

```



**! 备注：**如果没有复杂的错误处理，则很可能不需要嵌套的 `then` 处理器。相反，可以使用扁平链，将错误处理逻辑放在最后。



#### Catch的后续链式操作

```JS
new Promise((resolve, reject) => {
  console.log("初始化");

  resolve();
})
  .then(() => {
    throw new Error("有哪里不对了");

    console.log("执行「这个」");
  })
  .catch(() => {
    console.log("执行「那个」");
  })
  .then(() => {
    console.log("执行「这个」，无论前面发生了什么");
  });
```

输出结果如下：

```JS
初始化
执行「那个」
执行「这个」，无论前面发生了什么
```



在 `async`/`await` 中，这段代码看起来像这样：

```JS
async function main() {
  try {
    await doSomething();
    throw new Error("有哪里不对了");
    console.log("执行「这个」");
  } catch (e) {
    console.error("执行「那个」");
  }
  console.log("执行「这个」，无论前面发生了什么");
}
```



#### [Promise 拒绝事件](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises#promise_拒绝事件)

当一个 Promise 拒绝事件未被任何处理器处理时，它会冒泡到调用栈的顶部，主机需要将其暴露出来。在 Web 上，当 Promise 被拒绝时，会有下文所述的两个事件之一被派发到全局作用域（通常而言，就是 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)；如果是在 web worker 中使用的话，就是 [`Worker`](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker) 或者其他基于 worker 的接口）。这两个事件如下所示：

- [`rejectionhandled`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/rejectionhandled_event)

  当 Promise 被拒绝、并且在 `reject` 函数处理该拒绝事件之后会派发此事件。

- [`unhandledrejection`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/unhandledrejection_event)

  当 Promise 被拒绝，但没有提供 `reject` 函数来处理该拒绝事件时，会派发此事件。

上述两种事件（类型为 [`PromiseRejectionEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/PromiseRejectionEvent)）都有两个属性，一个是 [`promise`](https://developer.mozilla.org/zh-CN/docs/Web/API/PromiseRejectionEvent/promise) 属性，该属性指向被拒绝的 Promise，另一个是 [`reason`](https://developer.mozilla.org/en-US/docs/Web/API/PromiseRejectionEvent/reason) 属性，该属性用来说明 Promise 被拒绝的原因。

因此，我们可以通过以上事件为 Promise 失败时提供补偿处理，也有利于调试 Promise 相关的问题。在每一个上下文中，该处理都是全局的，因此不管源码如何，所有的错误都会在同一个处理函数中被捕捉并处理。

在 [Node.js](https://developer.mozilla.org/zh-CN/docs/Glossary/Node.js) 中，对拒绝事件的处理稍有不同。你可以通过为 Node.js 的 `unhandledRejection` 事件添加处理器（注意名称的大小写不同）来捕获未处理的拒绝，就像这样：

JSCopy to Clipboard

```
process.on("unhandledRejection", (reason, promise) => {
  /* 你可以在这里添加一些代码，以便检查 promise 和 reason */
});
```

对于 Node.js 来说，为了防止错误被记录到控制台（否则默认会发生），添加 `process.on()` 监听器就足够了；不需要类似浏览器运行时的 [`preventDefault()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 方法这样的等效操作。

然而，如果你添加了 `process.on` 监听器，但没有在其中添加代码来处理被拒绝的 Promise，那么它们就会被丢弃，而且不会有任何提示。因此，最好在监听器中添加代码来检查每个被拒绝的 Promise，并确保它们不是由于代码错误而导致的。



### 组合

有四个[组合工具](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise#promise_并发)可用来并发异步操作：[`Promise.all()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)、[`Promise.allSettled()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)、[`Promise.any()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/any) 和 [`Promise.race()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)。



### [时序](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises#时序)

#### Basic

另一方面，Promise 是一种[控制反转](https://zh.wikipedia.org/wiki/控制反转)的形式——API 的实现者不控制回调何时被调用。相反，维护回调队列并决定何时调用回调的工作被委托给了 Promise 的实现者，这样一来，API 的使用者和开发者都会自动获得强大的语义保证，包括：

- 被添加到 [`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 的回调永远不会在 JavaScript 事件循环的[当前运行完成](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Event_loop#执行至完成)之前被调用。
- 即使异步操作已经完成（成功或失败），在这之后通过 [`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 添加的回调函数也会被调用。
- 通过多次调用 [`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 可以添加多个回调函数，它们会按照插入顺序进行执行。

以防万一的提醒：传入 [`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 的函数永远不会被同步调用，即使 Promise 已经被解决了（resolved）：

```JS
Promise.resolve().then(() => console.log(2));
console.log(1); // 1, 2
```

传入 `then()` 的函数不会立即运行，而是被放入微任务队列中，这意味着它会在稍后运行（仅在创建该函数的函数退出后，且 JavaScript 执行堆栈为空时），也就是在控制权返回事件循环之前。总而言之，不会等待太久：

```JS
const wait = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

wait().then(() => console.log(4));
Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
console.log(1); // 1, 2, 3, 4
```



#### [任务队列 vs. 微任务](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises#任务队列_vs._微任务)

Promise 回调被处理为[微任务](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)，而 [`setTimeout()`](https://developer.mozilla.org/zh-CN/docs/Web/API/setTimeout) 回调被处理为任务队列。

```JS
const promise = new Promise((resolve, reject) => {
  console.log("Promise 执行函数");
  resolve();
}).then((result) => {
  console.log("Promise 回调（.then）");
});

setTimeout(() => {
  console.log("新一轮事件循环：Promise（已完成）", promise);
}, 0);

console.log("Promise（队列中）", promise);
```

上述代码的输出如下：

```JS
Promise 执行函数
Promise（队列中）Promise {<pending>}
Promise 回调（.then）
新一轮事件循环：Promise（已完成）Promise {<fulfilled>}
```

详见[深入：微任务与 Javascript 运行时环境](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)。



#### [当 Promise 与 任务冲突时](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises#当_promise_与_任务冲突时)

你可能遇到如下情况：你的一些 Promise 和任务（例如事件或回调）会以不可预测的顺序启动。此时，你或许可以通过使用微任务检查状态或平衡 Promise，并以此有条件地创建 Promise。

如果你认为微任务可能会帮助你解决问题，那么请阅读[微任务指南](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide)，学习如何用 [`queueMicrotask()`](https://developer.mozilla.org/zh-CN/docs/Web/API/queueMicrotask) 来将一个函数作为微任务添加到队列中。



## 元编程



## Js模块
