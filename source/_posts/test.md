---
title: 测试
date: 2022/11/13 20:46:25
categories:
- 面试
tags:
- JS
---


## 一、数组基础
### 1. 数组概述
数组是我们最常用的数据类型之一，ECMAScript数组跟其他语言的数组一样，都是一组有序的数据，但跟其他语言不同的是，数组中每个槽位可以存储任意类型的数据。除此之外，ECMAScript数组的长度也是动态的，会随着数据的增删而改变。

数组是被等分为许多小块的连续内存段，每个小块都和一个整数关联，可以通过这个整数快速访问对应的小块。除此之外，数组拥有一个length属性，该属性表示的并不是数组元素的数量，而是指数组元素的最高序号加1。
```javascript
let a = [1, 2, 3];
a.length === 3  // true
```
在ES6中，可以使用扩展运算符（...）来获取数组元素：
```javascript
let a = [1, 2, 3];
let b = [0, ...a, 4];  // [0, 1, 2, 3, 4]
```
### 2. 数组创建
数组的创建方式有以下两种。
#### （1）字面量
最常用的创建数组的方式就是**数组字面量，**数组元素的类型可以是任意的，如下：
```javascript
let colors = ["red", [1, 2, 3], true];  
```
#### （2）构造函数
使用构造函数创建数组的形式如下：
```javascript
let array = new Array(); 
```
如果已知数组元素数量，那么就可以给构造函数传入一个数值，然后length属性就会被自动创建并保存这个值，比如创建一个长度为10的数组：
```javascript
let array = new Array();  // [undefined × 10]
```
这样，就可以创建一个长度为10的数组，数组每个元素的值都是undefined。

还可以给Array构造函数传入要保存的元素，比如：
```javascript
let colors = new Array("red", "blue", "green");  
```
这就出现问题了，当我们创建数组时，如果给数组传入一个值，如果传入的值是数字，那么就会创建一个长度为指定数字的数组；如果这个值是其他类型，就会创建一个质保函该特定制度额数组。这样我们就无法直接创建一个只包含一个数字的数组了。

Array 构造函数根据参数长度的不同，有如下两种不同的处理方式：

- **new Array(arg1, arg2,…)**：参数长度为 0 或长度大于等于 2 时，传入的参数将按照顺序依次成为新数组的第 0 至第 N 项（参数长度为 0 时，返回空数组）；
- **new Array(length)**：当 length 不是数值时，返回一个只包含 length 元素一项的数组；当 length 为数值时，length 最大不能超过 32 位无符号整型，即需要小于 232，否则将抛出 RangeError。

在使用Array构造函数时，也可以省略 new 操作符，结果是一样的：
```javascript
let array = Array();  
```
#### （3）ES6 构造器
鉴于数组的常用性，ES6 专门扩展了数组构造器 Array ，新增了 2 个方法：Array.of和Array.from。Array.of 用得比较少，Array.from 具有很强的灵活性。

**1）Array.of**
Array.of 用于**将参数依次转化为数组项**，然后返回这个新数组。它基本上与 Array 构造器功能一致，唯一的区别就在单个数字参数的处理上。

比如，在下面的代码中，可以看到：当参数为2个时，返回的结果是一致的；当参数是一个时，Array.of 会把参数变成数组里的一项，而构造器则会生成长度和第一个参数相同的空数组：
```javascript
Array.of(8.0); // [8]
Array(8.0); // [empty × 8]

Array.of(8.0, 5); // [8, 5]
Array(8.0, 5); // [8, 5]

Array.of('8'); // ["8"]
Array('8'); // ["8"]
```

**2）Array.from**
Array.from 的设计初衷是快速基于其他对象创建新数组，准确来说就是从一个类似数组的可迭代对象中创建一个新的数组实例。其实，只要一个对象有迭代器，Array.from 就能把它变成一个数组（注意：该方法会返回一个的数组，不会改变原对象）。

从语法上看，Array.from 有 3 个参数：

- 类似数组的对象，必选；
- 加工函数，新生成的数组会经过该函数的加工再返回；
- this 作用域，表示加工函数执行时 this 的值。

这三个参数里面第一个参数是必选的，后两个参数都是可选的：
```javascript
var obj = {0: 'a', 1: 'b', 2:'c', length: 3};

Array.from(obj, function(value, index){
  console.log(value, index, this, arguments.length);
  return value.repeat(3);   //必须指定返回值，否则返回 undefined
}, obj);
```
结果如图：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1500604/1611730183856-26a3b305-e13f-420c-ae0b-6e2befdd325f.png#height=89&id=oAFws&margin=%5Bobject%20Object%5D&name=image.png&originHeight=89&originWidth=336&originalType=binary&ratio=1&size=7024&status=done&style=shadow&width=336)
以上结果表明，通过 Array.from 这个方法可以自定义加工函数的处理方式，从而返回想要得到的值；如果不确定返回值，则会返回 undefined，最终生成的是一个包含若干个 undefined 元素的空数组。

实际上，如果这里不指定 this，加工函数就可以是一个箭头函数。上述代码可以简写为以下形式。
```javascript
Array.from(obj, (value) => value.repeat(3));
//  控制台打印 (3) ["aaa", "bbb", "ccc"]
```
除了上述 obj 对象以外，拥有迭代器的对象还包括 String、Set、Map 等，`Array.from` 都可以进行处理：
```javascript
// String
Array.from('abc');                             // ["a", "b", "c"]
// Set
Array.from(new Set(['abc', 'def']));           // ["abc", "def"]
// Map
Array.from(new Map([[1, 'ab'], [2, 'de']]));   // [[1, 'ab'], [2, 'de']]
```
### 3. 数组空位
当我们使用数组字面量初始化数组时，可以使用一串逗号来创建空位，ECMAScript会将逗号之间相应索引位置的值当成空位，ES6 重新定义了该如何处理这些空位。

