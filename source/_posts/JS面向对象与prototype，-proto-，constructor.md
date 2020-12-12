---
title: JS面向对象与prototype，__proto__，constructor
date: 2020-12-12 18:08:08
tags: [原型链, 面向对象]
categories: JavaScript
---


## 一、Java中的面向对象与继承

1. 下面代码中，我们定义了一个小狗类，在类中定义了一个属性和两个方法，一个构造方法用于初始化小狗的年龄 age，一个公有方法 say 用于打印。

```javascript
public class Puppy{
    int puppyAge;
    
    public Puppy(age){
      puppyAge = age;
    }
    
    public void say() {
      System.out.println("汪汪汪"); 
    }
}
```
<br>

2. 这是一个通用的类，当我们需要一个两岁的小狗的实例是这样写的，这个实例同时具有父类的方法。

```javascript
Puppy myPuppy = new Puppy(2);
muPuppy.say(); // 汪汪汪
```
<br>

3. 以上的类和实例的实现均基于 java 的语法来的，但是相比于相对完善的 java 语法来说，早期的 js 没有 class 关键字啊（以下说 js 没有 class 关键字都是指 ES6 之前的 js ，主要帮助大家理解概念）。JS为了支持面向对象，使用了一种比较曲折的方式，具体如下。

## 二、JS中的面向对象与继承


1. **没有 class，用函数代替** ：早期的 js 没有 class 关键字，是怎么办的呢？对，是用函数来代替，函数不仅能执行普通功能，还能当 class 使用，栗子如下。

```javascript
function Puppy() {}
```
<br>

2. 以上代码实现了一个函数。下面我们就可以生成以上函数的实例了。

> 构造函数本身就是一个函数，与普通函数没有任何区别，不过为了规范一般将其首字母大写。构造函数和普通函数的区别在于，使用 new 生成实例的函数就是构造函数，直接调用的就是普通函数。

```javascript
let myPuppy = new Puppy()
```
<br>

3. **函数本身就是构造函数** ：虽然我们有了小狗的实例，但是不像 java 语法似的可以在类中定义构造函数来不能设置小狗的年龄啊。不慌，其实，充当类使用的函数本身就是构造函数，而且它就是默认的构造函数，下面我们重写以上代码，让构造函数接收函数来初始化小狗的年龄 age 。

```javascript
// 构造函数：可接收参数来初始化属性值
function Puppy(age) {
  this.puppyAge = age
}

// 实例化时可以传年龄参数了
let myPuppy = new Puppy(2)
```
<br>

4. **构造函数中的 this 指向实例化对象** ：构造函数中的 this 指向需要注意：被作为类使用的函数里面 this 总是指向实例化对象，也就是 myPuppy 。这么设计的目的就是让使用者可以通过构造函数给实例对象设置属性，这时候打印出来看 myPuppy.puppyAge 就是 2 。

```javascript
console.log(myPuppy.puppyAge)   // 2
```
<br>

5. **prototype 上定义实例方法** ：以上 4 点，我们实现了构造函数定义以及实例化。java 语法可以直接在类中定义公共方法来让实例小狗汪汪汪，js 如何办呢？对此，js 给出的解决方案是给构造方法添加一个 `prototype` 属性，挂载在这上面的方法，实例化时就会给到实例对象。

```javascript
// 在构造函数的 prototype 上添加方法
Puppy.prototype.say = function() {
  console.log("汪汪汪")
}

// 实例对象调用相应方法
myPuppy.say()    // 汪汪汪
```
<br>

6. **实例方法的查找用 proto** ：以上可能有的同学就会有疑问了，方法在构造函数的 `prototype` 上，实例对象 myPuppy 怎么会找到 say 方法了呢？我们来打印 myPuppy 。

（1）当你访问一个对象上没有的属性时，比如 myPuppy.say，对象会去 `__proto__` 查找。 `__proto__` 的值就等于父类的 prototype , `myPuppy.__proto__` 指向了 Puppy.prototype。

![实例方法的查找](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143617741-537961056.png)

（2）如果你访问的属性在 `Puppy.prototype` 也不存在，那又会继续往 `Puppy.prototype.__proto__` 上找，这时候其实就找到了 `Object.prototype` 了，`Object.prototype` 再往上找就没有了，也就是 null，这其实就是 `原型链`。

