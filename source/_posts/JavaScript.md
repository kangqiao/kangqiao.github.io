---
title: JavaScript
date: 2017-09-26 18:46:59
tags: [JavaScript, JS]
---


## 数据类型

### 6种数据类型

#### object对象

##### Function

##### Array

##### Data

#### number

#### string

#### boolean

#### null

#### undefined

### 隐式转换

#### +/-
```javascript
num - 0; //num作为数字来运算
num + ''; //num数字作字符处理

"37" - 7 //30
"37" + 7 //"377"
```



#### 等于 == 和 严格等于 ===

** 等于 **

```
“1.23” == 1.23
0 == false
null == undefined
new Object() == new Object()
[1, 2] == [1, 2]
```

- 类型相同，同===
- 类型不同，尝试类型转换和比较:
```
null == undefined 相等
number == string 转number     1 == “1.0" // true
boolean == ?  转number       1 == true  // true
object == number | string 尝试对象转为基本类型  new String('hi') == ‘hi’ // true
其它：false
```

** 严格等于 **
类型不同，返回false
类型相同
NaN ≠ NaN
new Object ≠ new Object
null === null
undefined === undefined



### 类型检测

typeof
适合基本类型及function检测，遇到null失效。

[[Class]]
通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效(IE678等返回[object Object])。

instanceof
适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。


当基本类型使用时, 会临时转换成包装类型对象, 用完后会销毁这个对象.

例:
```
var a = "string";
alert(a.length);
//把变量a包装成字体串对象, 并为它增加属性t=3;
a.t = 3
//当使用完变量a后, 即释放了包装对象, 所以a.t为undefined.
alert(a.t) // undefined
```



#### typeof

```
typeof 100 === “number”
typeof true === “boolean”
typeof function () {} === “function”

typeof(undefined) ) === “undefined”
typeof(new Object() ) === “object”
typeof( [1， 2] ) === “object”
typeof(NaN ) === “number”
typeof(null) === “object”
typeof('string') === "string"
```

#### instanceof

#### Object.prototype.toString

```
Object.prototype.toString.apply([]); === “[object Array]”;
Object.prototype.toString.apply(function(){}); === “[object Function]”;
Object.prototype.toString.apply(null); === “[object Null]”
Object.prototype.toString.apply(undefined); === “[object Undefined]”

IE6/7/8 Object.prototype.toString.apply(null) 返回”[object Object]”
```


#### constructor

#### duck type

### 运算符

#### 优先级

#### delete

删除对象的属性

只有对象的configurable为true时, 才可以delete掉对象的属性.
```
var obj = {};
Object.defineProperty(obj, 'x', {
	configurable : false, 
	value : 1
});
delete obj.x; // false
obj.x;            // 1

```

#### in

```
window.x = 1;
‘x’ in window; // true
```


#### new

```javascript
function Foo(){}
Foo.prototype.x = 1;
var obj = new Foo();
obj.x;  // 1
obj.hasOwnProperty('x'); // false
obj.__proto__.hasOwnProperty('x'); // true
```

#### this


```javascript
this;  // window (浏览器)
var obj = {
    func : function(){
        return this;
    }
};
obj.func(); // obj
```

#### void

```javascript
void 0  // undefined
void(0) // undefined
```

## 对象

### 属性


```javascript
var obj = {};
obj.y = 2;
obj.x = 1;
```
javascript属性是可以动态添加的.

属性标签:


#### 标签

** 获取属性描述信息 ** 
```javascript
Object.getOwnPropertyDescriptor({pro : true}, 'pro');
// Object {value: true, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor({pro : true}, 'a'); // undefined
```
** 例子 *
```javascript
// cat: 对象, 'price': 属性名, {} 是属性的标签集
Object.defineProperty(cat, 'price', {enumerable : false, value : 1000});
cat.propertyIsEnumerable('price'); // false
cat.hasOwnProperty('price'); // true


if (cat && cat.legs) {
    cat.legs *= 2;
}

if (cat.legs != undefined) {
    // !== undefined, or, !== null
}

//严格不等于.
if (cat.legs !== undefined) {
    // only if cat.legs is not undefined
}
```

##### enumerable


```javascript
var o = {x : 1, y : 2, z : 3};
'toString' in o; // true
o.propertyIsEnumerable('toString'); // false
var key;
for (key in o) {
    console.log(key); // x, y, z
}
```

```javascript
var obj = Object.create(o);
obj.a = 4;
var key;
for (key in obj) {
    console.log(key); // a, x, y, z
}

var obj = Object.create(o);
obj.a = 4;
var key;
for (key in obj) {
    //仅显示对象的属性, 过滤掉原型上的属性
    if (obj.hasOwnProperty(key)) {
        console.log(key); // a
    }
}
```

##### configurable/writable


```javascript
var o = {};
Object.defineProperty(o, 'x', {value : 1}); // writable=false, configurable=false
var obj = Object.create(o);
obj.x; // 1
obj.x = 200;
obj.x; // still 1, can't change it

Object.defineProperty(obj, 'x', {writable:true, configurable:true, value : 100});
obj.x; // 100
obj.x = 500;
obj.x; // 500
```

##### value

##### 自定义对象属性
defineproperty
defineProperties


** defineProperty自定义属性 **
```javascript
var person = {};
Object.defineProperty(person, 'name', {
    configurable : false,
    writable : false,
    enumerable : true,
    value : "Bosn Ma"
});

person.name; // Bosn Ma
person.name = 1;
person.name; // still Bosn Ma
delete person.name; // false
```

** 同时定义多个属性defineProperties ** 
```javascript
Object.defineProperties(person, {
    title : {value : 'fe', enumerable : true},
    corp : {value : 'BABA', enumerable : true},
    salary : {value : 50000, enumerable : true, writable : true}
});

Object.getOwnPropertyDescriptor(person, 'salary');
// Object {value: 50000, writable: true, enumerable: true, configurable: false}
Object.getOwnPropertyDescriptor(person, 'corp');
// Object {value: "BABA", writable: false, enumerable: true, configurable: false}
```


###### 例子

```javascript
Object.defineProperties(person, {
    title : {value : 'fe', enumerable : true},
    corp : {value : 'BABA', enumerable : true},
    salary : {value : 50000, enumerable : true, writable : true},
    luck : {
        get : function() {
        return Math.random() > 0.5 ? 'good' : 'bad';
        }
    },
    promote : {
        set : function (level) {
            this.salary *= 1 + level * 0.1;
        }
    }
});

Object.getOwnPropertyDescriptor(person, 'salary');
// Object {value: 50000, writable: true, enumerable: true, configurable: false}
Object.getOwnPropertyDescriptor(person, 'corp');
// Object {value: "BABA", writable: false, enumerable: true, configurable: false}
person.salary; // 50000
person.promote = 2;
person.salary; // 60000
```

####  删除


```javascript
var person = {age : 28, title : 'fe'};
delete person.age; // true
delete person['title']; // true
person.age; // undefined
delete person.age; // true

delete Object.prototype; // false,

var descriptor = Object.getOwnPropertyDescriptor(Object, 'prototype');
descriptor.configurable; // false
```

** delete 不能删除全局变量, 局部变量, 函数声明.
但是可以删除隐式的全局变量 **
```javascript
ohNo = 1;
window.ohNo; // 1
delete ohNo; // true
```

#### 属性检测


```javascript
var cat = new Object;
cat.legs = 4;
cat.name = "Kitty";

'legs' in cat; // true
'abc' in cat; // false
//in 会向上查找原型上是否有.
"toString" in cat; // true, inherited property!!!
```

```javascript

cat.hasOwnProperty('legs'); // true
cat.hasOwnProperty('toString'); // false

// 属性是否可枚举
cat.propertyIsEnumerable('legs'); // true
cat.propertyIsEnumerable('toString'); // false
```