我们可以这样来创建一个空位数组：
```javascript
let array = [,,,,,];
console.log(array.length);
console.log(array)
```
运行结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1500604/1628425506519-3de80030-b418-4062-bed8-7a40f86ac2d1.png#height=113&id=jrEPd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=226&originWidth=504&originalType=binary&ratio=1&size=20090&status=done&style=shadow&width=252)
ES6新增的方法和迭代器与早期版本中存在的方法的行为不同，ES6新增方法普遍将这些空位当成存在的元素，只不过值为undefined，使用字面量形式创建如下数组：
```javascript
let array = [1,,,5];
for(let i of array){
  console.log(i === undefined)
}
// 输出结果：false true true false
```
使用ES6的Array.form创建数组：
```javascript
let array = Array.from([1,,,5]);
for(let i of array){
  console.log(i === undefined)
}
// 输出结果：false true true false
```
而ES6之前的方法则会忽略这个空位：
```javascript
let array = [1,,,5];
console.log(array.map(() => 10))

// 输出结果：[10, undefined, undefined, 10]
```
由于不同方法对空位数组的处理方式不同，因此尽量避免使用空位数组。
### 4. 数组索引
在数组中，我们可以通过使用数组的索引来获取数组的值：
```javascript
let colors = new Array("red", "blue", "green");  
console.log(array[1])  // blue
```
如果指定的索引值小于数组的元素数，就会返回存储在相应位置的元素，也可以通过这种方式来设置一个数组元素的值。如果设置的索引值大于数组的长度，那么就会将数组长度扩充至该索引值加一。


数组长度length的独特之处在于，他不是只读的。通过length属性，可以在数组末尾增加删除元素：
```javascript
let colors = new Array("red", "blue", "green");  
colors.length = 2
console.log(colors[2])  // undefined

colors.length = 4
console.log(colors[3])  // undefined
```
数组长度始终比数组最后一个值的索引大1，这是因为索引值都是从0开始的。
### 5. 数组判断
一个很经典的ECMASript问题就是如何判断一个对象是不是数组，下面来看常用的数据类型检测的方法。

在 ES6 之前，至少有如下 5 种方式去判断一个对象是否为数组。

- 通过**Object.prototype.toString.call()**做判断：
```javascript
Object.prototype.toString.call(obj).slice(8,-1) === 'Array';
```

- 通过**constructor**做判断：
```javascript
obj.constructor === Array;
```

- 通过**instanceof**做判断：
```javascript
obj instanceof Array
```

- 通过**Array.prototype.isPrototypeOf**做判断：
```javascript
Array.prototype.isPrototypeOf(obj)
```

- 通过基于**getPrototypeOf**做判断：
```javascript
Object.getPrototypeOf(obj) === Array.prototype;
```
如果obj是一个数组，那么上面这 5 个判断全部为 true，推荐通过 Object.prototype.toString 去判断一个值的类型。

ES6 新增了 `Array.isArray` 方法，可以直接判断数据类型是否为数组：
```javascript
Array.isArrray(obj);
```
如果 isArray 不存在，那么 `Array.isArray` 的 polyfill 通常可以这样写：
```javascript
if (!Array.isArray){
  Array.isArray = function(arg){
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```
## 二、数组方法
数字就像是一个森林，里面有很多函“树”，有些方法纯净如水，并不会改变原数组，有些则会改变原数组。

- 改变原数组的方法：fill()、pop()、push()、shift()、splice()、unshift()、reverse()、sort()；
- 不改变原数组的方法：concat()、every()、filter()、find()、findIndex()、forEach()、indexOf()、join()、lastIndexOf()、map()、reduce()、reduceRight()、slice()、some。
### 1. 复制和填充方法
ES提供了两个方法：批量复制方法copeWithin()，以及填充数组方法fill()。这两个方法的签名类似，都需要指定已有数组实例上的一个范围，包含开始索引，不包含结束索引。下面就分别来看一下这两个方法。
#### （1）fill()
使用fill()方法可以向一个已有数组中插入全部或部分相同的值，开始索引用于指定开始填充的位置，它是可选的。如果不提供结束索引，则一直填充到数组末尾。如果是负值，则将从负值加上数组的长度而得到的值开始。该方法的语法如下：
```javascript
array.fill(value, start, end)
```
其参数如下：

- _value**：**_必需。填充的值；
- _start：_可选。开始填充位置；
- _end：_可选。停止填充位置 (默认为 _array_.length)。

使用示例如下：
```javascript
const arr = [0, 0, 0, 0, 0];

// 用5填充整个数组
arr.fill(5);
console.log(arr); // [5, 5, 5, 5, 5]
arr.fill(0);      // 重置

// 用5填充索引大于等于3的元素
arr.fill(5, 3);
console.log(arr); // [0, 0, 0, 5, 5]
arr.fill(0);      // 重置

// 用5填充索引大于等于1且小于等于3的元素
arr.fill(5, 3);
console.log(arr); // [0, 5, 5, 0, 0]
arr.fill(0);      // 重置

// 用5填充索引大于等于-1的元素
arr.fill(5, -1);
console.log(arr); // [0, 0, 0, 0, 5]
arr.fill(0);      // 重置
```
#### （2）copyWithin()
copyWithin()方法会按照指定范围来浅复制数组中的部分内容，然后将它插入到指定索引开始的位置，开始与结束索引的计算方法和fill方法一样。该方法的语法如下：
```javascript
array.copyWithin(target, start, end)
```
其参数如下：

- _target'**：**_必需。复制到指定目标索引位置；
- _start：_可选。元素复制的起始位置；
- _end：_可选。停止复制的索引位置 (默认为 _array_.length)。如果为负值，表示倒数。

