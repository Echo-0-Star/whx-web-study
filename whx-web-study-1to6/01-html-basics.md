# 01 · HTML 基础

> 本节目标：从零开始理解 **HTML 在做什么、怎么写、怎么写"对"**。
>
> 学完本节，你应当能徒手写一个语义良好、SEO 友好、可访问性合格的网页骨架，并理解它在浏览器里是如何变成"页面"的。
>
> 📚 系列目录见 [README.md](./README.md)
>
> ⚠️ 这是整个系列里**最靠前**的一节，建议在 [03 环境与工具](./03-env-and-tools.md) 装好 VS Code 之后立刻开始。

---

## 目录

- [1. HTML 是什么？](#1-html-是什么)
- [2. 一个最简 HTML 文档](#2-一个最简-html-文档)
- [3. 标签、属性、内容三件套](#3-标签属性内容三件套)
- [4. 文本与排版标签](#4-文本与排版标签)
- [5. 链接、图片与多媒体](#5-链接图片与多媒体)
- [6. 列表、表格与表单](#6-列表表格与表单)
- [7. 语义化：让"机器"也看得懂](#7-语义化让机器也看得懂)
- [8. 可访问性（A11y）入门](#8-可访问性a11y入门)
- [9. HTML5 文档结构模板](#9-html5-文档结构模板)
- [10. HTML5 进阶：Canvas / SVG / Web API](#10-html5-进阶canvas--svg--web-api)
- [12. 常见布局 7 模板：HTML 骨架 + 配套 CSS 实战](#12-常见布局-7-模板html-骨架--配套-css-实战)
- [11. 小结与自检](#11-小结与自检)

---

## 1. HTML 是什么？

**HTML（HyperText Markup Language，超文本标记语言）** 是网页的"骨架"。

- 它**不是**编程语言（没有 if / for / 变量），而是**标记语言**——用一对对"标签"告诉浏览器：这里是一段标题，那里是一张图片。
- 浏览器拿到 HTML 之后，按它的"语义"渲染出页面；CSS 负责外观，JavaScript 负责行为。
- HTML 文件后缀通常是 `.html` 或 `.htm`。

```
HTML = 骨架   CSS = 衣服   JavaScript = 动作
```

> 一句话：**HTML 决定"页面上有什么"，CSS 决定"它长什么样"，JS 决定"它能做什么"。**

---

## 2. 一个最简 HTML 文档

把下面这段保存为 `hello.html`，双击用浏览器打开：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>我的第一个网页</title>
</head>
<body>
  <h1>你好，HTML！</h1>
  <p>这是我的第一段文字。</p>
</body>
</html>
```

你会看到：

- 浏览器标签页标题是"我的第一个网页"；
- 页面里有一个大标题"你好，HTML！"和一段普通文字。

### 2.1 逐行解释

| 行 | 作用 |
| --- | --- |
| `<!DOCTYPE html>` | 声明这是 HTML5 文档，**必须放在第一行** |
| `<html lang="zh-CN">` | 根元素；`lang` 属性告诉浏览器"内容是中文"，影响拼写检查和读屏 |
| `<head>` | 元信息区：标题、字符编码、CSS / JS 引用都在这里 |
| `<meta charset="UTF-8">` | 声明字符编码为 UTF-8，避免中文乱码 |
| `<title>...</title>` | 浏览器标签页、收藏夹、搜索结果标题 |
| `<body>` | 真正显示在页面里的所有内容 |

---

## 3. 标签、属性、内容三件套

### 3.1 标签的两种写法

```html
<!-- 1. 成对标签（开始 + 结束） -->
<p>这是一段文字</p>
<a href="https://example.com">链接</a>

<!-- 2. 自闭合标签（没有内容） -->
<img src="cat.jpg" alt="一只猫">
<br>     <!-- 换行 -->
<hr>     <!-- 横线 -->
<input>  <!-- 输入框 -->
```

> 自闭合标签**也可以**写成 `<br />` / `<img />`，加个斜杠更"像 XML"。在 HTML5 里两种写法都合法，团队约定用哪种就用哪种。

### 3.2 属性的语法

```html
<标签名 属性名="属性值" 属性名2="属性值2">内容</标签名>
```

```html
<a href="https://vuejs.org" target="_blank" rel="noopener">Vue 官网</a>
<img src="./logo.png" alt="Logo" width="120">
<input type="text" placeholder="请输入用户名" maxlength="20">
```

| 写法 | 含义 |
| --- | --- |
| `属性="值"` | 绝大多数情况 |
| `属性='值'` | 也合法，**但推荐用双引号** |
| `属性`（无值） | 布尔属性，如 `<input disabled>` |

> ⚠️ HTML 标签和属性**不区分大小写**（`<P>` 等同 `<p>`），但**强烈建议全部小写**——团队代码统一更好看。

### 3.3 注释

```html
<!-- 这是单行注释 -->
<!--
  这是
  多行注释
-->
```

> HTML 注释会**发送给浏览器**，但不会显示。**不要在注释里写密码、密钥、内部链接**等敏感信息。

---

## 4. 文本与排版标签

### 4.1 标题与段落

```html
<h1>一级标题（一个页面建议只出现 1 次）</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题（最小）</h6>

<p>这是一个段落。浏览器会在段落前后自动加一点空隙。</p>
<p>这是另一个段落。</p>
```

> **经验法则**：标题层级像"目录"一样按顺序使用：`h1` → `h2` → `h3`，不要跳级。

### 4.2 文本修饰

| 标签 | 含义 | 例子 |
| --- | --- | --- |
| `<strong>` | **重要**（加粗 + 语义） | `<strong>警告</strong>：请勿...` |
| `<em>` | *强调*（斜体 + 语义） | `<em>务必</em>完成` |
| `<b>` | 加粗（无语义） | 不推荐，新场景基本被 `<strong>` 替代 |
| `<i>` | 斜体（无语义） | 同上 |
| `<u>` | 下划线 | 仅作非链接的下划线时用 |
| `<s>` | 删除线 | `<s>原价 ¥999</s> ¥499` |
| `<small>` | 附属细则（小字） | 版权声明 |
| `<mark>` | 高亮（黄底） | 搜索结果关键字 |
| `<sub>` / `<sup>` | 下标 / 上标 | H<sub>2</sub>O，x<sup>2</sup> |
| `<code>` | 行内代码 | `const a = 1` |
| `<pre>` | 预格式化（保留空格换行） | 代码块 |
| `<kbd>` | 键盘按键 | 按 <kbd>Ctrl</kbd> + <kbd>S</kbd> |
| `<br>` | 强制换行 | 仅在**真的需要**时用 |
| `<hr>` | 主题分隔线 | 段落之间 |

> **强烈建议**：能用语义化标签（`<strong>`、`<em>`）就不要用纯样式标签（`<b>`、`<i>`）——前者对屏幕阅读器和搜索引擎更友好。

### 4.3 实体（转义字符）

HTML 里有些字符有特殊含义，需要用"实体"表示：

| 写法 | 含义 | 备注 |
| --- | --- | --- |
| `&lt;` | `<` | 小于号 |
| `&gt;` | `>` | 大于号 |
| `&amp;` | `&` | 和号 |
| `&quot;` | `"` | 双引号 |
| `&apos;` | `'` | 单引号（部分浏览器不识别） |
| `&nbsp;` | 不间断空格 | 防止被自动折行 |
| `&copy;` | © | 版权符号 |
| `&reg;` | ® | 注册商标 |

```html
<p>5 &lt; 10 &amp;&amp; 10 &gt; 5</p>
<!-- 显示为：5 < 10 && 10 > 5 -->
```

---

## 5. 链接、图片与多媒体

### 5.1 链接 `<a>`

```html
<!-- 外部链接 -->
<a href="https://example.com">Example</a>

<!-- 新标签页打开（注意加 rel="noopener" 防安全漏洞） -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">新窗口打开</a>

<!-- 内部链接（用相对路径） -->
<a href="./about.html">关于我们</a>
<a href="../index.html">回首页</a>

<!-- 锚点：跳到页面里某个 id 的位置 -->
<a href="#section-1">跳到第一节</a>
...
<h2 id="section-1">第一节</h2>

<!-- 邮件 / 电话 -->
<a href="mailto:hi@example.com">发邮件</a>
<a href="tel:+8613800000000">打电话</a>

<!-- 下载文件 -->
<a href="/files/report.pdf" download>下载报告.pdf</a>
```

### 5.2 图片 `<img>`

```html
<img
  src="./cat.jpg"      <!-- 图片地址 -->
  alt="一只橘猫"        <!-- 替代文字，图片加载失败时显示，对可访问性至关重要 -->
  width="600"          <!-- 宽度（CSS 里写更灵活） -->
  height="400"
  loading="lazy"       <!-- 懒加载：滚动到才加载 -->
  decoding="async"
>
```

> **alt 一定要写**。如果是纯装饰图，写 `alt=""` 让屏幕阅读器跳过它。

### 5.3 响应式图片

让浏览器根据屏幕宽度自动选择合适的图片，避免大图浪费流量：

```html
<img
  src="./cat-800.jpg"
  srcset="
    ./cat-400.jpg 400w,
    ./cat-800.jpg 800w,
    ./cat-1200.jpg 1200w
  "
  sizes="(max-width: 600px) 100vw, 50vw"
  alt="一只橘猫"
>
```

### 5.4 视频与音频

```html
<!-- 视频 -->
<video src="./demo.mp4" controls width="600" poster="./cover.jpg">
  你的浏览器不支持 video 标签。
</video>

<!-- 音频 -->
<audio src="./bgm.mp3" controls loop>
  你的浏览器不支持 audio 标签。
</audio>
```

> 商业项目里通常用第三方播放器（DPlayer、Video.js）或 `<iframe>` 嵌入 B 站 / YouTube。

---

## 6. 列表、表格与表单

### 6.1 列表

```html
<!-- 无序列表（unordered list） -->
<ul>
  <li>苹果</li>
  <li>香蕉</li>
  <li>橘子</li>
</ul>

<!-- 有序列表（ordered list） -->
<ol>
  <li>第一步</li>
  <li>第二步</li>
  <li>第三步</li>
</ol>

<!-- 描述列表（description list） -->
<dl>
  <dt>HTML</dt>
  <dd>超文本标记语言</dd>
  <dt>CSS</dt>
  <dd>层叠样式表</dd>
</dl>
```

### 6.2 表格

```html
<table>
  <caption>2025 年前端框架使用率</caption>
  <thead>
    <tr>
      <th>框架</th>
      <th>占比</th>
      <th>趋势</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Vue</td>
      <td>35%</td>
      <td>↑</td>
    </tr>
    <tr>
      <td>React</td>
      <td>42%</td>
      <td>→</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="3">数据来源：某调研</td>
    </tr>
  </tfoot>
</table>
```

| 标签 | 作用 |
| --- | --- |
| `<table>` | 表格 |
| `<caption>` | 标题（对屏幕阅读器友好） |
| `<thead>` / `<tbody>` / `<tfoot>` | 表头 / 表体 / 表尾 |
| `<tr>` | 一行 |
| `<th>` | 表头单元格（默认加粗居中） |
| `<td>` | 数据单元格 |
| `colspan` | 跨列合并 |
| `rowspan` | 跨行合并 |

> ⚠️ **表格只用来展示数据**，不要用 `<table>` 排版——这是 2003 年之前的做法，CSS 布局才是正道。

### 6.3 表单

```html
<form action="/api/login" method="POST">
  <!-- 文本输入 -->
  <label for="username">用户名：</label>
  <input
    type="text"
    id="username"
    name="username"
    placeholder="请输入用户名"
    required
    minlength="3"
    maxlength="20"
  >

  <!-- 密码 -->
  <label for="pwd">密码：</label>
  <input type="password" id="pwd" name="password" required minlength="6">

  <!-- 邮箱（自动校验格式） -->
  <label for="email">邮箱：</label>
  <input type="email" id="email" name="email">

  <!-- 数字 -->
  <label for="age">年龄：</label>
  <input type="number" id="age" name="age" min="0" max="150">

  <!-- 单选 -->
  <fieldset>
    <legend>性别</legend>
    <label><input type="radio" name="gender" value="m" checked> 男</label>
    <label><input type="radio" name="gender" value="f"> 女</label>
  </fieldset>

  <!-- 多选 -->
  <fieldset>
    <legend>爱好</legend>
    <label><input type="checkbox" name="hobby" value="code"> 写代码</label>
    <label><input type="checkbox" name="hobby" value="music"> 听音乐</label>
  </fieldset>

  <!-- 下拉 -->
  <label for="city">城市：</label>
  <select id="city" name="city">
    <option value="">请选择</option>
    <option value="bj">北京</option>
    <option value="sh" selected>上海</option>
  </select>

  <!-- 多行文本 -->
  <label for="msg">留言：</label>
  <textarea id="msg" name="message" rows="4" cols="40"></textarea>

  <!-- 提交按钮 -->
  <button type="submit">登录</button>
  <button type="reset">重置</button>
</form>
```

#### `<input type="...">` 速查

| type | 用途 | 备注 |
| --- | --- | --- |
| `text` | 单行文本 | 默认 |
| `password` | 密码 | 输入自动遮罩 |
| `email` | 邮箱 | 移动端弹邮箱键盘 |
| `url` | 网址 | |
| `number` | 数字 | 带上下箭头 |
| `tel` | 电话 | 移动端弹数字键盘 |
| `search` | 搜索 | 多数浏览器带"清除"按钮 |
| `date` / `time` / `datetime-local` | 日期时间 | 弹原生选择器 |
| `color` | 颜色选择 | |
| `range` | 滑块 | 配合 `min` `max` `step` |
| `file` | 上传文件 | 多文件加 `multiple` |
| `checkbox` | 复选框 | |
| `radio` | 单选框 | 同一组用相同 `name` |
| `hidden` | 隐藏字段 | 提交时附带、用户看不到 |

> 最重要的两点：
> 1. **`<label for="id">` 一定要绑**——点击 label 会自动聚焦输入框，对屏幕阅读器、键盘用户、移动端都更友好。
> 2. **`<button>` 一定要写 `type`**——在 `<form>` 里默认是 `submit`，可能误触。

---

## 7. 语义化：让"机器"也看得懂

**语义化** = 用**有意义的标签**表达内容，而不是万能的 `<div>` + `<span>`。

### 7.1 反例：没有语义的"div 大杂烩"

```html
<div class="header">
  <div class="nav">...</div>
</div>
<div class="main">
  <div class="article">...</div>
  <div class="side">...</div>
</div>
<div class="footer">...</div>
```

人看得懂，但**机器**（搜索引擎、屏幕阅读器、爬虫）只能"看到一堆 div"。

### 7.2 正例：使用 HTML5 语义标签

```html
<header>
  <nav>...</nav>
</header>
<main>
  <article>
    <h1>文章标题</h1>
    <section>
      <h2>第一节</h2>
      <p>...</p>
    </section>
  </article>
  <aside>侧边栏</aside>
</main>
<footer>页脚</footer>
```

### 7.3 常用语义标签速查

| 标签 | 用途 | 出现位置 |
| --- | --- | --- |
| `<header>` | 头部 / 横幅 | 页面顶部、`<article>` 顶部 |
| `<nav>` | 导航链接组 | 主导航、面包屑、TOC |
| `<main>` | 主体内容 | **一页面只一个** |
| `<article>` | 独立可分发的内容 | 文章、帖子、商品卡 |
| `<section>` | 主题性分组 | 带标题的段落 |
| `<aside>` | 侧边 / 附属 | 引用、相关推荐、广告 |
| `<footer>` | 尾部 | 版权、联系方式 |
| `<figure>` / `<figcaption>` | 图 + 图注 | 配图、代码片段、表格 |
| `<time>` | 时间 | `<time datetime="2025-01-01">元旦</time>` |
| `<address>` | 联系方式 | 作者邮箱、地址 |
| `<details>` / `<summary>` | 可折叠 | FAQ、可选展开内容 |

#### 折叠示例

```html
<details>
  <summary>点击展开答案</summary>
  <p>这是被折叠起来的内容……</p>
</details>
```

> 不需要写一行 JavaScript，**纯 HTML 就能做出折叠面板**。

### 7.4 语义化的三大好处

1. **SEO 友好**：搜索引擎能识别 `article` / `nav` / `h1`，给页面更准确的排名；
2. **可访问性**：屏幕阅读器按语义结构朗读，视障用户也能"读懂"页面；
3. **代码可读性**：团队成员看到 `<main>` 就知道这是主体，不用猜 `class="container"` 是啥意思。

---

## 8. 可访问性（A11y）入门

> A11y = Accessibility 的缩写，"a" 开头 + 11 个字母 + "y" 结尾。

### 8.1 四条黄金法则

| # | 法则 | 例子 |
| --- | --- | --- |
| 1 | **所有图片必须写 `alt`** | `<img alt="一只猫">` |
| 2 | **表单必须有 `<label>`** | `<label for="x">用户名</label> <input id="x">` |
| 3 | **结构用语义标签** | `<main>` `<nav>` `<article>` 而不是 `<div class="main">` |
| 4 | **颜色不是唯一信息载体** | 错误提示除了红色，还要有图标或文字 |

### 8.2 其他易忽视的细节

- 标题层级不要跳：`<h1>` → `<h3>` 是不对的；
- 链接文字要表意：`点这里` ❌，`<a>查看 2024 年度报告</a>` ✅；
- 按钮和链接分清楚：跳转用 `<a>`，触发动作用 `<button>`；
- 焦点可见：键盘 `Tab` 选中时**不要** `outline: none` 删掉焦点环。

### 8.3 ARIA 属性（进阶）

当原生标签无法表达语义时，用 ARIA 增强：

```html
<!-- 弹窗 -->
<div role="dialog" aria-modal="true" aria-labelledby="title">
  <h2 id="title">确认删除？</h2>
  <button>确定</button>
  <button>取消</button>
</div>

<!-- 加载状态 -->
<button aria-busy="true" aria-live="polite">提交中…</button>
```

> **经验法则**：能用原生标签就别用 ARIA；ARIA 是补位，不是替代。

---

## 9. HTML5 文档结构模板

把下面这段直接保存为 `index.html`，就是一个合格的"开箱即用"模板：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="页面描述，搜索结果会显示，建议 80–160 字">
  <meta name="keywords" content="关键词1, 关键词2">
  <meta name="author" content="作者名">

  <!-- Open Graph：分享到微信/微博/掘金时的卡片 -->
  <meta property="og:title" content="页面标题">
  <meta property="og:description" content="页面描述">
  <meta property="og:image" content="https://example.com/cover.png">
  <meta property="og:url" content="https://example.com/">

  <title>我的网页标题</title>
  <link rel="icon" href="/favicon.ico">
  <link rel="stylesheet" href="./style.css">
</head>
<body>
  <header>
    <nav>...</nav>
  </header>

  <main>
    <article>
      <h1>正文标题</h1>
      <p>正文内容……</p>
    </article>
    <aside>侧边栏</aside>
  </main>

  <footer>© 2025 My Website</footer>

  <script src="./main.js" defer></script>
</body>
</html>
```

### 9.1 重点 meta 速记

| meta | 作用 |
| --- | --- |
| `charset` | 字符编码，**必须**且**只能出现一次** |
| `viewport` | 移动端必须，否则页面会按 980px 渲染 |
| `description` | SEO 摘要 |
| `og:*` | 社交分享卡片 |

---

## 10. HTML5 进阶：Canvas / SVG / Web API

> HTML5 远不止"语义化标签"——它还引入了**绘图能力**（Canvas / SVG）、**本地存储**、**定位**、**拖拽**、**历史控制**等一整套浏览器侧 API。这一节是"选读 + 知道有这些"，**初学阶段不必全部掌握**，但**知道它存在**会帮你看文档 / 写需求时不再发蒙。

### 10.1 Canvas：位图绘图

**Canvas** 是一张"可以拿 JS 在上面画画的画布"，**像素级**控制（位图）。适合图表、游戏、图像处理。

```html
<canvas id="myCanvas" width="400" height="200" style="border:1px solid #ccc;">
  你的浏览器不支持 canvas。
</canvas>

<script>
  const cvs = document.getElementById('myCanvas')
  const ctx = cvs.getContext('2d')

  // 画矩形
  ctx.fillStyle = '#42b883'
  ctx.fillRect(20, 20, 100, 60)         // 起点 (20,20)，宽 100 高 60

  // 画文字
  ctx.font = '20px sans-serif'
  ctx.fillStyle = '#333'
  ctx.fillText('Hello Canvas', 20, 120)

  // 画路径
  ctx.beginPath()
  ctx.arc(200, 100, 40, 0, Math.PI * 2) // 圆心 (200,100)，半径 40
  ctx.fillStyle = 'tomato'
  ctx.fill()
</script>
```

| Canvas 的能力 | 例子 |
| --- | --- |
| 2D 绘图 | 图表、签名板 |
| `getContext('webgl')` | 3D 渲染、Three.js |
| `toDataURL()` | 把画布转成图片 |
| `captureStream()` | 截图、录屏 |

> **学习曲线**：原生 API 很底层，实际项目里几乎都用 [ECharts](https://echarts.apache.org/) / [Chart.js](https://www.chartjs.org/) / [Three.js](https://threejs.org/) 等库。

### 10.2 SVG：矢量图

**SVG（Scalable Vector Graphics）** 是**基于 XML 的矢量图形**——放大缩小都不失真，**可以用 CSS 改样式、用 JS 监听事件**。

```html
<svg width="200" height="200" viewBox="0 0 100 100">
  <!-- 圆 -->
  <circle cx="50" cy="50" r="40" fill="#42b883" />
  <!-- 矩形 -->
  <rect x="10" y="10" width="30" height="30" fill="tomato" />
  <!-- 路径 -->
  <path d="M10 80 Q 50 20 90 80" stroke="#333" stroke-width="2" fill="none" />
  <!-- 文字 -->
  <text x="50" y="95" text-anchor="middle" font-size="10">SVG</text>
</svg>
```

#### Canvas vs SVG 速记

| 维度 | Canvas | SVG |
| --- | --- | --- |
| 类型 | 位图（栅格） | 矢量 |
| 放大失真 | 会（像素被放大） | 不会（始终清晰） |
| 适合场景 | 图、画、游戏 | 图标、Logo、地图、可缩放插图 |
| 操作方式 | JS 画 | DOM 操作 / CSS 改样式 |
| 性能 | 大量元素时仍快 | 元素过多（>1k）会变慢 |
| 可访问性 | 弱（要自己加 ARIA） | 强（DOM 可被读屏） |

> **经验法则**：图标 / Logo / 简单插画用 **SVG**；复杂图表 / 像素操作 / 动画用 **Canvas**。

### 10.3 Web Storage：浏览器本地存储

HTML5 提供两种"键值对"本地存储：

| API | 容量 | 是否随请求发送 | 适用 |
| --- | --- | --- | --- |
| **localStorage** | 5–10 MB | 否 | 长期保存：主题、登录态、用户偏好 |
| **sessionStorage** | 5–10 MB | 否 | 会话级：临时表单数据 |
| **Cookie** | 4 KB | **是**（每次请求都带） | 鉴权、跟踪 |

```js
// localStorage
localStorage.setItem('theme', 'dark')
localStorage.getItem('theme')          // 'dark'
localStorage.removeItem('theme')
localStorage.clear()                   // 清空所有

// 只能存字符串，对象要 JSON 化
const user = { name: 'Tom', age: 18 }
localStorage.setItem('user', JSON.stringify(user))
JSON.parse(localStorage.getItem('user'))  // { name:'Tom', age:18 }
```

> **重要**：localStorage 受**同源策略**限制，跨域名读不到。**不要在 localStorage 存密码或 token**——XSS 攻击可直接读出来。

### 10.4 Geolocation：定位

```js
// 必须用户授权
navigator.geolocation.getCurrentPosition(
  pos => {
    const { latitude, longitude } = pos.coords
    console.log('你在：', latitude, longitude)
  },
  err => console.error('定位失败：', err.message)
)

// 持续监听（地图类应用）
const watchId = navigator.geolocation.watchPosition(/* 同上 */)
// 停止
navigator.geolocation.clearWatch(watchId)
```

> 浏览器定位通常用 IP / Wi-Fi / GPS，**精度和速度因设备而异**。HTTPS 站点才能用。

### 10.5 Drag & Drop：拖拽

HTML5 让任何元素都可以被拖动。

```html
<div id="src" draggable="true">把我拖到右边</div>
<div id="dst" style="width:200px;height:100px;border:1px dashed #ccc">目标</div>

<script>
  const src = document.getElementById('src')
  const dst = document.getElementById('dst')

  src.addEventListener('dragstart', e => {
    e.dataTransfer.setData('text/plain', 'hello')
  })

  dst.addEventListener('dragover', e => {
    e.preventDefault()  // 必须阻止默认，否则 drop 不触发
  })

  dst.addEventListener('drop', e => {
    e.preventDefault()
    const data = e.dataTransfer.getData('text/plain')
    dst.textContent = `收到：${data}`
  })
</script>
```

> 简单拖拽用原生 API；复杂场景（如 Sortable.js 风格的列表排序）推荐用 [SortableJS](https://github.com/SortableJS/Sortable)、[vue-draggable-plus](https://github.com/Alfred-Skyblue/vue-draggable-plus) 等库。

### 10.6 History API：单页应用"假路由"的基础

SPA 切换页面**不发新请求**，靠的就是 History API。

```js
// 入栈一条历史（URL 栏会变化，但不会向服务器发请求）
history.pushState({ page: 'about' }, '关于', '/about')

// 替换当前历史（不增加新条目）
history.replaceState({ page: 'home' }, '首页', '/')

// 监听前进 / 后退
window.addEventListener('popstate', e => {
  console.log('用户按了前进/后退：', e.state)
})
```

> 这就是 Vue Router / React Router 路由跳转的底层原理。

### 10.7 其它常用 HTML5 API 速览

| API | 作用 | 典型场景 |
| --- | --- | --- |
| **Drag & Drop** | 拖拽 | 文件上传、看板 |
| **Geolocation** | 定位 | 地图、外卖 |
| **Web Storage** | 本地存储 | 用户偏好、表单缓存 |
| **History** | 路由控制 | SPA |
| **Canvas / WebGL** | 绘图 | 游戏、图表、3D |
| **WebSocket** | 全双工通信 | 聊天室、实时行情 |
| **WebRTC** | 端到端音视频 | 视频会议 |
| **Notification** | 系统通知 | 消息推送 |
| **Service Worker** | 离线 / 缓存 | PWA |
| **Web Workers** | 后台线程 | 大计算不卡 UI |
| **IndexedDB** | 浏览器端数据库 | 离线应用 |
| **File API** | 读本地文件 | 上传预览、在线编辑器 |
| **Clipboard** | 剪贴板 | 一键复制 |
| **IntersectionObserver** | 元素可见性 | 懒加载、无限滚动 |
| **`<input type="date/color/range">`** | 原生 UI | 表单体验优化 |

> **初学阶段**：先掌握 `localStorage` + `History` + `Canvas / SVG` 这三个就足够；其余**用到再查**。

### 10.8 推荐阅读

- MDN HTML5 API 总览：<https://developer.mozilla.org/zh-CN/docs/Web/API>
- Canvas 教程：<https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial>
- SVG 教程：<https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial>
- Web Storage 规范：<https://html.spec.whatwg.org/multipage/webstorage.html>

---

## 11. 小结与自检

### 11.1 自检清单

| # | 题目 | 关键点 |
| --- | --- | --- |
| 1 | 手写一个 HTML5 文档，标题"我的主页"，包含一段 h1 和一段 p | 文档结构、`<head>` vs `<body>` |
| 2 | 用 `<ul><li>` 列出你最喜欢的 3 个网站，外链用 `target="_blank" rel="noopener"` | 链接安全 |
| 3 | 插入一张图片，写出有意义的 `alt`；再插入一个视频 | 媒体标签 + alt 必备 |
| 4 | 写一个登录表单：用户名、密码、记住我 checkbox、提交按钮 | `<form>`、`<label>`、`<input>` |
| 5 | 把 5 个 `<div>` 改写成 `<header><nav><main><article><footer>` | 语义化 |
| 6 | 用 `<details>` 写一个 FAQ 折叠面板 | 语义标签的隐藏能力 |
| 7 | 用 `<canvas>` 画一个红圆 + 一行文字 | Canvas 2D 基础 |
| 8 | 用 `<svg>` 画一个三角形 | SVG 基础 |
| 9 | 用 `localStorage` 存一个计数器，刷新页面后数值不丢 | Web Storage |

### 11.2 推荐练习

1. 找 2 个你常去的网站 → View Source → 看它们的 HTML 结构，试着挑出语义化标签；
2. 用本节模板抄一个自己的"个人主页"骨架（不用 CSS，纯结构）；
3. 用 [W3C HTML Validator](https://validator.w3.org/) 检查自己写的 HTML 是否合规；
4. （进阶）用 Canvas 写一个"鼠标轨迹"——移动鼠标时画线，松开时清空。

### 11.3 下一步

- 学完本节后，进入 [02 CSS 基础](./02-css-basics.md)，让刚才写的骨架变漂亮；
- 之后是 [03 环境与工具](./03-env-and-tools.md) / [04 Web 概念](./04-web-concepts.md)，了解浏览器是怎么渲染这些 HTML 的；
- 等你掌握 JS 后，可以用 [07 Vue 3 基础](./07-vue3-basics.md) 把 HTML 写成可复用的"组件"。

### 推荐阅读

- MDN HTML 教程：<https://developer.mozilla.org/zh-CN/docs/Learn/HTML>
- W3C HTML5 规范：<https://html.spec.whatwg.org/>
- HTML 语义化参考：<https://www.w3schools.com/html/html5_semantic_elements.asp>

---

## 12. 常见布局 7 模板：HTML 骨架 + 配套 CSS 实战

> 学完前 11 节你已经会写"对的" HTML 标签——但**怎么搭出一个完整页面**还是无从下手。这一节给你 7 个**业界最常用**的页面布局模板，**每个都包含完整 HTML 骨架 + 配套 CSS**，复制即可用。
>
> 这些模板是"页面骨架"，不依赖任何框架（Vue / React / Svelte）。**在 Vue / React 项目里，`<template>` 部分就是这些 HTML**。

### 12.1 选哪个模板？速查表

| 模板 | 适用场景 | 关键特征 |
| --- | --- | --- |
| 1. **顶部导航 + 内容** | 文档 / 博客 / 后台简单页 | `<header>` + `<main>` + `<footer>` |
| 2. **后台经典布局** | 后台管理系统 | 左侧菜单 + 右侧主区，**固定高度** |
| 3. **三列布局（圣杯）** | 门户网站 / 营销页 | header + left + main + right + footer |
| 4. **响应式卡片网格** | 商品列表 / 博客列表 / 仪表盘 | 移动 1 列、平板 2 列、桌面 3 列 |
| 5. **居中卡片 + 行动按钮** | 登录 / 注册 / 错误页 | 单个 `<form>` 居中 |
| 6. **侧边栏 + 主内容** | 文档站 / Notion 类应用 | 左侧导航可折叠 |
| 7. **底部固定栏（页脚/CTA）** | 移动端操作栏 / 聊天按钮 | 始终在视口底部 |

---

### 模板 1：顶部导航 + 内容（最常用）

**适用**：博客、文档、官网首页、营销落地页。**90% 的简单页面**就用这个。

**HTML 骨架**：

```html
<header class="site-header">
  <div class="logo">MySite</div>
  <nav class="nav">
    <a href="/">首页</a>
    <a href="/docs">文档</a>
    <a href="/blog">博客</a>
    <a href="/about">关于</a>
  </nav>
</header>

<main class="content">
  <h1>页面标题</h1>
  <p>正文内容……</p>
</main>

<footer class="site-footer">
  © 2025 MySite · <a href="/privacy">隐私</a>
</footer>
```

**配套 CSS**：

```css
:root {
  --color-fg: #333;
  --color-muted: #888;
  --color-bg: #fff;
  --color-border: #eee;
  --max-width: 960px;
  --space: 16px;
}

body {
  margin: 0;
  font-family: system-ui, sans-serif;
  color: var(--color-fg);
  background: var(--color-bg);
}

/* 顶部：水平排布 + 间距 */
.site-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space);
  border-bottom: 1px solid var(--color-border);
}

.nav { display: flex; gap: 16px; }
.nav a { color: var(--color-fg); text-decoration: none; }
.nav a:hover { color: #42b883; }

/* 主内容：最大宽度 + 居中 */
.content {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 32px var(--space);
}

/* 底部：灰底小字 */
.site-footer {
  padding: var(--space);
  text-align: center;
  color: var(--color-muted);
  border-top: 1px solid var(--color-border);
}
```

---

### 模板 2：后台经典布局（左侧菜单 + 右侧主区）

**适用**：后台管理系统、仪表盘、CMS。**几乎所有后台框架**（Ant Design / Element Plus）都用这个。

**HTML 骨架**：

```html
<div class="layout">
  <aside class="sidebar">
    <div class="logo">Admin</div>
    <nav>
      <a href="/dashboard" class="active">仪表盘</a>
      <a href="/users">用户</a>
      <a href="/orders">订单</a>
      <a href="/settings">设置</a>
    </nav>
  </aside>

  <div class="main">
    <header class="topbar">
      <input class="search" placeholder="搜索…" />
      <div class="user">Tom</div>
    </header>

    <section class="page">
      <h1>仪表盘</h1>
      <p>欢迎回来</p>
    </section>
  </div>
</div>
```

**配套 CSS**：

```css
html, body { height: 100%; margin: 0; }
body { font-family: system-ui; }

.layout {
  display: flex;
  height: 100vh;                  /* 关键：占满视口高度 */
}

/* 侧栏：固定宽度 + 满高 */
.sidebar {
  width: 220px;
  background: #1f2937;
  color: #fff;
  display: flex;
  flex-direction: column;
}
.sidebar .logo { padding: 16px; font-size: 20px; }
.sidebar nav { display: flex; flex-direction: column; }
.sidebar nav a {
  color: #d1d5db;
  text-decoration: none;
  padding: 12px 16px;
}
.sidebar nav a:hover { background: #374151; }
.sidebar nav a.active { background: #42b883; color: #fff; }

/* 右侧主区：占满剩余 */
.main { flex: 1; display: flex; flex-direction: column; min-width: 0; }

/* 顶栏 */
.topbar {
  height: 56px;
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 24px;
  border-bottom: 1px solid #eee;
}
.search { width: 200px; padding: 6px 12px; border: 1px solid #ddd; border-radius: 4px; }
.user { font-weight: 500; }

/* 页面内容区：自己滚动 */
.page { flex: 1; padding: 24px; overflow-y: auto; }
```

---

### 模板 3：三列布局（圣杯 / Holy Grail）

**适用**：门户网站首页（**左侧导航 + 中间内容 + 右侧广告 / 工具栏**）、博客带侧栏。

**HTML 骨架**：

```html
<header class="header">Header</header>
<div class="container">
  <aside class="left">左</aside>
  <main class="main">中</main>
  <aside class="right">右</aside>
</div>
<footer class="footer">Footer</footer>
```

**配套 CSS（Grid 版，最简）**：

```css
body { margin: 0; }

.header, .footer {
  background: #1f2937;
  color: #fff;
  text-align: center;
  padding: 16px;
}

.container {
  display: grid;
  grid-template-columns: 200px 1fr 240px;   /* 三列：固定 / 自适应 / 固定 */
  min-height: calc(100vh - 96px);          /* 减去 header + footer 高度 */
}

.left, .right { background: #f5f5f5; padding: 16px; }
.main { padding: 16px; }
```

**经典坑**：如果用 `flex` 实现三列，**中间列必须先渲染**才能"撑起"两侧——所以**推荐 Grid 一行搞定**。

---

### 模板 4：响应式卡片网格

**适用**：商品列表、博客列表、仪表盘、Portfolio 作品集。

**HTML 骨架**：

```html
<div class="grid">
  <article class="card">
    <img src="https://picsum.photos/300/200" alt="" />
    <h3>卡片标题 1</h3>
    <p>描述文字……</p>
  </article>
  <article class="card">
    <img src="https://picsum.photos/300/201" alt="" />
    <h3>卡片标题 2</h3>
    <p>描述文字……</p>
  </article>
  <!-- 更多卡片…… -->
</div>
```

**配套 CSS**：

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
  padding: 16px;
}

.card {
  border: 1px solid #eee;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s;
}
.card:hover { transform: translateY(-4px); box-shadow: 0 4px 12px rgba(0,0,0,0.08); }

.card img { width: 100%; aspect-ratio: 16 / 10; object-fit: cover; display: block; }
.card h3 { margin: 12px 16px 4px; }
.card p  { margin: 0 16px 16px; color: #666; }

/* 移动端调整：< 600px 自动变 1 列（auto-fill 天然支持） */
```

> **关键**：`auto-fill + minmax(280px, 1fr)` 让"屏幕能放几列就放几列"——移动端 1 列、桌面 3–4 列，**不用写媒体查询**。

---

### 模板 5：居中卡片 + 行动按钮（登录页）

**适用**：登录 / 注册 / 404 / 支付成功 / "请稍候"等待页。

**HTML 骨架**：

```html
<main class="auth-page">
  <form class="auth-card" method="post">
    <h1>登录</h1>
    <label>
      <span>用户名</span>
      <input type="text" name="username" required autofocus>
    </label>
    <label>
      <span>密码</span>
      <input type="password" name="password" required>
    </label>
    <button type="submit">登录</button>
    <p class="hint"><a href="/signup">还没有账号？</a></p>
  </form>
</main>
```

**配套 CSS**：

```css
.auth-page {
  min-height: 100vh;
  display: grid;
  place-items: center;               /* 完美居中 */
  background: #f5f7fa;
  padding: 16px;
}

.auth-card {
  width: 100%;
  max-width: 360px;
  background: #fff;
  padding: 32px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
  display: flex; flex-direction: column; gap: 16px;
}

.auth-card h1 { margin: 0 0 8px; text-align: center; }

.auth-card label { display: flex; flex-direction: column; gap: 4px; }
.auth-card input {
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}
.auth-card input:focus { outline: 2px solid #42b883; border-color: transparent; }

.auth-card button {
  padding: 12px;
  background: #42b883;
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
}
.auth-card button:hover { background: #369870; }

.hint { text-align: center; color: #666; margin: 0; }
```

---

### 模板 6：侧边栏 + 主内容（文档站 / Notion 风格）

**适用**：文档站、Notion 类应用、Settings 页面。

**HTML 骨架**：

```html
<div class="doc-layout">
  <nav class="doc-sidebar">
    <div class="search">搜索…</div>
    <ul>
      <li><a href="#intro" class="active">介绍</a></li>
      <li><a href="#install">安装</a></li>
      <li><a href="#usage">用法</a></li>
      <li><a href="#api">API</a></li>
    </ul>
  </nav>

  <main class="doc-content">
    <h1>文档标题</h1>
    <p>正文……</p>
  </main>
</div>
```

**配套 CSS**：

```css
html, body { height: 100%; margin: 0; }

.doc-layout {
  display: flex;
  min-height: 100vh;
}

.doc-sidebar {
  width: 260px;
  flex-shrink: 0;
  background: #fafafa;
  border-right: 1px solid #eee;
  padding: 16px;
  position: sticky;                  /* 滚动时钉住 */
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

.doc-sidebar .search {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  margin-bottom: 16px;
}

.doc-sidebar ul { list-style: none; padding: 0; margin: 0; }
.doc-sidebar a {
  display: block;
  padding: 8px 12px;
  color: #333;
  text-decoration: none;
  border-radius: 4px;
}
.doc-sidebar a:hover { background: #eee; }
.doc-sidebar a.active { background: #42b883; color: #fff; }

.doc-content {
  flex: 1;
  padding: 48px 64px;
  max-width: 880px;
}
```

---

### 模板 7：底部固定栏（移动端操作栏 / 客服按钮）

**适用**：移动端"加入购物车 / 立即购买"栏、悬浮客服按钮。

**HTML 骨架**：

```html
<main class="content">
  <h1>商品详情</h1>
  <p>这里是商品描述……</p>
</main>

<div class="bottom-bar">
  <button class="cart">加入购物车</button>
  <button class="buy">立即购买</button>
</div>
```

**配套 CSS**：

```css
body { margin: 0; padding-bottom: 72px; }   /* 给底部栏留位置 */

.content {
  max-width: 720px;
  margin: 0 auto;
  padding: 24px 16px;
}

.bottom-bar {
  position: fixed;                 /* 关键：固定在视口底部 */
  bottom: 0; left: 0; right: 0;
  background: #fff;
  border-top: 1px solid #eee;
  padding: 8px 16px;
  display: flex;
  gap: 12px;
  z-index: 10;
  /* 适配 iPhone 底部安全区 */
  padding-bottom: calc(8px + env(safe-area-inset-bottom));
}

.bottom-bar button {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 6px;
  font-size: 16px;
}
.cart { background: #f5f5f5; }
.buy  { background: #42b883; color: #fff; }
```

---

### 12.x 配套：圆形头像 / 渐变占位 / 卡片阴影常用类

```css
/* 圆形头像 */
.avatar {
  width: 40px; height: 40px;
  border-radius: 50%;
  object-fit: cover;
}

/* 渐变占位（无图时显示） */
.gradient-placeholder {
  background: linear-gradient(135deg, #42b883 0%, #369870 100%);
  color: #fff;
  display: grid; place-items: center;
  font-weight: bold;
}

/* 4 级卡片阴影 */
.shadow-sm { box-shadow: 0 1px 2px rgba(0,0,0,0.05); }
.shadow-md { box-shadow: 0 4px 8px rgba(0,0,0,0.08); }
.shadow-lg { box-shadow: 0 10px 20px rgba(0,0,0,0.10); }

/* 文字截断 1 / 2 / 3 行 */
.truncate-1 {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.truncate-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.truncate-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

---

### 12.xi 一句话选型

> **不会搭页面？** → 模板 1（顶部导航 + 内容）
> **做后台？** → 模板 2（左侧菜单）
> **做商品 / 列表？** → 模板 4（响应式卡片）
> **做登录？** → 模板 5（居中卡片）
> **做文档？** → 模板 6（侧边栏）
> **移动端有底部按钮？** → 模板 7