#### getter/setter方法


```javascript
var man = {
    name : 'Bosn',
    weibo : '@Bosn',
    get age() {
        return new Date().getFullYear() - 1988;
    },
    set age(val) {
        console.log('Age can\'t be set to ' + val);
    }
}
console.log(man.age); // 27
man.age = 100; // Age can't be set to 100
console.log(man.age); // still 27
```

##### 例子

```javascript
var man = {
    weibo : '@Bosn',
    $age : null,
    get age() {
        if (this.$age == undefined) {
            return new Date().getFullYear() - 1988;
        } else {
            return this.$age;
        }
    },
    set age(val) {
        // 用一元运算+ 转换val为数字.
        val = +val;
        if (!isNaN(val) && val > 0 && val < 150) {
            this.$age = +val;
        } else {
            throw new Error('Incorrect val = ' + val);
        }
    }
}
//测试结果:
console.log(man.age); // 27
man.age = 100;
console.log(man.age); // 100;
man.age = 'abc'; // error:Incorrect val = NaN
```


##### get/set与原型链结合


```javascript
function foo() {}

Object.defineProperty(foo.prototype, 'z', {get : function(){return 1;}});
    
var obj = new foo();

obj.z; // 1
obj.z = 10;
obj.z; // still 1
```

```javascript
Object.defineProperty(obj, 'z', 
{value : 100, configurable: true});
obj.z; // 100;
delete obj.z;
obj.z; // back to 1
```

### 对象标签

#### __proto__指向原型

#### class

```javascript
var toString = Object.prototype.toString;
function getType(o){return toString.call(o).slice(8,-1);};

toString.call(null); // "[object Null]"
getType(null); // "Null"
getType(undefined); // "Undefined"
getType(1); // "Number"
getType(new Number(1)); // "Number"
typeof new Number(1); // "object"
getType(true); // "Boolean"
getType(new Boolean(true)); // "Boolean"
````

#### extensible / seal / freeze

```javascript
var obj = {x : 1, y : 2};
//是否可扩展, 默认true.
Object.isExtensible(obj); // true
//阻止对象扩展, 对于原有的属性x, y还是可以修改的.
Object.preventExtensions(obj);
Object.isExtensible(obj); // false
obj.z = 1;
obj.z; // undefined, add new property failed
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: true, enumerable: true, configurable: true}

//锁定::设置对象上所有属性的configurable=false.
Object.seal(obj);
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: true, enumerable: true, configurable: false}
Object.isSealed(obj); // true

//冻结::设置对象上的所有属性的configurable=false, writable=false.
Object.freeze(obj);
Object.getOwnPropertyDescriptor(obj, 'x');
// Object {value: 1, writable: false, enumerable: true, configurable: false}
Object.isFrozen(obj); // true

// [caution] not affects prototype chain!!!
```

### 对象创建

#### {}字面量方式创建


```javascript
var obj1 = {x : 1, y : 2};

var obj2 = {
    x : 1,
    y : 2,
    o : {
        z : 3,
        n : 4
    }
};
```

#### new/原型链


```javascript

function foo(){}
foo.prototype.z = 3;

var obj =new foo();
obj.y = 2;
obj.x = 1;

obj.x; // 1
obj.y; // 2
obj.z; // 3
typeof obj.toString; //‘function'
'z' in obj; // true
obj.hasOwnProperty('z'); // false

obj.z = 5; //仅在obj对象上创建z属性.

obj.hasOwnProperty('z'); // true
foo.prototype.z; // still 3
obj.z; // 5

obj.z = undefined;
obj.z; // undefined

//如果仍想访问原型上的z, 需要删除对象上的z.
delete obj.z; // true
obj.z; // 3

//delete只能删除对象的属性, 不能删除原型上的.
delete obj.z; // true
obj.z; // still 3!!!
```


#### Object.create

Object.create是系统内置的函数,它会返回一个新创建的对象, 并且这个创建的对象的原型指向这个参数对象.
```javascript
var obj = Object.create({x : 1});
obj.x // 1
typeof obj.toString // "function"
//由于obj的原型是参数{x:}, 所以x是原型的属性, 而不是对象的属性.
obj.hasOwnProperty('x');// false
```

### 序列化-- JSON.stringify()

```javascript
var obj = {x : 1, y : true, z : [1, 2, 3], nullVal : null};
JSON.stringify(obj); // "{"x":1,"y":true,"z":[1,2,3],"nullVal":null}"

obj = {val : undefined, a : NaN, b : Infinity, c : new Date()};
JSON.stringify(obj); // "{"a":null,"b":null,"c":"2015-01-20T14:15:43.910Z"}"

obj = JSON.parse('{"x" : 1}');
obj.x; // 1
```


#### toJSON 自定义-序列化

```javascript
var obj = {
    x : 1,
    y : 2,
    o : {
        o1 : 1,
        o2 : 2,
        toJSON : function () {
            return this.o1 + this.o2;
        }
    }
};
JSON.stringify(obj); // "{"x":1,"y":2,"o":3}"
```

#### valueOf 实现基本类型

** 其他对象方法 ** 
```javascript
var obj = {x : 1, y : 2};
obj.toString(); // "[object Object]"
obj.toString = function() {return this.x + this.y};
"Result " + obj; // "Result 3", by toString

+obj; // 3, from toString
//
obj.valueOf = function() {return this.x + this.y + 100;};
+obj; // 103, from valueOf

"Result " + obj; // still "Result 3"
```

## 语句

### block{}

请注意：没有块级作用域

```javascript
if(true){
    var ss = 'aaa';
}
ss
"aaa"
```

```javascript
function foo() {
    var a = b = 1;
}
foo();

console.log(typeof a);  // ‘undefined’
console.log(typeof b);  // ‘number’
```
如何避免上面函数中变为全局变量.
`var a=1, b=1;`

### for...in


1. 顺序不确定
2. enumerable为false时不会出现
3. for in对象属性时受原型链影响

```javascript
var p;
var obj = {x : 1, y: 2}

for (p in obj) {
}
```

### with

JS的严格模式下已经禁用with

- 让JS引擎优化更难
- 可读性差
- 可被变量定义代替
-严格模式下被禁用

```javascript
with ({x : 1}) {
    console.log(x);
}
with (document.forms[0]) {
    console.log(name.value);
}
var form = document.forms[0];
console.log(form.name.value);
```


### 严格模式

严格模式是一种特殊的执行模式，
它修复了部分语言上的不足，
提供更强的错误检查，并增强安全性。

```javascript
function func() {
    'use strict';
}
```
```javascript
'use strict';
function func() {
}
```

不允许用with  //SyntaxError
不允许未声明的变量被赋值







#### 不允许用with

#### 不允许未声明的变量被赋值

ReferenceError
```javascript
!function() {
	'use strict';
	 x = 1;
      console.log(window.x);
}();
```

#### arguments变为参数的静态副本


```javascript
!function(a) {
	arguments[0] = 100;
	console.log(a); //100
}(1);
```

```javascript
!function(a) {
	'use strict';
	arguments[0] = 100;
	console.log(a); //1
}(1);
```

```javascript
!function(a) {
	'use strict';
	arguments[0].x = 100;
	console.log(a.x); //100
}({x:1});
```

#### delete参数、函数名报错

```javascript
!function(a) {
	console.log(delete a); //false
}(1);
```

```javascript
!function(a) {
	'use strict';
	delete a; //SyntaxError

}(1);
```

#### delete不可配置的属性报错


```javascript
!function(a) {
	var obj = {};
	Object.defineProperty(obj, 
        'a', {configurable : false});
	console.log(delete obj.a); //false
}(1);
```

```javascript
!function(a) {
	'use strict';
	var obj = {};
	Object.defineProperty(obj, 
        'a', {configurable : false});
	delete obj.a; //TypeError
}(1);
```

#### 对象字面量重复属性名报错

```javascript
!function() {
	var obj = {x : 1, x : 2};
    console.log(obj.x); //2
}();
```

```javascript
!function() {
	'use strict';
	var obj = {x : 1, x : 2}; //SyntaxError
}();
```

#### 禁止八进制字面量


```javascript
!function() {
	console.log(0123); //83
}();
```

```javascript
!function() {
	'use strict';
	console.log(0123); //SyntaxError
}();
```

#### eval, arguments变为关键字，不能作为变量、函数名

```javascript
!function() {
	function eval(){}
    console.log(eval);
}();