使用示例如下：
```javascript
const array = [1,2,3,4,5]; 
console.log(array.copyWithin(0,3));  // [4, 5, 3, 4, 5]
```
### 2. 转化方法
数组的转化方法主要有四个：toLocaleString()、toString()、valueOf()、join()。下面就分别来看一下这4个方法。
#### （1）toString()
toString()方法返回的是由数组中每个值的等效字符串拼接而成的一个逗号分隔的字符串，也就是说，对数组的每个值都会调用toString()方法，以得到最终的字符串：
```javascript
let colors = ["red", "blue", "green"];  
console.log(colors.toString())  // red,blue,green
```
#### （2）valueOf()
valueOf()方法返回的是数组本身，如下面代码：
```javascript
let colors = ["red", "blue", "green"];  
console.log(colors.valueOf())  // ["red", "blue", "green"]
```
#### （3）toLocaleString()
toLocaleString()方法可能会返回和toString()方法相同的结果，但也不一定。在调用toLocaleString()方法时会得到一个逗号分隔的数组值的字符串，它与toString()方法的区别是，为了得到最终的字符串，会调用每个值的toLocaleString()方法，而不是toString()方法，看下面的例子：
```javascript
let array= [{name:'zz'}, 123, "abc", new Date()];
let str = array.toLocaleString();
console.log(str); // [object Object],123,abc,2016/1/5 下午1:06:23
```
需要注意，如果数组中的某一项是null或者undefined，则在调用上述三个方法后，返回的结果中会以空字符串来表示。
#### （4）join()
join() 方法用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的。其使用语法如下：
```javascript
arrayObject.join(separator)
```
其中参数separator是可选的，用来指定要使用的分隔符。如果省略该参数，则使用逗号作为分隔符。

该方法返回一个字符串。该字符串是通过把 arrayObject 的每个元素转换为字符串，然后把这些字符串连接起来，在两个元素之间插入 separator 字符串而生成的。

使用示例如下：
```javascript
let array = ["one", "two", "three","four", "five"];
console.log(array.join());      // one,two,three,four,five
console.log(array.join("-"));   // one-two-three-four-five
```
### 3. 栈方法
ECMAScript给数组添加了几个方法来使它像栈一样。众所周知，栈是一种后进先出的结构，也就是最近添加的项先被删除。数据项的插入（称为推入，push），和删除（称为弹出，pop）只在栈顶发生。数组提高了push()和pop()来实现类似栈的行为。下面就分别来看看这两个方法。
#### （1）push()
push()方法可以接收任意数量的参数，并将它们添加了数组末尾，并返回数组新的长度。**该方法会改变原数组。**其语法形式如下：
```javascript
arrayObject.push(newelement1,newelement2,....,newelementX)
```
使用示例如下：
```javascript
let array = ["football", "basketball",  "badminton"];
let i = array.push("golfball");
console.log(array); // ["football", "basketball", "badminton", "golfball"]
console.log(i);     // 4
```
#### （2）pop()
pop() 方法用于删除并返回数组的最后一个元素。它没有参数。**该方法会改变原数组。**其语法形式如下：
```javascript
arrayObject.pop()
```
使用示例如下：
```javascript
let array = ["cat", "dog", "cow", "chicken", "mouse"];
let item = array.pop();
console.log(array); // ["cat", "dog", "cow", "chicken"]
console.log(item);  // mouse
```
### 4. 队列方法

队列是一种先进先出的数据结构，队列在队尾添加元素，在对头删除元素。上面我们已经说了在结果添加数据的方法push()，下面就再来看看从数组开头删除和添加元素的方法：shift()和unshift()。实际上unshift()并不属于操作队列的方法，不过这里也一起说了。
#### （1）shift()
shift()方法会删除数组的第一项，并返回它，然后数组长度减一，**该方法会改变原数组。**语法形式如下：

```javascript
arrayObject.shift()
```
使用示例如下：
```javascript
let array = [1,2,3,4,5];
let item = array.shift();
console.log(array); // [2,3,4,5]
console.log(item);  // 1
```
注意：如果数组是空的，那么 shift() 方法将不进行任何操作，返回 undefined 值。
#### （2）unshift()
unshift()方法可向数组的开头添加一个或更多元素，并返回新的长度。**该方法会改变原数组。**其语法形式如下：
```javascript
arrayObject.unshift(newelement1,newelement2,....,newelementX)
```
使用示例如下：
```javascript
let array = ["red", "green", "blue"];
let length = array.unshift("yellow");
console.log(array);  // ["yellow", "red", "green", "blue"]
console.log(length); // 4
```
### 5. 排序方法

数组有两个方法可以对数组进行重新排序：sort()和reverse()。下面就分别来看看这两个方法。
#### （1）sort()

sort()方法是我们常用给的数组排序方法，该方法会在原数组上进行排序，会改变原数组，其使用语法如下：
```javascript
arrayObject.sort(sortby)
```
其中参数sortby是可选参数，用来规定排序顺序，它是一个比较函数，用来判断哪个值应该排在前面。默认情况下，sort()方法会按照升序重新排列数组元素。为此，sort()方法会在每一个元素上调用String转型函数，然后比较字符串来决定顺序，即使数组的元素都是数值，也会将数组元素先转化为字符串在进行比较、排序。这就造成了排序不准确的情况，如下代码：
```javascript
let array = [5, 4, 3, 2, 1];
let array2 = array.sort();
console.log(array2)  // [1, 2, 3, 4, 5]

let array = [0, 1, 5, 10, 15];
let array2 = array.sort();
console.log(array2)  //  [0, 1, 10, 15, 5]
```
可以看到，上面第二段代码就出现了问题，虽然5是小于10的，但是字符串10在5的前面，所以10还是会排在5前面，因此可知，在很多情况下，不添加参数是不行的。