![](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143646241-1354675868.png)

<br>

7. **constructor** ：

（1）每个实例都有一个 constructor（构造函数）属性，该属性指向对象本身。

![](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143656225-984188307.png)

（2）prototype.constructor 是 prototype 上的一个保留属性，这个属性就指向类函数本身，用于指示当前类的构造函数。

![](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143703547-376698011.png)

（3）既然 prototype.constructor 是指向构造函数的一个指针，那我们是不是可以通过它来修改构造函数呢？我们来试试就知道了。我们先修改下这个函数，然后新建一个实例看看效果

```javascript
function Puppy(age) {
  this.puppyAge = age;
}

Puppy.prototype.constructor = function myConstructor(age) {
  this.puppyAge2 = age + 1;
}

const myPuppy2 = new Puppy(2);
console.log(myPuppy2.puppyAge);    // 2
```
上例说明，我们修改 `prototype.constructor` 只是修改了这个指针而已，并没有修改真正的构造函数。

（4）上面我们其实已经说清楚了 `prototype`，`__proto__`，`constructor` 几者之间的关系，下面画一张图来更直观的看下

![关系图](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143713505-1807018330.png)

<br>

8. **静态方法** ：我们知道很多面向对象有静态方法这个概念，比如 java 直接是加一个 static 关键字就能将一个方法定义为静态方法。js  中定义一个静态方法更简单，直接将它作为类函数的属性就行。

```javascript
// 在构造函数上定义静态方法 statciFunc
Puppy.statciFunc = function() {
  console.log('我是静态方法，this拿不到实例对象')
}      

// 直接通过类名调用
Puppy.statciFunc(); 
```

<br>

9. **继承**：面向对象怎么能没有继承呢，根据前面所讲的知识，我们其实已经能够自己写一个继承了。所谓继承不就是子类能够继承父类的属性和方法吗？换句话说就是子类能够找到父类的 `prototype` ，最简单的方法就是子类原型的 `__proto__` 指向父类原型就行了。

（1）以下继承方法只是让 Child 访问到了 Parent 原型链，但是没有执行 Parent 的构造函数

```javascript
function Parent() {}
function Child() {}

Child.prototype.__proto__ = Parent.prototype;

const obj = new Child();
console.log(obj instanceof Child );   // true
console.log(obj instanceof Parent );   // true
```

（2）为了解决上述问题，我们不能单纯的修改 `Child.prototype.__proto__` 指向，还需要用 new 执行下 Parent 的构造函数。

```javascript
function Parent() {
  this.parentAge = 50;
}
function Child() {}

Child.prototype.__proto__ = new Parent();

const obj = new Child();
console.log(obj.parentAge);    // 50
```
（3）上述方法会多一个 `__proto__` 层级，可以换成修改 `Child.prototype` 的指向来解决，注意将 `Child.prototype.constructor` 重置回来。

```javascript
function Parent() {
  this.parentAge = 50;
}
function Child() {}

Child.prototype = new Parent();
// 注意重置constructor
Child.prototype.constructor = Child;

const obj = new Child();
console.log(obj.parentAge);   // 50
```
<br>

10. **自己实现一个new**：结合上面讲的，我们知道 new 其实就是生成了一个对象，这个对象能够访问类的原型，知道了原理，我们就可以自己实现一个 new 了。

```javascript
function myNew(func, ...args) {
    // 新建一个空对象
  const obj = {};     
  // 执行构造函数
  func.call(obj, ...args);  
  // 设置原型链
  obj.__proto__ = func.prototype;    

  return obj;
}

function Puppy(age) {
  this.puppyAge = age;
}

Puppy.prototype.say = function() {
  console.log("汪汪汪");
}

const myPuppy3 = myNew(Puppy, 2);

console.log(myPuppy3.puppyAge);  // 2
console.log(myPuppy3.say());     // 汪汪汪
```

<br>

11. **自己实现一个 instanceof**：知道了原理，其实我们也知道了 instanceof 是干啥的。instanceof 不就是检查一个对象是不是某个类的实例吗？换句话说就是检查一个对象的的原型链上有没有这个类的 prototype ，知道了这个我们就可以自己实现一个了