function eval(){}
```

```javascript
!function() {
	'use strict';
	function eval(){} //SyntaxError
}();
```

#### eval独立作用域

```javascript
!function() {
	eval('var evalVal = 2;');
	console.log(typeof evalVal); //number
}();
```

undefined
```javascript
!function() {
	'use strict';
	eval('var evalVal = 2;');
	console.log(typeof evalVal); //undefined
}();
```

#### 其他

不允许用with
所有变量必须声明, 赋值给为声明的变量报错，而不是隐式创建全局变量。
eval中的代码不能创建eval所在作用域下的变量、函数。而是为eval单独创建一个作用域，并在eval返回时丢弃。
函数中得特殊对象arguments是静态副本，而不像非严格模式那样，修改arguments或修改参数变量会相互影响。
删除configurable=false的属性时报错，而不是忽略
禁止八进制字面量，如010 (八进制的8)
eval, arguments变为关键字，不可作为变量名、函数名等
_ 一般函数调用时(不是对象的方法调用，也不使用apply/call/bind等修改this)this指向null，而不是全局对象。
若使用apply/call，当传入null或undefined时，this将指向null或undefined，而不是全局对象。
试图修改不可写属性(writable=false)，在不可扩展的对象上添加属性时报TypeError，而不是忽略。
arguments.caller, arguments.callee被禁用 _


## 数组

数组是值的有序集合。每个值叫做元素，每个元素在数组中都有数字位置编号，也就是索引。JS中的数组是弱类型的，数组中可以含有不同类型的元素。数组元素甚至可以是对象或其它数组。

```javascript
{}   =>  Object.prototype
[]   =>  Array.prototype

var arr = [];
arr.__proto__ === Array.prototype //true

/属于Array实例
[] instanceof Array; // true
//
({}).toString.apply([]) === '[object Array]'; // true
//构造器
[].constructor === Array; // true
```
### 数组  VS.  一般对象
- 相同: 
 - 都可以继承. 
 - 数组是对象，对象不一定是数组
 - 都可以当做对象添加删除属性
- 不同:
 - 数组自动更新length
 - 按索引访问数组常常比访问一般对象属性明显迅速。
 - 数组对象继承Array.prototype上的大量数组操作方法


 

###  创建/读写

```javascript
var BAT = ['Alibaba', 'Tencent', 'Baidu'];
var students = [{name : 'Bosn', age : 27}, {name : 'Nunnly', age : 3}];
var arr = ['Nunnly', 'is', 'big', 'keng', 'B', 123, true, null];
var arrInArr = [[1, 2], [3, 4, 5]];

var commasArr1 = [1, , 2]; // 1, undefined, 2
var commasArr2 = [,,]; // undefined * 2
```
** 通过Array构造器创建**
```javascript
var arr = new Array(); 
var arrWithLength = new Array(100); // undefined * 100
var arrLikesLiteral = new Array(true, false, null, 1, 2, "hi");
// 等价于[true, false, null, 1, 2, "hi"];
```
** 读写 ** 
```javascript
var arr = [1, 2, 3, 4, 5];
arr[1]; // 2
arr.length; // 5

arr[5] = 6;
arr.length; // 6

delete arr[0];
arr[0]; // undefined
```

### 动态的增删元素

```javascript
var arr = [];
arr[0] = 1;
arr[1] = 2;
arr.push(3);
arr; // [1, 2, 3]

arr[arr.length] = 4; // equal to arr.push(4);
arr; // [1, 2, 3, 4]

arr.unshift(0);
arr; // [0, 1, 2, 3, 4];

delete arr[2];
arr; // [0, 1, undefined, 3, 4]
arr.length; // 5
2 in arr; // false

arr.length -= 1;
arr; // [0, 1, undefined, 3, 4],  4 is removed

arr.pop(); // 3 returned by pop
arr; // [0, 1, undefined], 3 is removed

arr.shift(); // 0 returned by shift
arr; // [1, undefined]

```



### for...in 迭代


```javascript
var i = 0, n = 10;
var arr = [1, 2, 3, 4, 5];
for (; i < n; i++) {
    console.log(arr[i]); // 1, 2, 3, 4, 5
}

for(i in arr) {
    console.log(arr[i]); // 1, 2, 3, 4, 5
}
```
** for...in迭代, 过滤掉原型上的属性 ** 
```javascript
Array.prototype.x = 'inherited';

for(i in arr) {
    console.log(arr[i]); // 1, 2, 3, 4, 5, inherited
}

for(i in arr) {
    if (arr.hasOwnProperty(i)) {
        console.log(arr[i]); // 1, 2, 3, 4, 5
    }
}
```


### 二维数组

```javascript
var arr = [[0, 1], [2, 3], [4, 5]];
var i = 0, j = 0;
var row;
for (; i < arr.length; i++) {
     row = arr[i];
     console.log('row ' + i);
     for (j = 0; j < row.length; j++) {
          console.log(row[j]);
     }
}

//结果:
// result:
// row 0
// 0
// 1
// row 1
// 2
// 3
// row 2
// 4
// 5
```

### 稀疏数组

_稀疏数组并不含有从0开始的连续索引。一般length属性值比实际元素个数大。_

```javascript
var arr1 = [undefined];
var arr2 = new Array(1);
0 in arr1; // true
0 in arr2; // false
arr1.length = 100;
arr1[99] = 123;
99 in arr1; // true
98 in arr1; // false

var arr = [,,];
0 in arr; // false
```

### 字符串和数组

** _ 字符串是类数组 _ **
** 字符串不是数组, 是不可变的.
字符串上没有数组上的方法, 
如果要使用数组的一些方法, 需要通过Array.prototype原型来调用. **
```javascript
var str = "hello world";
str.charAt(0); // "h"
str[1]; // e

//字符串不是数组, 但是可以通过数组原型的方法操作字符串.
Array.prototype.join.call(str, "_");
// "h_e_l_l_o_ _w_o_r_l_d"
```

### Array 原型上的方法

** 数组Array原型上的方法 ** 
- Array.prototype.join
- Array.prototype.reverse
- Array.prototype.sort
- Array.prototype.concat
- Array.prototype.slice 切片
- Array.prototype.splice 胶接
- Array.prototype.forEach  (ES5)
- Array.prototype.map    (ES5)
- Array.prototype.filter (ES5)
- Array.prototype.every  (ES5)
- Array.prototype.some   (ES5)
- Array.prototype.reduce/reduceRight  (ES5)
- Array.prototype.indexOf/lastIndexOf  (ES5)
- Array.isArray(ES5)



#### join

```javascript
var arr = [1, 2, 3];
arr.join(); // "1,2,3"
arr.join("_"); // "1_2_3"

function repeatString(str, n) {
     return new Array(n + 1).join(str);
}
repeatString("a", 3); // "aaa"
repeatString("Hi", 5); // "HiHiHiHiHi"
```

#### indexOf / lastIndexOf 数组检索

```javascript
var arr = [1, 2, 3, 2, 1];
arr.indexOf(2); // 1
arr.indexOf(99); // -1
arr.indexOf(1, 1); // 4
arr.indexOf(1, -3); // 4
arr.indexOf(2, -1); // -1
arr.lastIndexOf(2); // 3
arr.lastIndexOf(2, -2); // 3
arr.lastIndexOf(2, -3); // 1
```

#### forEach 

```javascript
var arr = [1, 2, 3, 4, 5];
arr.forEach(function(x, index, a){
    console.log(x + '|' + index + '|' + (a === arr));
});
// 1|0|true
// 2|1|true
// 3|2|true
// 4|3|true
// 5|4|true
````