对于sort()方法的参数，它是一个比较函数，它接收两个参数，如果第一个参数应该排在第二个参数前面，就返回-1；如果两个参数相等，就返回0；如果第一个参数应该排在第二个参数后面，就返回1。一个比较函数的形式可以如下：
```javascript
function compare(value1, value2) {
	if(value1 < value2){
  	return -1
  } else if(value1 > value2){
  	return 1
  } else{
  	return 0
  }
}

let array = [0, 1, 5, 10, 15];
let array2 = array.sort(compare);
console.log(array2)  // [0, 1, 5, 10, 15]
```
我们使用箭头函数来定义：
```javascript
let array = [0, 1, 5, 10, 15];

let array2 = array.sort((a, b) => a - b);  // 正序排序
console.log(array2)  // [0, 1, 5, 10, 15]

let array3 = array.sort((a, b) => b - a);  // 倒序排序
console.log(array3)  // [15, 10, 5, 1, 0]
```
#### （2）reverse()
reverse() 方法用于颠倒数组中元素的顺序。该方法会改变原来的数组，而不会创建新的数组。其使用语法如下：
```javascript
arrayObject.reverse()
```
使用示例如下：
```javascript
let array = [1,2,3,4,5];
let array2 = array.reverse();
console.log(array);   // [5,4,3,2,1]
console.log(array2 === array);   // true
```
### 6. 操作方法
对于数组，还有很多操作方法，下面我们就来看看常用的concat()、slice()、splice()方法。
#### （1）concat()
concat() 方法用于连接两个或多个数组。该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。其适用语法如下：
```javascript
arrayObject.concat(arrayX,arrayX,......,arrayX)
```
其中参数arrayX是必需的。该参数可以是具体的值，也可以是数组对象。可以是任意多个。

使用示例如下：
```javascript
let array = [1, 2, 3];
let array2 = array.concat(4, [5, 6], [7, 8, 9]);
console.log(array2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(array);  // [1, 2, 3], 可见原数组并未被修改
```
该方法还可以用于数组扁平化，后面会介绍。
#### （2）slice()
slice() 方法可从已有的数组中返回选定的元素。返回一个新的数组，包含从 start 到 end （不包括该元素）的数组元素。方法并不会修改数组，而是返回一个子数组。其使用语法如下：
```javascript
arrayObject.slice(start,end)
```
其参数如下：

- **start**：必需。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推；
- **end**：可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。

使用示例如下：
```javascript
let array = ["one", "two", "three", "four", "five"];
console.log(array.slice(0));    // ["one", "two", "three","four", "five"]
console.log(array.slice(2,3)); // ["three"]
```
#### （3）splice()
splice()方法可能是数组中的最强大的方法之一了，使用它的形式有很多种，它会向/从数组中添加/删除项目，然后返回被删除的项目。该方法会改变原始数组。其使用语法如下：
```javascript
arrayObject.splice(index, howmany, item1,.....,itemX)
```
其参数如下：

- index：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
- howmany：必需。要删除的项目数量。如果设置为 0，则不会删除项目。
- item1, ..., itemX：可选。向数组添加的新项目。

从上面参数可知，splice主要有三种使用形式：

- **删除：**需要给splice()传递两个参数，即要删除的第一个元素的位置和要删除的元素的数量；
- **插入：**需要给splice()传递至少三个参数，即开始位置、0（要删除的元素数量）、要插入的元素。
- **替换：**splice()方法可以在删除元素的同事在指定位置插入新的元素。同样需要传入至少三个参数，即开始位置、要删除的元素数量、要插入的元素。要插入的元素数量是任意的，不一定和删除的元素数量相等。

使用示例如下：
```javascript
let array = ["one", "two", "three","four", "five"];
console.log(array.splice(1, 2));           // 删除：["two", "three"]

let array = ["one", "two", "three","four", "five"];
console.log(array.splice(2, 0, 996));      // 插入：[]

let array = ["one", "two", "three","four", "five"];
console.log(array.splice(2, 1, 996));      // 替换：["three"]
```
### 7. 归并方法
ECMAScript为数组提供了两个归并方法：reduce()和reduceRight()。下面就分别来看看这两个方法。
#### （1）reduce()
reduce() 方法对数组中的每个元素执行一个reducer函数(升序执行)，将其结果汇总为单个返回值。其使用语法如下：
```javascript
arr.reduce(callback,[initialValue])
```
reduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce 的数组。
(1) `callback` （执行数组中每个值的函数，包含四个参数）

- previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
- currentValue （数组中当前被处理的元素）
- index （当前元素在数组中的索引）
- array （调用 reduce 的数组）

(2) `initialValue` （作为第一次调用 callback 的第一个参数。）
```javascript
let arr = [1, 2, 3, 4]
let sum = arr.reduce((prev, cur, index, arr) => {
    console.log(prev, cur, index);
    return prev + cur;
})
console.log(arr, sum);  
```
输出结果如下：
```javascript
1 2 1
3 3 2
6 4 3
[1, 2, 3, 4] 10
```
再来加一个初始值看看：
```javascript
let arr = [1, 2, 3, 4]
let sum = arr.reduce((prev, cur, index, arr) => {
    console.log(prev, cur, index);
    return prev + cur;
}, 5)
console.log(arr, sum);  
```
输出结果如下：
```javascript
5 1 0
6 2 1
8 3 2
11 4 3
[1, 2, 3, 4] 15
```
通过上面例子，可以得出结论：**如果没有提供initialValue，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供initialValue，从索引0开始。**

注意，该方法如果添加初始值，就会改变原数组，将这个初始值放在数组的最后一位。
#### （2）reduceRight()
该方法和的上面的`reduce()`用法几乎一致，只是该方法是对数组进行倒序查找的。而`reduce()`方法是正序执行的。
```javascript
let arr = [1, 2, 3, 4]
let sum = arr.reduceRight((prev, cur, index, arr) => {
    console.log(prev, cur, index);
    return prev + cur;
}, 5)
console.log(arr, sum);
```
输出结果如下：
```javascript
5 4 3
9 3 2
12 2 1
14 1 0
[1, 2, 3, 4] 15
```
### 8. 搜索和位置方法
ECMAScript提供了两类搜索数组的方法：按照严格相等搜索和按照断言函数搜索。
#### （1）严格相等
ECMAScript通过了3个严格相等的搜索方法：indexOf()、lastIndexOf()、includes()。这些方法都接收两个参数：要查找的元素和可选的其实搜索位置。lastIndexOf()方法会从数组结尾元素开始向前搜索，其他两个方法则会从数组开始元素向后进行搜索。indexOf()和lastIndexOf()返回的是查找元素在数组中的索引值，如果没有找到，则返回-1。includes()方法会返回布尔值，表示是否找到至少一个与指定元素匹配的项。在比较第一个参数和数组的每一项时，会使用全等（===）比较，也就是说两项必须严格相等。