```javascript
function myInstanceof(targetObj, targetClass) {
  // 参数检查
  if(!targetObj || !targetClass || !targetObj.__proto__ || !targetClass.prototype){
    return false;
  }

  let current = targetObj;

  while(current) {   // 一直往原型链上面找
    if(current.__proto__ === targetClass.prototype) {
      return true;    // 找到了返回true
    }

    current = current.__proto__;
  }

  return false;     // 没找到返回false
}

// 用我们前面的继承实验下
function Parent() {}
function Child() {}

Child.prototype.__proto__ = Parent.prototype;

const obj = new Child();
console.log(myInstanceof(obj, Child) );   // true
console.log(myInstanceof(obj, Parent) );   // true
console.log(myInstanceof({}, Parent) );   // false
```

<br>

## 三、ES6的 class

ES6 的 class 就是前面说的函数类的语法糖，比如我们的 Puppy 用 ES6 的 class 写就是这样

```javascript
class Puppy {
  // 构造函数
  constructor(age) {            
    this.puppyAge = age;
  }

  // 实例方法
  say() {
    console.log("汪汪汪")
  }

  // 静态方法
  static statciFunc() {
    console.log('我是静态方法，this拿不到实例对象');
  }
}

const myPuppy = new Puppy(2);
console.log(myPuppy.puppyAge);    // 2
console.log(myPuppy.say());       // 汪汪汪
console.log(Puppy.statciFunc());  // 我是静态方法，this拿不到实例对象
```

> 使用class可以让我们的代码看起来更像标准的面向对象，构造函数，实例方法，静态方法都有明确的标识。但是他本质只是改变了一种写法，所以可以看做是一种语法糖，如果你去看babel编译后的代码，你会发现他其实也是把class编译成了我们前面的函数类，extends关键字也是使用我们前面的原型继承的方式实现的。

<br>

## 四、总结

1. JS中的函数可以作为函数使用，也可以作为类使用

2. 作为类使用的函数实例化时需要使用new

3. 为了让函数具有类的功能，函数都具有`prototype`属性。

4. 为了让实例化出来的对象能够访问到prototype上的属性和方法，实例对象的 `__proto__` 指向了类的 `prototype`。所以`prototype`是函数的属性，不是对象的。对象拥有的是`__proto__`，是用来查找`prototype`的。

5. `prototype.constructor`指向的是构造函数，也就是类函数本身。改变这个指针并不能改变构造函数。

6. 对象本身并没有`constructor`属性，你访问到的是原型链上的`prototype.constructor`。

7. 函数本身也是对象，也具有`__proto__`，他指向的是JS内置对象Function的原型 Function.prototype 。所以你才能调用func.call, func.apply这些方法，你调用的其实是 Function.prototype.call 和 Function.prototype.apply 。

8. `prototype`本身也是对象，所以他也有`__proto__`，指向了他父级的prototype。`__proto__`和`prototype`的这种链式指向构成了JS的原型链。原型链的最终指向是Object的原型。Object上面原型链是null，即 `Object.prototype.__proto__ === null`。

9. 另外评论区有朋友提到：`Function.__proto__ === Function.prototype `。这是因为JS中所有函数的原型都是 Function.prototype ，也就是说所有函数都是 Function 的实例。Function 本身也是可以作为函数使用的---- Function()，所以他也是 Function 的一个实例。类似的还有Object，Array等，他们也可以作为函数使用: Object(), Array() 。所以他们本身的原型也是Function.prototype，即 `Object.__proto__ === null Function.prototype` 。换句话说，这些可以 new 的内置对象其实都是一个类，就像我们的 Puppy 类一样。

10. ES6 的 class 其实是函数类的一种语法糖，书写起来更清晰，但原理是一样的。


![关系图](https://img2020.cnblogs.com/blog/1855591/202010/1855591-20201013143726912-1477686856.png)


<br><br><br>

> [轻松理解JS中的面向对象，顺便搞懂prototype和__proto__](https://www.toutiao.com/i6797216661217739275/?tt_from=mobile_qq&utm_campaign=client_share&timestamp=1585615280&app=news_article&utm_source=mobile_qq&utm_medium=toutiao_android&req_id=20200331084119010131074200316BEA8F&group_id=6797216661217739275)、
[JS 系列二：深入 constructor、prototype、__proto__、[[Prototype]] 及 原型链](https://juejin.im/post/6844903924290289671#heading-11)