#### every / some 数组判断

** _ 数组判断 _ **
```javascript
var arr = [1, 2, 3, 4, 5];
arr.every(function(x) {
     return x < 10;
}); // true

arr.every(function(x) {
     return x < 3;
}); // false
```

```javascript
var arr = [1, 2, 3, 4, 5];
arr.some(function(x) {
     return x === 3;
}); // true

arr.some(function(x) {
     return x === 100;
}); // false
```

#### reverse 修改

原数组被修改
```javascript
var arr = [1, 2, 3];
arr.reverse(); // [3, 2, 1]
arr; // [3, 2, 1]
```



#### sort 修改

```javascript
var arr = ["a", "d", "c", "b"];
arr.sort(); // ["a", "b", "c", "d"]

arr = [13, 24, 51, 3];
arr.sort(); // [13, 24, 3, 51]
arr; // [13, 24, 3, 51]
//原数组被修改.

arr.sort(function(a, b) {
     return a - b;
}); // [3, 13, 24, 51]

arr = [{age : 25}, {age : 39}, {age : 99}];
arr.sort(function(a, b) {
     return a.age - b.age;
});
arr.forEach(function(item) {
     console.log('age', item.age);
});
// result:
// age 25
// age 39
// age 99
```

#### splice 修改

** _ 原数组被修改 _ **
```javascript
var arr = [1, 2, 3, 4, 5];
arr.splice(2); // returns [3, 4, 5]
arr; // [1, 2];

arr = [1, 2, 3, 4, 5];
arr.splice(2, 2); // returns [3, 4]
arr; // [1, 2, 5];

arr = [1, 2, 3, 4, 5];
arr.splice(1, 1, 'a', 'b'); // returns [2]
arr; // [1, "a", "b", 3, 4, 5]
```

#### slice 不修改

** _ slice原数组未被修改 _ **
```javascript
var arr = [1, 2, 3, 4, 5];
arr.slice(1, 3); // [2, 3]
arr.slice(1); // [2, 3, 4, 5]
arr.slice(1, -1); // [2, 3, 4]
arr.slice(-4, -3); // [2]

```

#### concat 不修改

** _ concat 原数组未被修改 _ **
```javascript
var arr = [1, 2, 3];
arr.concat(4, 5); // [1, 2, 3, 4, 5]
arr; // [1, 2, 3]

arr.concat([10, 11], 13); // [1, 2, 3, 10, 11, 13]

arr.concat([1, [2, 3]]); // [1, 2, 3, 1, [2, 3]]
```

#### filter 不修改

** _ filter不修改原ovxe _ *
```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
arr.filter(function(x, index) {
     return index % 3 === 0 || x >= 8;
}); // returns [1, 4, 7, 8, 9, 10]
arr; // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### map 不修改

** _ map不修改原数组 _ **
```javascript
var arr = [1, 2, 3];
arr.map(function(x) {
     return x + 10;
}); // [11, 12, 13]
arr; // [1, 2, 3]
```

#### reduce / reduceRight 不修改

```javascript
max = arr.reduceRight(function(x, y) {
     console.log(x + "|" + y);
     return x > y ? x : y;
});
// 6|9
// 9|3
max; // 9
```

```javascript
var arr = [1, 2, 3];
var sum = arr.reduce(function(x, y) {
     return x + y
}, 0); // 6
arr; //[1, 2, 3]

arr = [3, 9, 6];
var max = arr.reduce(function(x, y) {
     console.log(x + "|" + y);
     return x > y ? x : y;
});
// 3|9
// 9|6
max; // 9
```

#### isArray 是否为数组

```javascript
Array.isArray([]); // true

[] instanceof Array; // true
({}).toString.apply([]) === '[object Array]'; // true
[].constructor === Array; // true
```

## 函数

函数是一块JavaScript代码，被定义一次，但可执行和调用多次。
JS中的函数也是对象，所以JS函数可以像其它对象那样操作和传递
所以我们也常叫JS中的函数为函数对象。 

### 调用方式


_ 不同的调用方式 _
- 直接调用: foo();
- 对象方法: o.method();
- 构造器: new Foo();
- call/apply/bind: func.call(o);








### 函数声明与函数表达式

** 函数声明 **
_可以前置_
```javascript
function add (a, b) {
  a = +a;
  b = +b;
  if (isNaN(a) || isNaN(b)) {
    return;
  }
  return a + b;
}
```
** 函数表达式 **
_变量提前为undefined, 函数不前置_
```javascript
// 函数变量
// function variable
var add = function (a, b) {
  // do sth
};

// 立即执行函数表达式.
// IEF(Immediately Executed Function)
//()和!是把函数作为表达式来处理
(function() {
  // do sth
})();
!function() {
  // do sth
}();

// 将匿名函数对象作为返回值
// first-class function
return function() {
  // do sth
};

// 命名式函数表达式
// NFE (Named Function Expression)
var add = function foo (a, b) {
  // do sth
};
alert(add === foo); //IE6~8: false ; IE9:foo is undefined.
// 递归调用
var func = function nfe() {
  /** do sth.**/ 
  nfe();
}
```
** Function构造器 **
```javascript
var func = new Function('a', 'b', 'console.log(a + b);');
func(1, 2); // 3
var func = Function('a', 'b', 'console.log(a + b);');
func(1, 2); // 3

// CASE 1 => localVal仍为局部变量
Function('var localVal = "local"; console.log(localVal);')();
console.log(typeof localVal);
// result: local, undefined

// CASE 2 => local不可访问, 全局变量global可以访问.
var globalVal = 'global';
(function() {
var localVal = 'local';
Function('console.log(typeof localVal, typeof globalVal);')();
})();
// result: undefined, string
```












### this

#### 全局的this(浏览器)


```javascript
console.log(this.document === document); // true
console.log(this === window); // true
this.a = 37;
console.log(window.a); // 37
```

#### 一般函数的this(浏览器)

```javascript
function f1(){
  return this;
}
f1() === window; // true, global object

function f2(){
  "use strict"; // see strict mode
  return this;
}
f2() === undefined; // true
```

#### 作为对象方法的函数的this

```javascript
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};
console.log(o.f()); // logs 37

var o = {prop: 37};
function independent() {
  return this.prop;
}
o.f = independent;
console.log(o.f()); // logs 37
```

#### 对象原型链上的this

```javascript
var o = {f:function(){ return this.a + this.b; }};
// 创建一个对象p, p的原型为对象o
var p = Object.create(o);
p.a = 1;
p.b = 4;
// p.f调用的是原型上的方法, 在原型中可以使用this.访问到p实例的属性.
console.log(p.f()); // 5
```

#### get/set方法与this

```javascript
function modulus(){
  return Math.sqrt(this.re * this.re + this.im * this.im);
}
var o = {
  re: 1,
  im: -1,
  get phase(){
    return Math.atan2(this.im, this.re);
  }
};

// 为对象o定义属性modelus, 并指定其get方法为modulus函数. 在其中也是可以用this来访问对象o的属性的.
Object.defineProperty(o, 'modulus', {
  get: modulus, enumerable:true, configurable:true
});
console.log(o.phase, o.modulus); // logs -0.78 1.4142
```

#### 构造器中的this

```javascript
function MyClass(){
  this.a = 37;
}

var o = new MyClass();
console.log(o.a); // 37

function C2(){
  this.a = 37;
  return {a : 38};
}

o = new C2();
console.log(o.a); // 38
```

#### call/apply方法与this

```javascript
function add(c, d){
  return this.a + this.b + c + d;
}