使用示例如下：
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.indexOf(2))      // 1
console.log(arr.lastIndexOf(3))  // 2
console.log(arr.includes(4))     // true
```
#### （2）断言函数
ECMAScript也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数，断言函数的返回值决定了相应索引的元素是否被认为匹配。使用断言函数的方法有两个，分别是find()和findIndex()方法。这两个方法对于空数组，函数是不会执行的。并且没有改变数组的原始值。他们的都有三个参数：元素、索引、元素所属的数组对象，其中元素是数组中当前搜索的元素，索引是当前元素的索引，而数组是当前正在搜索的数组。

这两个方法都从数组的开始进行搜索，find()返回的是第一个匹配的元素，如果没有符合条件的元素返回 undefined；findIndex()返回的是第一个匹配的元素的索引，如果没有符合条件的元素返回 -1。

使用示例如下：
```javascript
let arr = [1, 2, 3, 4, 5]
arr.find(item => item > 2)      // 结果： 3
arr.findIndex(item => item > 2) // 结果： 2
```
### 9. 迭代器方法
在ES6中，Array的原型上暴露了3个用于检索数组内容的方法：keys()、values()、entries()。keys()方法返回数组索引的迭代器，values()方法返回数组元素的迭代器，entries()方法返回索引值对的迭代器。

使用示例如下（因为这些方法返回的都是迭代器，所以可以将他们的内容通过Array.from直接转化为数组实例）：
```javascript
let array = ["one", "two", "three", "four", "five"];
console.log(Array.from(array.keys()))     // [0, 1, 2, 3, 4]
console.log(Array.from(array.values()))   // ["one", "two", "three", "four", "five"]
console.log(Array.from(array.entries()))  // [[0, "one"], [1, "two"], [2, "three"], [3, "four"], [4, "five"]]
```
### 10. 迭代方法
ECMAScript为数组定义了5个迭代方法，分别是every()、filter()、forEach()、map()、some()。这些方法都不会改变原数组。这五个方法都接收两个参数：以每一项为参数运行的函数和可选的作为函数运行上下文的作用域对象（影响函数中的this值）。传给每个方法的函数接收三个参数，分别是当前元素、当前元素的索引值、当前元素所属的数对象。
#### （1）forEach()
`forEach` 方法用于调用数组的每个元素，并将元素传递给回调函数。该方法没有返回值，使用示例如下：
```javascript
let arr = [1,2,3,4,5]
arr.forEach((item, index, arr) => {
  console.log(index+":"+item)
})
```
该方法还可以有第二个参数，用来绑定回调函数内部this变量（回调函数不能是箭头函数，因为箭头函数没有this）：
```javascript
let arr = [1,2,3,4,5]
let arr1 = [9,8,7,6,5]
arr.forEach(function(item, index, arr){
  console.log(this[index])  //  9 8 7 6 5
}, arr1)
```
#### （2）map()
`map()` 方法会返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。该方法按照原始数组元素顺序依次处理元素。该方法不会对空数组进行检测，它会返回一个新数组，**不会改变原始数组**。使用示例如下：
```javascript
let arr = [1, 2, 3];
 
arr.map(item => {
    return item+1;
})
// 结果： [2, 3, 4]
```
第二个参数用来绑定参数函数内部的this变量：
```javascript
var arr = ['a', 'b', 'c'];
 
[1, 2].map(function (e) {
    return this[e];
}, arr)
 // 结果： ['b', 'c']
```
该方法可以进行链式调用：
```javascript
let arr = [1, 2, 3];
 
arr.map(item => item+1).map(item => item+1)
 // 结果： [3, 4, 5]
```
**forEach和map区别如下：**

- forEach()方法：会针对每一个元素执行提供的函数，对数据的操作会改变原数组，该方法没有返回值；
- map()方法：不会改变原数组的值，返回一个新数组，新数组中的值为原数组调用函数处理之后的值；
#### （3）filter()
`filter()`方法用于过滤数组，满足条件的元素会被返回。它的参数是一个回调函数，所有数组元素依次执行该函数，返回结果为true的元素会被返回。该方法会返回一个新的数组，不会改变原数组。
```javascript
let arr = [1, 2, 3, 4, 5]
arr.filter(item => item > 2) 
// 结果：[3, 4, 5]
```
可以使用`filter()`方法来移除数组中的undefined、null、NAN等值
```javascript
let arr = [1, undefined, 2, null, 3, false, '', 4, 0]
arr.filter(Boolean)
// 结果：[1, 2, 3, 4]
```
#### （4）every()
该方法会对数组中的每一项进行遍历，只有所有元素都符合条件时，才返回true，否则就返回false。
```javascript
let arr = [1, 2, 3, 4, 5]
arr.every(item => item > 0) 
// 结果： true
```
#### （5）some()
该方法会对数组中的每一项进行遍历，只要有一个元素符合条件，就返回true，否则就返回false。
```javascript
let arr = [1, 2, 3, 4, 5]
arr.some(item => item > 4) 
// 结果： true
```
### 11. 其他方法
除了上述方法，遍历数组的方法还有for...in和for...of。下面就来简单看一下。
#### （1）for…in
`for…in` 主要用于对数组或者对象的属性进行循环操作。循环中的代码每执行一次，就会对对象的属性进行一次操作。其使用语法如下：
```javascript
for (var item in object) {
  执行的代码块
}
```
其中两个参数：

- item：必须。指定的变量可以是数组元素，也可以是对象的属性。
- object：必须。指定迭代的的对象。

使用示例如下：
```javascript
const arr = [1, 2, 3]; 
 
