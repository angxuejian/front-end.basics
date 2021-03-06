# JavaScript

## 继承
[关于实例、构造函数、原型、原型链](https://www.cnblogs.com/leftJS/p/10943482.html)

- <span id='1'>**原型链继承**</span>

  **特点：将父类的实例作为子类的原型**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  function Son() {

  }


  /**
    * 缺点 a: new 之前在原型链创建的属性 或方法 都为 undefined 或 not a function 
  */
  Son.prototype.age = 12 
  Son.prototype.getAge = function() {
    return `getAge: ${this.age}`
  }


  /**
    * 缺点 b: Son.prototype = new xx() 会覆盖之前 prototype, 无法多继承
  */
  Son.prototype = new Father() 


  /**
    *  缺点 c: 创建子类实例、无法向父类构造函数传参
  */
  const son = new Son()
  console.log(son.name) // 雪健
  console.log(son.getName()) // getName: 雪健


  /**
    * 缺点 d: 修改原型属性 值后，所有实例属性全被修改
  */
  const son2 = new Son()
  
  // 示例一
  son2.name = 'xuejian' // 修改对象属性 值
  console.log('这是实例二: ' + son2.name) // 这是实例二: xuejian
  console.log('这是实例一: ' + son.name)  // 这是实例一: 雪健

  // 示例二
  son2.list.push(5)
  console.log('这是实例二：' + son2.list) // 这是实例二：[1, 2, 3, 4, 5]
  console.log('这是实例一：' + son.list)  // 这是实例一：[1, 2, 3, 4, 5]

  // Q 为什么示例一中的 name 的值没有改变呢？
  
  // A 当实例对象上出现与原型同名属性时、会屏蔽原型属性。
  //   所以示例一中的 son2.name = 'xuejian' 只是添加到实例对象中、并未改变原型上的name

  ```

  - 优点

    1. 父类属性及方法、子类都可获取
    2. 简单、方便
  - 缺点

    1. 子类新增原型链上的方法及属性时、只能在 new 之后增加
    2. 无法多继承
    3. 创建子类实例时、无法向父类构造函数传参
    4. 同一原型属性, 被所有实例共享。一改全改

<br>
<br>

- <span id='2'>**构造函数继承**</span>

  **特点：将父类实例属性、方法 call到子类上(并未使用到原型)**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  function Son() {
    /**
      * 缺点 a: call时会复制一份副本
    */
    Father.call(this)
  }
  const son = new Son()


  /**
    * 缺点 b: 实例原型不等于父类原型、无继承
  */
  console.log(son.__proto__ === Father.prototype) // false


  /**
    * 缺点 c: 实例等于子类、不等于父类
  */
  console.log(son instanceof Son)    // true
  console.log(son instanceof Father) // false

  ```

  - 优点

    1. 解决所有实例共享 属性问题
    2. 可以多继承(call多个)
    3. 创建子类实例时、可以向父类构造函数传参(call时 传参)

  - 缺点

    1. 函数无复用、每个子类都会有父类函数的副本
    2. 只能使用实例属性、方法。并未继承原型
    3. 只是子类的实例、并不是父类的实例

<br>
<br>

- **实例继承(原型式继承)**
  
  **特点：基于已有原型对象、创建新实例; (将父类实例 作为子类实例返回, Object.create()同理)**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  function Son() {
    const father = new Father()

    /**
      * 缺点 a: 一次只能返回一个、不支持多继承
    */
    return father
  }
  const son = new Son()
  

  /**
    * 缺点 b: 是父类的实例、不是子类的实例
  */
  console.log(son instanceof Son)    // false
  console.log(son instanceof Father) // true

  ```

  - 优点

    1. 是否 new, 返回的对象具有相同效果

  - 缺点

    1. 不支持多继承
    2. 是父类的实例、并不是子类的实例

<br>
<br>

- **拷贝继承**

  **特点：循环拷贝父类实例属性及方法**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  function Son() {
    const father = new Father()

    /**
      * 缺点 a: 循环效率低
      * 缺点 b: for in 无法获取不可枚举属性
    */
    for (const key in father) {
      Son.prototype[key] = father[key]
    }
  }

  const son = new Son()
  ```

  - 优点

    1. 支持多继承

  - 缺点

    1. 效率低、循环拷贝占用内存
    2. 无法获取不可枚举属性

<br>
<br>

- <span id='3'>**组合式继承**</span>

  **特点：结合原型链继承 + 构造继承**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }


  // 子类
  function Son() {
    Father.call(this) // 第二次调用父类
  }

  Son.prototype = new Father() // 第一次调用父类
  Son.prototype.constructor = Son // 指向 自身

  const son = new Son()
  console.log(son instanceof Father) // true
  console.log(son instanceof Son)   // true
  ```

  - 优点

    1. 解决所有实例共享 属性问题
    2. 可以多继承(call多个)
    3. 创建子类实例时、可以向父类构造函数传参(call时 传参)
    4. 实例即使父类的也是子类的
    5. 函数复用

  - 缺点
    
    1. 调用两次父类构造函数、生成两次实例

<br>
<br>

- <span id='4'>**寄生式继承**</span>

  **特点：创建一个封装继承过程的函数**
  ```
  // 父类
  function Father() {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 创建实例
  function CreateProto(obj) {
    function Proto() {}
    Proto.prototype = obj
    return new Proto()
  }

  // 封装继承过程的函数
  function CreateClass(data) {
    const obj = CreateProto(data)

    // 增强该实例的 方法
    obj.getList = function() {
      console.log(this.list)
    }

    return obj
  }

  const father = new Father()
  const son = CreateClass(new Father)
  son.getList() // [1, 2, 3, 4]

  /**
    * 缺点 a: 函数无复用
  */
  console.log(son.getName === father.getName)
  ```

  - 优点

    1. 解决所有实例共享 属性问题
    2. 可传参

  - 缺点

    1. 函数无复用

<br>
<br>

- <span id='5'>**寄生组合式继承**</span>

  **特点：结合组合继承 + 寄生式继承**
  ```
  // 父类
  function Father() {
    this.name = '雪健'
    this.list = [1, 2, 3, 4]
    this.getName = function() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  function Son() {
    Father.call(this)
  }

  // 创建实例
  function CreateProto(obj) {
    function Proto() {}
    Proto.prototype = obj
    return new Proto()
  }

  Son.prototype = CreateProto(Father.prototype)
  Son.prototype.constructor = Son // 指向自身

  const son = new Son()
  console.log(son instanceof Father) // true
  console.log(son instanceof Son)    // true

  ```

  - 优点

    1. 都说完美！

  - 缺点

    1. 需要沉淀！

  - 步骤
    
    1. [原型链继承](#1) + [构造继承](#2) = [组合继承](#3)
    2. [寄生式继承](#4)
    3. [寄生式继承](#4) + [组合继承](#3) = [寄生组合继承](#5)

<br>
<br>

- **ES6 Class继承**

  **特点：ES5继承的语法糖**

  ```
  // 父类
  class Father {
    constructor(name = '雪健') {
      this.name = name
      this.list = [1, 2, 3, 4]
    }

    getName() {
      return `getName: ${this.name}`
    }
  }

  // 子类
  class Son extends Father {
    constructor() {
      super() // 必须调用、否则调用this会报错
    }

    getList() {
      return `getList: ${this.list}`
    }
  }

  const son = new Son()
  ```

  - ES6 与 ES5继承有什么区别？

    1. ES6: 创建父类、通过extends继承后、调用super 获取父类this、获取this后修改子类

    2. ES5: 创建子类实例、通过prototype将父类原型属性赋值给子类实例
