# 05 · JavaScript 基础

> 本节目标：补齐学习 Vue 3 之前必须的 JavaScript 底子。
>
> 学完本节，你应当能读懂 Vue 源码里出现的 `const`、`=>`、`解构`、`Promise`、`async/await`、`模块导入`等语法。
>
> 📚 系列目录见 [README.md](./README.md)

---

## 目录

- [1. 为什么要先学 JavaScript？](#1-为什么要先学-javascript)
- [2. 第一个 JS 程序](#2-第一个-js-程序)
- [3. 变量与声明方式](#3-变量与声明方式)
- [4. 数据类型](#4-数据类型)
- [5. 运算符速查](#5-运算符速查)
- [6. 控制流：分支与循环](#6-控制流分支与循环)
- [7. 函数](#7-函数)
- [8. 对象与数组](#8-对象与数组)
- [9. 字符串与模板字符串](#9-字符串与模板字符串)
- [10. 异步：回调 → Promise → async/await](#10-异步回调--promise--asyncawait)
- [11. ES6+ 模块化](#11-es6-模块化)
- [12. 解构、展开、剩余参数](#12-解构展开剩余参数)
- [13. 常用内置对象速查](#13-常用内置对象速查)
- [14. 调试与排错技巧](#14-调试与排错技巧)
- [15. 小结与自检](#15-小结与自检)

---

## 1. 为什么要先学 JavaScript？

Vue / React / Angular 这些"框架"只是**写法更舒服的 JavaScript**。看不懂 JS，就永远只能照搬模板。学完这一节，你会知道：

- `const app = createApp(...)` 里的 `app` 是什么；
- `import { ref } from 'vue'` 是什么意思；
- Vue 文档里突然出现的 `?.`、`??`、`=>` 都在做什么。

> 经验法则：先学"原生 JS"再学"框架"，不要反过来。

---

## 2. 第一个 JS 程序

### 2.1 浏览器里直接跑

打开 DevTools，切到 Console 面板，输入：

```js
console.log('Hello, JavaScript!')
let name = 'Tom'
console.log(`Hello, ${name}!`)   // 模板字符串
```

### 2.2 在 HTML 里跑

```html
<!DOCTYPE html>
<html>
<head><title>JS Demo</title></head>
<body>
  <h1 id="title">你好</h1>
  <script>
    document.getElementById('title').textContent = 'Hello!'
  </script>
</body>
</html>
```

> 注意：`<script>` 放在 `<body>` 末尾，否则 HTML 还没解析完就执行 JS 容易报错。

### 2.3 在 Node 里跑

新建 `hello.js`：

```js
console.log('Hello from Node!')
```

终端执行：`node hello.js`

---

## 3. 变量与声明方式

ES6 起，推荐只使用 `const` 和 `let`，**彻底告别 `var`**。

| 关键字 | 是否可重复声明 | 是否可重新赋值 | 作用域 | 推荐度 |
| --- | --- | --- | --- | --- |
| `var` | ✅ | ✅ | 函数级 | ❌ 历史遗留 |
| `let` | ❌ | ✅ | 块级 | ✅ 需要变就用它 |
| `const` | ❌ | ❌（但对象内容可变） | 块级 | ⭐⭐⭐ 默认用它 |

```js
const PI = 3.14159           // 常量：声明后不能再赋值
let count = 0                // 变量：可以重新赋值
count++                      // ✅
// PI = 3.14                 // ❌ TypeError

const user = { name: 'Tom' }
user.name = 'Jerry'          // ✅ 对象内部属性可以改
// user = { name: 'Bob' }   // ❌ 整个对象不能换
```

> 经验法则：**先 `const`，发现要变再改成 `let`，永远不用 `var`。**

---

## 4. 数据类型

JavaScript 一共有 **8 种数据类型**，分两类：

### 4.1 原始类型（Primitive）

| 类型 | 例子 | 说明 |
| --- | --- | --- |
| `number` | `42`, `3.14`, `NaN`, `Infinity` | 数字不分整数浮点 |
| `string` | `'hi'`, `"hi"`, `` `hi ${name}` `` | 字符串 |
| `boolean` | `true`, `false` | 布尔 |
| `null` | `null` | "有意为空" |
| `undefined` | `undefined` | "尚未赋值" |
| `symbol` | `Symbol('id')` | 唯一值（高级用法） |
| `bigint` | `123n` | 超大整数 |

### 4.2 引用类型

| 类型 | 例子 |
| --- | --- |
| `object` | 普通对象、数组、函数、Date、Map、Set …… |

> 关键区别：**原始类型按值传递、引用类型按引用传递**。

```js
let a = { n: 1 }
let b = a            // b 和 a 指向同一个对象
b.n = 2
console.log(a.n)     // 2  ← a 也被改掉了！
```

### 4.3 类型检测

```js
typeof 123             // 'number'
typeof 'abc'           // 'string'
typeof undefined       // 'undefined'
typeof null            // 'object'  ← JS 经典 bug
typeof []              // 'object'  ← 数组也是 object
Array.isArray([])      // true      ← 判断数组用它
```

更精确的类型检查用 `===`：

```js
value === null          // 判断 null
value === undefined     // 判断 undefined
typeof value === 'string'
```

---

## 5. 运算符速查

### 5.1 算术运算符

| 运算符 | 例子 | 结果 |
| --- | --- | --- |
| `+` | `1 + 2` | `3` |
| `-` `*` `/` `%` | 同理 | 算术 |
| `**` | `2 ** 3` | `8`（幂） |
| `++` / `--` | `i++` | 自增 / 自减 |
| `+`（字符串拼接） | `'a' + 'b'` | `'ab'` |

> ⚠️ 注意 `+`：遇到字符串会"全部当字符串"处理：

```js
1 + 2          // 3
1 + '2'        // '12'
'1' + 2        // '12'
1 + 2 + '3'    // '33'   ← 从左到右
```

### 5.2 比较运算符

| 运算符 | 含义 | 用法 |
| --- | --- | --- |
| `==` | 弱等于（**会做隐式转换**） | ❌ 不推荐 |
| `===` | 强等于（值 + 类型都相同） | ✅ 永远用它 |
| `!=` / `!==` | 不等于 | 同理 |

```js
0 == ''          // true   （两个都被转成 0）
0 === ''         // false
null == undefined// true   （这个是 JS 唯一例外）
null === undefined // false
```

### 5.3 逻辑运算符

```js
a && b    // a 为真才返回 b，否则返回 a
a || b    // a 为真就返回 a，否则返回 b
!a        // 取反

// 实战：给变量一个默认值（旧写法）
const name = input || '匿名'

// ?? 空值合并：只在 null/undefined 时取右侧
const port = config.port ?? 3000   // 0 不会触发
```

### 5.4 三元运算符

```js
const label = isLogin ? '退出' : '登录'
```

### 5.5 可选链 `?.`

```js
const city = user?.address?.city     // 任意一级为 null/undefined 就返回 undefined
user?.sayHello?.()                   // 函数可能不存在？照样安全调用
```

---

## 6. 控制流：分支与循环

### 6.1 分支

```js
if (score >= 90) {
  console.log('优秀')
} else if (score >= 60) {
  console.log('及格')
} else {
  console.log('加油')
}

// 多分支
switch (week) {
  case 1: console.log('周一'); break
  case 2: console.log('周二'); break
  default: console.log('其他')
}
```

### 6.2 循环

```js
// for
for (let i = 0; i < 5; i++) {
  console.log(i)
}

// while
let n = 0
while (n < 5) { n++ }

// for...of 遍历数组
const arr = [10, 20, 30]
for (const v of arr) console.log(v)   // 10 / 20 / 30

// for...in 遍历对象 key
const obj = { a: 1, b: 2 }
for (const k in obj) console.log(k, obj[k])

// 数组的高阶遍历（更推荐）
arr.forEach((v, i) => console.log(i, v))
const doubled = arr.map(v => v * 2)
const evens = arr.filter(v => v % 20 === 0)
const sum = arr.reduce((acc, v) => acc + v, 0)
```

> 经验法则：**能用 `for...of` 就别用 `for (let i...)`，能用 `map/filter/reduce` 就别手写循环。**

---

## 7. 函数

### 7.1 声明方式

```js
// 1. 函数声明（可提升）
function add(a, b) {
  return a + b
}

// 2. 函数表达式（不会提升）
const add = function (a, b) {
  return a + b
}

// 3. 箭头函数（最常用，简短）
const add = (a, b) => a + b
const double = x => x * 2      // 单参数可省括号
const hello = () => 'Hello'    // 无参数
```

### 7.2 参数

```js
// 默认参数
function greet(name = '游客') { return `Hi, ${name}` }

// 剩余参数（把多余的参数收成数组）
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0)
}
sum(1, 2, 3, 4)   // 10
```

### 7.3 箭头函数和普通函数的区别

| 维度 | 普通函数 | 箭头函数 |
| --- | --- | --- |
| `this` 绑定 | 动态（谁调用就是谁） | 词法（**定义时的外层 `this`**） |
| 能不能当构造函数 | ✅ | ❌ |
| 能不能当生成器 | ✅ | ❌ |
| 是否能作为方法 | ✅ | ❌（容易丢 `this`） |

```js
const timer = {
  count: 0,
  start() {
    setInterval(() => {
      this.count++       // ✅ 箭头函数继承 start 的 this
    }, 1000)
  }
}
```

---

## 8. 对象与数组

### 8.1 对象的增删改查

```js
const user = { name: 'Tom', age: 18 }

// 读
user.name             // 'Tom'
user['age']           // 18

// 增 / 改
user.email = 'tom@x.com'
user['gender'] = 'M'

// 删
delete user.gender

// 遍历
Object.keys(user)                     // ['name', 'age', 'email']
Object.values(user)                   // ['Tom', 18, 'tom@x.com']
Object.entries(user)                  // [['name','Tom'], ...]
for (const [k, v] of Object.entries(user)) console.log(k, v)
```

### 8.2 数组的高阶方法

| 方法 | 作用 | 例子 |
| --- | --- | --- |
| `map` | 映射出新数组 | `[1,2,3].map(x => x*2)` → `[2,4,6]` |
| `filter` | 过滤 | `[1,2,3,4].filter(x => x%2)` → `[1,3]` |
| `reduce` | 累加 | `[1,2,3].reduce((a,b)=>a+b,0)` → `6` |
| `find` | 找首个匹配 | `[1,2,3].find(x => x>1)` → `2` |
| `some` | 是否存在匹配 | `[1,2,3].some(x => x>2)` → `true` |
| `every` | 是否全部匹配 | `[2,4].every(x => x%2===0)` → `true` |
| `includes` | 是否包含 | `[1,2,3].includes(2)` → `true` |
| `sort` | 排序（**会修改原数组**） | `[3,1,2].sort((a,b)=>a-b)` |
| `slice` | 切片（不修改） | `[1,2,3,4].slice(1,3)` → `[2,3]` |
| `splice` | 增删改（**修改**） | 谨慎使用 |
| `flat` | 拍平 | `[1,[2,[3]]].flat(Infinity)` → `[1,2,3]` |

> **不要混淆 `slice` 和 `splice`**：slice 切片（安全），splice 手术刀（原地改）。

### 8.3 解构 + 展开（高频考点）

```js
const user = { name: 'Tom', age: 18, address: { city: 'Beijing' } }

// 解构 + 重命名 + 默认值
const { name: n, age, address: { city }, phone = '未知' } = user

const arr = [1, 2, 3, 4]
const [first, second, ...rest] = arr   // first=1, rest=[3,4]
```

---

## 9. 字符串与模板字符串

### 9.1 模板字符串（必会）

```js
const name = 'Tom'
const age = 18

// 旧写法（不推荐）
const s1 = 'Hello, ' + name + ', age ' + age

// 模板字符串
const s2 = `Hello, ${name}, age ${age}`
// 多行也可以
const s3 = `
  <div>
    <h1>${name}</h1>
  </div>
`
```

### 9.2 常用方法

| 方法 | 作用 | 例子 |
| --- | --- | --- |
| `length` | 长度 | `'abc'.length` → `3` |
| `includes` | 是否包含 | `'hello'.includes('he')` → `true` |
| `startsWith` / `endsWith` | 前缀/后缀 | 同上 |
| `indexOf` | 找位置，找不到返回 `-1` | `'abc'.indexOf('b')` → `1` |
| `slice` | 切片 | `'hello'.slice(1,4)` → `'ell'` |
| `split` | 分割 | `'a,b,c'.split(',')` → `['a','b','c']` |
| `trim` | 去首尾空白 | `'  hi  '.trim()` → `'hi'` |
| `toLowerCase` / `toUpperCase` | 大小写 | 同名 |
| `replace` / `replaceAll` | 替换 | `'hi'.replaceAll('i','ello')` → `'hello'` |
| `padStart` / `padEnd` | 补齐长度 | `'1'.padStart(2,'0')` → `'01'` |

---

## 10. 异步：回调 → Promise → async/await

**同步代码**：一行一行执行，上一行没完下一行不能开始。
**异步代码**：发起一个操作后**继续往下走**，等操作完再回头处理结果。

### 10.1 回调函数（旧写法，**容易回调地狱**）

```js
fs.readFile('a.txt', (err, data) => {
  if (err) return console.error(err)
  fs.readFile('b.txt', (err, data2) => {
    if (err) return console.error(err)
    // ... 一层套一层
  })
})
```

### 10.2 Promise（中间方案）

```js
fetch('/api/users')
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err))
  .finally(() => console.log('done'))
```

### 10.3 async/await（现代写法，**推荐**）

```js
async function loadUsers() {
  try {
    const res = await fetch('/api/users')
    const data = await res.json()
    console.log(data)
  } catch (err) {
    console.error('请求失败：', err)
  } finally {
    console.log('done')
  }
}
```

> 一句话：**`async` 让函数返回 Promise，`await` 让代码"等" Promise 完成，看起来像同步。**

### 10.4 并发：Promise.all / Promise.race

```js
// 同时发 3 个请求，全部完成后再继续
const [a, b, c] = await Promise.all([
  fetch('/api/a'),
  fetch('/api/b'),
  fetch('/api/c')
])

// 谁先回来就用谁
const fastest = await Promise.race([
  fetch('/api/a'),
  fetch('/api/b')
])
```

---

## 11. ES6+ 模块化

### 11.1 导出（export）

```js
// utils.js
export const PI = 3.14
export function add(a, b) { return a + b }
export default class User { /* ... */ }
```

### 11.2 导入（import）

```js
// 默认导入（一个文件只能有一个 default）
import User from './utils.js'

// 命名导入（按名字）
import { PI, add } from './utils.js'

// 重命名
import { add as sum } from './utils.js'

// 全部导入
import * as utils from './utils.js'
```

> **在 Vue 项目里 99% 用的都是命名导入**：`import { ref, computed } from 'vue'`。

### 11.3 为什么需要模块化？

想象 1000 行代码都堆在一个 `app.js` 里：

- 改一处要翻很久；
- 命名容易冲突；
- 没法被复用。

模块化把代码按"功能"拆成多个文件，按需引入、按需加载，是现代前端的基石。

---

## 12. 解构、展开、剩余参数

这三个语法 Vue 模板里几乎天天见，请务必熟练。

### 12.1 解构（Destructuring）

```js
// 对象解构
const { name, age = 18 } = { name: 'Tom' }
console.log(name, age)   // 'Tom' 18

// 数组解构
const [a, b, c] = [1, 2, 3]
// 嵌套
const { user: { address: { city } } } = { user: { address: { city: 'BJ' } } }
```

### 12.2 展开（Spread）

```js
// 数组
const a = [1, 2]
const b = [...a, 3, 4]              // [1, 2, 3, 4]

// 对象
const base = { a: 1, b: 2 }
const full = { ...base, c: 3 }      // { a:1, b:2, c:3 }
```

### 12.3 剩余（Rest）

```js
function f(first, ...rest) { console.log(first, rest) }
f(1, 2, 3, 4)    // 1, [2, 3, 4]

const [head, ...tail] = [1, 2, 3, 4]   // head=1, tail=[2,3,4]
```

---

## 13. 常用内置对象速查

| 对象 | 用途 | 高频方法 |
| --- | --- | --- |
| `JSON` | 序列化 | `JSON.parse(str)` / `JSON.stringify(obj)` |
| `Math` | 数学 | `Math.random()` / `Math.floor()` / `Math.max(...arr)` |
| `Date` | 日期 | `new Date()` / `date.toISOString()` |
| `Map` | 字典 | `set` / `get` / `has` / `size` |
| `Set` | 集合 | `add` / `has` / `size` |
| `URL` | URL 解析 | `new URL('https://a.com/x?y=1')` → `.searchParams.get('y')` |
| `localStorage` | 浏览器本地存储 | `setItem` / `getItem` / `removeItem` |
| `console` | 调试输出 | `log` / `warn` / `error` / `table` / `time` |

```js
// console 调试小技巧
console.log('值：', value)
console.table([{a:1}, {a:2}])    // 表格形式输出
console.time('loop')
for (let i = 0; i < 1e6; i++) {}
console.timeEnd('loop')          // 输出耗时
```

---

## 14. 调试与排错技巧

### 14.1 三大调试方式

| 方式 | 适用 |
| --- | --- |
| `console.log` | 80% 的情况都够用 |
| VS Code / Chrome 断点 | 复杂逻辑、循环、异步 |
| `try / catch` | 给可能崩的代码加兜底 |

### 14.2 常见报错与翻译

| 报错 | 含义 | 怎么办 |
| --- | --- | --- |
| `xxx is not defined` | 用了未声明的变量 | 检查拼写、是否漏 `import` |
| `xxx is not a function` | 不是函数 | 看是不是写错名字 / 类型不对 |
| `Cannot read properties of undefined` | 访问了 undefined 的属性 | 用 `?.`、先判空 |
| `Unexpected token` | 语法错误 | 检查括号、引号 |
| `Maximum call stack size exceeded` | 死循环 / 无限递归 | 检查退出条件 |

### 14.3 `use strict` 模式

在文件顶部加 `'use strict'`，会把一些不规范的写法变成错误。建议现代项目都默认开启（Vite 创建的项目里**已经默认开启**）。

---

## 15. 小结与自检

下面是这一节你应当能完成的小练习，建议全部手写一遍再进入下一节。

| # | 题目 | 关键考察点 |
| --- | --- | --- |
| 1 | 实现 `const sum = (a, b) => a + b` | 箭头函数 |
| 2 | 用解构从 `{ name, age, address: { city } }` 取出 `city` | 嵌套解构 |
| 3 | 写一个 `async` 函数 `getUser(id)`：调 `GET /api/users/${id}`，返回 JSON；失败要 catch | `async/await` + `try/catch` |
| 4 | 数组 `[1,2,3,4,5]` 过滤出偶数并乘 2，结果 `[4, 8]` | `filter` + `map` |
| 5 | 把 `{ a:1, b:2, c:3 }` 拼接成查询字符串 `?a=1&b=2&c=3` | `URLSearchParams` / `Object.entries` |
| 6 | 用 `localStorage` 存一个计数器，刷新页面后数值不丢 | `localStorage` + JSON |

### 推荐阅读

- MDN 官方教程（最权威）：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide>
- 现代 JavaScript 教程：<https://zh.javascript.info/>

> 学完这些之后，进入 [06 TypeScript 入门](./06-typescript-basics.md)。如果你时间紧，**先跳过 TS 直接进 [07 Vue 3 基础](./07-vue3-basics.md)** 也没问题，TS 任何时候都能补。