for (var i in arr) { 
    console.log('键名：', i); 
    console.log('键值：', arr[i]); 
}
```
输出结果如下：
```javascript
键名： 0
键值： 1
键名： 1
键值： 2
键名： 2
键值： 3
```
需要注意，该方法**不仅会遍历当前的对象所有的可枚举属性，还会遍历其原型链上的属性。**除此之外，该方法遍历数组时候，遍历出来的是数组的索引值，遍历对象的时候，遍历出来的是键值名。
#### （2）for...of
`for...of` 语句创建一个循环来迭代可迭代的对象。在 ES6 中引入的 `for...of` 循环，以替代 `for...in` 和 `forEach()` ，并支持新的迭代协议。`for...of` 允许遍历 Arrays（数组）, Strings（字符串）, Maps（映射）, Sets（集合）等可迭代的数据结构等。

语法：
```javascript
for (var item of iterable) {
    执行的代码块
}
```
其中两个参数：

- item：每个迭代的属性值被分配给该变量。
- iterable：一个具有可枚举属性并且可以迭代的对象。

该方法允许获取对象的键值：
```javascript
var arr = ['a', 'b', 'c', 'd'];
for (let a in arr) {
  console.log(a); // 0 1 2 3
}
for (let a of arr) {
  console.log(a); // a b c d
}
```
该方法只会遍历当前对象的属性，不会遍历其原型链上的属性。

**注意：**

- for...of适用遍历 **数组/ 类数组/字符串/map/set** 等拥有迭代器对象的集合；
- 它可以正确响应break、continue和return语句；
- for...of循环不支持遍历普通对象，因为没有迭代器对象。如果想要遍历一个对象的属性，可以用`for-in`循环。

**总结，for…of 和for…in的区别如下：**

- for…of 遍历获取的是对象的键值，for…in 获取的是对象的键名；
- for… in 会遍历对象的整个原型链，性能非常差不推荐使用，而 for … of 只遍历当前对象不会遍历原型链；
- 对于数组的遍历，for…in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；
#### （3）flat()
在ES2019中，flat()方法用于创建并返回一个新数组，这个新数组包含与它调用flat()的数组相同的元素，只不过其中任何本身也是数组的元素会被打平填充到返回的数组中：
```javascript
[1, [2, 3]].flat()   // [1, 2, 3]
[1, [2, [3, 4]]].flat()   // [1, 2, [3, 4]]
```
在不传参数时，flat()默认只会打平一级嵌套，如果想要打平更多的层级，就需要传给flat()一个数值参数，这个参数表示要打平的层级数：
```javascript
[1, [2, [3, 4]]].flat(2)   // [1, 2, 3, 4]
```
## 三、类数组对象
JavaScript 中一直存在一种类数组的对象，它们不能直接调用数组的方法，但是又和数组比较类似，在某些特定的编程场景中会出现，下面就来看一下什么是类数组。

在 JavaScript 中，主要有以下情况中的对象是类数组：

- 函数里面的参数对象 arguments；
- 用 getElementsByTagName/ClassName/Name 获得的 HTMLCollection；
- 用 querySelector 获得的 NodeList。
### 1. 类数组概述
#### （1）arguments
在日常开发中经常会遇到各种类数组对象，最常见的就是在函数中使用的 arguments，它的对象只定义在函数体中，包括了函数的参数和其他属性。先来看下 arguments 的使用方法：
```javascript
function foo(name, age, sex) {
    console.log(arguments);
    console.log(typeof arguments);
    console.log(Object.prototype.toString.call(arguments));
}
foo('jack', '18', 'male');
```
打印结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1500604/1612679309803-e5208c79-c41f-4dce-87a7-6b8f16f5afc8.png#height=169&id=ol1vU&margin=%5Bobject%20Object%5D&name=image.png&originHeight=174&originWidth=686&originalType=binary&ratio=1&size=18821&status=done&style=shadow&width=666)
可以看到，typeof 这个 arguments 返回的是 object，通过 Object.prototype.toString.call 返回的结果是 [object arguments]，而不是 [object array]，说明 arguments 和数组还是有区别的。

length 属性就是函数参数的长度。另外 arguments 还有一个 callee 属性，下面看看这个 callee 是干什么的：
```javascript
function foo(name, age, sex) {
    console.log(arguments.callee);
}

