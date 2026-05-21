# 一、JavaScript 核心八股（含案例讲解）

[← 返回目录](../README.md)

---

## 1. 变量类型 & 区别

**八股要点**

- 基本类型（7 种）：`String、Number、Boolean、Null、Undefined、Symbol、BigInt` → **值传递**
- 引用类型：`Object、Array、Function…` → **地址传递**（栈里存的是堆地址）

### 案例讲解：值传递 vs 引用传递

```javascript
// 基本类型：改 b 不影响 a
let a = 1;
let b = a;
b = 2;
console.log(a); // 1

// 引用类型：改的是同一块堆内存
const obj1 = { name: '张三' };
const obj2 = obj1;
obj2.name = '李四';
console.log(obj1.name); // '李四' —— 两个变量指向同一对象

// 面试常考：如何「断开」引用？
const copy = { ...obj1 }; // 浅拷贝，第一层独立
copy.name = '王五';
console.log(obj1.name); // 仍是 '李四'
```

**记忆口诀**：基本类型像「复印一张纸」，引用类型像「两个人拿着同一把钥匙开同一扇门」。

---

## 2. null 和 undefined

```javascript
let x;                    // undefined：声明了没赋值
console.log(x == null);   // true（== 会类型转换）
console.log(x === null);  // false

const user = { name: 'a' };
user.avatar = null;       // null：故意表示「没有头像」
```

---

## 3. var / let / const 与暂时性死区

```javascript
console.log(a); // undefined（var 提升）
var a = 1;

// console.log(b); // ReferenceError（let 在 TDZ 里）
let b = 2;

const PI = 3.14;
// PI = 3;        // 报错
const list = [1, 2];
list.push(3);     // ✅ const 锁的是「引用」，数组内容仍可改
```

---

## 4. 闭包

**八股**：内层函数引用外层变量，外层执行完后变量仍被保留。

### 案例 1：私有变量（模块化雏形）

```javascript
function createCounter() {
  let count = 0; // 外部访问不到
  return {
    inc() { return ++count; },
    get() { return count; },
  };
}
const c = createCounter();
c.inc(); // 1
c.inc(); // 2
// count 无法从外部直接改 —— 这就是闭包的「私有化」
```

### 案例 2：防抖（闭包 + 定时器）

```javascript
function debounce(fn, delay = 300) {
  let timer = null; // 被闭包「记住」
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

const onSearch = debounce((kw) => console.log('搜索:', kw), 500);
// 用户连续输入时，只有停手 500ms 后才真正请求
```

**注意**：闭包长期持有 DOM/大对象会导致内存泄漏，用完要 `clearTimeout`、解绑事件。

---

## 5. this 指向（5 种场景）

```javascript
// 1. 普通函数（非严格）→ window
function f() { console.log(this); }
f(); // window（浏览器）

// 2. 对象方法 → 调用者
const user = {
  name: 'Tom',
  say() { console.log(this.name); },
};
user.say(); // 'Tom'

// 3. new → 新实例
function Person(name) { this.name = name; }
const p = new Person('Amy'); // this 指向 p

// 4. 箭头函数 → 继承外层 this（没有自己的 this）
const obj = {
  name: 'Obj',
  fn: () => console.log(this), // 外层是全局/模块，不是 obj
  method() {
    const inner = () => console.log(this.name); // 'Obj'
    inner();
  },
};
obj.method();

// 5. call / apply / bind 手动指定
function greet(city) { console.log(`${this.name} @ ${city}`); }
greet.call({ name: 'Lee' }, '广州'); // Lee @ 广州
```

**面试陷阱**：`const fn = user.say; fn();` → this 丢失，变成 window/undefined，要用 `bind` 或箭头函数包一层。

---

## 6. call / apply / bind

```javascript
function sum(a, b, c) {
  return this.prefix + (a + b + c);
}
const ctx = { prefix: '结果：' };

sum.call(ctx, 1, 2, 3);      // '结果：6'，参数逐个传
sum.apply(ctx, [1, 2, 3]);   // 同上，参数是数组

const bound = sum.bind(ctx, 1); // 先绑 this，再「预填」第一个参数
bound(2, 3); // '结果：6'
```

---

## 7. 原型链

```javascript
function Animal(type) { this.type = type; }
Animal.prototype.speak = function () {
  return `${this.type} 叫了一声`;
};

function Dog(type) { Animal.call(this, type); }
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

const d = new Dog('狗');
console.log(d.speak());           // 狗 叫了一声
console.log(d instanceof Dog);    // true
console.log(d instanceof Animal); // true

// 查找顺序：d → Dog.prototype → Animal.prototype → Object.prototype → null
```

---

## 8. Promise 与事件循环

### 案例：执行顺序（必考）

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

// 输出：1 → 4 → 3 → 2
// 同步先跑完 → 清空微任务(Promise.then) → 再跑一个宏任务(setTimeout)
```

### 案例：async/await 本质是 Promise

```javascript
async function fetchUser() {
  try {
    const res = await fetch('/api/user'); // 暂停，等 Promise 完成
    const data = await res.json();
    return data;
  } catch (e) {
    console.error(e);
  }
}
// async 函数一定返回 Promise；await 后面跟的是 thenable
```

---

## 9. 深浅拷贝

```javascript
const origin = {
  a: 1,
  nested: { b: 2 },
  fn: () => {},
  date: new Date(),
};

// 浅拷贝：nested 仍共享
const shallow = { ...origin };
shallow.nested.b = 99;
console.log(origin.nested.b); // 99

// JSON 深拷贝：简单对象可用，函数/undefined/循环引用会丢
const jsonCopy = JSON.parse(JSON.stringify({ a: 1, nested: { b: 2 } }));

// 面试手写思路见 chapters/11-手写代码.md
```

---

## 10. 防抖 vs 节流（对比案例）

| | 防抖 debounce | 节流 throttle |
| --- | --- | --- |
| 思想 | 停下来的那一刻才执行 | 固定间隔最多执行一次 |
| 场景 | 搜索联想、resize 结束再布局 | 滚动监听、按钮防连点 |

```javascript
// 节流：1 秒内最多触发一次
function throttle(fn, interval = 1000) {
  let last = 0;
  return function (...args) {
    const now = Date.now();
    if (now - last >= interval) {
      last = now;
      fn.apply(this, args);
    }
  };
}
```

---

[下一章：ES6+ →](./02-ES6+.md)
