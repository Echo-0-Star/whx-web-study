# 03 · 环境与工具

> 本节目标：在自己电脑上装好一个能写、能跑、能调试前端代码的完整环境，并掌握把它**部署到生产环境**的整套工具链。
>
> 学完本节，你应当能够独立完成浏览器、Node.js、包管理器、编辑器的安装与基础配置；并能把自己的项目用 Docker 打包、推送到镜像仓库、跑在云服务器上。
>
> 📚 系列目录见 [README.md](./README.md)

---

## 目录

- [1. 浏览器](#1-浏览器)
- [2. Node.js 环境](#2-nodejs-环境)
- [3. 包管理工具](#3-包管理工具)
- [4. 开发工具：VS Code](#4-开发工具vs-code)
- [5. 终端与 Shell 速览](#5-终端与-shell-速览)
- [6. 小结与自检](#6-小结与自检)
- [7. 部署环境与容器化](#7-部署环境与容器化)

---

## 1. 浏览器

浏览器是前端的"目的地"——你写的所有 HTML / CSS / JavaScript 最终都要在浏览器里执行。所以第一件事是装一个对 Web 标准支持良好、并且自带开发者工具的浏览器。

### 1.1 三大主流浏览器对比

| 浏览器 | 内核 | 特点 | 推荐场景 |
| --- | --- | --- | --- |
| **Firefox Developer Edition** | Gecko | 对最新 Web 标准支持最激进，DevTools 体验出色 | 想深入理解标准、写实验性特性的同学 |
| **Chrome** | Blink | 市场份额最大、DevTools 生态最强、扩展最丰富 | 日常开发首选，强烈建议**至少装一个** |
| **Microsoft Edge** | Blink（Chromium 内核） | Win10 / Win11 系统自带，IE 模式兼容老旧系统 | 公司电脑、调试仅在 Edge 上出现的 bug |

> 小知识：Chrome 和 Edge 的内核都叫 **Chromium / Blink**，所以两者在网页渲染上几乎一致；真正不同的是扩展商店和内置功能。

### 1.2 下载地址

- **Firefox Developer Edition**：<https://www.firefox.com/zh-CN/download/all/>
- **Chrome 正式版**：<https://www.google.cn/chrome/>
- **Chrome Dev / Canary（尝鲜版）**：<https://www.google.cn/intl/zh-CN/chrome/dev/>
- **Edge**：Win10 / Win11 已自带，单独下载：<https://www.microsoft.com/zh-cn/edge>

### 1.3 初学者建议

- **首选 Chrome**：DevTools 教程最多、Stack Overflow 上的截图也基本是它；
- 想要"原汁原味"体验标准，可以再装一个 **Firefox Developer Edition** 做交叉验证；
- **不要用 IE**：它早已停止支持，很多新语法（ES6+、CSS Grid 等）都跑不起来。

### 1.4 DevTools 必会快捷键

按 `F12` 或 `Ctrl + Shift + I`（Mac：`Cmd + Opt + I`）打开开发者工具，常用面板：

| 面板 | 用途 | 何时打开它 |
| --- | --- | --- |
| **Elements / 元素** | 查看 / 临时修改页面的 HTML 和 CSS | 调样式、排查布局 |
| **Console / 控制台** | 执行 JS 表达式、查看 `console.log` 和报错 | 调试 JS、查看接口返回 |
| **Sources / 源代码** | 打断点调试 JS | 复杂逻辑排错 |
| **Network / 网络** | 查看每一次 HTTP 请求的状态、耗时、返回内容 | 排查"接口没拿到数据" |
| **Application / 应用** | 查看 LocalStorage、Cookie、Service Worker 等 | 清缓存、看登录态 |

> 调试小技巧：选中 Elements 中的节点按 `H` 可快速隐藏该元素；Console 中按 `↑` 调出历史命令；`$0` 表示当前选中的元素，`$_` 表示上一次执行结果。

---

## 2. Node.js 环境

Node.js 让你**在浏览器之外**运行 JavaScript 代码。前端工程化几乎全部依赖它：构建工具（Vite、Webpack）、包管理器、代码格式化工具、Mock 服务等都跑在 Node 上。

### 2.1 两种安装思路

| 思路 | 适用人群 | 优点 | 缺点 |
| --- | --- | --- | --- |
| **官网直装** | 只做一两个项目、懒得折腾 | 一键安装、零配置 | 多项目并存时切换版本麻烦 |
| **使用版本管理工具** | 长期学习、要做多个项目 | 一台机器装多个 Node 版本、按项目自动切换 | 初次配置多花 10 分钟 |

> 新手建议：先**官网直装**起步，等遇到"项目 A 要 Node 18，项目 B 要 Node 20"再换版本管理工具。

### 2.2 官网直接安装

- 下载地址：<https://nodejs.org/zh-cn>
- 推荐下载 **LTS（长期支持版）**，目前是 20.x 或 22.x。LTS 稳定、文档多、生态兼容性最好。
- 安装时 Windows 用户**保持默认勾选 "Add to PATH"** 即可。

安装完成后打开终端验证：

```bash
node -v    # 查看 Node 版本
npm -v     # 查看内置 npm 版本
```

只要能输出版本号（例如 `v20.18.0`），就成功了。

### 2.3 版本管理工具（nvm / fnm / volta）

当你需要同时维护多个项目、不同项目要求不同 Node 版本时，强烈建议使用版本管理工具。三者对比如下：

| 工具 | 平台 | 安装难度 | 速度 | 推荐度 |
| --- | --- | --- | --- | --- |
| **nvm**（最流行） | macOS / Linux 原生；Windows 需用 `nvm-windows` | 中等 | 中 | ⭐⭐⭐⭐ |
| **fnm**（Fast Node Manager） | 全平台（Rust 写的） | 简单 | **最快** | ⭐⭐⭐⭐⭐（最推荐） |
| **volta** | 全平台 | 简单 | 快 | ⭐⭐⭐⭐ |

下面以 **fnm** 为例演示安装流程，其他工具的官方文档都很完善。

#### macOS / Linux 一键安装

```bash
curl -o- https://fnm.vercel.app/install | bash
```

#### Windows（推荐使用预编译二进制）

1. 前往 fnm 的 GitHub Releases：<https://github.com/Schniz/fnm/releases>
2. 下载最新版本的 `fnm-windows.zip`
3. 解压到任意目录，例如 `C:\Tools\fnm`
4. 把 `C:\Tools\fnm` 加到系统环境变量 `PATH` 中
5. 在 PowerShell / cmd 中执行 `fnm --version` 验证

#### 常用命令

```bash
fnm install --lts          # 安装最新 LTS 版本
fnm install 20             # 安装指定主版本号为 20 的最新版
fnm use 20                 # 当前 shell 切换到 20.x
fnm default 20             # 把 20.x 设为全局默认
fnm list                   # 查看已安装的所有版本
```

#### 让终端自动识别项目中的 Node 版本

fnm 提供了 `--use-on-cd` 选项，进入目录时若发现 `.node-version` 或 `package.json` 里的 `engines.node` 字段，会自动切换版本。配置方式：

**PowerShell（Windows 推荐）**

把下面这行追加到 PowerShell 的 profile 文件中：

```powershell
fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
```

profile 文件位置：
- macOS / Linux：`~/.config/powershell/Microsoft.PowerShell_profile.ps1`
- Windows PowerShell 5：`%userprofile%\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`
- Windows PowerShell 6+：`%userprofile%\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`

如果文件还不存在，可以一键创建并编辑：

```powershell
if (-not (Test-Path $profile)) { New-Item $profile -Force }
Invoke-Item $profile
```

**cmd（Windows）**

把下面这段保存为 `fnm-init.cmd` 并放到 `C:\Tools\fnm` 目录下，然后右键"我的电脑 → 属性 → 高级系统设置 → 环境变量 → 用户变量 → `AutoRun`"中写入该文件路径：

```bat
@echo off
:: for /F 会启动新的 cmd 实例，所以用守卫变量避免无限循环
if not defined FNM_AUTORUN_GUARD (
    set "FNM_AUTORUN_GUARD=AutorunGuard"
    FOR /f "tokens=*" %%z IN ('fnm env --use-on-cd') DO CALL %%z
)
```

### 2.4 一组容易混淆的概念

- **Node.js**：JS 运行时（让 JS 能在电脑 / 服务器上跑）；
- **npm / pnpm / yarn**：包管理器（装卸第三方 JS 库、管理 `package.json`）；
- **nvm / fnm / volta**：Node 版本管理工具（在多个 Node 之间切换）；
- **bun**：新的"全能选手"，**既是运行时又是包管理器**，但生态尚未完全成熟，初学阶段了解即可。

> 简单记：**Node 是引擎，npm/pnpm/yarn 是加油站，nvm/fnm 是换引擎的工具箱。**

---

## 3. 包管理工具

包管理工具负责三件事：**装**第三方库（React、Vue、Lodash ……）、**卸**不再用的库、**锁**住版本让团队成员装到一致的内容。

### 3.1 四大包管理工具对比

| 工具 | 出身 | 核心特点 | 适用人群 |
| --- | --- | --- | --- |
| **npm** | Node.js 官方内置 | 生态最全、文档最多、零额外安装 | 零基础入门首选 |
| **pnpm** | 社区 | 用**硬链接 + 符号链接**复用磁盘，安装极快、占用极小 | 多人协作、磁盘空间敏感 |
| **yarn** | Facebook 推出 | 曾经的"npm 杀手"，`yarn.lock` 锁定更早 | 维护老项目、Berry 模式体验派 |
| **bun** | 创业公司 | **运行时 + 包管理器二合一**，速度炸裂 | 追求极致性能、愿意尝鲜 |

> 新手建议：**从 npm 起步**。学会用 `package.json`、`node_modules`、`lock 文件`三个概念后，再按需切换 pnpm 或 yarn 都不难。

### 3.2 npm 入门速查

#### 初始化一个项目

```bash
mkdir my-project
cd my-project
npm init            # 一问一答式生成 package.json
npm init -y         # 全部用默认值，快速生成
```

#### 用模板新建项目（最常用的姿势）

```bash
# 用 Vue 官方模板创建一个 Vue 3 项目
npm create vue@latest

# 用 Vite 模板创建一个通用项目（你可以再选 Vue / React / Svelte ...）
npm create vite@latest
```

> 看到 `npm create xxx` 时，本质上等价于"先临时下载 `create-xxx` 工具，再用它生成项目"。

#### 装卸依赖

```bash
npm install vue              # 安装 vue，作为运行依赖
npm install -D eslint        # -D 表示作为"开发依赖"
npm install                  # 根据 package.json 一次性装齐所有依赖
npm uninstall vue            # 卸载
```

#### 常见问题：装包慢 / 卡住 / 报错

国内访问 npm 官方源（`registry.npmjs.org`）有时会卡，**切到国内镜像**是最直接的解决办法：

```bash
# 临时使用淘宝镜像
npm install vue --registry=https://registry.npmmirror.com

# 全局设置默认镜像
npm config set registry https://registry.npmmirror.com

# 验证当前源
npm config get registry
```

如果以后切回官方源：`npm config set registry https://registry.npmjs.org`。

### 3.3 pnpm 速查（强烈建议作为长期方案）

```bash
# 安装 pnpm（需要先装好 Node.js）
npm install -g pnpm

# 装依赖
pnpm install
pnpm add vue
pnpm add -D eslint

# 装全局包
pnpm add -g create-vite
```

**pnpm 最大的优点**：用内容寻址存储 + 硬链接，**同一台电脑上的所有项目共享同一份依赖文件**，磁盘占用相比 npm 经常能省下 50%–70%。

### 3.4 yarn 速查

```bash
# 经典 v1
npm install -g yarn
yarn install
yarn add vue
yarn add -D eslint
```

> 现在 yarn 已经演进到 Berry（v2+），引入了 PnP 模式，配置更灵活但学习曲线更陡。**新手阶段先掌握 v1 即可**。

### 3.5 pnpm 实战：装得少、装得快、装得稳

> 前面 3.3 只给了速查表；这一节展开 pnpm 的"为什么快""workspace monorepo"等进阶能力。

#### 3.5.1 为什么 pnpm 比 npm 快

| 维度 | npm | pnpm |
| --- | --- | --- |
| 装包速度 | 中 | **快 2-3 倍** |
| 磁盘占用 | 每个项目一份 `node_modules` | **全局内容寻址 + 硬链接**，只存一份 |
| 装包严格度 | 允许"幽灵依赖" | **默认严格隔离** |
| monorepo | 需第三方工具 | **内置 workspace** |
| Node 版本要求 | ≥ 10 | ≥ 16 |

#### 3.5.2 安装与日常命令

```bash
# 装 pnpm
npm install -g pnpm
# 或用独立安装脚本（macOS / Linux）
curl -fsSL https://get.pnpm.io/install.sh | sh -

# 验证
pnpm -v

# 替代 npm 命令
pnpm init              # 替代 npm init
pnpm install           # 替代 npm install
pnpm add vue           # 替代 npm install vue
pnpm add -D eslint     # 替代 npm install -D eslint
pnpm remove vue        # 替代 npm uninstall vue
pnpm update            # 升级依赖
pnpm run dev           # 替代 npm run dev
```

#### 3.5.3 严格依赖隔离（避免"幽灵依赖"）

`pnpm` 默认**只允许你 `import` 在 `package.json` 写明的依赖**。如果你在 `package.json` 没写 `vue`，但 `node_modules` 里"碰巧"有：

- **npm / yarn**：能跑（幽灵依赖，**移植到 CI 就挂**）
- **pnpm**：直接报错

> 这个"麻烦"恰恰是**保护你**——避免"在我电脑能跑、到 CI 挂了"的经典问题。

#### 3.5.4 Monorepo：pnpm workspace

公司多个前端项目共享工具库？用 pnpm workspace 一行配置搞定：

```
my-monorepo/
├── pnpm-workspace.yaml      ← 关键配置
├── package.json
├── apps/
│   ├── web/                 ← 项目 A
│   └── admin/               ← 项目 B
└── packages/
    └── shared/              ← 共享工具库
```

**`pnpm-workspace.yaml`**：

```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

```bash
# 在 monorepo 根目录一次性装所有
pnpm install

# 在 apps/web 里用 packages/shared
# apps/web/package.json
{
  "dependencies": {
    "shared": "workspace:*"   # 用 workspace 协议引用本地包
  }
}

# 在 apps/web 里跑
pnpm --filter web dev
pnpm --filter admin build
```

#### 3.5.5 何时不用 pnpm

- 公司老项目全用 npm + lock 文件锁死 → 没必要换；
- 某些冷门原生模块（如 sharp / node-canvas）需要编译，硬链接偶尔出问题 → 临时切回 npm 装；
- 你的工具链是 **Bun** 生态 → 直接用 bun。

#### 3.5.6 一句话

> **公司新项目首选 pnpm**——速度、磁盘、严格性、monorepo 四项全能；个人项目也强烈推荐。

### 3.6 bun：既是运行时又是包管理器

> **Bun** 是 2022 年冒出的"全能选手"——既**替代 Node.js 跑 JS/TS**，也**替代 npm/pnpm 装依赖**，还内置了 **test runner / bundler / 转码器**。初学阶段了解即可，**大量生产项目还没切**，但增长极快。**注意：Bun 早期用 Zig 写，1.2+ 已用 Rust 全面重写**（启动 < 50ms、冷启动比 Node 快 4 倍）。

#### 3.6.1 一张速览：它能做什么

| 能力 | 对标 | 备注 |
| --- | --- | --- |
| **运行时** | Node.js | **更快**（1.2+ 用 **Rust** 重写），原生支持 TS / JSX |
| **包管理器** | npm / pnpm | 装包比 npm 快 20+ 倍 |
| **打包器** | Webpack / Vite | `bun build` 极快 |
| **测试器** | Jest / Vitest | `bun test` 内置 |
| **转码器** | Babel / SWC | 内置 |

#### 3.6.2 安装

```bash
# macOS / Linux
curl -fsSL https://bun.sh/install | bash

# Windows（PowerShell）
irm bun.sh/install.ps1 | iex

# 升级
bun upgrade

# 验证
bun --version
```

#### 3.6.3 日常命令速查

```bash
# 包管理（替代 npm / pnpm）
bun init                    # 替代 npm init
bun install                 # 替代 npm install
bun add vue                 # 替代 npm install vue
bun add -d eslint           # 替代 npm install -D eslint
bun remove vue
bun update
bun run dev                 # 替代 npm run dev

# 跑 .ts / .tsx 文件（不用先编译！）
bun run ./script.ts
bun ./script.ts

# 跑测试
bun test

# 打包
bun build ./src/index.ts --outdir ./dist
```

#### 3.6.4 用 bun 跑一个 Vue 3 项目

```bash
# Vite 官方支持 bun 作为包管理器
bun create vite my-app -- --template vue
cd my-app
bun install
bun run dev
```

> 注意：Vite 的 `dev server` 仍然跑在 Node 上（Vite 自身是 Node 写的）；`bun` 在这里只是**装包 + 跑脚本**。要让 Vite 跑在 Bun runtime 上，需要等官方适配（暂未稳定）。**1.2+ 的 Bun runtime 已用 Rust 重写**——冷启动比 Node 快约 4 倍，但 Vite / ESLint 等工具链仍以 Node 为准。

#### 3.6.5 bun 的"坑"

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| 某些 npm 包不工作 | 包用了 Node 独有的 API，Bun 1.2+ 兼容性大幅改善但仍有死角 | 临时切回 Node 跑 |
| `bun build` 与 Vite 输出不完全一致 | bundler 实现细节不同 | 生产环境仍用 Vite |
| 团队成员不熟悉 | 学习成本 | 在 README 写清"本项目用 bun 装包" |

#### 3.6.6 选型建议

| 场景 | 选 bun 吗？ |
| --- | --- |
| 个人新项目 / Demo | ✅ 强烈推荐（装包快到飞起） |
| 公司新项目 | ⚠️ 先小范围试点，确认所有依赖能跑 |
| 已有 npm/pnpm 大项目 | ❌ 别折腾 |
| Node 服务端项目 | ⚠️ 试过再切，Bun 在 SSR 场景兼容性还有小坑 |

#### 3.6.7 一句话

> **bun 是个"未来可能替代 Node + npm"的超级新星**——现在用它装包、跑脚本、跑测试**很爽**；**1.2+ 已用 Rust 重写 runtime，冷启动比 Node 快约 4 倍**；但**生产环境用 Bun runtime 替代 Node 仍需谨慎**。

### 3.7 一张图理解 `package.json`

```jsonc
{
  "name": "my-project",          // 项目名
  "version": "0.1.0",            // 版本号
  "private": true,               // 不发布到 npm 仓库
  "scripts": {                   // 自定义命令
    "dev": "vite",               // npm run dev
    "build": "vite build",       // npm run build
    "preview": "vite preview"
  },
  "dependencies": {              // 运行依赖：项目跑起来要用
    "vue": "^3.4.0"
  },
  "devDependencies": {           // 开发依赖：只在开发时用
    "vite": "^5.0.0",
    "eslint": "^9.0.0"
  }
}
```

`node_modules/` 是真正装着所有依赖代码的目录；`package-lock.json`（npm）/ `pnpm-lock.yaml` / `yarn.lock` 是**锁文件**，记录了"每个依赖到底装的哪个小版本"，必须提交到 Git，**不要手动改**。

---

## 4. 开发工具：VS Code

写代码的编辑器没有"唯一正确答案"，但**对初学者而言 Visual Studio Code（VS Code） 是最稳妥的选择**：免费、跨平台、插件生态极丰富、内置终端、几乎所有前端框架官方都默认推荐它。

### 4.1 安装 VS Code

- 官网：<https://code.visualstudio.com/>
- 下载后按提示安装即可，建议勾选"**添加到 PATH**"和"**将"通过 Code 打开"操作添加到资源管理器目录上下文菜单**"。

### 4.2 推荐的必备插件

| 插件 | 作用 |
| --- | --- |
| **Chinese (Simplified) Language Pack** | 把 VS Code 界面翻译成中文（可选，英语熟练可跳过） |
| **Vue - Official**（原 `Volar`） | Vue 3 单文件组件语法高亮、智能提示、类型检查 |
| **ESLint** | 实时检查代码风格和潜在错误，配合项目里的 ESLint 配置使用 |
| **Prettier - Code formatter** | 保存时自动格式化代码，统一团队风格 |
| **Stylelint** | CSS / SCSS / Less 风格检查 |
| **Path Intellisense** | 文件路径自动补全 |
| **Auto Rename Tag** | 改一个 HTML 标签时自动同步闭合标签 |
| **Live Server** | 写纯 HTML/CSS/JS 时一键起本地服务器、自动刷新 |
| **GitLens** | 在编辑器内直接看每一行代码是谁、什么时候改的 |

> 装插件不要贪多。**Vue - Official + ESLint + Prettier 三件套**就够用好几个月了。

### 4.3 让终端更顺手（可选）

- **macOS / Linux**：系统自带终端已经够用，想更顺手可以装 [iTerm2](https://iterm2.com/) + [Oh My Zsh](https://ohmyz.sh/)。
- **Windows**：建议把默认的 `cmd` 换成 **Windows Terminal**（微软商店搜索安装），它支持多标签页、字体渲染漂亮，并能同时跑 PowerShell / cmd / WSL。

### 4.4 编辑器调试技巧

VS Code 自带强大的调试器。新建 `.vscode/launch.json` 后，可以直接在编辑器里打断点调试前端项目：

```jsonc
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "在 Chrome 中调试",
      "url": "http://localhost:5173",
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

按 `F5` 启动后，VS Code 会以调试模式拉起 Chrome，断点、调用栈、变量查看与 Chrome DevTools 一致，但能直接看到工程内的源码。

### 4.5 推荐的项目结构

当你用 `npm create vue@latest` 创建一个 Vue 3 项目时，会得到这样的目录：

```
my-project/
├─ public/                 # 静态资源（不会被打包处理）
│  └─ favicon.ico
├─ src/                    # 源代码
│  ├─ assets/              # 图片、字体等需要被打包的资源
│  ├─ components/          # 公共组件
│  ├─ views/ 或 pages/     # 页面级组件
│  ├─ router/              # 路由配置
│  ├─ stores/              # Pinia 状态
│  ├─ App.vue              # 根组件
│  └─ main.js              # 入口文件
├─ index.html              # HTML 入口
├─ package.json
├─ vite.config.js          # Vite 配置
└─ .gitignore
```

`src/` 是你 90% 时间会待的地方。

---

## 5. 终端与 Shell 速览

终端是前端的"工作台"，下面 10 条命令就够你用 80% 的时间。

| 命令 | 作用 | 例子 |
| --- | --- | --- |
| `pwd` | 看当前所在目录 |  |
| `ls` / `dir` | 列出当前目录内容 | `ls -la` 看详情 |
| `cd <路径>` | 切换目录 | `cd src/components` |
| `mkdir <名>` | 新建目录 | `mkdir utils` |
| `touch <文件>` / `ni` | 新建空文件 | `touch index.js` |
| `rm <文件>` | 删除文件 | `rm temp.log` |
| `cp <src> <dst>` | 复制 | `cp .env.example .env` |
| `mv <src> <dst>` | 移动 / 重命名 | `mv old.js new.js` |
| `cat <文件>` | 看小文件内容 | `cat package.json` |
| `clear` / `cls` | 清屏 |  |

> Windows PowerShell 中 `ls`、`cat` 已经做了兼容，可以直接用；实在不习惯再装 [Git for Windows](https://git-scm.com/) 获得 `bash` 环境。

### 5.1 路径中的两个特殊符号

- `.` 表示"当前目录"，例如 `./src/main.js`；
- `..` 表示"上一级目录"，例如 `../components/Hello.vue`；
- `~` 表示"用户主目录"：macOS/Linux 是 `/Users/your-name`，Windows 是 `C:\Users\your-name`。

---

## 6. 小结与自检

装好上面这些东西后，请你依次验证以下 6 件事，每件都成功才算"开发环境就绪"：

| # | 自检项 | 验证命令 / 步骤 |
| --- | --- | --- |
| 1 | 浏览器能打开 DevTools | F12 出现 Elements 面板 |
| 2 | Node 已安装 | `node -v` 输出 `v20.x` 或更高 |
| 3 | npm 已可用 | `npm -v` 输出 10.x 以上 |
| 4 | VS Code 已安装 | 终端输入 `code --version` |
| 5 | 能创建并启动一个 Vite 项目 | `npm create vite@latest my-app` → `cd my-app` → `npm i` → `npm run dev`，浏览器出现 Vite 欢迎页 |
| 6 | 能用 `Ctrl+C` 停掉 dev server | 在终端按 `Ctrl + C` |

全部通过后，进入 [04 Web 概念](./04-web-concepts.md)。**想把代码推到线上让全世界访问**，继续往下读第 7 章「部署环境与容器化」。

---

## 6.5 AI 辅助编程：Copilot / Cursor / Claude Code

> 2026 年**所有前端开发者标配**——AI 不是"取代你"，是"放大你"。本节教你**怎么把 AI 用成最强工具**而不是"被它坑"。

### 6.5.1 五大主流工具对比

| 工具 | 类型 | 强项 | 适合谁 |
| --- | --- | --- | --- |
| **GitHub Copilot** | IDE 插件（VS Code / JetBrains） | 实时补全 / Chat / PR 摘要 | 想要"IDE 里无缝集成" |
| **Cursor** | **AI 优先**编辑器（VS Code fork） | 整文件编辑 / 多文件重构 / Agent 模式 | 想要"AI 深度整合" |
| **Claude Code** | CLI 工具 | 终端 agent / 长任务自动化 | 想要"命令行里跑 AI" |
| **opencode** | **开源** Terminal agent（兼容 Claude / OpenAI / 本地模型） | 多模型切换 / 配置全在 `~/.opencode/config.json` / **MIT 开源** | 想要"自己掌控 AI 工具链、不被厂商绑定" |
| **atomcode** | **AtomGit 生态** VS Code / JetBrains 插件（"AtomCode" by AtomGit） | 国产中文界面 / 内置国产大模型 / 与 **AtomGit PR / Issue** 深度打通 | 国内团队 + 已经在用 AtomGit 当代码托管 |

> **国内替代**：通义灵码（阿里）、CodeGeeX（智谱）、文心快码（百度）——不输 OpenAI。
>
> **2026 年新势力**：
> - **opencode**（<https://opencode.ai>）——开源、可换模型、不锁定厂商，类似"AI 工具的 Vim/Emacs 之战"。
> - **atomcode**（AtomGit 推出，VS Code 插件市场搜 "AtomCode"）——专为国内团队优化，与 AtomGit 仓库 PR / Issue 联动。

### 6.5.2 Copilot 最快上手

```bash
# VS Code 装扩展
code --install-extension GitHub.copilot
# 登录 GitHub → 自动启用
```

| 快捷键 | 作用 |
| --- | --- |
| `Tab` | 接受补全 |
| `Ctrl+Enter` | 弹出 10 个候选 |
| `Ctrl+I` | 打开 Copilot Chat |
| `Ctrl+L` | 解释选中代码 |
| `Ctrl+K` | 生成新代码 / 重构 |

### 6.5.3 5 个让 AI 写出"能跑代码"的关键

> 99% 的"AI 写错代码"不是 AI 蠢——是**你的提示不到位**。

| 关键 | 烂提示 | 好提示 |
| --- | --- | --- |
| **给上下文** | "写个登录" | "用 Vue 3 + Pinia 写一个登录页，POST /api/login，错误时显示在 v-form 的 error slot，不要用第三方 UI 库" |
| **给约束** | "写个工具函数" | "写一个 isValidEmail 函数，匹配 RFC 5322，TS 写，要 export type" |
| **给反例** | "加个校验" | "加个校验，**不要**用正则，**不要**依赖任何 npm 包" |
| **分步** | "重构这个文件" | "第 1 步：把 user.name 改成 username；第 2 步：把组件拆成 3 个文件" |
| **贴报错** | "代码报错" | 贴完整 stack trace + 相关代码 10 行 |

### 6.5.4 Claude Code：终端里的"实习生"

> 把 AI 装进终端，让它直接读写文件、跑命令、提交 PR——**最强 agent 工作流**。

```bash
# 安装（macOS / Linux）
curl -fsSL https://claude.com/install.sh | sh

# 进入项目
cd my-app

# 交互式
claude
> 帮我把 src/components/Header.vue 拆成两个组件，一个放 logo，一个放导航
> 跑一下 npm run build 看看有没有报错
> 帮我写 commit message 然后 commit
```

> **真实场景**：让 Claude Code 自己读 README + 自己写测试 + 自己跑 + 自己 commit + 自己开 PR——**"AI 实习生"**。

### 6.5.4.1 opencode：开源 / 可换模型 / 终端 agent

> **opencode** = "AI 工具的 Vim/Emacs"——**MIT 开源**、**支持多模型后端**、**不锁定任何厂商**。想"自己掌控 AI 工具链"的人首选。

```bash
# macOS / Linux 一行装
curl -fsSL https://opencode.ai/install.sh | sh

# 或用 npm / pnpm
pnpm add -g opencode-ai

# 初始化
opencode init                        # 生成 ~/.opencode/config.json

# 进项目
cd my-app
opencode                             # 进入交互式 REPL
> 把 src/api 下的请求都改成用 fetch 而不是 axios
> 跑 pnpm test 看有没有失败
> 写 commit 并 push
```

**`~/.opencode/config.json` 关键字段**：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "anthropic":  { "apiKey": "sk-ant-..." },
    "openai":     { "apiKey": "sk-..." },
    "ollama":     { "baseUrl": "http://localhost:11434/v1" }
  },
  "model": "anthropic/claude-sonnet-4.5",   // 切模型只改一行
  "theme": "system"
}
```

| 维度 | opencode | Claude Code |
| --- | --- | --- |
| **开源** | ✅ MIT | ❌ 闭源 |
| **模型可换** | ✅ Claude / GPT / Gemini / Ollama / 国产 | ❌ 仅 Claude |
| **自部署** | ✅ 跑本地 Ollama 即可离线 | ❌ 必须连 Anthropic |
| **生态成熟度** | 中（2025 崛起） | 高（生态 + 文档） |
| **适合** | 想要"我的 AI 我做主" | 想要"开箱即用、企业级" |

> **何时选 opencode**：**想用本地 Ollama / 想换多家模型 / 公司不让用 Anthropic / 想让 AI 走内网**——**opencode 是唯一解**。
> **何时选 Claude Code**：想要"最聪明的模型 + 最稳的 agent"——**opencode 也支持 Claude，但 Claude Code 跑得最熟**。

### 6.5.4.2 atomcode 快速开始：安装、首次运行与第一个任务

> **atomcode** 是 **AtomGit（码云的开源 Git 平台）** 官方推出的终端 AI 编程代理——5.x 版本是 **Rust 写的 TUI 工具**，不是 IDE 插件。**和 AtomGit 仓库、AtomGit OAuth、CodingPlan 免费额度** 深度打通。**几分钟内** 装好、跑起来、做第一个任务。

#### 6.5.4.2.1 系统要求

| 维度 | 要求 |
| --- | --- |
| **操作系统** | macOS（Apple Silicon / Intel）、Linux、Windows、HarmonyOS PC |
| **Rust 工具链** | 1.75+（**仅在从源码构建时需要**） |
| **LLM 模型** | 任一支持 OpenAI 风格 API 的 provider 的 **API Key**，**或一个 AtomGit 账号**（用于 OAuth 登录 + CodingPlan 免费额度） |

#### 6.5.4.2.2 安装（4 种方式任选）

##### 方式 1：一键安装（推荐）

**macOS / Linux / HarmonyOS PC 终端**：

```bash
curl -fsSL https://raw.atomgit.com/atomgit_atomcode/atomcode/raw/main/scripts/install.sh | sh
```

脚本会自动下载对应平台的预编译二进制，并放置到 `~/.local/bin/atomcode`。**请确保该目录在 `PATH` 中**。

**Windows PowerShell**：

```powershell
irm https://raw.atomgit.com/atomgit_atomcode/atomcode/raw/main/scripts/install.ps1 | iex
```

脚本会自动下载 Windows x64 预编译二进制并完成 PATH 配置，**新开终端**即可使用 `atomcode` 命令。

##### 方式 2：通过 npm

已安装 Node.js 18+ 时，可直接走 npm 全局安装：

```bash
npm install -g @atomgit.com/atomcode
```

包会按当前平台从 `optionalDependencies` 自动解析对应预编译二进制（macOS x64 / arm64、Linux x64 / arm64、Windows x64、HarmonyOS arm64），**一行命令搞定跨平台安装**。

##### 方式 3：通过 Homebrew

已安装 Homebrew 时，可通过 cask 安装（支持 macOS x64 / arm64、Linux x64 / arm64）：

```bash
brew install --cask atomcode
```

Homebrew 会自动下载对应平台的预编译二进制并配置 PATH。

##### 方式 4：从源码构建

```bash
git clone https://atomgit.com/atomgit_atomcode/atomcode.git
cd atomcode
cargo build --release
cp target/release/atomcode ~/.local/bin/
```

Release 构建编译时间较长，但生成的二进制体积小、运行速度快。**开发调试**时用 `cargo build` 即可。

#### 6.5.4.2.3 验证安装

```bash
atomcode --version
# 应该看到类似 atomcode 5.0.x (build-id) 的输出
```

#### 6.5.4.2.4 Shell 补全（可选）

AtomCode 可以为常用 Shell 生成命令与参数补全脚本：

```bash
atomcode completion zsh
atomcode completion bash
atomcode completion fish
atomcode completion powershell
```

请按所用 Shell 的方式加载生成结果。**这只影响终端命令补全**；TUI 中 `Tab` 用于补全，`Shift+Tab` 用于切换执行模式。

#### 6.5.4.2.5 首次运行：3 步首次启动向导

直接在任意目录运行：

```bash
atomcode
```

第一次启动会自动弹出 **3 步首次启动向导**（也可以随时用 `/welcome` 重新打开）：

##### 第 1/3 步 · 欢迎

```
             ███  █████  ███  █     █  ████  ███  ████  █████
            █   █   █   █   █ ██   ██ █     █   █ █   █ █
            █████   █   █   █ █ █ █ █ █     █   █ █   █ ████
            █   █   █   █   █ █  █  █ █     █   █ █   █ █
            █   █   █    ███  █     █  ████  ███  ████  █████

  AtomCode
  版本 5.2.3  ·  在终端里运行的 AI 编程代理

  • 多步骤 agent loop · 内置代码图工具
  • 兼容所有 OpenAI 风格 API
  • 通过 CodingPlan 获取免费额度

  按 Enter 继续。   Ctrl+C 可随时退出。
```

##### 第 2/3 步 · 语言 (Choose your language / 选择语言)

```
  1. 自动检测 (LC_ALL / LANG)
  2. English
  3. 简体中文 (Simplified Chinese)

  1-3 选择 · Enter 确认 · ← 返回 · Esc 跳过
```

选完会立即应用，并写回 `~/.atomcode/config.toml` 的 `locale` 字段。之后任意时刻可以用 `/language` 再改。

##### 第 3/3 步 · 配置（想怎么开始？）

```
  1. 用 AtomGit CodingPlan 一键接入  (推荐 · 免费额度 + 自动配 provider)
  2. 手动配置 provider              (已有 API Key)
  3. 跳过,先进 TUI 探索             (之后再 /login 或 /provider)

  1-3 选择 · Enter 确认 · ← 返回 · Esc 跳过
```

- **CodingPlan —— 推荐路径**，等价于在 TUI 内执行 `/login`：浏览器唤起 AtomGit OAuth，登录后自动申领免费额度，并把额度模型列表写成 provider 配置。
- **手动配置** —— 等价于 `/provider`：先添加供应商账号，再为它添加模型。
- **跳过** —— 直接进 TUI 探索，之后再用 `/login` 或 `/provider` 完成配置。

更详细的选择参见"登录方式"。任何时候可以用 `/welcome` 重新打开这个向导。

#### 6.5.4.2.6 第一个任务

进入一个你熟悉的项目目录，启动 `atomcode`，然后**直接描述你想做的事**：

```bash
cd ~/projects/my-web-app
atomcode

> 简要介绍一下这个项目的目录结构和技术栈
```

AtomCode 会自主地：

- 运行 `list_directory`、`read_file` 等工具探索代码
- 识别关键描述文件（`package.json` / `Cargo.toml` 等）
- 给出结构化的总结

> **Tip**：不确定要问什么？试试：
> - "修复 README 里所有的拼写错误"
> - "把 `src/utils.ts` 里的函数加上 TypeScript 类型"
> - "跑一下测试并修复失败用例"

#### 6.5.4.2.7 卸载

不再需要 AtomCode 时，用内置的 `uninstall` 子命令一步清理。它会按组询问你是否删除以下三类内容：**二进制本身 + 安装脚本写入的 PATH 编辑**、**`~/.atomcode/auth.toml` 等凭据**、**`~/.atomcode/sessions/` 等运行状态**。

##### 交互式（推荐）

```bash
atomcode uninstall
```

每个组单独询问 `y/N`，**按当前默认决策**（二进制=是、凭据=否、状态=是）预填 —— 直接回车也是合理选择。

##### 非交互式

```bash
# 全自动：按默认决策走，不弹任何提示
atomcode uninstall --yes

# 一键全清（包括 ~/.atomcode/ 整个目录）
atomcode uninstall --purge

# 只删二进制和 PATH 编辑，~/.atomcode/ 完整保留
atomcode uninstall --keep-data

# 只打印执行计划，什么都不做
atomcode uninstall --dry-run
```

> **注意**：`--purge` 会连带删除
> - 所有会话历史、记忆、自定义 provider 配置、OAuth token
> - **不可逆**。
>
> 如果以后还可能用 AtomCode，优先用默认交互式或 `--keep-data`。

#### 6.5.4.2.8 下一步

- **配置文件** —— 学会管理多个 provider 和模型。
- **基本使用** —— 熟悉 CLI 参数和描述任务的最佳实践。
- **斜杠命令** —— 掌握 `/resume`、`/undo`、`/diff` 等高频命令。

### 6.5.4.3 五大工具选型速查

| 你的情况 | 推荐 |
| --- | --- |
| 个人 / 初学 | **GitHub Copilot**（最易上手） |
| 想要"最强 AI 编辑器" | **Cursor**（最智能的 IDE 体验） |
| 想要"终端里跑 agent" | **Claude Code**（生态最熟） |
| 想要"开源 / 可换模型 / 不被绑定" | **opencode**（MIT 开源） |
| 已经在用 AtomGit | **atomcode**（国产 + 深度打通） |
| 团队在国内、想用国产模型 | 通义灵码 / CodeGeeX / atomcode |
| 想"完全本地 / 离线" | **opencode + Ollama**（自部署 LLaMA / Qwen） |

---

### 6.5.5 避坑 4 条

1. **永远人工 review**：AI 写的代码**没有 100% 正确**——尤其边界条件、错误处理
2. **不要直接用 AI 的依赖列表**：它会装一堆你不需要的包
3. **不要把密钥 / 客户数据贴给 AI**：敏感信息会进模型训练数据
4. **保留 prompt 历史**：好提示可以反复用，建个 `prompts.md` 仓库

> **口诀**：**AI 让你从"打字"变"指挥"**——但**指挥也需要会这门语言**。基础不扎实，AI 写错你都看不出。

---

## 7. 部署环境与容器化

> 这一节是**开发环境 → 生产环境**的桥梁。学完之后，你应当能把自己的 Vue 3 项目用 Docker 打包、推送到服务器、让用户通过域名访问。

### 7.1 部署形态速览

| 形态 | 适合谁 | 优点 | 缺点 |
| --- | --- | --- | --- |
| **虚拟主机 / 共享空间** | 静态站、个人博客 | 便宜、上传即用 | 性能差、不能装 Node、几乎不能定制 |
| **云服务器 ECS / VM** | 中小项目、Node 全栈 | 灵活、什么都能装 | 要自己运维：Nginx、SSL、备份、监控…… |
| **PaaS（平台即服务）** | 不想管服务器的同学 | 推代码即部署、自动 HTTPS | 平台锁定、价格按用量涨 |
| **Serverless** | 流量小、波动大 | 几乎零运维、按请求计费 | 冷启动、本地调试体验一般 |
| **容器化（Docker）** | 中大型项目、团队协作 | 环境一致、易扩展、跨云迁移 | 学习曲线较陡 |

> 经验法则：**学习阶段用云服务器 ECS + Docker**；个人小项目想偷懒用 Vercel / Netlify / Cloudflare Pages；公司项目几乎一定会走容器化 + Kubernetes。

### 7.2 三种主流前端部署方式

#### A. 纯静态站：构建产物直接丢 CDN

最省钱的方案。Vite 默认的 `npm run build` 产物在 `dist/`，里面就是纯 HTML / CSS / JS + 资源文件，**没有 Node 运行时**。

```bash
npm run build           # 产物在 dist/
# 上传到 OSS / S3 / Cloudflare Pages / Vercel
```

- 优点：极快、几乎免费、天然 CDN
- 缺点：没有 SSR、没有服务端逻辑、不能配 `proxy` 转发

#### B. Node 全栈：自己用 Nginx + PM2

适合"前端 + Node API"的小项目。流程是：

```
用户 ──HTTPS──► Nginx（反向代理 + 静态文件）
                  │
                  ├─► /         → 前端 dist/ 静态资源
                  └─► /api/*    → 转发到 Node 服务（PM2 守护）
```

#### C. 容器化：把整个应用打包成 Docker 镜像

把"Nginx + Node + 你的代码 + 系统依赖"全部塞进一个**镜像**，在任何装了 Docker 的机器上**跑起来都一样**。

下面重点讲这条路。

### 7.3 容器化三件套：镜像 / 容器 / 仓库

| 术语 | 类比 | 作用 |
| --- | --- | --- |
| **镜像（Image）** | 安装包 / 类（class） | 只读模板，包含代码、运行时、依赖、系统工具 |
| **容器（Container）** | 正在运行的进程 / 实例（object） | 镜像跑起来后的实例，相互隔离 |
| **仓库（Registry）** | 应用商店 / npm | 存放镜像的地方（Docker Hub、阿里云 ACR、腾讯云 TCR、自建 Harbor） |

### 7.4 安装 Docker

#### macOS / Windows

直接装 **Docker Desktop**，自带 GUI：<https://www.docker.com/products/docker-desktop/>

#### Linux（以 Ubuntu 22.04 为例）

```bash
# 一键安装
curl -fsSL https://get.docker.com | bash

# 把当前用户加入 docker 组，避免每次 sudo
sudo usermod -aG docker $USER
newgrp docker

# 验证
docker --version
docker run hello-world
```

> 验证最后那条命令能输出"Hello from Docker!"就说明装好了。

### 7.5 Dockerfile：把前端项目装进镜像

#### Vue 3 SPA 的多阶段构建（推荐）

```dockerfile
# ── 阶段 1：构建 ─────────────────────────────
FROM node:20-alpine AS builder
WORKDIR /app

# 先复制依赖清单，最大化利用 Docker 缓存
COPY package.json pnpm-lock.yaml* package-lock.json* yarn.lock* ./
RUN \
  if [ -f pnpm-lock.yaml ]; then npm i -g pnpm && pnpm i --frozen-lockfile; \
  elif [ -f package-lock.json ]; then npm ci; \
  elif [ -f yarn.lock ]; then yarn install --frozen-lockfile; \
  else npm install; fi

# 复制源码并构建
COPY . .
RUN npm run build

# ── 阶段 2：运行（Nginx 托管静态产物） ─────
FROM nginx:1.27-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

> 多阶段构建的好处：**最终镜像里没有 node_modules、没有源码**，体积小（通常 < 50 MB）、攻击面小。

#### 配套的 `nginx.conf`

```nginx
server {
  listen 80;
  server_name _;
  root /usr/share/nginx/html;
  index index.html;

  # SPA 路由：所有未知路径都回到 index.html，让前端路由接管
  location / {
    try_files $uri $uri/ /index.html;
  }

  # 长缓存静态资源（带 hash 的文件可以永久缓存）
  location /assets/ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }

  # gzip
  gzip on;
  gzip_types text/plain text/css application/javascript application/json image/svg+xml;
}
```

#### Node 后端（Express / Koa / Nest）的 Dockerfile

```dockerfile
# ── 阶段 1：构建 ─────────────────────────────
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build          # 输出到 dist/ 或类似目录

# ── 阶段 2：运行 ─────────────────────────────
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production

# 只装运行时依赖
COPY package*.json ./
RUN npm ci --omit=dev && npm cache clean --force

# 复制构建产物
COPY --from=builder /app/dist ./dist

# 用非 root 用户运行（安全）
RUN addgroup -g 1001 nodejs && adduser -S -u 1001 nodejs
USER nodejs

EXPOSE 3000
CMD ["node", "dist/main.js"]
```

### 7.6 `.dockerignore`：别把垃圾塞进镜像

和 `.gitignore` 类似，告诉 Docker "这些文件不要复制进构建上下文"：

```gitignore
node_modules
dist
.git
.gitignore
.vscode
.idea
*.log
.env
.env.*
!.env.example
.DS_Store
Dockerfile
docker-compose*.yml
README.md
```

### 7.7 docker-compose：一条命令启动整套服务

> 真实项目通常有"前端 + 后端 API + 数据库 + 缓存"，一个个 `docker run` 太麻烦。`docker-compose` 用一个 YAML 描述并启动所有服务。

```yaml
# docker-compose.yml
version: "3.9"

services:
  web:
    build: .
    image: my-app-web:latest
    container_name: my-app-web
    ports:
      - "8080:80"
    depends_on:
      - api
    restart: unless-stopped

  api:
    build: ../my-api
    image: my-app-api:latest
    container_name: my-app-api
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_PORT=5432
      - JWT_SECRET=${JWT_SECRET}        # 从 .env 读取
    depends_on:
      - db
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    container_name: my-app-db
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=app
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=app
    ports:
      - "5432:5432"
    restart: unless-stopped

volumes:
  pgdata:
```

```bash
docker compose up -d            # 后台启动所有服务
docker compose ps               # 看运行状态
docker compose logs -f web      # 看 web 服务日志
docker compose down             # 停掉
```

### 7.8 把镜像推到镜像仓库

```bash
# 1. 登录（以阿里云容器镜像服务为例）
docker login registry.cn-hangzhou.aliyuncs.com -u your-username

# 2. 给镜像打 tag
docker tag my-app-web:latest registry.cn-hangzhou.aliyuncs.com/your-namespace/my-app-web:v1.0.0

# 3. 推送
docker push registry.cn-hangzhou.aliyuncs.com/your-namespace/my-app-web:v1.0.0

# 4. 在服务器上拉取
docker pull registry.cn-hangzhou.aliyuncs.com/your-namespace/my-app-web:v1.0.0
```

> 主流仓库对比：
>
> | 服务 | 适合 |
> | --- | --- |
> | **Docker Hub** | 海外项目、开源镜像 |
> | **阿里云 ACR** | 国内项目，访问快、和阿里云 ECS 打通 |
> | **腾讯云 TCR** | 腾讯云用户 |
> | **Harbor（自建）** | 企业内网、严格权限管控 |

### 7.9 Web 服务器选型：Nginx / Caddy / Apache / OpenResty / Tengine

> 如果你不想（或暂时不能）上 Docker，部署就是"一个 Node 进程 + 一个 Web 服务器"的组合——其中 **Web 服务器的选择** 直接决定 HTTPS 怎么配、反代怎么写、性能调优怎么做。这一节把五大常见选择摆在一起对比。

#### 7.9.1 五方速览

| 服务器 | 出身 | 内存 | 配置风格 | HTTPS / 反代 | 国内生态 | 适合谁 |
| --- | --- | --- | --- | --- | --- | --- |
| **Nginx** | 俄罗斯 Igor Sysoev，2004 | 中（~3 MB/worker） | 命令式 DSL | 强 + 需 Certbot | 高 | **绝大多数生产项目**默认选 |
| **Caddy** | Go 写，2015 | 中（单文件） | 声明式 JSON / Caddyfile | **全自动**（Let's Encrypt 内置） | 中 | 想要"配置完就跑 HTTPS" |
| **Apache** | 老牌，1995 | 较高 | `.htaccess` + 虚拟主机 | 强 + 需 Certbot | 高 | 传统 PHP / 旧项目维护 |
| **OpenResty** | 章亦春，国内，2008 | 中-高 | Nginx + LuaJIT | 同 Nginx | **很高** | 需要**高并发网关**、自研 WAF、限流灰度 |
| **Tengine** | 淘宝，2011（基于 Nginx） | 中 | Nginx 兼容 + 自有指令 | 同 Nginx | **很高** | 阿里云 / 淘宝 / 1688 生态、巨型流量 |

> 简单记：
> - **个人 / 中小项目** → Nginx 或 Caddy
> - **想要 HTTPS 零配置** → Caddy
> - **维护老 PHP** → Apache
> - **国内中大型业务、高并发网关** → OpenResty 或 Tengine
> - **99% 的学习场景** → Nginx 完全够用

#### 7.9.2 Nginx：事实标准

**优点**：
- 社区最广，文档最全，几乎所有问题都能搜到
- 反代 / 负载均衡 / 静态文件 / gzip / 限流全部开箱即用
- 国内云厂商（阿里云、腾讯云）默认自带

**反代 Node 最小配置** `/etc/nginx/conf.d/my-app.conf`：

```nginx
server {
  listen 80;
  server_name api.example.com;

  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```

**HTTPS（结合 Certbot）**：

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d api.example.com
```

证书自动 90 天续期。

#### 7.9.3 Caddy：HTTPS 零配置的现代派

**最大卖点**：写完 `Caddyfile` **直接就有 HTTPS**——证书自动申请、自动续期，**完全不用 Certbot**。

**Caddyfile**（通常放在 `/etc/caddy/Caddyfile`）：

```caddyfile
# 全站反代 + 自动 HTTPS
api.example.com {
  reverse_proxy 127.0.0.1:3000
}

# 静态站（自动 HTTPS + gzip）
web.example.com {
  root * /var/www/web
  file_server
  encode gzip zstd
}
```

**优点**：
- 配置文件极简，比 nginx.conf 少 50% 行数
- HTTPS / HTTP/3 / gzip / zstd / 访问日志全默认开
- 用 Go 写的，单文件部署，无外部依赖

**缺点**：
- 国内云厂商默认不预装，要自己装
- 极端高并发下不如 Nginx 调优后的版本
- 出了问题中文资料相对少

> **学习 / Demo / 个人项目** 强烈建议试一次 Caddy，配置体验爽到飞起。

#### 7.9.4 Apache：老当益壮

`httpd` + `.htaccess` 是早期 LAMP 时代的标志。如今主要场景：

- 维护 PHP 老项目（很多 CMS 如 WordPress、Discuz 默认 `.htaccess`）
- 需要"目录级配置"（每个用户自己写 `.htaccess` 而不动主配置）
- 公司有历史包袱、不能换技术栈

**最小虚拟主机**：

```apache
<VirtualHost *:80>
  ServerName example.com
  DocumentRoot /var/www/example
  <Directory /var/www/example>
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>
```

> **新项目不要选 Apache**——它在前端静态站 + Node 反代场景下比 Nginx 重 30–50% 内存。

#### 7.9.5 OpenResty：Nginx + LuaJIT 的高性能网关

OpenResty = Nginx + LuaJIT + 一组三方模块。**核心能力**：在 Nginx 配置里**直接写 Lua 脚本**，做 WAF、灰度发布、限流、A/B 测试、动态路由等"网关层逻辑"，**性能远高于"Node 网关"**。

**典型场景**：
- 大型 API 网关（单实例 QPS 上万）
- 自研限流 / 鉴权 / 灰度
- 边缘计算（Kong / APISIX 都基于 OpenResty）

**最小示例**：

```nginx
# 装好 OpenResty 后，配置几乎和 Nginx 一模一样
server {
  listen 80;
  server_name api.example.com;

  location / {
    content_by_lua_block {
      ngx.say("Hello from OpenResty!")
    }
  }
}
```

> **学习路径**：先熟练 Nginx → 再学 Lua 基础 → 再看 OpenResty 官方 cookbook。

#### 7.9.6 Tengine：阿里 / 淘宝系深度定制

Tengine 是**基于 Nginx 1.x 分支**二次开发的产物，由淘宝团队维护，**和 Nginx 几乎完全兼容**（配置文件 99% 通用），但加了大量针对"国内巨型流量场景"的优化：

- **动态模块加载**：不用重新编译就能加模块
- **主动健康检查**：自动剔除"假死"的后端
- **更细粒度的限流**：按 cookie / header / 参数限流
- **请求级连接复用**：高并发下更省内存
- **更好的中文文档与社区响应**（毕竟国人在维护）

**使用方式**：和 Nginx 完全一样，只是二进制换成 `tengine`。

```bash
# Ubuntu 装 Tengine
sudo apt-get install -y software-properties-common
sudo add-apt-repository -y ppa:tengine-ng/tengine
sudo apt-get update
sudo apt-get install -y tengine
```

> **选 Nginx 还是 Tengine**？
> - 一般业务 → Nginx；
> - 阿里云 / 淘宝 / 1688 / 优酷 系生态、或者团队里有人熟 Tengine 特有指令 → Tengine；
> - 想要"国内厂商原生支持" → Tengine（阿里云官方文档里大量例子）。

#### 7.9.7 选型决策表

| 你的场景 | 推荐 |
| --- | --- |
| 个人博客 / Demo / 想要"配完即 HTTPS" | **Caddy** |
| 中小项目 + Node 反代 | **Nginx** |
| 维护老 PHP / WordPress | **Apache** |
| 高并发 API 网关 / WAF / 限流 | **OpenResty** |
| 阿里云 / 淘宝系生态 / 大流量 | **Tengine** |
| 完全没头绪 | **Nginx**（最不缺文档） |

#### 7.9.8 Node 进程守护：PM2（无论用上面哪个 Web 服务器都要配）

```bash
npm install -g pm2

pm2 start dist/main.js --name my-api

pm2 list                    # 列出所有进程
pm2 logs my-api             # 看日志
pm2 restart my-api          # 重启
pm2 stop my-api             # 停掉
pm2 save                    # 保存当前进程列表
pm2 startup                 # 开机自启（按提示执行）
```

> 这一步是"让 Node 进程挂了自动重启"——和具体选哪个 Web 服务器**正交**。Web 服务器负责"把请求转给 Node"，PM2 负责"Node 挂了再拉起"。

#### 7.9.9 实战：一台机器上同时跑 3 个站点

需求：
- `blog.example.com` → 静态博客（VitePress）
- `app.example.com` → Node API（PM2 守护 3000 端口）
- `admin.example.com` → 同一个 Node API，但带 `/admin` 路径

**Nginx 配置文件** `/etc/nginx/conf.d/all.conf`：

```nginx
# 1. 静态博客
server {
  listen 80;
  server_name blog.example.com;
  root /var/www/blog;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;   # SPA fallback
  }
}

# 2. Node API
server {
  listen 80;
  server_name app.example.com;

  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}

# 3. 同一个 Node API，走 /admin 路径
server {
  listen 80;
  server_name admin.example.com;

  location / {
    proxy_pass http://127.0.0.1:3000;   # 后端根据 Host 头判断走管理端 UI
    proxy_set_header Host $host;
  }
}
```

**Caddy 等价配置** `/etc/caddy/Caddyfile`：

```caddyfile
blog.example.com {
  root * /var/www/blog
  file_server
  try_files {path} {path}/ /index.html
  encode gzip zstd
}

app.example.com, admin.example.com {
  reverse_proxy 127.0.0.1:3000
}
```

> 看得出：**Caddy 在多站点配置上明显更紧凑**。

### 7.10 在云上跑起来

#### 阿里云 ECS（最常见的国内选择）

1. 控制台 → 云服务器 ECS → 创建实例 → 选 2 核 4G、Ubuntu 22.04、带宽 5 Mbps；
2. 安全组里**放通 22（SSH）、80、443**；
3. SSH 登录：`ssh root@your-server-ip`；
4. 装 Docker（见 7.4）；
5. `docker compose up -d`，搞定。

#### 腾讯云轻量 / Cloudflare Pages / Vercel

- **Vercel / Netlify**：连 GitHub，自动构建、自动 HTTPS，**个人静态站零成本**；
- **Cloudflare Pages**：同上 + 全球 CDN 加速，国内访问比 Vercel 稍好；
- **腾讯云轻量应用服务器**：比 ECS 便宜，但功能受限。

### 7.11 部署的"最后一公里"：CI/CD

手把手 `docker build && docker push && ssh 上去 docker pull && docker compose up` 很快会让人抓狂，**自动化**才是答案。

#### GitHub Actions 最小示例：推送 tag 自动部署

```yaml
# .github/workflows/deploy.yml
name: Build & Deploy

on:
  push:
    tags: ['v*']           # 推送 v1.0.0 这种 tag 时触发

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: 登录阿里云 ACR
        uses: docker/login-action@v3
        with:
          registry: registry.cn-hangzhou.aliyuncs.com
          username: ${{ secrets.ACR_USER }}
          password: ${{ secrets.ACR_PASS }}

      - name: 构建并推送镜像
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            registry.cn-hangzhou.aliyuncs.com/your-namespace/my-app-web:latest
            registry.cn-hangzhou.aliyuncs.com/your-namespace/my-app-web:${{ github.ref_name }}

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: 远程拉取并重启
        uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            cd /srv/my-app
            docker compose pull
            docker compose up -d
            docker image prune -f
```

#### 简化版流程图

```
git push tag v1.0.0
        │
        ▼
GitHub Actions 触发
        │
        ├─► docker build & push 到阿里云 ACR
        │
        └─► SSH 到服务器
              │
              └─► docker compose pull
                  docker compose up -d
                        │
                        ▼
                   新版本上线
```

> 主流 CI/CD 对比：
>
> | 平台 | 适合 |
> | --- | --- |
> | **GitHub Actions** | 开源 / 私有项目、与 GitHub 集成最深 |
> | **GitLab CI** | 公司用 GitLab |
> | **Jenkins** | 自建、对复杂流水线友好 |
> | **Drone** | 轻量、容器原生 |
> | **云厂商流水线** | 阿里云/腾讯云用户，能直接用云资源 |

### 7.12 容器化自检清单

| # | 自检项 | 通过标准 |
| --- | --- | --- |
| 1 | 知道三种部署形态的取舍 | 能说出 SPA / Node 全栈 / SSR 的推荐部署方式 |
| 2 | 能读懂 Dockerfile 多阶段构建 | 解释每个 `FROM` / `COPY` / `RUN` 的作用 |
| 3 | 能写一份 `docker-compose.yml` | 至少启动 web + api 两个服务 |
| 4 | 能用 `docker build / push / pull` | 镜像从本地走到仓库再走到服务器 |
| 5 | 知道 PM2 + Nginx 是传统方案的兜底 | 即使不上 Docker 也能让 Node 服务跑起来 |
| 6 | 能在 GitHub Actions 里跑通最小流水线 | tag 推送后服务器自动更新 |

### 7.13 进阶主题预告

| 主题 | 何时学 |
| --- | --- |
| **Kubernetes（K8s）** | 多服务、多环境、需要自动扩缩容 |
| **Helm Chart** | 用模板管理 K8s 资源 |
| **服务网格（Istio）** | 微服务之间的流量治理 |
| **Serverless（Cloudflare Workers / 阿里云函数计算）** | 流量小、想极致省钱 |
| **可观测性（Prometheus + Grafana + Loki）** | 想知道服务跑得怎么样 |

> 入门阶段只需掌握 **Dockerfile + docker compose + 阿里云 ECS** 这一套，就足以应对 90% 的中小项目。

---

📚 继续阅读：[README.md](./README.md) 总目录 · [04 Web 概念](./04-web-concepts.md) · [05 JavaScript 基础](./05-javascript-basics.md)