foo('jack', '18', 'male');
```
打印结果如下：
```javascript
ƒ foo(name, age, sex) {
    console.log(arguments.callee);
}
```
可以看出，输出的就是函数自身，如果在函数内部直接执行调用 callee，那它就会不停地执行当前函数，直到执行到内存溢出。
#### （2）HTMLCollection
HTMLCollection 简单来说是 HTML DOM 对象的一个接口，这个接口包含了获取到的 DOM 元素集合，返回的类型是类数组对象，如果用 typeof 来判断的话，它返回的是 object。它是及时更新的，当文档中的 DOM 变化时，它也会随之变化。

下面来 HTMLCollection 最后返回的是什么，在一个**有 form 表单**的页面中，在控制台中执行下述代码：
```javascript
var elem1, elem2;
// document.forms 是一个 HTMLCollection
elem1 = document.forms[0];
elem2 = document.forms.item(0);
console.log(elem1);
console.log(elem2);
console.log(typeof elem1);
console.log(Object.prototype.toString.call(elem1));
```
打印结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1500604/1612679850622-ec390549-4b7a-49d2-ab58-45156d86d33c.png#height=116&id=LSXFQ&margin=%5Bobject%20Object%5D&name=image.png&originHeight=116&originWidth=599&originalType=binary&ratio=1&size=32190&status=done&style=shadow&width=599)
可以看到，这里打印出来了页面第一个 form 表单元素，同时也打印出来了判断类型的结果，说明打印的判断的类型和 arguments 返回的也比较类似，typeof 返回的都是 object，和上面的类似。

注意：HTML DOM 中的 HTMLCollection 是即时更新的，当其所包含的文档结构发生改变时，它会自动更新。
#### （3）NodeList
NodeList 对象是节点的集合，通常是由 querySlector 返回的。NodeList 不是一个数组，也是一种类数组。虽然 NodeList 不是一个数组，但是可以使用 for...of 来迭代。在一些情况下，NodeList 是一个实时集合，也就是说，如果文档中的节点树发生变化，NodeList 也会随之变化。
```javascript
var list = document.querySelectorAll('input[type=checkbox]');
for (var checkbox of list) {
  checkbox.checked = true;
}
console.log(list);
console.log(typeof list);
console.log(Object.prototype.toString.call(list));
```
打印结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1500604/1612680061951-5c2803a4-8d30-49c9-b50f-a1147d4d4274.png#height=208&id=TktkX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=208&originWidth=608&originalType=binary&ratio=1&size=37151&status=done&style=shadow&width=608)
### 2. 类数组应用场景
#### （1）遍历参数操作
在函数内部可以直接获取 arguments 这个类数组的值，那么也可以对于参数进行一些操作，比如下面这段代码可以将函数的参数默认进行求和操作：
```javascript
function add() {
    var sum =0,
        len = arguments.length;
    for(var i = 0; i < len; i++){
        sum += arguments[i];
    }
    return sum;
}
add()                            // 0
add(1)                           // 1
add(1，2)                        // 3
add(1,2,3,4);                    // 10
```
结合上面这段代码，在函数内部可以将参数直接进行累加操作，以达到预期的效果，参数多少也可以不受限制，根据长度直接计算，返回出最后函数的参数的累加结果，其他操作也类似。
#### （2）定义连接字符串函数
可以通过 arguments 这个例子定义一个函数来连接字符串。这个函数唯一正式声明了的参数是一个字符串，该参数指定一个字符作为衔接点来连接字符串。该函数定义如下：
```javascript
function myConcat(separa) {
  var args = Array.prototype.slice.call(arguments, 1);
  return args.join(separa);
}
myConcat(", ", "red", "orange", "blue");
// "red, orange, blue"
myConcat("; ", "elephant", "lion", "snake");
// "elephant; lion; snake"
myConcat(". ", "one", "two", "three", "four", "five");
// "one. two. three. four. five"
```
这段代码说明可以传递任意数量的参数到该函数，并使用每个参数作为列表中的项创建列表进行拼接。从这个例子中也可以看出，可以在日常编码中采用这样的代码抽象方式，把需要解决的这一类问题，都抽象成通用的方法，来提升代码的可复用性。
#### （3）传递参数
可以借助apply 或 call 与 arguments 相结合，将参数从一个函数传递到另一个函数：
```javascript
1. // 使用 apply 将 foo 的参数传递给 bar
2. function foo() {
3.     bar.apply(this, arguments);
4. }
5. function bar(a, b, c) {
6. console.log(a, b, c);
7. }
8. foo(1, 2, 3)   //1 2 3
```
上述代码中，通过在 foo 函数内部调用 apply 方法，用 foo 函数的参数传递给 bar 函数，这样就实现了借用参数的妙用。
### 3. 类数组转为数组
#### （1）借用数组方法
类数组因为不是真正的数组，所以没有数组类型上自带的那些方法，所以就需要利用下面这几个方法去借用数组的方法。比如借用数组的 push 方法，代码如下：
```javascript
var arrayLike = { 
  0: 'java',
  1: 'script',
  length: 2
} 
Array.prototype.push.call(arrayLike, 'jack', 'lily'); 
console.log(typeof arrayLike); // 'object'
console.log(arrayLike);
// {0: "java", 1: "script", 2: "jack", 3: "lily", length: 4}
```
可以看到，arrayLike 其实是一个对象，模拟数组的一个类数组，从数据类型上说它是一个对象，新增了一个 length 的属性。还可以看出，用 typeof 来判断输出的是 object，它自身是不会有数组的 push 方法的，这里用 call 的方法来借用 Array 原型链上的 push 方法，可以实现一个类数组的 push 方法，给 arrayLike 添加新的元素。

从打印结果可以看出，数组的 push 方法满足了我们想要实现添加元素的诉求。再来看下 arguments 如何转换成数组：
```javascript
function sum(a, b) {
  let args = Array.prototype.slice.call(arguments);
 // let args = [].slice.call(arguments); // 这样写也是一样效果
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);  // 3
function sum(a, b) {
  let args = Array.prototype.concat.apply([], arguments);
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);  // 3
```
可以看到，借用 Array 原型链上的各种方法，来实现 sum 函数的参数相加的效果。一开始都是将 arguments 通过借用数组的方法转换为真正的数组，最后都又通过数组的 reduce 方法实现了参数转化的真数组 args 的相加，最后返回预期的结果。
#### （2）借用ES6方法
还可以采用 ES6 新增的 Array.from 方法以及展开运算符的方法来将类数组转化为数组。那么还是围绕上面这个 sum 函数来进行改变，看下用 Array.from 和展开运算符是怎么实现转换数组的：
```javascript
function sum(a, b) {
  let args = Array.from(arguments);
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
function sum(a, b) {
  let args = [...arguments];
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
function sum(...args) {
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
```
可以看到，Array.from 和 ES6 的展开运算符，都可以把 arguments 这个类数组转换成数组 args，从而实现调用 reduce 方法对参数进行累加操作。其中第二种和第三种都是用 ES6 的展开运算符，虽然写法不一样，但是基本都可以满足多个参数实现累加的效果。
## 四、数组常见操作
### 1. 数组扁平化
下面再来看看数组的扁平化。所谓扁平化，其实就是将一个嵌套多层的数组 array（嵌套可以是任何层数）转换为只有一层的数组。举个简单的例子，假设有个名为 flatten 的函数可以做到数组扁平化，那么输出效果如下：
```javascript
let arr = [1, [2, [3, 4，5]]];
console.log(flatten(arr));  // [1, 2, 3, 4，5]
```
简单来说就是把多维的数组“拍平”，输出最后的一维数组。下面来看看实现flatten函数的方式。
#### （1）递归实现
普通的递归思路很容易理解，就是通过循环递归的方式，一项一项地去遍历，如果某一项还是一个数组，那么就继续往下遍历，利用递归来实现数组的每一项的连接：
```javascript
let arr = [1, [2, [3, 4, 5]]];
function flatten(arr) {
  let result = [];

  for(let i = 0; i < arr.length; i++) {
    if(Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result.push(arr[i]);
    }
  }
  return result;
}
flatten(arr);  //  [1, 2, 3, 4，5]
```
可以看到，最后返回的结果是扁平化的结果，这段代码核心就是循环遍历过程中的递归操作，就是在遍历过程中发现数组元素还是数组的时候进行递归操作，把数组的结果通过数组的 concat 方法拼接到最后要返回的 result 数组上，那么最后输出的结果就是扁平化后的数组。
#### （2）reduce 函数迭代
从上面的递归函数可以看出，其实就是对数组的每一项进行处理，那么其实也可以用 reduce 来实现数组的拼接，从而简化上面方法的代码，改造后的代码如下：
```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
    return arr.reduce(function(prev, next){
        return prev.concat(Array.isArray(next) ? flatten(next) : next)
    }, [])
}
console.log(flatten(arr));//  [1, 2, 3, 4，5]
```
这段代码在控制台执行之后，也可以得到想要的结果。上面我们说了 reduce 的第一个参数用来返回最后累加的结果，思路和第一种递归方法是一样的，但是通过使用 reduce 之后代码变得更简洁了，也同样解决了扁平化的问题。
#### （3）扩展运算符实现
这个方法的实现，采用了扩展运算符和 some 的方法，两者共同使用，达到数组扁平化的目的：
```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr);
    }
    return arr;
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```
从执行的结果中可以发现，先用数组的 some 方法把数组中仍然是组数的项过滤出来，然后执行 concat 操作，利用 ES6 的展开运算符，将其拼接到原数组中，最后返回原数组，达到了预期的效果。
#### （4）split 和 toString 
可以通过 split 和 toString 两个方法来共同实现数组扁平化，由于数组会默认带一个 toString 的方法，所以可以把数组直接转换成逗号分隔的字符串，然后再用 split 方法把字符串重新转换为数组，如下面的代码所示：
```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
    return arr.toString().split(',');
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```
通过这两个方法可以将多维数组直接转换成逗号连接的字符串，然后再重新分隔成数组。
#### （5）ES6 中的 flat
我们还可以直接调用 ES6 中的 flat 方法来实现数组扁平化。flat 方法的语法：`arr.flat([depth])`

其中 depth 是 flat 的参数，depth 是可以传递数组的展开深度（默认不填、数值是 1），即展开一层数组。如果层数不确定，参数可以传进 Infinity，代表不论多少层都要展开：
```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
  return arr.flat(Infinity);
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```
可以看出，一个嵌套了两层的数组，通过将 flat 方法的参数设置为 Infinity，达到了我们预期的效果。其实同样也可以设置成 2，也能实现这样的效果。在编程过程中，如果数组的嵌套层数不确定，最好直接使用 Infinity，可以达到扁平化的效果。
#### （6）正则和 JSON 方法
在第4种方法中已经使用 toString 方法，其中仍然采用了将 JSON.stringify 的方法先转换为字符串，然后通过正则表达式过滤掉字符串中的数组的方括号，最后再利用 JSON.parse 把它转换成数组：
```javascript
let arr = [1, [2, [3, [4, 5]]], 6];
function flatten(arr) {
  let str = JSON.stringify(arr);
  str = str.replace(/(\[|\])/g, '');
  str = '[' + str + ']';
  return JSON.parse(str); 
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```
可以看到，其中先把传入的数组转换成字符串，然后通过正则表达式的方式把括号过滤掉，匹配规则是：全局匹配（g）左括号或者右括号，将它们替换成空格，最后返回处理后的结果。之后拿着正则处理好的结果重新在外层包裹括号，最后通过 JSON.parse 转换成数组返回。
### 2. 数组去重
去除无序数组中的重复元素并且返回新的无重复数组。
#### （1）Set实现
ES6方法（使用数据结构集合）：
```javascript
const array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
```
#### （2）map实现
ES5方法：使用map存储不重复的数字
```javascript
const array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

function uniqueArray(array) {
  let map = {};
  let res = [];
  for(var i = 0; i < array.length; i++) {
    if(!map.hasOwnProperty([array[i]])) {
      map[array[i]] = 1;
      res.push(array[i]);
    }
  }
  return res;
}

uniqueArray(array); // [1, 2, 3, 5, 9, 8]
```
### 3. 数组求和
#### （1）reduce实现
```javascript
let arr = [1, 2, 3, 4, 5, 6]
let sum = arr.reduce( (total,i) => total += i,0);
console.log(sum);     // 21
```
#### （2）递归实现
```javascript
let arr = [1, 2, 3, 4, 5, 6] 
function add(arr) {
    if (arr.length == 1) return arr[0] 
    return arr[0] + add(arr.slice(1)) 
}
console.log(add(arr))  // 21
```
### 4. 数组乱序
#### （1）正向遍历
主要的实现思路就是：

1. 取出数组的第一个元素，随机产生一个索引值，将该第一个元素和这个索引对应的元素进行交换；
2. 第二次取出数据数组第二个元素，随机产生一个除了索引为1的之外的索引值，并将第二个元素与该索引值对应的元素进行交换；
3. 按照上面的规律执行，直到遍历完成。
```javascript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
for (var i = 0; i < arr.length; i++) {
  const randomIndex = Math.round(Math.random() * (arr.length - 1 - i)) + i;
  [arr[i], arr[randomIndex]] = [arr[randomIndex], arr[i]];
}
console.log(arr)
```
#### （2）倒序遍历
倒序遍历和上面实现思路类似，代码如下：
```javascript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let length = arr.length,
    randomIndex,
    temp;
  while (length) {
    randomIndex = Math.floor(Math.random() * length--);
    temp = arr[length];
    arr[length] = arr[randomIndex];
    arr[randomIndex] = temp;
  }
console.log(arr)
```