var o = {a:1, b:3};

// call接收参数列表.
add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16

// apply最后一个参数接收数组.
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34

function bar() {
  console.log( Object.prototype.toString.call(this));
}

bar.call(7); // "[object Number]"
```

#### bind方法与this

```javascript
function f(){
  return this.a;
}

// 绑定是将对象{a:"test"}作为函数f的this. 并返回一个新的g对象.
var g = f.bind({a : "test"});
console.log(g()); // test

// g对象并不会因改变对象而改变.
var o = {a : 37, f : f, g : g};
console.log(o.f(), o.g()); // 37, test
```

### 函数属性 & arguments

foo.name 函数名
foo.length 形参个数
arguments.length 实参个数
```javascript
function foo(x, y, z) {
  arguments.length; // 2
  arguments[0]; // 1
  arguments[0] = 10;
  x; // change to 10;
  arguments[2] = 100;
  z; // still undefined !!!
  arguments.callee === foo; // true
}

foo(1, 2);
foo.length; // 3
foo.name; // "foo"
```
** apply/call第一个参数作为函数this对象; 若为null或undefined时, 函数的this为全局的window对象. **
```javascript
function foo(x, y) {
  console.log(x, y, this);
}
foo.call(100, 1, 2); // 1, 2, Number(100)
foo.apply(true, [3, 4]); // 3, 4, Boolean(true)
foo.apply(null); // undefined, undefined, window
foo.apply(undefined); // undefined, undefined, window
```
** 严格模式下, 第一个参数不会变为默认window对象. **
```javascript
function foo(x, y) {
  'use strict';
  console.log(x, y, this);
}
foo.apply(null); // undefined, undefined, null
foo.apply(undefined); // undefined, undefined, undefined
```

### bind

#### bind方法

** 函数对象的绑定方法是将第一个参数作为函数的this. 并返回一个新的g函数对象. *
```javascript
this.x = 9;
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81; getX的this是module.

var getX = module.getX;
getX(); // 9; getX的this是window.

var boundGetX = getX.bind(module);
boundGetX(); // 81; bind后getX的this是bind的第一个参数module对象.
```

#### bind与currying

** 当第一个参数为null时, 仅改变了参数的绑定. 这样的话就指定了默认的配置 *
```javascript
function getConfig(colors, size, otherOptions) {
  console.log(colors, size, otherOptions);
}
var defaultConfig = getConfig.bind(null, "#CC0000", "1024 * 768");
defaultConfig("123"); // #CC0000 1024 * 768 123
defaultConfig("456"); // #CC0000 1024 * 768 456
```

#### bind与new

** new创建对象时, 会忽略构造器的return 返回值. **
```javascript
function foo() {
  this.b = 100;
  return this.a;
}

var func = foo.bind({a:1});

func(); // 1
new func(); // {b : 100}
```

#### bind方法模拟

** bind方法是ES5才提供的, 之前是没有的."

_ 在低版本上模拟bind方法实现 _
```javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('What is trying to be bound is not callable');
    }
  
  var aArgs = Array.prototype.slice.call(arguments, 1),
  fToBind = this,
  fNOP = function() {},
  fBound = function() {
    return fToBind.apply(this instanceof fNOP? this : oThis,
        aArgs.concat(Array.prototype.slice.call(arguments)));
  };
  fNOP.prototype = this.prototype;
  fBound.prototype = new fNOP();
  
  return fBound;
};

}
```

### 闭包

在计算机科学中，闭包（也称词法闭包或函数闭包）是指一个函数或函数的引用，与一个引用环境绑定在一起。这
个引用环境是一个存储该函数每个非局部变量（也叫自由变量）的表。
闭包，不同于一般的函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量。
from 维基百科

#### 实用例子


```javascript
!function() {
 var localData = "localData here";
  document.addEventListener('click',
    function(){
      console.log(localData);
   });
}();
```

```javascript
!function() {
 var localData = "localData here";
  var url = "http://www.baidu.com/";
  $.ajax({
    url : url,
    success : function() {
      // do sth...
      console.log(localData);
    }
  });
}();
```

#### 闭包-常见错误之循环闭包


```javascript
document.body.innerHTML = "<div id=div1>aaa</div>" + "<div id=div2>bbb</div><div id=div3>ccc</div>";
for (var i = 1; i < 4; i++) {
  document.getElementById('div' + i).
    addEventListener('click', function() {
      alert(i); // all are 4!
    });
}
```
** 正确在循环中使用 ** 

```javascript
for (var i = 1; i < 4; i++) {
  !function(i) {
    document.getElementById('div' + i).addEventListener('click', function() {
      alert(i); // 1, 2, 3
    });
  }(i);
}
```

#### 闭包-封装

** 由于函数中的局部变量在函数执行完成后销毁了. 通过闭包函数持有局部变量, 即可将变量隐藏起来, 只能通过返回的闭包去访问变量. *
```javascript
// 立即执行函数
(function() {
  var _userId = 23492;
  var _typeId = 'item';
  var export = {};

  function converter(userId) {
    return +userId;
  }

  export.getUserId = function() {
    return converter(_userId);
  }

  export.getTypeId = function() {
    return _typeId;
  }

  // 向外暴露export对象. 
  window.export = export;
}());

export.getUserId(); // 23492
export.getTypeId(); // item
export._userId; // undefined
export._typeId; // undefined
export.converter; // undefined
```

### 作用域

** _ JavaScript是没有块级作用域_ **

_ JavaScript作用域包括: _
- 全局
- 函数
- eval `eval("var a = 1;");`

利用函数作用域封装
```javascript
// () 和 ! 可以将函数作为表达式来处理.
(function() {
  // do sth here
  var a, b;
})();

!function() {
  // do sth here
  var a, b;
}();
```

_ Function构造函数中不能访问局部变量 _
```javascript
function outer() {
  var i = 1;
  var func = new Function("console.log(typeof i);");
  func(); // undefined
}
outer(); // undefined
```

### ES3执行上下文 Execution Context

** 抽象概念：执行上下文、变量对象...在ECMA-262 第三版标准规范中定义 **

_ JS解释器如何找到我们定义的函数和变量？ _

变量对象(Variable Object, 缩写为VO)是一个抽象概念中的“对象”，它用于存储执行上下文中的：
1. 变量
2. 函数声明
3. 函数参数

#### 执行上下文与变量对象


_ 全局上下文的this在浏览器中是window, 在NodeJs中是global对象. _
```javascript
# 执行上下文与变量对象的定义:
activeExecutionContext = {
  VO : {
    data_var,
    data_func_declaration,
    data_func_arguments
  }
};
GlobalContextVO => (VO === this === global)
```
**例子:**
```javascript
var a = 10;
function test(x) {
  var b = 20;
}
test(30);
```
**解析:**
```javascript
VO(globalContext) = {
  a : 10,
  test : <ref to function>
};
// 当执行test(30)后的test函数的执行上下文:
VO(test functionContext) = {
  x : 30,
  b: 20
};
```

#### 全局执行上下文

** JS在初始化时, 就会将Math String window ...放入到全局的VO对象中. 当调用时其实就是访问全局的VO对象中对应的值 **
```javascript
VO(globalContext) === [[global]];

[[global]] = {
  Math : <...>,
  String : <...>,
  isNaN : function() {[Native Code]}
  ...
  ...
  window : global // applied by browser(host)
};

