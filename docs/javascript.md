# JavaScript

## 继承
[关于实例、构造函数、原型、原型链](https://www.cnblogs.com/leftJS/p/10943482.html)

- 原型链继承

  **特点：将父类的实例作为子类的原型**
  ```
  // 父类
  function Father(age) {
    this.name = '雪健'
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
    * 缺点 b：Son.prototype = new xx() 会覆盖之前 prototype, 无法多继承
  */
  Son.prototype = new Father() 


  /**
    *  缺点 c: 创建子类实例、无法向父类构造函数传参
  */
  const son = new Son()
  console.log(son.name) // 雪健
  console.log(son.getName()) // getName: 雪健


  /**
    * 缺点 d：修改原型属性 值后，所有实例属性全被修改
  */
  const son2 = new Son()
  son2.__proto__.name = 'xuejian' // 修改 原型 name的值
  console.log('这是实例二: ' + son2.name) // 这是实例二: xuejian
  console.log('这是实例一: ' + son.name)  // 这是实例一: xuejian

  ```

  - 优点

    1. 父类属性及方法、子类都可获取
    2. 简单、方便
  - 缺点

    1. 子类新增原型链上的方法及属性时、只能在 new 之后增加
    2. 无法多继承
    3. 创建子类实例时、无法向父类构造函数传参
    4. 同一原型属性, 被所有实例共享。一改全改
- 构造函数继承

  - 优点

    1. 解决所有实例共享 属性问题
    2. 可以多继承
    3. 创建子类时、可以向父类传参

  - 缺点

    1. 函数无复用、每个子类都会有父类函数的副本
- 实例继承
- 拷贝继承
- 组合式继承
- 寄生组合式继承