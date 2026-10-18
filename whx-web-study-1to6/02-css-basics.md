# 02 · CSS 基础

> 本节目标：理解 **CSS 在做什么、怎么写、怎么写"对"**，并能用 Flexbox / Grid 解决 80% 的日常布局。
>
> 学完本节，你应当能徒手给一份 HTML 骨架配上漂亮的样式，做出响应式页面，并对"盒模型 / 层叠 / 优先级"这些基础概念有清晰认识。
>
> 📚 系列目录见 [README.md](./README.md)
>
> ⚠️ 建议先学 [01 HTML 基础](./01-html-basics.md)。

---

## 目录

- [1. CSS 是什么？](#1-css-是什么)
- [2. 三种引入方式](#2-三种引入方式)
- [3. 选择器：从"找谁"开始](#3-选择器从找谁开始)
- [4. 盒模型：每个元素都是"盒子"](#4-盒模型每个元素都是盒子)
- [5. 排版基础：文字、字体、颜色](#5-排版基础文字字体颜色)
- [6. 背景、边框与阴影](#6-背景边框与阴影)
- [7. Flexbox：一维布局的终极答案](#7-flexbox一维布局的终极答案)
- [8. Grid：二维布局的终极答案](#8-grid二维布局的终极答案)
- [9. 响应式：同一份代码，适配多端](#9-响应式同一份代码适配多端)
- [10. 过渡与动画](#10-过渡与动画)
- [11. CSS 变量与层叠](#11-css-变量与层叠)
- [12. 现代特性速览](#12-现代特性速览)
- [13. CSS3 进阶：3D / Filter / 蒙版 / 混合](#13-css3-进阶3d--filter--蒙版--混合)
- [15. CSS 实战片段速查：常见布局配套 CSS](#15-css-实战片段速查常见布局配套-css)
- [14. 小结与自检](#14-小结与自检)

---

## 1. CSS 是什么？

**CSS（Cascading Style Sheets，层叠样式表）** 控制网页的"长相"。

```
HTML = 骨架   CSS = 衣服   JavaScript = 动作
```

CSS 的核心任务：

- **选谁**：用选择器挑出要修饰的元素；
- **怎么变**：用"声明"告诉它颜色、尺寸、位置、动画等；
- **什么时候变**：用伪类、媒体查询、动画等条件触发样式变化。

---

## 2. 三种引入方式

### 2.1 行内样式（不推荐）

直接写在标签的 `style` 属性里：

```html
<p style="color: red; font-size: 18px;">这是一段红字</p>
```

> 缺点：不能复用、优先级过高、HTML 和 CSS 混在一起——**只在调试时临时用**。

### 2.2 内嵌样式（少量特殊场景）

写在 `<style>` 里，嵌在 HTML 头部：

```html
<head>
  <style>
    p { color: red; }
  </style>
</head>
```

> 适合"单文件 demo"。**项目里不推荐**。

### 2.3 外链样式（**绝大多数情况**）

```html
<head>
  <link rel="stylesheet" href="./style.css">
</head>
```

```css
/* style.css */
p { color: red; }
```

> 优点：HTML 与 CSS 分离、文件可缓存、团队共享、CDN 加速。

---

## 3. 选择器：从"找谁"开始

### 3.1 基础选择器

| 选择器 | 含义 | 例子 |
| --- | --- | --- |
| `*` | 全部元素 | `* { box-sizing: border-box; }` |
| `tag` | 标签名 | `p { line-height: 1.6; }` |
| `.class` | 类名 | `.btn { ... }` |
| `#id` | id | `#app { ... }` |
| `[attr]` | 有某属性 | `[disabled] { color: gray; }` |
| `[attr="v"]` | 属性等于某值 | `input[type="text"] { ... }` |

### 3.2 组合选择器

| 选择器 | 含义 | 例子 |
| --- | --- | --- |
| `A, B` | 多选一 | `h1, h2 { color: #333; }` |
| `A B` | 后代（任意层级） | `.card p { ... }` |
| `A > B` | 直接子元素 | `.list > li { ... }` |
| `A + B` | 紧邻的下一个兄弟 | `h2 + p { ... }` |
| `A ~ B` | 后面所有兄弟 | `h2 ~ p { ... }` |

### 3.3 伪类与伪元素

#### 常用伪类

| 伪类 | 作用 | 例子 |
| --- | --- | --- |
| `:hover` | 鼠标悬停 | `a:hover { color: red; }` |
| `:active` | 鼠标按下 | `button:active { transform: scale(0.97); }` |
| `:focus` | 获得焦点 | `input:focus { border-color: #42b883; }` |
| `:visited` | 已访问链接 | `a:visited { color: purple; }` |
| `:first-child` / `:last-child` | 首/末子元素 | `li:last-child { ... }` |
| `:nth-child(n)` | 第 n 个 | `tr:nth-child(odd) { background: #f5f5f5; }` |
| `:not(x)` | 排除 | `li:not(.active) { ... }` |
| `:checked` | 选中的复选框/单选 | `input:checked + label { ... }` |
| `:disabled` / `:enabled` | 禁用/可用 | `input:disabled { opacity: 0.5; }` |
| `:root` | 根元素（HTML） | `:root { --primary: #42b883; }` |

#### 伪元素

| 伪元素 | 作用 | 例子 |
| --- | --- | --- |
| `::before` | 元素前插入内容 | `.star::before { content: "★"; }` |
| `::after` | 元素后插入内容 | `.clearfix::after { content: ""; display: block; clear: both; }` |
| `::placeholder` | 占位符样式 | `input::placeholder { color: #999; }` |
| `::selection` | 选中文本 | `::selection { background: #ffe066; }` |
| `::first-line` / `::first-letter` | 首行/首字 | `p::first-letter { font-size: 2em; }` |

> **记忆口诀**：`:` 一个冒号 = 伪类（状态）；`::` 两个冒号 = 伪元素（内容片段）。CSS3 之后两者**严格区分**，但浏览器对单冒号也兼容。

### 3.4 选择器优先级

当多个规则作用于同一元素时，浏览器按下面规则决定用哪条：

```
内联样式 > ID 选择器 > 类/属性/伪类 > 标签/伪元素 > 通配符/继承
```

更精确的优先级用四组数字表示（a, b, c, d）：

- `a`：内联样式
- `b`：id 选择器个数
- `c`：类/属性/伪类个数
- `d`：标签/伪元素个数

```css
/* 0,0,1,1 = 11 */
p.title { color: red; }

/* 0,1,0,0 = 100 */
#header { color: blue; }

/* 0,0,0,2 = 2 */
body p { color: green; }
```

数值越大优先级越高。**经验法则**：能不写 `!important` 就别写；非写不可时只在"覆盖第三方库"时使用。

---

## 4. 盒模型：每个元素都是"盒子"

### 4.1 盒子的五层结构

```
┌─────────────────────────────────┐
│            margin（外边距）       │  ← 元素与外部的距离
│  ┌───────────────────────────┐  │
│  │        border（边框）      │  │
│  │  ┌─────────────────────┐  │  │
│  │  │    padding（内边距）   │  │  ← 内容与边框的距离
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │   content 内容 │  │  │  │
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

### 4.2 两种盒模型

| 盒模型 | `width` 包含 | 经典代表 |
| --- | --- | --- |
| **content-box**（默认） | 只有 content | 旧 IE、很多老教程 |
| **border-box** | content + padding + border | 现代项目首选 |

**强烈建议全局设为 border-box**：

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

此时 `width: 200px` 永远等于"你看到的总宽度"，padding 再大也不会撑爆布局。

### 4.3 盒子属性速查

```css
.box {
  width: 200px;
  height: 100px;
  padding: 10px 20px;          /* 上右下左；3 个值 = 上 左右 下；2 个值 = 上下 左右 */
  margin: 10px auto;           /* 左右 auto 居中 */
  border: 1px solid #ccc;
  border-radius: 8px;          /* 圆角 */
  background: #fff;
}
```

> ⚠️ **margin 合并**：上下相邻的兄弟元素的 margin 会"合并"成较大的那个；**这是设计**，不是 bug。想避免用 `padding` 或 `display: flex`。

### 4.4 行内元素 vs 块级元素 vs 行内块

| 类型 | 例子 | 特点 |
| --- | --- | --- |
| **行内（inline）** | `span` `a` `strong` | 不会换行，宽高无效 |
| **块级（block）** | `div` `p` `h1` | 占满一行，可设宽高 |
| **行内块（inline-block）** | `img` `button` | 不换行，可设宽高 |

切换：

```css
.inline { display: inline; }
.block  { display: block; }
.box    { display: inline-block; }
.none   { display: none; }   /* 完全消失，不占空间 */
.hidden { visibility: hidden; } /* 隐藏但占空间 */
```

---

## 5. 排版基础：文字、字体、颜色

### 5.1 字体

```css
body {
  font-family:
    -apple-system,                /* macOS / iOS */
    BlinkMacSystemFont,
    "Segoe UI",                   /* Windows */
    "PingFang SC",                /* macOS 中文 */
    "Microsoft YaHei",            /* Windows 中文 */
    "Helvetica Neue",
    Arial,
    sans-serif;
  font-size: 16px;
  line-height: 1.6;              /* 推荐 1.5–1.8 之间 */
  font-weight: 400;              /* normal=400, bold=700 */
  font-style: normal;
  text-align: left;
  letter-spacing: 0.02em;        /* 字间距 */
}
```

> **永远给一个 font-family 回退栈**，最后一定有 `sans-serif` 或 `serif` 兜底。

### 5.2 颜色

| 写法 | 例子 | 说明 |
| --- | --- | --- |
| 关键字 | `red` / `transparent` / `currentColor` | 简单 |
| 十六进制 | `#fff` / `#42b883` / `#42b883cc` | 最常用 |
| rgb / rgba | `rgb(66, 184, 131)` / `rgba(66, 184, 131, 0.5)` | 红绿蓝 + 透明度 |
| hsl / hsla | `hsl(153, 50%, 49%)` | 色相/饱和度/亮度，更易调色 |
| `color()` / `lab()` / `lch()` | 现代 | 更广色域 |

```css
.title {
  color: #42b883;
  background: rgba(0, 0, 0, 0.5);
}
```

### 5.3 文本

```css
.t {
  text-align: center;            /* left | right | center | justify */
  text-decoration: underline;     /* none | underline | line-through */
  text-transform: uppercase;      /* lowercase | uppercase | capitalize */
  text-indent: 2em;              /* 首行缩进 2 字 */
  white-space: nowrap;           /* 不换行 */
  overflow: hidden;
  text-overflow: ellipsis;       /* 省略号（需配合 overflow + nowrap） */
  word-break: break-all;         /* 长单词强制断行 */
  -webkit-line-clamp: 2;         /* 多行省略 */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

---

## 6. 背景、边框与阴影

### 6.1 背景

```css
.box {
  background-color: #f5f5f5;
  background-image: url('./bg.jpg');
  background-repeat: no-repeat;       /* repeat | no-repeat | repeat-x */
  background-position: center center; /* 关键字 / 50% 50% / 10px 20px */
  background-size: cover;             /* cover | contain | 100% auto */
  background-attachment: fixed;       /* 滚动时背景不动 */
}
```

简写：

```css
background: #f5f5f5 url('./bg.jpg') no-repeat center / cover;
```

#### 渐变背景

```css
.box {
  background: linear-gradient(to right, #ff7e5f, #feb47b);
  background: radial-gradient(circle at center, #fff, #000);
  background: conic-gradient(from 0deg, red, yellow, green, red);  /* 圆锥渐变 */
}
```

### 6.2 边框

```css
.box {
  border: 1px solid #ccc;
  border-top: 2px dashed red;
  border-radius: 8px;                /* 圆角 */
  border-radius: 50%;                /* 正圆 */
}
```

### 6.3 阴影

```css
.card {
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  /*           x  y  模糊半径  颜色 */
}

.text {
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
}
```

#### 常用阴影预设

```css
.shadow-sm  { box-shadow: 0 1px 2px rgba(0,0,0,0.05); }
.shadow-md  { box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
.shadow-lg  { box-shadow: 0 10px 15px rgba(0,0,0,0.1); }
```

---

## 7. Flexbox：一维布局的终极答案

**Flexbox** 解决"一行 / 一列"内的元素排列问题。

### 7.1 概念

```
┌──────────────────── 容器 container ────────────────────┐
│ ┌────┐ ┌────┐ ┌────┐ ┌────┐                            │
│ │item│ │item│ │item│ │item│   ← 子元素 item           │
│ └────┘ └────┘ └────┘ └────┘                            │
└────────────────────────────────────────────────────────┘
        ↑ 主轴（默认水平）   ↑ 交叉轴（垂直）
```

### 7.2 父容器常用属性

```css
.container {
  display: flex;                /* 开启 flex */
  flex-direction: row;          /* row | row-reverse | column | column-reverse */
  justify-content: center;      /* 主轴对齐：flex-start | flex-end | center | space-between | space-around | space-evenly */
  align-items: center;          /* 交叉轴对齐：stretch | flex-start | flex-end | center | baseline */
  flex-wrap: wrap;              /* nowrap | wrap | wrap-reverse */
  gap: 16px;                    /* 子元素之间的间距（推荐用 gap 而非 margin） */
}
```

> 记忆：**`justify` 管主轴，`align` 管交叉轴**。

### 7.3 子元素常用属性

```css
.item {
  flex: 1;                      /* flex-grow:1; flex-shrink:1; flex-basis:0; —— 等分剩余空间 */
  flex-grow: 1;                 /* 放大比例 */
  flex-shrink: 1;               /* 缩小比例 */
  flex-basis: 0;                /* 初始大小 */
  align-self: flex-end;         /* 单个元素覆盖父容器的 align-items */
  order: 2;                     /* 调整顺序，数字越小越靠前 */
}
```

### 7.4 实战模板

#### 水平居中

```css
.center { display: flex; justify-content: center; align-items: center; }
```

#### 导航栏（左 logo + 右菜单）

```css
.nav { display: flex; justify-content: space-between; align-items: center; }
```

#### 三列等宽 + 列间 16px

```css
.row { display: flex; gap: 16px; }
.col { flex: 1; }
```

#### 卡片网格自动换行

```css
.grid {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.card {
  flex: 0 1 calc((100% - 32px) / 3);  /* 一行 3 个，gap 16 */
}
```

> 现在的卡片网格**更推荐 CSS Grid**（见第 8 节），Flexbox 更擅长"一维分布"。

---

## 8. Grid：二维布局的终极答案

**CSS Grid** 解决"行 + 列同时控制"的二维布局。

### 8.1 最简网格

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;   /* 三列等宽 */
  gap: 16px;
}
```

`fr` = fraction（分数单位），把可用空间按比例分。

### 8.2 常用模式

#### 固定列宽

```css
grid-template-columns: 200px 1fr 200px;   /* 中间自适应 */
```

#### 自动填充

```css
/* 每列最小 250px，能放多少放多少 */
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```

#### 行定义

```css
grid-template-rows: auto 1fr auto;   /* 头部、内容、底部 —— 经典三行布局 */
```

#### 区域命名

```css
.layout {
  display: grid;
  grid-template:
    "header  header"  60px
    "sidebar main"    1fr
    "footer  footer"  60px
    / 200px 1fr;
}

header  { grid-area: header; }
aside   { grid-area: sidebar; }
main    { grid-area: main; }
footer  { grid-area: footer; }
```

#### 子元素定位

```css
.item {
  grid-column: 1 / 3;       /* 跨 1~3 列 */
  grid-row: 2 / 4;          /* 跨 2~4 行 */
  grid-area: header;        /* 用区域名 */
}
```

### 8.3 圣杯布局（最常用）

```html
<div class="layout">
  <header>头部</header>
  <aside>侧边栏</aside>
  <main>主体</main>
  <footer>底部</footer>
</div>
```

```css
.layout {
  min-height: 100vh;
  display: grid;
  grid-template:
    "header header" 60px
    "side   main"   1fr
    "footer footer" 40px
    / 200px 1fr;
}
```

> **结论**：能用 Grid 优先用 Grid，Flexbox 处理剩余细节。两者经常一起用。

---

## 9. 响应式：同一份代码，适配多端

### 9.1 视口 meta

**必加**，否则移动端按 980px 渲染：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### 9.2 媒体查询

```css
/* 默认：手机 */
.container { padding: 16px; }

/* ≥ 768px：平板 */
@media (min-width: 768px) {
  .container { padding: 24px; }
}

/* ≥ 1024px：桌面 */
@media (min-width: 1024px) {
  .container { padding: 32px; max-width: 1200px; margin: 0 auto; }
}

/* 横屏 / 竖屏 */
@media (orientation: landscape) { ... }

/* 暗色模式 */
@media (prefers-color-scheme: dark) { ... }

/* 打印 */
@media print { ... }
```

#### 常见断点

| 断点 | 设备 |
| --- | --- |
| < 640px | 手机 |
| 640–768px | 大屏手机 |
| 768–1024px | 平板 |
| 1024–1280px | 桌面 |
| > 1280px | 大屏桌面 |

### 9.3 单位

| 单位 | 含义 | 适用 |
| --- | --- | --- |
| `px` | 像素 | 边框、阴影 |
| `%` | 父元素百分比 | 宽度 |
| `em` | 相对**自身** `font-size` | 文字相关 |
| `rem` | 相对**根** `font-size` | 全局尺寸（**最推荐**） |
| `vw` / `vh` | 视口宽/高的 1% | 全屏布局 |
| `vmin` / `vmax` | 视口较小/较大边的 1% | 横竖屏切换 |
| `clamp(min, val, max)` | 范围限定 | 流体字号 |

#### 推荐：根字号 + rem

```css
html { font-size: 16px; }
@media (max-width: 768px) { html { font-size: 14px; } }

.title { font-size: 2rem; }   /* = 32px（桌面） / 28px（手机） */
```

#### 流体字号

```css
h1 { font-size: clamp(1.5rem, 2vw + 1rem, 3rem); }
```

### 9.4 移动优先 vs 桌面优先

| 策略 | 写法 | 适用 |
| --- | --- | --- |
| **移动优先（推荐）** | 默认写手机样式，`min-width` 逐步加桌面 | 内容为主的项目 |
| 桌面优先 | 默认写桌面样式，`max-width` 减功能 | 内部管理系统 |

> **99% 的项目都该用移动优先**。

---

## 10. 过渡与动画

### 10.1 transition：过渡

一个状态平滑变到另一个状态：

```css
.btn {
  background: #42b883;
  color: white;
  transition:
    background 0.3s ease,
    transform 0.2s ease;
}
.btn:hover {
  background: #369870;
  transform: translateY(-2px);
}
```

#### transition-timing-function 速查

| 关键字 | 效果 | 何时用 |
| --- | --- | --- |
| `linear` | 匀速 | 颜色淡入淡出 |
| `ease` | 慢-快-慢（默认） | 大多数情况 |
| `ease-in` | 慢-快 | 元素"飞入" |
| `ease-out` | 快-慢 | 元素"降落" |
| `ease-in-out` | 慢-快-慢 | 优雅出场 |
| `cubic-bezier()` | 自定义曲线 | 高阶 |

### 10.2 animation：关键帧动画

```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

.loader {
  width: 40px;
  height: 40px;
  border: 4px solid #ccc;
  border-top-color: #42b883;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}
```

#### 常用动画属性

```css
animation:
  fade-in 0.5s ease forwards,    /* 名字 时长 缓动 保持末态 */
  slide-up 0.6s 0.2s ease backwards;  /* 延迟 0.2s，初始用首帧 */
```

| 属性 | 含义 |
| --- | --- |
| `animation-name` | 关键帧名 |
| `animation-duration` | 时长 |
| `animation-delay` | 延迟 |
| `animation-iteration-count` | 次数（`infinite` 无限） |
| `animation-direction` | `normal` / `reverse` / `alternate` |
| `animation-fill-mode` | `forwards`（保留末态）/ `backwards`（保留首态） |
| `animation-timing-function` | 缓动 |

### 10.3 transform：变形

```css
.box {
  transform:
    translate(10px, 20px)   /* 平移 */
    rotate(45deg)            /* 旋转 */
    scale(1.2)               /* 缩放 */
    skew(10deg)              /* 倾斜 */;
  transform-origin: center;  /* 变形原点：默认中心 */
}
```

> 性能小贴士：`transform` 和 `opacity` 触发的重排最小（只走"合成"层），**做动画优先用这两个属性**。

### 10.4 性能：will-change

提前告诉浏览器哪些属性会变，让它创建独立合成层：

```css
.animated { will-change: transform, opacity; }
/* 用完最好移除，否则会占内存 */
```

---

## 11. CSS 变量与层叠

### 11.1 变量

```css
:root {
  --primary: #42b883;
  --radius: 8px;
  --shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.btn {
  background: var(--primary);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
}

/* 变量可以动态覆盖 */
.dark { --primary: #1a1a1a; }
```

> 比 Sass 变量更强——**可以在 JS / 媒体查询里运行时修改**：

```js
document.documentElement.style.setProperty('--primary', '#ff7e5f')
```

### 11.2 层叠上下文

"层叠"是 CSS 里**最容易让新人懵**的概念——同一个元素可能有 5 条规则，浏览器到底听谁的？

按以下顺序决定（后写覆盖前写）：

1. **来源**：用户样式 < 浏览器默认 < 作者样式 < 作者 `!important` < 用户 `!important`
2. **优先级**：内联 > id > 类/属性/伪类 > 标签
3. **顺序**：后写的赢

> 工程实践：**少用 `!important`、少写高优先级选择器、按特性就近写样式**。

---

## 12. 现代特性速览

下面这些特性都可以放心使用，浏览器支持都很好：

### 12.1 嵌套（CSS Nesting）

```css
.card {
  padding: 16px;

  & .title {
    font-size: 1.5rem;
  }

  & .btn:hover {
    background: #42b883;
  }
}
```

### 12.2 `:has()` 父选择器

```css
/* 当父元素包含 .error 子元素时 */
form:has(.error) { border: 1px solid red; }
```

### 12.3 `clamp()` 流体尺寸

```css
.title { font-size: clamp(1rem, 2vw + 1rem, 3rem); }
```

### 12.4 `aspect-ratio`

```css
.box { aspect-ratio: 16 / 9; }   /* 锁定宽高比 */
```

### 12.5 `gap`（Flex/Grid 间距）

```css
.row { display: flex; gap: 16px; }   /* 子元素间距 */
```

### 12.6 `position: sticky` 吸顶

```css
.header { position: sticky; top: 0; z-index: 10; }
```

### 12.7 容器查询 `@container`

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card { display: flex; }
}
```

### 12.8 逻辑属性

```css
.box {
  margin-inline: auto;     /* 替代 margin-left + margin-right，自动适配 RTL */
  padding-block: 16px;     /* 上下内边距 */
  text-align: start;       /* 替代 left，自动适配阅读方向 */
}
```

### 12.9 `prefers-reduced-motion`

```css
@media (prefers-reduced-motion: reduce) {
  * { animation: none !important; transition: none !important; }
}
```

---

## 13. CSS3 进阶：3D / Filter / 蒙版 / 混合

> CSS3 是 CSS 的"现代大礼包"——它给 CSS 加入了**圆角、阴影、渐变、变换、动画、多背景、媒体查询**等一整套能力。本节挑 6 个最常用、性价比最高的特性做"补强 + 实战"。

### 13.1 多重背景与 `background-clip`

CSS3 允许**一个元素叠多层背景**，从上到下依次排列。

```css
.box {
  width: 300px;
  height: 200px;
  background:
    url('./icon.png') no-repeat right top,        /* 顶层：图标 */
    linear-gradient(45deg, #ff7e5f, #feb47b),     /* 中层：渐变 */
    #fff                                          /* 底层：纯色兜底 */
  ;
  background-size: 60px, cover, auto;
}
```

`background-clip` 控制背景的"绘制范围"：

| 值 | 含义 |
| --- | --- |
| `border-box` | 背景延伸到边框外缘（默认） |
| `padding-box` | 背景到内边距外缘止 |
| `content-box` | 背景只在内容区 |
| `text` | 背景只绘制在文字里——**最炫**的效果之一 |

```css
.gradient-text {
  background: linear-gradient(90deg, #ff7e5f, #42b883);
  -webkit-background-clip: text;   /* Safari 仍需 -webkit- 前缀 */
  background-clip: text;
  color: transparent;              /* 文字本身透明，让背景透出来 */
  font-size: 3rem;
  font-weight: bold;
}
```

### 13.2 圆角、阴影、渐变三件套（CSS3 基础回顾）

```css
.btn {
  border-radius: 12px;                                /* 圆角 */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);        /* 阴影 */
  background: linear-gradient(135deg, #42b883, #369870); /* 渐变 */
}
```

#### 渐变三大类

| 类型 | 用途 | 例子 |
| --- | --- | --- |
| **线性渐变** `linear-gradient` | 沿直线方向 | 按钮背景、分割线 |
| **径向渐变** `radial-gradient` | 从中心向外 | 高光、光晕 |
| **圆锥渐变** `conic-gradient` | 绕中心旋转 | 饼图、加载圈 |

```css
.pie {
  background: conic-gradient(
    #42b883 0 30%,         /* 0%–30% 绿色 */
    #feb47b 30% 70%,       /* 30%–70% 橙色 */
    #ccc    70% 100%       /* 70%–100% 灰色 */
  );
  border-radius: 50%;
}
```

### 13.3 Transform 3D：让元素"立起来"

2D 变换只能改 X / Y 轴，**3D 变换**多一个 Z 轴（深度），可以做立体旋转、翻转、视差。

```css
.scene {
  perspective: 800px;     /* 父元素加 perspective，越大越"远"，变形越轻 */
}

.card {
  width: 200px;
  height: 280px;
  transform-style: preserve-3d;  /* 子元素的 3D 效果保留 */
  transform: rotateY(45deg);     /* 沿 Y 轴转 45° */
  transition: transform 0.6s;
}

.card-face {
  position: absolute;
  inset: 0;
  backface-visibility: hidden;   /* 背面不可见 —— 经典翻牌效果 */
}
```

#### 常用 3D 变换函数

| 函数 | 含义 |
| --- | --- |
| `translateZ(z)` | 沿 Z 轴"拉近"（必须配合 `perspective`） |
| `translate3d(x, y, z)` | 三轴平移 |
| `rotateX(deg)` | 沿 X 轴翻 |
| `rotateY(deg)` | 沿 Y 轴转 |
| `rotateZ(deg)` | 沿 Z 轴转（其实等价 2D rotate） |
| `scaleZ(s)` | Z 轴缩放 |
| `perspective(n)` | 视距（一般写在父元素上） |

#### 实战：3D 翻牌

```html
<div class="scene">
  <div class="card">
    <div class="card-face front">正面</div>
    <div class="card-face back">背面</div>
  </div>
</div>
```

```css
.scene { perspective: 1000px; width: 200px; }
.card {
  position: relative; width: 200px; height: 300px;
  transform-style: preserve-3d;
  transition: transform 0.8s;
}
.scene:hover .card { transform: rotateY(180deg); }
.card-face {
  position: absolute; inset: 0;
  backface-visibility: hidden;
  display: grid; place-items: center;
  font-size: 2rem; color: white;
}
.front { background: #42b883; }
.back  { background: #feb47b; transform: rotateY(180deg); }
```

### 13.4 Filter：图片滤镜

CSS Filter 让你**不打开 PS** 也能做出各种滤镜效果。

```css
.img {
  filter:
    blur(4px)              /* 模糊 */
    brightness(1.2)        /* 亮度，0 全黑，1 原图，>1 变亮 */
    contrast(1.5)          /* 对比度 */
    grayscale(0.5)         /* 灰度 0–1 */
    sepia(0.3)             /* 复古 */
    hue-rotate(90deg)      /* 色相旋转 */
    invert(1)              /* 反相 */
    saturate(2)            /* 饱和度 */
    drop-shadow(2px 4px 6px rgba(0,0,0,0.5));  /* 投影（比 box-shadow 更精准） */
}

.img:hover { filter: none; }   /* 鼠标悬停时恢复原色 —— 经典交互 */
```

#### 经典组合

| 效果 | 写法 |
| --- | --- |
| 黑白照片 | `filter: grayscale(1);` |
| 毛玻璃 | `backdrop-filter: blur(10px);`（背景模糊） |
| 旧电影 | `filter: sepia(0.6) contrast(1.2) brightness(0.9);` |
| 暗色悬浮 | `filter: brightness(0.7) contrast(1.2);` |

> **小贴士**：`backdrop-filter`（背景模糊）和 `filter`（元素模糊）不要混——前者作用于元素**背后**的内容，后者作用于元素**本身**。毛玻璃用前者。

### 13.5 混合模式：让元素"透"出底色

`mix-blend-mode`（元素与下层内容的混合）和 `background-blend-mode`（背景层之间的混合）让设计从"贴图层"升级到"调色"。

```css
.text {
  mix-blend-mode: difference;   /* 文字与背景取反色对比 */
  color: white;
}
```

| 模式 | 效果 |
| --- | --- |
| `normal` | 默认 |
| `multiply` | 正片叠底（去白留黑） |
| `screen` | 滤色（去黑留白） |
| `overlay` | 上面两者的结合 |
| `difference` | 差值（文字与底色始终反色，**最常用**） |
| `hue` / `saturation` / `color` / `luminosity` | HSL 通道混合 |

> **实战技巧**：把品牌主色与图片用 `multiply` 混合，可以"给图片上色"，不用做两张图。

### 13.6 clip-path：任意形状裁剪

**`clip-path`** 让你把元素裁成**任意形状**——圆形、多边形、星形、文字、SVG 路径。

```css
.avatar {
  width: 100px; height: 100px;
  clip-path: circle(50% at 50% 50%);     /* 圆形 */
}

.triangle {
  width: 100px; height: 100px;
  background: #42b883;
  clip-path: polygon(50% 0, 100% 100%, 0 100%);  /* 三角形 */
}

.blob {
  width: 200px; height: 200px;
  background: #feb47b;
  clip-path: polygon(
    30% 0%, 70% 0%, 100% 30%, 100% 70%,
    70% 100%, 30% 100%, 0% 70%, 0% 30%
  );  /* 八边形 */
}
```

#### 实用预设

| 形状 | `clip-path` |
| --- | --- |
| 圆 | `circle(50%)` |
| 椭圆 | `ellipse(50% 30%)` |
| 圆角矩形 | `inset(10px round 8px)` |
| 三角形 | `polygon(50% 0, 100% 100%, 0 100%)` |
| 箭头 | `polygon(0 0, 100% 50%, 0 100%, 25% 50%)` |
| 五角星 | 5 个顶点的 `polygon()` |
| 任意 SVG 路径 | `path('M0 0 L100 0 L100 100 Z')` |

> **优势**：与 `border-radius` 相比，`clip-path` 能实现**不规则形状**（三角、星、曲线），并且**形状外不响应点击**（天然是"挖空"）。
> **陷阱**：裁掉的部分**真的没了**，连文字选中都做不到；做"磨砂玻璃"应该用 `backdrop-filter`。

### 13.7 mask：蒙版与渐隐

**`mask`** 让元素的"形状"由**另一张图**决定——比 `clip-path` 更自由。

```css
.fade-img {
  mask-image: linear-gradient(to right, transparent, black);
  /* 左侧完全透明，右侧完全显示 → 渐隐效果 */
}

.text-mask {
  background: url('./bg.jpg') center/cover;
  -webkit-mask-image: url('./text-shape.svg');
  mask-image: url('./text-shape.svg');
  /* 蒙版里"白"的部分显示，"黑"的部分隐藏 */
}
```

> 蒙版里的**亮度**决定可见性：`mask-mode: alpha` 时按 alpha 通道；`mask-mode: luminance` 时按亮度。

### 13.8 @supports：特性检测

相当于 CSS 版的"if 语句"——**如果浏览器支持某属性才应用规则**。

```css
.container {
  display: flex;                              /* 兜底 */
}

@supports (display: grid) {
  .container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  }
}

@supports not (display: grid) {
  /* 给老浏览器一段提示 */
  .container::before {
    content: '您的浏览器太老了，建议升级';
  }
}
```

#### 实战用法

| 场景 | 写法 |
| --- | --- |
| 检测 Grid 支持 | `@supports (display: grid) { ... }` |
| 检测 `:has()` 支持 | `@supports selector(:has(*)) { ... }` |
| 检测容器查询 | `@supports (container-type: inline-size) { ... }` |
| 检测 CSS 变量 | `@supports (--x: 0) { ... }` |

> **结合 `@media` 写渐进增强**：先保证基础体验可用，再为现代浏览器加新特性。

### 13.9 推荐阅读

- MDN CSS 3D 教程：<https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transforms>
- clip-path 生成器：<https://bennettfeely.com/clippy/>
- Filter Playground：<https://www.cssfiltergenerator.com/>
- Can I Use（查浏览器兼容）：<https://caniuse.com/>
- 现代 CSS 总览：<https://web.dev/learn/css/>

---

## 14. 小结与自检

### 14.1 自检清单

| # | 题目 | 关键点 |
| --- | --- | --- |
| 1 | 给一段 HTML 写 CSS，让所有 `<p>` 字号 16px、行高 1.6、颜色 `#333` | 标签选择器、字体属性 |
| 2 | 实现一个水平垂直居中的盒子 | Flexbox / 定位 |
| 3 | 用 Grid 做一个 3 列等宽 + 列间距 20px 的网格 | Grid 基础 |
| 4 | 用媒体查询实现移动优先：手机 1 列、平板 2 列、桌面 3 列 | 响应式 |
| 5 | 给一个按钮写 hover 过渡：背景从蓝到红、向上移动 2px | transition + transform |
| 6 | 定义 CSS 变量 `--primary` 并在多处使用 | CSS 变量 |
| 7 | 用 `background-clip: text` 做一个渐变文字 | 背景裁剪 |
| 8 | 用 `transform: rotateY` + `perspective` 做一个 3D 翻牌 | 3D 变换 |
| 9 | 用 `clip-path: polygon()` 把一个方块裁成五角星 | clip-path |
| 10 | 用 `@supports` 给"支持 Grid 的浏览器"用 Grid，否则降级到 Flex | 渐进增强 |

### 14.2 推荐练习

1. 跟着 [CSS Battle](https://cssbattle.dev/) 做 10 个静态小图，练习选择器与盒模型；
2. 用纯 CSS（不写 JS）做一个"汉堡菜单"的展开收起：用 `<input type="checkbox">` 配 `:checked`；
3. 用 `clip-path` 做一个不规则形状的"卡片堆叠"；
4. 找一个你喜欢的网站，**禁用 CSS** 后看结构；再单独看 CSS 文件，找它用了哪些布局技巧。

### 14.3 常见错误清单

| 现象 | 原因 | 修正 |
| --- | --- | --- |
| 设了 `width` 但实际更宽 | 用了默认 `content-box` | 全局 `box-sizing: border-box` |
| 上下 margin 合并不生效 | margin 合并 | 改用 `padding` 或 `display: flex` |
| 浮动塌陷 | 子元素 `float` 后父容器高度为 0 | 父容器加 `.clearfix::after { content: ""; display: block; clear: both; }` 或改用 flex |
| 移动端页面被缩放成 980px | 漏 `viewport` meta | 补上 |
| 动画卡顿 | 动画 `width` `height` `top` `left` | 改用 `transform` `opacity` |
| Grid 跨列不生效 | 写成了 `grid-rows` | 改成 `grid-row` |
| 渐变文字不显示 | `color` 仍是实色 | 改 `color: transparent` |
| 3D 翻牌没有立体感 | 父元素没加 `perspective` | 父元素加 `perspective: 1000px` |
| clip-path 内的文字不可选 | 被裁掉了 | 改用 `mask` 或 `backdrop-filter` |

### 推荐阅读

- MDN CSS 教程：<https://developer.mozilla.org/zh-CN/docs/Learn/CSS>
- Flexbox 练习场：<https://flexboxfroggy.com/>
- Grid 练习场：<https://cssgridgarden.com/>
- CSS Tricks Flexbox 完整指南：<https://css-tricks.com/snippets/css/a-guide-to-flexbox/>
- 现代 CSS 总览：<https://web.dev/learn/css/>

---

## 15. CSS 实战片段速查：常见布局配套 CSS

> 这一节是 [01 第 12 节"常见布局 7 模板"](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) 的 **CSS 配套**——把每个模板里"最值得抄"的 CSS 片段集中在一起，按**功能**（不是按模板）速查。
>
> 完整 HTML 骨架在 01 那一节；这里只讲"为什么这么写"和"还能怎么改"。

### 15.1 顶部导航（模板 1）

```css
.site-header {
  display: flex;
  align-items: center;             /* 垂直居中 */
  justify-content: space-between;  /* 左右两端 */
  padding: 16px 24px;
  border-bottom: 1px solid #eee;
  position: sticky;                 /* 滚动时钉住 */
  top: 0;
  background: #fff;
  z-index: 10;
}
```

> **口诀**：导航条 = `display:flex` + `sticky top:0` + `z-index`。

### 15.2 后台侧栏（模板 2）

```css
.layout {
  display: flex;
  height: 100vh;                    /* 占满视口 */
}

.sidebar {
  width: 220px;
  flex-shrink: 0;                   /* 不被压缩 */
  background: #1f2937;
  color: #fff;
  overflow-y: auto;                 /* 侧栏独立滚动 */
}

.main {
  flex: 1;                          /* 占满剩余 */
  display: flex;
  flex-direction: column;
  min-width: 0;                     /* 防止内容溢出撑大 */
}
```

> **坑**：不加 `min-width: 0` 时，flex 子项的内容（如超长表格）会撑破父容器。

### 15.3 圣杯三列（模板 3，Grid 版）

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr 240px;
  min-height: calc(100vh - 96px);  /* 减去 header + footer */
}
```

> **现代写法的三列**：永远优先用 `grid-template-columns`，不要用 `flex` 算 margin 负值。

### 15.4 响应式卡片（模板 4）

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
}

.card {
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s, box-shadow 0.2s;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.08);
}
```

> **关键**：`auto-fill + minmax` 让"屏幕能放几列就放几列"，**不用写媒体查询**。
> **背景图固定比例**：`aspect-ratio: 16/10`（见 [02 第 12.4 节](./02-css-basics.md#124-aspect-ratio)）。

### 15.5 居中卡片（模板 5，登录页）

```css
.page {
  min-height: 100vh;
  display: grid;
  place-items: center;               /* 完美居中 */
  background: #f5f7fa;
}
```

> **`place-items: center` = `align-items + justify-items: center`**，是 Flex/Grid 中**最快居中**的写法。

### 15.6 侧边栏文档站（模板 6，sticky）

```css
.doc-sidebar {
  position: sticky;
  top: 0;
  height: 100vh;                    /* 视口高度 = 可滚动范围 */
  overflow-y: auto;                 /* 自己滚 */
  align-self: flex-start;           /* 在 flex 容器中不拉伸 */
}
```

> **`position: sticky` + 父级是 flex** 时，必须加 `align-self: flex-start`——否则会因 stretch 撑满高度而无法滚动。

### 15.7 底部固定栏（模板 7，移动端）

```css
.bottom-bar {
  position: fixed;
  bottom: 0; left: 0; right: 0;
  z-index: 10;
  background: #fff;
  display: flex;
  gap: 12px;
  padding: 8px 16px;
  /* iPhone 底部小白条适配 */
  padding-bottom: calc(8px + env(safe-area-inset-bottom));
}

body { padding-bottom: 72px; }     /* 给固定栏让位置 */
```

> **iOS Safari 底部安全区**：`env(safe-area-inset-bottom)` 必须加，否则按钮被小白条遮住一半。

### 15.8 通用"工具类"速查

#### 居中

```css
/* 块级元素水平居中（需要宽度） */
.center-x { margin: 0 auto; }

/* 不需要宽度：transform */
.center-both {
  position: absolute;
  inset: 0;
  margin: auto;
  width: fit-content;
  height: fit-content;
}

/* Flex 容器内 */
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Grid 容器内（最短） */
.grid-center { display: grid; place-items: center; }
```

#### 文字截断

```css
.truncate-1 {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.truncate-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

#### 卡片阴影 4 档

```css
.shadow-sm { box-shadow: 0 1px 2px rgba(0,0,0,0.05); }
.shadow-md { box-shadow: 0 4px 8px rgba(0,0,0,0.08); }
.shadow-lg { box-shadow: 0 10px 20px rgba(0,0,0,0.10); }
.shadow-xl { box-shadow: 0 20px 40px rgba(0,0,0,0.12); }
```

#### 间距系统（Tailwind 风格）

```css
:root {
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;
}
.p-4  { padding: var(--space-4); }
.py-2 { padding: var(--space-2) 0; }
.mt-6 { margin-top: var(--space-6); }
.mb-4 { margin-bottom: var(--space-4); }
.gap-3 { gap: var(--space-3); }
```

#### 头像 / Logo 占位

```css
.avatar {
  width: 40px; height: 40px;
  border-radius: 50%;
  object-fit: cover;
}
.gradient-placeholder {
  background: linear-gradient(135deg, #42b883 0%, #369870 100%);
color: #fff;
display: grid;
place-items: center;
font-weight: bold;
text-transform: uppercase;
}

> 背景图固定比例：`aspect-ratio: 16/10`（见 [02 第 12.4 节](./02-css-basics.md#124-aspect-ratio)）。
```

### 15.9 7 模板配套 CSS 一句话索引

| 模板 | 完整骨架在 01 | CSS 关键技巧 | 速查位置 |
| --- | --- | --- | --- |
| 1. 顶部导航 + 内容 | [01 § 12 模板 1](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `display:flex` + `space-between` + `sticky` | 15.1 |
| 2. 后台经典布局 | [01 § 12 模板 2](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `flex` 100vh + `min-width: 0` | 15.2 |
| 3. 圣杯三列 | [01 § 12 模板 3](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `grid-template-columns: 200px 1fr 240px` | 15.3 |
| 4. 响应式卡片 | [01 § 12 模板 4](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `auto-fill + minmax` | 15.4 |
| 5. 居中卡片 | [01 § 12 模板 5](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `place-items: center` | 15.5 |
| 6. 侧边栏文档站 | [01 § 12 模板 6](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `sticky` + `align-self: flex-start` | 15.6 |
| 7. 底部固定栏 | [01 § 12 模板 7](./01-html-basics.md#12-常见布局-7-模板html-骨架--配套-css-实战) | `position: fixed` + `env(safe-area-inset-bottom)` | 15.7 |

### 15.10 一句话

> **CSS 实战没什么神秘——95% 的页面是"Flex 排一行 / Grid 分两列 / Sticky 钉导航 / position fixed 钉底栏"四种动作的组合**。把 15.1–15.7 这 7 段代码收藏起来，新页面 80% 能直接套。

---

## 16. 布局方式选型指南：Flex / Flexbox / Grid / Float / Position / Table 怎么选

> 前面 7 / 8 / 9 章分别讲了 Flexbox / Grid / 响应式，但**它们之间怎么选、跟老古董 `float` / `position` / 表格布局比差在哪**——这一节一次说清。

### 16.1 一句话关系

| 名称 | 关系 |
| --- | --- |
| **Flex** | `display: flex` 的"俗称"，**指代 Flexbox 布局** |
| **Flexbox** | 官方名称（W3C），**一维布局**——排一行或一列 |
| **CSS Grid** | 二维布局——**同时**管行 + 列 |
| **Float** | 1996 年的"让文字环绕图片"老古董——**现在 99% 不该用** |
| **Position** | 绝对 / 相对 / 固定 / 粘性定位，**配合其他布局用** |
| **Table 布局** | 1995 年的"用 `<table>` 排版"——**绝不该再用** |

> **结论**：**2026 年只学 Flexbox + Grid + Position 3 件套**就够；Float 仅维护老项目；Table 布局完全不要碰。

### 16.2 六大布局方式总览

| 布局方式 | 一句话 | 维度 | 现状 | 主要场景 |
| --- | --- | --- | --- | --- |
| **Flexbox** | 排一行 / 一列 | **一维** | ✅ 现代默认 | 导航、按钮组、卡片列表、表单行、垂直居中 |
| **CSS Grid** | 同时排行 + 列 | **二维** | ✅ 现代默认 | 整页布局、仪表盘、相册、杂志风排版 |
| **Position** | 把元素"钉"在指定位置 | 0 维 | ✅ 配合用 | 弹窗、下拉、固定导航、固定底栏、tooltip |
| **Float** | 文字环绕图片 | — | ⚠️ 老项目 | 维护 2015 年前的 IE 项目 |
| **Table 布局** | 用 `<table>` 排版 | 二维 | ❌ 禁 | **不要用**——布局不是表格的用途 |
| **inline-block** | 行内块元素排成行 | 1 维 | ⚠️ 极少 | 仅在"按钮 + 文字"等小场景 |

### 16.3 三大现代布局深度对比

| 维度 | **Flexbox** | **CSS Grid** | **Position** |
| --- | --- | --- | --- |
| **维度** | 一维（行或列） | 二维（行 + 列同时） | 无维度（直接定位置） |
| **适用规模** | 单行 / 单列内容 | 整页 / 大区块 | 单个元素精确定位 |
| **响应式** | ✅ 配 `flex-wrap` + `flex: 1` | ✅✅✅ 配 `auto-fill` + `minmax` | ❌（需手动调） |
| **居中** | ✅ 1 行 `place-items` | ✅ 1 行 `place-items` | ✅ `top: 50%; left: 50%; transform: translate(-50%, -50%)` |
| **对齐** | ✅ `justify-content` + `align-items` | ✅ `justify-items` + `align-items` | ❌ 需手动算 |
| **重排** | 中（dom 变化触发回流） | 中（同） | 独立 |
| **学习曲线** | **低** | 中 | 低 |
| **浏览器支持** | 99.9% | 99.5% | 100% |
| **核心场景** | 行内 / 列内分布 | 整页骨架 | 浮层 / 钉死 |

### 16.4 6 大使用场景速查

| 场景 | 推荐布局 | 原因 |
| --- | --- | --- |
| **顶部导航 + Logo + 菜单** | Flexbox | 一行排布 + `space-between` 完美 |
| **卡片列表自动换行** | Flexbox `flex-wrap: wrap` | 移动端自然堆叠 |
| **仪表盘 / 后台** | **Grid**（整页） + **Flexbox**（区块内） | 嵌套使用最常见 |
| **整页三列 / 圣杯布局** | **Grid** | 二维布局，1 行 CSS 搞定 |
| **登录卡片居中** | Grid `place-items: center` | 全屏 1 行居中 |
| **弹窗 / Modal / 下拉** | **Position: fixed/absolute** | 浮在内容之上 |
| **固定底栏 / 顶部导航** | **Position: sticky / fixed** | 滚动时钉住 |
| **响应式卡片网格** | Grid `auto-fill + minmax` | 不用媒体查询 |
| **垂直居中一个元素** | Flexbox / Grid `place-items: center` | **3 选 1 最简单** |
| **图文混排（文字环绕图）** | **Grid** 时代请用 `float: left` | 唯一保留 float 的场景 |
| **数据展示（多列对齐）** | 真 `<table>`（**不是布局**） | 表格做表格的事 |

### 16.5 选型决策树

```
你要排什么？
├─ 钉死一个元素（弹窗 / 固定栏 / tooltip）
│   └─ ✅ Position（fixed / absolute / sticky）
│
├─ 一行 / 一列内部分布（导航 / 按钮组 / 卡片行）
│   └─ ✅ Flexbox
│
├─ 整页 / 大区二维布局（后台 / 仪表盘 / 圣杯）
│   └─ ✅ Grid（父容器）+ Flexbox（子项）
│
├─ 文字环绕图片
│   └─ ⚠️ Float（仅此场景保留）
│
├─ 响应式卡片自动折行
│   └─ ✅ Flex `flex-wrap` 或 Grid `auto-fill + minmax`
│
├─ 全屏垂直居中
│   └─ ✅ Grid `place-items: center`（最快）
│
└─ 真表格数据展示
    └─ ✅ <table>（**不是布局**，是数据）
```

### 16.6 现代项目的"布局组合拳"

> **真实项目里 90% 用的是组合**——Grid 管整页，Flexbox 管区块，Position 管弹窗。

```html
<!-- 整页用 Grid 分三行 -->
<body style="display: grid; grid-template-rows: 60px 1fr 40px; min-height: 100vh;">
  <header>导航</header>
  <main style="display: grid; grid-template-columns: 200px 1fr;">
    <!-- 主区又用 Grid 分两列 -->
    <aside>侧栏</aside>
    <section style="display: flex; flex-wrap: wrap; gap: 16px;">
      <!-- 卡片区用 Flexbox 排一行 -->
      <div class="card">卡片 1</div>
      <div class="card">卡片 2</div>
      <div class="card">卡片 3</div>
    </section>
  </main>
  <footer>底栏</footer>
</body>

<!-- 弹窗用 Position -->
<div class="modal" style="position: fixed; inset: 0; background: rgba(0,0,0,0.5);">
  <div class="modal-body" style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);">
    弹窗内容
  </div>
</div>
```

> **口诀**：**Grid 画格子，Flex 排内容，Position 钉位置**——三件套覆盖 99% 场景。

### 16.7 老古董为什么该淘汰

| 老方式 | 问题 | 替代 |
| --- | --- | --- |
| **`<table>` 布局** | 语义错、不能响应式、SEO 差 | CSS Grid（**表格只用来展示数据**） |
| **float 布局** | 需要清浮动、难调、文档流破坏 | Flexbox / Grid |
| **inline-block 排版** | 有 4px 间隙 bug、垂直对齐差 | Flexbox |
| **`<frame>` / `<frameset>`** | HTML5 已废弃 | `<iframe>` / SPA 路由 |
| **`<center>` / `<font>`** | 已废弃 | CSS `text-align` / `font-family` |

### 16.8 实战速记

| 想做 | 1 行 |
| --- | --- |
| **一行水平居中** | `display: flex; justify-content: center;` |
| **一行垂直居中** | `display: flex; align-items: center;` |
| **两轴都居中** | `display: grid; place-items: center;` |
| **两列等高分栏** | `display: flex; > div { flex: 1; }` |
| **三列：固定 + 自适应 + 固定** | `display: grid; grid-template-columns: 200px 1fr 240px;` |
| **卡片自动换行** | `display: flex; flex-wrap: wrap; gap: 16px;` |
| **响应式卡片网格** | `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));` |
| **滚动时钉住** | `position: sticky; top: 0;` |
| **全屏覆盖** | `position: fixed; inset: 0;` |
| **滚动时让父容器可滚** | 父 `overflow: auto` + 子 `position: sticky; top: 0;` |

### 16.9 一句话

> **CSS 布局 = 三件套**：**Grid 画整页骨架 + Flexbox 排区块内容 + Position 钉浮层元素**。**别再写 `<table>` / `float` 布局了**——10 年前的技术，2026 年只用来"维护老古董"。

---