GlobalContextVO (VO === this === globa

String(10); //[[global]].String(10);
window.a = 10; // [[global]].window.a = 10
this.b = 20; // [[global]].b = 20;
```

#### 函数中的激活对象

** 函数的激活对象是函数调用时会有一个arguments参数**

```javascript
VO(functionContext) === AO;

AO = {
  arguments : <Arg0>
};

arguments = {
  callee,
  length,
  properties-indexes
};
```

#### 变量初始化阶段

** _ VO按照如下顺序填充: _ *
1. 函数参数 (若未传⼊入，初始化该参数值为undefined) `把函数的参数放到VO中`
2. 函数声明 (若发⽣生命名冲突，会覆盖) `将内部的函数d声明放到VO中`
3. 变量声明 (初始化变量值为undefined，若发⽣生命名冲突，会忽略。) `将函数中的变量声明放到VO中`

```javascript
function test(a, b) {
 var c = 10;
  function d() {}
  var e = function _e() {};
  (function x() {});
  b = 20;
}
test(10);

AO(test) = {
  a: 10,
  b: undefined,
  c: undefined,
  d: <ref to func "d">
  e: undefined
};
// 函数表达式不会影响VO
// _e函数名不会影响到VO
```
** 例子 ** 
```javascript
// 函数声明覆盖了参数x
function foo(x, y, z){function x(){}; alert(x);} foo(100); 
// alert: function x(){}

// var func; 变量声明被忽略;
function foo(x, y, z){function func(){}; var func; console.log(func);} foo(100);
// ƒ func(){}

// var func=1; 为函数对象func重新赋值为1; 所以输出为1;
function foo(x, y, z){function func(){}; var func = 1; console.log(func);} foo(100);
// 1
```


#### 代码执行阶段

** 执行代码 ** 
```javascript
function test(a, b) {
  var c = 10;
  function d() {}
  var e = function _e() {};
  (function x() {});
  b = 20;
}

test(10);
```
** 初始化阶段 ** 
```javascript
AO(test) = {
 a: 10,
 b: undefined,
 c: undefined,
 d: <ref to func "d">
 e: undefined
};
```
** 执行阶段 ** 
```javascript
VO['c'] = 10;
VO['e'] = function _e() {};
VO['b'] = 20;

AO(test) = {
  a: 10,
  b: 20,
  c: 10,
  d: <reference to FunctionDeclaration "d">
  e: function _e() {};
};
```

#### 例子

_ 代码如下: _
```javascript
alert(x); // function

var x = 10;
alert(x); // 10
x = 20;

function x() {}
alert(x); // 20

if (true) {
  var a = 1;
} else {
  var b = true;
}

alert(a); // 1
alert(b); // undefined
```
_  变量初始化阶段步骤: _
1. 函数参数: 由于是在全局环境中, 忽略此步
2. 函数声明: 将function x声明放入VO中
3. 变量声明: 将var声明的变量都前置放入到全局的global VO变量中, 并初始化为undefined; 由于var x与函数x冲而被忽略.

_  执行阶段 _
1. alert(x); 在VO中x是声明的函数对象.
2. x = 10; 将VO中的x改变为Number(10)
3. alert(x); 输出number(10)
4. var a = 1; 为变量a赋值为1;
5. alert(b); b声明了但并未赋值, 所以仍为undefined


## OOP

** 面向对象程序设计(Object-oriented programming OOP)是一种程序设计范型，同时也是一种程序开发的方法。对象指的是类的实例，它将对象作为程序的基本单元，将程序和数据封装其中，以提高软件的重用性，灵活性和扩展性。 来自于 —-维基百科 ** 

### 概念与实现继承方式

** _ 实现继承的方式: _ **
1. 创建一个原型是Person.prototype的新对象, 作为Student的原型, 如此Student既继承了Person原型上的属性和方法, 又可扩展这个新对象,实现对Student原型的定制.
Student.prototype = Object.create(Person.prototype);
2. 改变Student原型上的构造器. 否则仍为从Person原型上继承的.
Person.Student.prototype.constructor = Student;

** Object.create()是ES5之后才支持的，在es5之前我们可以写一个模拟的方法. **
```javascript
if (!Object.create) {
    Object.create = function(proto) {
        function F() {};
        F.prototype = proto;
        return new F();
    };
}
```
----
** 通过指定父类的实例为子类的原型实现继承 (不推荐,  new per()在构造器的操作是没有意义的)**
```javascript
function per() {};

function sor() {};
sor.prototype = new per(); //per构造器中会有无意义的操作.
sor.prototype.constructor = sor;
```


#### 基于原型的继承

![基于原型的继承](http://kityminder-img.gz.bcebos.com/bafaa34ae89c96aea99949e83c79d64365e8b721)
```javascript
function Foo() {
    this.y = 2;
};
console.log(Foo.prototype); //object
Foo.prototype.x = 1;
var obj3 = new Foo();

console.log(obj3.y); //2
console.log(obj3.x); //1
```
函数声明创建Foo()函数，这个函数就会有一个内置的Foo.prototype，并且这个属性是对象，并且是预设的object。
![Foo.prototype](http://upload-images.jianshu.io/upload_images/3877962-81f6d095f48ed6dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当用new去调用函数的时候，那么构造器也就是说这样一个函数就会作为一个构造器来使用，并且this会指向一个对象，而对象的原型会指向构造器的Foo.prototype属性。obj3实际上会成为Foo构造器中的this，最后会作为返回值，并且在构造器里面调用的时候会把y赋值为2，并且obj3的原型，也就是他的proto会指向Foo.prototype内置属性，最后可以看到obj3.y会返回2，obj3.x会返回1，y是这个对象上的直接量，而x是原型链上的，也就是Foo.prototype的
![](http://upload-images.jianshu.io/upload_images/3877962-568a72e297d75aeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### prototype属性与原型


** 使用函数声明去中创建一个函数的时候，这个函数就会有一个prototype属性，并且他默认会有两个属性. **

1. 一个是constructor:Fooconstructor属性会指向它本身Foo。
2. 另外一个属性是`__proto__`，`__proto__`是Foo.prototype的原型，那么他的原型会指向Object.prototype也就是说一般的对象比如用花括号括起来的对象字面量，他也会有`__proto__`他会指向Object.prototype因此Object.prototype上面的一些方法比如说toString，valueOf才会被每一个一般的对象所使用，

#### 继承实例

** 继承实例 **
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
};

Person.prototype.hi = function() {
    console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
};

Person.prototype.legs_num = 2;
Person.prototype.arms_num = 2;
Person.prototype.walk = function() {
    console.log(this.name + "is walking...");
};

function Student(name, age, className) {
    Person.call(this, name, age);
    this.className = className;
};

//实现继承的方法:
//1. 创建一个原型是Person.prototype的新对象, 作为Student的原型, 如此Student既继承了Person原型上的属性和方法, 又可扩展这个新对象,实现对Student原型的定制.
Student.prototype = Object.create(Person.prototype);
//2. 改变Student原型上的构造器. 否则仍为从Person原型上继承的Person.
Student.prototype.constructor = Student;

Student.prototype.hi = function() {
    console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
};

Student.prototype.learn = function(subject) {
    console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
};

console.log(Student.prototype);

var t = new Student("黄继鹏", 23, "class2");
t.hi();
console.log(t.legs_num);
t.walk();
t.learn('math')
```

### 再谈原型链

** 返回对象原型`Object.getPrototypeOf(obj)` **

![再谈原型链](http://upload-images.jianshu.io/upload_images/3877962-37d17481b52e9489.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个图里面说明了上面代码例子的示意图

通过var peng = new Student("黄继鹏", 23, "class2");来创建了一个实例peng。

peng的实例他的原型我们用__proto__表示，就会指向构造器Student.prototype的属性。

Student.prototype上面有hi和learn方法，Student.prototype是通过Student.prototype = Object.create(Person.prototype);构造的，所以说Student.prototype是一个对象，并且的原型指向Person.prototype。

Person.prototype。也给她设置了很多属性，hi…等。

Person.prototype.hi = function() {
    console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
};
Person.prototype.hi其实是内置的普通对象，内置对象他本身也会有他的原型，他的原型就是Object.prototype。

也就是因为这样所以说随便一个对象才会有hasOwnProperty，valueOf，toString等一些公共的函数，这些函数都是从Object.prototype而来的。

当我们去调用

peng.hi();方法的时候，首先看这个对象上本身有没有hi方法，在本身没有所以会像上查找，差遭到peng原型也就是Student.prototype有这样一个函数方法，所以最终调用的是Student.prototype上面的hi方法。

如果Student.prototype不去写hi方法的时候peng.hi();会去调用Person.prototype.hi这样一个方法，

当我们调用peng.walk();的时候，先找peng上发现没有，然后Student.prototype上面，也没有，Person.prototype有walk所以最终调用结果是Person.prototype上面的walk方法。

那么我想去调用peng.toString的时候也是一层一层向上查找。找到Object.prototype那么最后到null也就是最根源没有了。

#### 特殊情况

** 并不是所有对象最终原型链上最终都有Object.prototype **
```javascript
var obj2=Object.create(null);
obj2.__proto__ //undefined
obj2.toString() //undefined
```
obj2.create(null)的作用是创建空对象，并且这个对象的原型指向这样一个参数，但是这里参数是null，obj2这个时候他的原型就是undefined，obj2.toString就是undefined那么通过Object.create(null)创建出来的对象，就没有Object.prototype的一些方法。所以说并不是所有的对象都继承Object.prototype

** 并不是所有的函数对象都有prototype这样一个预制属性的 **
```javascript
function abc() {};
console.log(abc.prototype);
var hh = abc.bind(null);
console.log(hh.prototype);
```
使用es5友谊和bind函数，bind函数是用来修改函数在运行时的this的，bind函数返回的也是一个函数，但是bind函数就没有prototype预设属性。



### prototype属性

** javascript中的prototype原型，不像java的class，是一旦写好了以后不太容易去动态改变的，但是javascript中原型实际上也是普通的对象，那么意味着在程序运行的阶段我们也可以动态的给prototype添加或者删除一些属性. **

```javascript
Student.prototype.x=101;
console.log(peng.x);//101

Student.prototype={y:2};
console.log(peng.y);//undefined
console.log(peng.x);//101

var nunnly=new Student("nunnly", 23, "class3");

console.log(nunnly.y);//2
console.log(nunnly.x);//undefined
```

1. 通过过Student.prototype.x=101;把huang的原型动态的添加一个属性x那么我们发现所有的实例都会受到影响，
2. 直接修改Student.prototype={y:2};构造器的属性, 并不能修改已经实例化的对象，也就是说已经实例化的peng他的原型已经指向当时的Student.prototype如果你修改了Student.prototype的话，并不会影响已经创建的实例.
3. 但是再去用new重新实例化对象，那么会发现x不见了，并且y是新的y值。

#### 内置构造器的prototype

** 为所有的对象都增加一个x属性, 并更改属性配置for...in时不枚举原型上的变量 ** 
```javascript
Object.defineProperty(Object.prototype, 'x', {writable: true,value: 1});
var obj = {
    y: 3
};
console.log(obj.x); //1
for (var key in obj) {
    console.log(key + "=" + obj[key]); //y=3
}
```
- value:属性的值给属性赋值
- writable:如果为false，属性的值就不能被重写。
- get: 一旦目标属性被访问就会调回此方法，并将此方法的运算结果返回用户。
- set:一旦目标属性被赋值，就会调回此方法。
- configurable:如果为false，则任何尝试删除目标属性或修改属性以下特性（writable, configurable, -enumerable）的行为将被无效化。
- enumerable:是否能在for…in循环中遍历出来或在Object.keys中列举出来。



#### 创建对象-new/原型链

```javascript
function foo(){}    //定义函数对象 foo
foo.prototype.z = 3;      //函数对象默认带foo.prototype对象属性  这个对象会作为new实例的对象原型  对象添加z属性=3

var obj =new foo();    //用构造器方式构造新的对象
obj.y = 2;    //通过赋值添加2个属性给obj
obj.x = 1;   //通过new去构造这样一个对象他的主要特点是，他的原型会指向构造器的foo.prototype属性

//一般foo.prototype对象他的原型又会指向Object.prototype
//Object.prototype他也会有他的原型最后指向null整个原型链的末端

obj.x; // 1  //访问obj.x发现对象上有x返回1
obj.y; // 2  //访问obj.y发现对象上有x返回2
obj.z; // 3  //obj上没有z并不会停止查找，会去查找他的原型foo.prototype.z返回3
typeof obj.toString; // ‘function'  这是一个函数，toString是Object.prototype上面的每个对象都有
'z' in obj; // true     obj.z是从foo.prototype继承而来的，所以说obj里面有z
obj.hasOwnProperty('z'); // false   表示z并不是obj直接对象上的，而是对象原型链上的。
```

### instanceof

** instanceof数据类型判断方法 **
```javascript
console.log([1, 2] instanceof Array); //true
console.log(new Object() instanceof Array); //false

'1' instanceof String //false 
String(1) instanceof String //false
new String(1) instanceof String //true
```
**左边要求是一个对象instanceof右边要求是一个函数或者说构造器他会判断右边的构造器的 prototype的属性是否出现在左边这个对象的原型链上。**

```javascript
function per() {};

function sor() {};
sor.prototype = new per();
sor.prototype.constructor = sor;

var peng = new sor();
var han = new per();
console.log(peng instanceof sor); //true
console.log(peng instanceof per); //true
console.log(han instanceof sor); //false
console.log(han instanceof per); //true
```


### OOP精碎

#### 模拟重载

** 模拟重载 **
```javascript
function person() {
    var args = arguments;
    if (typeof args[0] === 'object' && args[0]) {
        if (args[0].name) {
            this.name = args[0].name;
        }
        if (args[0].age) {
            this.age = args[0].age;
        }
    } else {
        if (args[0]) {
            this.name = args[0];
        }
        if (args[1]) {
            this.age = args[1];
        }
    }
};
person.prototype.toString = function() {
    return "姓名:" + this.name + "年龄:" + this.age
}

var peng = new person({
    name: "继小鹏",
    age: 23
});
console.log(peng.toString()); //姓名:继小鹏年龄:23

var peng1 = new person("是你", 23);
console.log(peng1.toString()); //姓名:是你年龄:23
```

#### 调用父类方法和子类重载父类方法

** 调用父类方法和子类重载父类方法 **
```javascript
function Person(name) {//基类
    this.name=name;
}

function Student(name,classname){   //学生类
    this.classname=classname;
    //子类调用父类的方法.
    person.call(this,name);
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

Person.prototype.init=function(){};

//子类重载父类的init方法
Student.prototype.init=function(){
  //子类实现中调用父类的方法.
  Person.prototype.init.apply(this, arguments);
}
```

#### 链式调用

** 链式调用 **
```javascript
function classman() {}
//给classman构造器prototype添加addClass属性方法
classman.prototype.addClass = function(str) {
    console.log('calss' + str + 'added');
    return this;
    //return this表示返回classman的实例因为返回了实例那么紧接着后面不需要加mang.ad
}
var mang = new classman();
mang.addClass('classA').addClass('classB').addClass('classC')

// calssclassAadded
// calssclassBadded
// calssclassCadded
```

#### 抽象类

** 抽象类 **
```javascript
function Detectorlse() {
    throw new Error("Abstract class can not be invoked directly!");
}
Detectorlse.detect = function() {
    console.log('Detcetion starting...');
}
Detectorlse.stop = function() {
    console.log('Detector stopped');
}
Detectorlse.init = function() {
    throw new Error("Error");
}

function linkDetector() {};
linkDetector.prototype = Object.create(Detectorlse.prototype)
linkDetector.prototype.constructor = linkDetector;

//...add methods to LinkDetector...
```

#### defineProperty(ES5)

```javascript
function Person(name) {
    Object.defineProperty(this, 'name', {
        value: name,
        enumerable: true
    });
};
Object.defineProperty(Person, 'arms_num', {
    value: 2,
    enumerable: true
});
Object.seal(Person.prototype);
Object.seal(Person);

function student(name, classname) {
    this.classname = classname;
    Person.call(this, name);
};
student.prototype = Object.create(Person.prototype);
student.prototype.constructor = student;
```

#### 模块化

** 定义简单模块化 ** 
```javascript
var moduleA;
moduleA=function(){
    var prop=1;
    function func(){};
    return {
        func:func,
        prop:prop
    }
}();
```
** 定义简单模块化2 **
```javascript
var moduleA;
moduleA = new function() {
    var prop = 1;

    function func() {};
    this.func = func;
    this.prop = prop;
}();
```

### 实践（探测器）

```javascript
(function(global) {
    function DetectorBase(configs) {
        if (!this instanceof DetectorBase) {
            throw new Error('Do not invoke without new.');
        }
        this.configs = configs;
        this.analyze();
    }

    DetectorBase.prototype.detect = function() {
        throw new Error('Not implemented');
    };

    DetectorBase.prototype.analyze = function() {
        console.log('analyze...');
        this.data = "###data###";
    };

    function LinkDetector(links) {
        DetectorBase.apply(this, arguments);
        if (!this instanceof LinkDetector) {
            throw new Error('Do not invoke without new.');
        }
        this.links = links;
    }

    function ContainerDetector(containers) {
        DetectorBase.apply(this, arguments);
        if (!this instanceof ContainerDetector) {
            throw new Error('Do not invoke without new.');
        }
        this.containers = containers;
    }

    // inherit first
    inherit(LinkDetector, DetectorBase);
    inherit(ContainerDetector, DetectorBase);

    // expand later
    LinkDetector.prototype.detect = function() {
        console.log('Loading data:' + this.data);
        console.log('link detection started.');
        console.log('Scaning links:' + this.links);
    };

    ContainerDetector.prototype.detect = function() {
        console.log('Loading data:' + this.data);
        console.log('link detection started.');
        console.log('Scaning containers:' + this.containers);
    };

    // prevent from being altered
    Object.freeze(DetectorBase);
    Object.freeze(DetectorBase.prototype);
    Object.freeze(LinkDetector);
    Object.freeze(LinkDetector.prototype);
    Object.freeze(ContainerDetector);
    Object.freeze(ContainerDetector.prototype);

    // export to global object
    Object.defineProperties(global, {
        LinkDetector: { value: LinkDetector },
        ContainerDetector: { value: ContainerDetector },
        DetectorBase: { value: DetectorBase }
    });

    function inherit(subClass, superClass) {
        subClass.prototype = Object.create(superClass.prototype);
        subClass.prototype.constructor = subClass;
    }
}(this));

var containerDetector = new ContainerDetector('#nav #banner #sidebar');
containerDetector.detect();

var linkDetector = new LinkDetector('https://www.alipay.com https://www.aliyun.com');
linkDetector.detect();
```

## 正则与模式匹配

** 正则表达式(regular expression)是一个描述字符模式的对象。ECMAScript的RegExp类表示正则表达式，而String和RegExp都定义了使用正则表达式进行强大的模式匹配和文本检索与替换的函数。**

一个简单的例子
```javascript
/\d\d\d/.test("123"); //true
/\d\d\d/.test("abc"); //false
new RegExp("Bosn").test("Hi, Bosn"); //true
```

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00755.jpeg)

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00756.jpeg)

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00757.jpeg)

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00758.jpeg)

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00759.jpeg)

