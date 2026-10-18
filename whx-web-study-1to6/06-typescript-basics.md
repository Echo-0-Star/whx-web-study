# 06 · TypeScript 入门

> 本节目标：理解 TypeScript 解决了什么问题，能在 Vue 项目里独立写出带类型的组件。
>
> 学完本节，你应当能用 TS 重写一段普通 JS，并在 `<script setup lang="ts">` 中为 ref / props / 事件标注类型。
>
> 📚 系列目录见 [README.md](./README.md)

---

## 目录

- [1. 为什么要学 TypeScript？](#1-为什么要学-typescript)
- [2. 快速上手：把 JS 改成 TS](#2-快速上手把-js-改成-ts)
- [3. 基础类型](#3-基础类型)
- [4. 联合类型与字面量类型](#4-联合类型与字面量类型)
- [5. 接口（Interface） vs 类型别名（Type Alias）](#5-接口interface-与类型别名type-alias)
- [6. 函数类型](#6-函数类型)
- [7. 泛型（Generics）](#7-泛型generics)
- [8. 工具类型速查](#8-工具类型速查)
- [9. 类型守卫与 narrowing](#9-类型守卫与-narrowing)
- [10. 常见类型错误与修正](#10-常见类型错误与修正)
- [11. TypeScript 在 Vue 3 中的实战](#11-typescript-在-vue-3-中的实战)
- [12. tsconfig.json 最小配置](#12-tsconfigjson-最小配置)
- [13. 小结与自检](#13-小结与自检)

---

## 1. 为什么要学 TypeScript？

**TypeScript（TS）= JavaScript + 类型系统**。它在 JS 之上加了一层"类型注解"，让编辑器在写代码时就能发现错误。

### 1.1 三个最直观的收益

| 收益 | 例子 |
| --- | --- |
| **编辑器智能提示** | 写 `user.` 时，编辑器弹出 `.name` / `.age` 让你选 |
| **早期发现错误** | 错把 `user.nmae` 拼错时立即红波浪线，**不用等到运行时** |
| **重构更安全** | 把 `user.age` 改成 `user.userAge`，所有引用处会一次性提示 |

### 1.2 TS 的本质

- **不是新语言**：TS 会被编译器 `tsc` 编译成普通 JS 运行；
- **是 JS 的超集**：合法的 JS 一定是合法的 TS；
- **学习成本**：能写 JS 就能学 TS，难度主要在"理解类型"，不在"学语法"。

> 一句话：**TS 不会改变 JS 的运行结果，只会在写代码时多一份"校验"。**

---

## 2. 快速上手：把 JS 改成 TS

### 2.1 最小例子

```js
// sum.js
function add(a, b) {
  return a + b
}
add(1, 2)        // 3
add('1', 2)      // '12'  ← 拼字符串，可能不是你想要的
```

```ts
// sum.ts
function add(a: number, b: number): number {
  return a + b
}
add(1, 2)        // 3
add('1', 2)      // ❌ 编译报错：'1' 不是 number
```

### 2.2 怎么跑 TS？

```bash
# 1. 装编译器和运行时
npm install -g typescript tsx

# 2. 单文件直接跑（推荐学习阶段）
tsx hello.ts

# 3. 或先编译再跑
tsc hello.ts       # 生成 hello.js
node hello.js
```

> 在真实项目里，Vite / Webpack 都会**自动**处理 TS，你不用自己手动 `tsc`。

---

## 3. 基础类型

```ts
// 原始类型
const age: number = 18
const name: string = 'Tom'
const isLogin: boolean = true
const nothing: null = null
const notSet: undefined = undefined

// 数组
const nums: number[] = [1, 2, 3]
const strs: Array<string> = ['a', 'b']

// 元组（长度固定、类型固定的数组）
const point: [number, number] = [10, 20]

// 对象
const user: { name: string; age: number } = { name: 'Tom', age: 18 }

// 任意类型（不推荐，能不用就不用）
const anything: any = '随便什么都行'

// unknown（比 any 安全，使用前必须先判断类型）
const input: unknown = getData()
if (typeof input === 'string') input.toUpperCase()   // ✅
```

> 经验法则：**默认禁止 `any`**，必要时用 `unknown`，再配合类型守卫使用。

---

## 4. 联合类型与字面量类型

### 4.1 联合类型（Union）

```ts
// 一个变量可能是多种类型之一
let id: number | string = 123
id = 'abc'        // ✅
id = true         // ❌

// 用 type 别名让代码更可读
type ID = number | string
const userId: ID = 'u_001'
```

### 4.2 字面量类型（Literal）

```ts
// 限定值只能是某几个固定选项
type Direction = 'up' | 'down' | 'left' | 'right'
let move: Direction = 'up'
move = 'forward'  // ❌ 只能是那 4 个

type Status = 0 | 1 | 2
```

> 联合 + 字面量是 TS 里**模拟枚举**的现代写法，比 `enum` 更轻量。

---

## 5. 接口（Interface） vs 类型别名（Type Alias）

两者 90% 情况下可以互换。社区共识：**描述对象形状用 interface，其他场景用 type**。

```ts
// interface：描述对象/类的"形状"
interface User {
  id: number
  name: string
  age?: number            // ? 表示可选
  readonly createdAt: Date // 只读
}

// type：可以是任何东西
type ID = number | string
type Direction = 'up' | 'down'
type UserPair = [User, User]

// interface 继承
interface Admin extends User {
  role: 'admin'
}

// type 交叉（合并）
type Admin = User & { role: 'admin' }
```

> 经验法则：**Vue 组件的 `defineProps` / `defineEmits` 推荐用 interface 描述**。

---

## 6. 函数类型

### 6.1 参数和返回值

```ts
function add(a: number, b: number): number {
  return a + b
}

// 可选参数 + 默认值
function greet(name: string, greeting = 'Hello'): string {
  return `${greeting}, ${name}`
}

// 箭头函数
const add = (a: number, b: number): number => a + b
```

### 6.2 函数类型表达式

```ts
type MathFn = (a: number, b: number) => number

const add: MathFn = (x, y) => x + y
const mul: MathFn = (x, y) => x * y
```

### 6.3 void / never

```ts
// 没有返回值
function log(msg: string): void {
  console.log(msg)
}

// 永远不会结束（抛错 / 死循环）
function fail(msg: string): never {
  throw new Error(msg)
}
```

---

## 7. 泛型（Generics）

**泛型 = 类型的"参数化"**，让一段代码能复用于多种类型而不丢失类型信息。

### 7.1 一个例子

```ts
// 不使用泛型 → 退化为 any
function identity(x: any) { return x }
identity(123).toFixed       // any 没有提示

// 使用泛型 → 类型被参数化
function identity<T>(x: T): T { return x }
identity(123).toFixed       // ✅ number 的方法自动提示
identity('hi').toUpperCase  // ✅ string 的方法自动提示
```

### 7.2 数组、对象、异步里的泛型

```ts
const list: Array<number> = [1, 2, 3]
const [first, ...rest] = list   // first: number, rest: number[]

async function fetchJson<T>(url: string): Promise<T> {
  const res = await fetch(url)
  return res.json() as T
}

interface ApiResult<T> {
  code: number
  data: T
  message: string
}

// 调用时指定 T
type User = { id: number; name: string }
const r = await fetchJson<ApiResult<User[]>>('/api/users')
r.data[0].name   // ✅ 完全类型安全
```

### 7.3 泛型约束

```ts
// 限制 T 必须有 length 属性
function logLen<T extends { length: number }>(x: T): void {
  console.log(x.length)
}
logLen('hello')     // 5
logLen([1, 2, 3])   // 3
logLen(123)         // ❌ number 没有 length
```

### 7.4 多个泛型 + 默认值

```ts
function map<T, U = T>(arr: T[], fn: (item: T) => U): U[] {
  return arr.map(fn)
}
const r = map([1, 2, 3], n => n.toString())   // string[]
```

---

## 8. 工具类型速查

TS 内置的"类型工具函数"，可以在不写新类型的情况下完成常见转换。

| 工具类型 | 作用 | 例子 |
| --- | --- | --- |
| `Partial<T>` | 全部属性变可选 | `Partial<User>` |
| `Required<T>` | 全部属性变必填 | `Required<Config>` |
| `Readonly<T>` | 全部属性变只读 | `Readonly<State>` |
| `Pick<T, K>` | 挑出部分属性 | `Pick<User, 'id'\|'name'>` |
| `Omit<T, K>` | 去掉部分属性 | `Omit<User, 'password'>` |
| `Record<K, V>` | 构造字典类型 | `Record<string, number>` |
| `Exclude<T, U>` | 从联合里排除 | `Exclude<'a'\|'b'\|'c', 'a'>` → `'b'\|'c'` |
| `Extract<T, U>` | 从联合里挑出 | 同上反向 |
| `NonNullable<T>` | 排除 null/undefined | `NonNullable<string \| null>` |
| `ReturnType<T>` | 取函数返回值类型 | `ReturnType<typeof fn>` |
| `Parameters<T>` | 取函数参数类型（元组） | `Parameters<typeof fn>` |

```ts
// 实战：组件 props
interface Props {
  id: number
  name: string
  email: string
}
type Preview = Pick<Props, 'id' | 'name'>   // 列表预览只要这两项
```

---

## 9. 类型守卫与 narrowing

TS 会根据代码上下文**自动收窄**类型范围：

```ts
function print(value: string | number) {
  if (typeof value === 'string') {
    console.log(value.toUpperCase())  // 这里 value 是 string
  } else {
    console.log(value.toFixed(2))     // 这里 value 是 number
  }
}
```

### 9.1 三种常见守卫

| 守卫 | 适用 | 例子 |
| --- | --- | --- |
| `typeof` | 原始类型 | `typeof x === 'string'` |
| `instanceof` | 类实例 | `x instanceof Date` |
| `in` | 对象属性 | `'name' in user` |
| 自定义 | 写一个"类型谓词函数" | 见下 |

### 9.2 自定义类型谓词

```ts
interface User { type: 'user'; name: string }
interface Admin { type: 'admin'; role: string }
type Person = User | Admin

function isAdmin(p: Person): p is Admin {
  return p.type === 'admin'
}

const p: Person = { type: 'admin', role: 'super' } as Person
if (isAdmin(p)) {
  console.log(p.role)   // ✅ 此时 p 是 Admin
}
```

---

## 10. 常见类型错误与修正

| 报错 | 原因 | 修正 |
| --- | --- | --- |
| `Object is possibly 'null'` | 可能为 null | 加 `if (x)` 守卫，或用 `!` 断言（不推荐） |
| `Property 'xxx' does not exist on type` | 类型上没有该属性 | 检查类型定义，必要时用 `?.` |
| `Type 'X' is not assignable to type 'Y'` | 类型不匹配 | 用 `as` 断言、或改类型定义 |
| `Argument of type 'Y' is not assignable to parameter` | 函数参数类型不对 | 检查函数签名 |
| `Cannot find name 'X'` | 缺类型 / 缺 import | 加 `import` 或全局声明 |

```ts
// 修正示例 1：可选链
const name = user?.profile?.name

// 修正示例 2：类型断言（确信类型时再用）
const el = document.getElementById('app') as HTMLDivElement

// 修正示例 3：非空断言（不推荐，慎用）
const el = document.getElementById('app')!   // 告诉 TS"它一定不是 null"
```

---

## 11. TypeScript 在 Vue 3 中的实战

### 11.1 项目启用 TS

```bash
npm create vue@latest
# 在提示中勾选 ✔ Add TypeScript? … Yes
```

或在已有项目里：

```bash
npm install -D typescript vue-tsc
```

`tsconfig.json` 关键字段：

```jsonc
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,                 // 严格模式
    "jsx": "preserve",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowImportingTsExtensions": true,
    "noUnusedLocals": true,         // 没用到的变量会报错
    "noUnusedParameters": true
  },
  "include": ["src/**/*", "src/**/*.vue"]
}
```

### 11.2 `<script setup lang="ts">` 基础

```vue
<script setup lang="ts">
import { ref, computed } from 'vue'

// 1. ref 的类型推断
const count = ref(0)              // Ref<number>
const name = ref('Tom')           // Ref<string>

// 2. 显式标注
const list = ref<User[]>([])
const input = ref<HTMLInputElement | null>(null)

// 3. computed
const double = computed(() => count.value * 2)
</script>
```

### 11.3 给 props / emits 标类型

```vue
<script setup lang="ts">
// 1. 用 interface 描述 props
interface Props {
  id: number
  name: string
  age?: number           // 可选
  tags?: string[]
}
const props = defineProps<Props>()

// 2. 描述 emits
interface Emits {
  (e: 'update', value: string): void
  (e: 'delete', id: number): void
}
const emit = defineEmits<Emits>()

// 3. 调用
emit('update', '新名字')
</script>
```

> 提示：在 Vue 3.3+ 中，`defineProps` 也支持**运行时声明 + 类型推导**：
>
> ```ts
> const props = defineProps({
>   id: { type: Number, required: true },
>   name: { type: String, default: '匿名' }
> })
> ```
> 但**纯类型声明**（`<Props>` 写法）更受官方推荐。

### 11.4 Pinia store 用 TS

```ts
// src/stores/user.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

interface UserInfo {
  id: number
  name: string
}

export const useUserStore = defineStore('user', () => {
  const user = ref<UserInfo | null>(null)
  const isLogin = computed(() => user.value !== null)

  const login = (info: UserInfo) => { user.value = info }
  const logout = () => { user.value = null }

  return { user, isLogin, login, logout }
})
```

### 11.5 路由 + axios 通用类型

```ts
// src/utils/request.ts
import axios, { type AxiosResponse } from 'axios'

interface ApiResult<T = unknown> {
  code: number
  data: T
  message: string
}

async function get<T>(url: string): Promise<T> {
  const res = await axios.get<ApiResult<T>>(url)
  if (res.data.code !== 0) throw new Error(res.data.message)
  return res.data.data
}

// 使用：自动推导返回值类型
interface User { id: number; name: string }
const user = await get<User>('/api/user/1')
console.log(user.name)   // ✅ string
```

### 11.6 组件 `<template>` 里用 TS 变量

```vue
<script setup lang="ts">
interface User { id: number; name: string; role: 'admin' | 'user' }
const user: User = { id: 1, name: 'Tom', role: 'admin' }
</script>

<template>
  <!-- 这里完全用 Vue 模板语法，不需要写 :string 之类的 -->
  <div>{{ user.name }} - {{ user.role }}</div>
</template>
```

---

## 12. tsconfig.json 最小配置

> 如果用 `npm create vue@latest` 选了 TS，框架已经帮你生成好。下面是"理解每一行"的精简版。

| 字段 | 作用 | 推荐 |
| --- | --- | --- |
| `target` | 编译到哪个 JS 版本 | `ES2020` |
| `module` | 模块系统 | `ESNext` |
| `strict` | 开启全部严格检查 | `true` |
| `noImplicitAny` | 禁止隐式 any | `true` |
| `strictNullChecks` | 严格 null 检查 | `true` |
| `esModuleInterop` | 兼容 CommonJS | `true` |
| `skipLibCheck` | 跳过 .d.ts 检查 | `true`（提速） |
| `paths` / `baseUrl` | 路径别名 | 与 Vite 对齐 |
| `include` | 参与编译的文件 | `["src/**/*"]` |

> **学习阶段**：直接用 Vite 模板生成的 `tsconfig.json`，**不要从零手写**。等你熟悉了再回来改。

---

## 13. 小结与自检

### 13.1 速记口诀

| 场景 | 写法 |
| --- | --- |
| 任何地方都想要"不会报错就行" | `unknown`（**不是 any**） |
| 描述对象形状 | `interface` |
| 描述类型别名 / 联合 / 工具类型 | `type` |
| 函数签名 | `(a: T, b: U) => R` |
| 复用同一段类型逻辑 | **泛型 `<T>`** |
| 不想写完整定义 | `Partial<T>` / `Pick<T, K>` / `Omit<T, K>` |
| 不确定类型时调用 | **类型守卫** `if (typeof x === ...)` |

### 13.2 自检清单

| # | 题目 | 关键考察 |
| --- | --- | --- |
| 1 | 把一段普通 JS 函数 `sum(a, b)` 加上类型注解 | 函数类型 |
| 2 | 用 `interface` 描述一个 `Article { id, title, tags: string[] }` | interface |
| 3 | 写一个 `async function fetchList<T>(url): Promise<T[]>` | 泛型 + Promise |
| 4 | 写一个 `type Status = 'pending' \| 'success' \| 'error'` | 联合字面量 |
| 5 | 用 `Pick<User, 'id'\|'name'>` 取出前两个字段 | 工具类型 |
| 6 | 在 Vue 里用 `defineProps<Props>()` 写一个 Card 组件 | Vue + TS 实战 |
| 7 | 用类型守卫区分 `string \| number`，分别调用 `.toUpperCase` / `.toFixed` | narrowing |

### 推荐阅读

- 官方手册（中文）：<https://www.typescriptlang.org/zh/docs/handbook/intro.html>
- TypeScript 深入理解：<https://jkchao.github.io/typescript-book-chinese/>
- Vue 3 + TS 官方示例：<https://cn.vuejs.org/guide/typescript/overview.html>

> 学到这里，你的 TypeScript 基础已经够支撑中型 Vue 项目。后续随着项目变大再补"声明文件、模块声明、装饰器"等高级主题即可。

进入下一节前，建议回过头把 [05 JavaScript 基础](./05-javascript-basics.md) 末尾的 6 道小题用 TypeScript 重写一遍——这是最快建立"TS 思维"的方式。
