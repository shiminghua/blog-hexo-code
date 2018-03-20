---
title: es6对象的扩展
date: 2018-03-20 22:59:36
categories:
- es6
tags:
- js
---
# 扩展对象的功能性

ECMAScript 6 规范清洗定义了每一个类别的对象，对象的类别如下：

- 普通（Ordinary）对象 具有 JavaScript 对象所有的默认行为。
- 特异（Exotic）对象 具有某些与默认行为不符的内部行为。
- 标准（Standard）对象 ECMAScript 6 规范中定义的对象，例如，Array、Date 等。标准对象既可以是普通对象，也可以是特异对象。
- 内建对象 脚本开始执行时存在于 JavaScript 执行环境中的对象，所有标准对象都是内建对象。

## 对象字面量语法扩展

- 属性初始值的简写
- 对象方法的简写语法
- 可计算属性名（Computed Property Name）

```code
function createPerson(name, age) {
    return {
        // 属性简写
        name,
        age,
        // 对象方法的简写
        sayName() {
            return 'minghua';
        },
    };
}

const suffix = ' name';
const person = {
    ['first' + suffix]: 'minghua',
    ['last' + suffix]: 'shi',
};
console.log(person['first name']); // minghua
console.log(person['last name']); // shi
```

## 新增方法

- Object.is() 接受两个参数，如果这两个参数类型相同且具有相同的值，则返回true。
- Object.assign() 接受一个接收对象和任意数量的源对象，并返回接收对象。
  - 浅拷贝；
  - 排位靠后的源对象会覆盖排位靠前的对象的相同属性名的值；
  - 不能将提供者的访问器属性复制到接收对象中
  - 由于 Object.assign() 方法执行了赋值操作
  - 因此提供者的访问器属性最终会转变为接收对象中的一个数据属性。

```code
const receive = {};
const supplier = {
    get name() {
        reutrn 'file.js';
    }
};
Object.assign(receive, supplier);
const descriptor = Object.getWwnPropertyDescriptor(receive, 'name');
console.log(descriptor.value); // 'file.js'
console.log(descriptor.get); // undefined
```

## 重复的对象字面量属性

- ECMAScript 6 移除了重复属性检查
- 对于每一组重复属性，都会选取最后一个取值

## 自有属性枚举顺序

- 所有数字键按升序排序
- 所有字符串键按照他们被加入对象的顺序排序
- 所有 Symbol 键按照他们被加入对象的顺序排序
- Object.keys() 和 JSON.stringify() 与 for-in 循环使用相同的枚举顺序，都未指定一个明确的枚举顺序。

```code
const obj = {
    a: 1,
    0: 1,
    c: 1,
    2: 1,
    b: 1,
    1: 1,
}
obj.d = 1;

console.log(Object.getOwnPropertyNames(obj).join('')); // 012acbd
```

## 增强对象原型

### 改变对象的原型

- Object.getPrototypeOf(obj) 返回任意指定对象的原型
- Object.setPrototypeOf(obj, prototype) 改变任意指定对象的原型

```code
const person = {
    getGreeting() {
        return 'Hello';
    }
};

const dog = {
    getGreeting() {
        return 'Woof';
    }
};

// 以 person 对象为原型
const friend = Object.create(person);
console.log(friend.getGreeting()); // "Hello"
console.log(Object.getPrototypeOf(friend) === person); // true

// 将原型设置为 dog
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting()); // "Woof"
console.log(Object.getPrototypeOf(friend) === dog); // true
```

### 简化原型访问的 Super 引用

必须在对象的方法中使用 Supper 引用。

```code
const friend = {
    getGreeting() {
        // 正确
        return super.getGreeting() + ', hi!';
    },
    getGreeting: function() {
        // 语法错误
        return super.getGreeting() + ', hi!';
    },
};
```

```code
const person = {
  getGreeting() {
    return 'Hello';
  },
};

// 以 person 对象为原型
const friend = {
  getGreeting() {
    return super.getGreeting() + ', hi!';
  },
};
Object.setPrototypeOf(friend, person);

// 原型是 friend
const relative = Object.create(friend);

console.log(person.getGreeting()); // 'Hello'
console.log(friend.getGreeting()); // 'Hello hi!'
console.log(relative.getGreeting()); // 'Hello hi!'
console.log(Object.getPrototypeOf(person) === Object); // false
console.log(Object.getPrototypeOf(friend) === person); // true
console.log(Object.getPrototypeOf(relative) === friend); // true
```

## 正式的方法定义

- ECMAScript 6 中正式将方法定义为一个函数，它会有一个内部的[[HomeObject]]属性来容纳这个方法从属的对象。
- Super 的所有引用都通过[[HomeObject]]属性来确定后续的运行过程。
  1. 在[[HomeObject]]属性上调用 Object.getPrototypeOf() 方法来检索原型的引用
  2. 搜索原型找到同名函数
  3. 设置 this 绑定并调用相应的方法