![](http://pclib.github.io/safari/program/javascript-the-definitive-guide-cn/OEBPS/Images/image00760.jpeg)

### 三个flag

** _ global ignoreCase multiline _ **
```javascript
/abc/gim.test("ABC"); //true
RegExp("abc", "mgi"
```

修饰符|描述
-|-
i|执行对大小写不敏感的匹配。
g|执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
m|执行多行匹配。

```javascript
    var reg1 = new RegExp("lisong",'ig');
	var reg2 = new RegExp("lisong");
	var reg3 = /lisong/ig;
	var reg4 = /lisong/;
```

### RegExp对象的属性

** _ global ignoreCase multiline source _ ** 
```javascript
/abc/g.global  //true
/abc/g.ignoreCase  //false
/abc/g.multiline  //false
/abc/g.source  //"abc"
```

### RegExp 对象的方法

** _ compile, exec, test, toString _ *
```javascript
/abc/.exec("abcdef");  //"abc"
/abc/.test("abcde");  //true
/abc/.toString();  //"/abc/"

var reg = /abc/; 
reg.compile("def");
reg.test("def"); //true
```
方法|描述|FF|IE
-|-|-|-
compile|编译正则表达式。|1|4
exec|检索字符串中指定的值。返回找到的值，并确定其位置。|1|4
test|检索字符串中指定的值。返回 true 或 false。|1|4

### RegExp静态属性


![](http://img.blog.csdn.net/20170202215754180?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTQwOTA1MTk4Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
```javascript
var str = "123abc456lisong789hehe";
	var regExp = new RegExp("[a-z]+(\\d+)","g");
	console.log( regExp.exec(str) ); //["abc456", "456", index: 3, input: "123abc456lisong789hehe"]
	console.log(RegExp.input); //123abc456lisong789hehe
	console.log(RegExp.leftContext); //123
	console.log(RegExp.rightContext); //lisong789hehe
	console.log(RegExp.lastMatch); //abc456
	console.log(RegExp.lastParen); //456
	console.log(RegExp.multiline); //false
	console.log("-------------------------------")
	console.log( regExp.exec(str) ); //["lisong789", "789", index: 9, input: "123abc456lisong789hehe"]
	console.log(RegExp.leftContext); //123abc456
	console.log(RegExp.rightContext); //hehe
	console.log(RegExp.lastMatch); //lisong789
	console.log(RegExp.lastParen); //789
	console.log("-------------------------------")
	var regExp2 = /lisong/;
	regExp2.test("123lisong456");
	console.log( regExp.exec(str) ); //null,如果没匹配到不会改变静态属性
	console.log(RegExp.leftContext); //123abc456
	console.log(RegExp.rightContext); //hehe
	console.log(RegExp.lastMatch); //abc456
	console.log(RegExp.lastParen); //789
```

### String类型与正则相关的方法


```javascript
String.prototype.search
"abcabcdef".search(/(abc)\1/);  //0

String.prototype.replace 
"aabbbbcc".replace(/b+?/, "1"); //aa1bbbcc

String.prototype.match
"aabbbbccbbaa".match(/b+/);
//["bbbb", index: 2, input: "aabbbbccbbaa"]
"aabbbbccbbaa".match(/b+/g); 
//["bbbb", "bb"]

String.prototype.split
"aabbbbccbbaa".split(/b+/);
// ["aa", "cc", "aa"]
```

## Promise


```javascript
let checkLogin = function () {
  return new Promise(function (resolve,reject) {
    let flag = document.cookie.indexOf("userId")>-1?true:false;

    if(flag=true){
      resolve({
        status:0,
        result:true
      })
    }else{
      reject("error");
    }
  })
};

let getUserInfo = ()=>{
  return new Promise((resolve,reject)=>{
    let userInfo = {
      userId:"101"
    }
    resolve(userInfo);
  });
}

checkLogin().then((res)=>{
  if(res.status==0){
    console.log("login success");
    return getUserInfo();
  }
}).catch((error)=>{
  console.log(`errrs:${error}`)
}).then((res2)=>{
  console.log(`userId:${res2.userId}`)
});

Promise.all([checkLogin(),getUserInfo()]).then(([res1,res2])=>{
  console.log(`result1:${res1.result},result2:${res2.userId}`)
})
```
