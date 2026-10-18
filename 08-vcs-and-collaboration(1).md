# 08 · Git 与团队协作 

- 本节⽬标：从"会⽤ Git 提交"到"能在团队⾥优雅协作"——分⽀策略、PR 流程、 Code Review、CI ⾃动化、Issue 管理⼀站搞定。 

学完本节，你应当能独⽴⽤ Git 完成⽇常开发、发起 Pull Request、⽤ GitHub Actions 跑⾃动化流⽔线，并在团队仓库⾥⾼效沟通。 

- 📚 系列⽬录⻅ README.md 

- ⚠ Git 是 ⼏乎所有现代软件⼯程 的地基——不管你学 Vue / React / 后端 / 嵌⼊ 式，它都跑不掉。把这⼀节读透，能省下后⾯⽆数次"找代码丢了怎么办"的痛苦。 

# ⽬录 

1. Git 是什么？为什么必须学？ 

2. 安装与⾸次配置 

3. 核⼼概念：三个区、四种状态 

4. 基础流程：本地 + 远程 

5. 分⽀策略：Trunk-Based / Git Flow / GitHub Flow 

6. 团队协作：Pull Request 与 Code Review 

7. 冲突处理：什么时候 rebase，什么时候 merge 

8. 提交规范：Conventional Commits 

9. ⾼频救命命令：找回误删 / 回退版本 / 改 commit 历史 

10. `.gitignore` 模板速查 

11. GitHub Issue / Project / Wiki 协作 

12. GitHub Actions ⾃动化⼊⻔ 

14. 其他 VCS 平台对⽐：GitHub / GitLab / Gitea / GitCode / Gitee / Bitbucket / SVN / Mercurial / Perforce 

15. monorepo 是什么：单仓多包 / 多仓多包 / ⼯具选型 

13. ⼩结与⾃检 

# 1. Git 是什么？为什么必须学？ 

Git 是事实上的"代码时间机器"——每⼀次提交都是⼀个可回滚的版本快照；配合 GitHub / GitLab / Gitee 还能多⼈协作。 

|你将解决的事|没有Git怎么活|
|---|---|
|代码改坏了找不回来|复制N份"项⽬_最终版_真的最终.zip"|
|想看"上周三是哪个版本"|全靠记忆/没有|
|多⼈同时改⼀个⽂件|QQ传⽂件互相覆盖/哭|
|⾃动跑测试/部署|⼿动SSH上服务器操作|
|接受开源社区贡献|邮件附件patch|



现实：哪怕你⼀个⼈写代码，也要⽤ Git。否则你迟早会丢⽂件、记不清某段代码 为什么改过、或者某天硬盘坏了。 

## 1.1 三个核⼼名词先混个脸熟 

仓库（Repository / repo）：⼀个项⽬的所有⽂件 + 历史的总和 提交（Commit）：⼀次完整的"修改快照"，有唯⼀哈希（如 `a1b2c3d` ） 分⽀（Branch）：从主线上"分叉"出来的独⽴⼯作流，合并后⼜能合回主线 

# 2. 安装与⾸次配置 

## 2.1 安装 

|平台|命令/链接|
|---|---|
|macOS|`xcode-select --install` 或 `brew install git`|
|Windows|https://git-scm.com/download/win（默认选项⼀路<br>Next）|
|Linux<br>(Debian/Ubuntu)|`sudo apt install git`|
|Linux (CentOS/RHEL)|`sudo yum install git`|



安装完成后打开终端验证： 

```
git --version
# git version 2.42.0 或更高
```

## 2.2 ⾸次配置 

```
git config --global user.name "你的名字"
git config --global user.email "your@email.com"
# 让 `git log` / `diff` 显示颜色
git config --global color.ui auto
# Windows 用户必做：换行符处理，避免 CRLF 引发冲突
git config --global core.autocrlf input
# macOS / Linux 用户
git config --global core.autocrlf false
# 默认分支名（GitHub 2022 年起默认 main）
git config --global init.defaultBranch main
```

⼩知识：上述配置存在 `~/.gitconfig` （Linux/macOS）或 `C:\Users\ 你 \.gitconfig` （Windows）。 `--global` 表⽰"对当前⽤⼾⽣效"。 

## 2.3 **`--global`** 什么时候⽤ / 什么时候不⽤ 

刚学 Git 时所有⼈都⽤ `git config --global ...` ，但它不是万能的。这⼀节说 清楚 Git 配置的"三档作⽤域"，以及多账号场景下 `--global` 会带来的⿇烦。 

### 2.3.1 Git 配置的三档作⽤域 

|作⽤域|配置⽂件|作⽤范<br>围|何时⽤|
|---|---|---|---|
|`--`<br>`system`|`/etc/gitconfig`（Win：`C:\Program`<br>`Files\Git\etc\gitconfig`）|这台电<br>脑的所<br>有⽤⼾|⼏乎不⽤（要管理<br>员权限）|
|||当前⽤|99%的⽇常配置|
|`--`<br>|`~/.gitconfig`|⼾的所|（⽤⼾名、邮箱、|
|`global`||有仓库|换⾏符）|



作⽤范 

作⽤域 配置⽂件 

何时⽤ 

围 

当前这 不带参 覆盖 `--global` 的 `< 仓库 >/.git/config` ⼀个仓 数 某些项 库 

规律：配置查找是"由近及远"——仓库级 → 全⽤⼾级 → 系统级，近的覆盖远的。 

```
# 查看所有配置 + 来源
git config --list --show-origin
# 只看当前仓库生效的配置
git config --list --local
```

### 2.3.2 三档"使⽤ vs 不使⽤ --global"决策表 

|配置项|该⽤哪⼀档？|为什么|
|---|---|---|
|`user.name`|`--global`|⼀台电脑就⼀个名字|
|`user.email`|`--global`（但多账号时<br>例外）|同上，除⾮你有多个Git平台<br>账号|
|`init.defaultBranch`|`--global`|主分⽀名对所有仓库⼀致|
|`core.autocrlf`|`--global`|换⾏符规则对所有仓库⼀致|
|`color.ui`|`--global`|UI偏好跟⼈⾛|
|`core.editor`|`--global`|个⼈编辑器偏好|
|`alias.*`|`--global`|命令别名跨仓库通⽤|
|`remote.origin.url`|不带参数（仓库级）|每个仓库的远程地址都不同|
|`branch.main.merge`|不带参数（仓库级）|每个仓库的上游分⽀不同|
|`user.email`（多账号时）|不带参数/条件include|不同项⽬⽤不同邮箱|
|`commit.gpgsign`|看场景|公司项⽬可能要签名、个⼈<br>项⽬不签|



### 2.3.3 什么时候不⽤ **`--global`** 

|场景|原因|替代⽅案|
|---|---|---|
|公司项⽬要署名公司|不想把公司邮箱写进|仓库级 `git config user.email`|
|邮箱|`~/.gitconfig`|`"work@company.com"`|
|个⼈项⽬⽤GitHub邮|||
|箱、开源项⽬⽤|不想混|仓库级覆盖|
|**`noreply`**|||
|多账号（⼀台电脑多<br>个GitHub账号）|全局只有⼀个邮箱，<br>多账号⽆法区分|条件include（⻅ 2.3.4）|
|只想临时改⼀项|不想动全局|仓库级配置|
|CI / Docker容器内|容器内不需要持久化|命令⾏ `-c user.email=...` 临时<br>传|



### 2.3.4 多账号场景：⽤条件 include 代替 **`--global`** 

- 核⼼思想：根据当前⽬录路径，⾃动⽤不同的 Git 配置——这是多账号的唯⼀"正 

### 确解"。 

### **`~/.gitconfig`** （顶层）： 

```
[user]
    name = 个人默认
    email = personal@gmail.com
# 公司项目放 ~/work/ 目录
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work
# 开源贡献放 ~/opensource/ 目录
[includeIf "gitdir:~/opensource/"]
    path = ~/.gitconfig-opensource
```

### **`~/.gitconfig-work`** ： 

```
[user]
    name = 张三
    email = zhangsan@company.com
```

**`~/.gitconfig-opensource`** ： 

```
[user]
    name = tom-personal
    email = tom@users.noreply.github.com
```

### 效果： 

```
cd ~/work/my-company-app
git config user.email    # → zhangsan@company.com
cd ~/opensource/my-lib
git config user.email    # → tom@users.noreply.github.com
```

判断顺序：Git 从当前仓库路径向上匹配 `gitdir:` ，第⼀个命中的 include ⽣效。 

### 2.3.5 ⼀句话 

个⼈单账号：放⼼⽤ `--global` ；多账号 / 多公司邮箱：⽤ `includeIf "gitdir:..."` 按⽬录⾃动切，别在仓库⾥瞎⽤ **`--global`** 。 

# 3. 核⼼概念：三个区、四种状态 

### 把 Git 想成"有三条传送带"： 



<!-- Start of picture text -->
┌──────────┐    git add     ┌──────────┐   git commit   ┌──────────┐<br>│  工作区     │ ─────────────►│  暂存区     │ ──────────────►│  版本库     │<br>│ (Working) │                │ (Index)   │                │ (Repo)    │<br>└──────────┘                └──────────┘                └──────────┘<br>     ▲                          ▲                            ▲<br>     │                          │                            │<br>编辑文件               git restore --staged           git checkout<br>删除文件 取消暂存 切换分支<br><!-- End of picture text -->

⽂件在这三个区之间有四种状态： 

|状态||含义|命令||
|---|---|---|---|---|
|未跟踪|（Untracked）|新建的⽂件，Git还没"管"它|`git`|`add`|
|已修改|（Modified）|改过但还没 `add`|`git`|`add`|
|已暂存|（Staged）|已 `add` 等着提交|`git`|`commit`|



状态 含义 命令 

### 已提交（Committed） 已经写⼊历史 

```
git status          # 看文件当前状态（最常用！）
```

# 4. 基础流程：本地 + 远程 

## 4.1 把本地代码推到 GitHub 

### ⽅式 A：本地先有项⽬ 

```
cd my-project
git init
git add .
git commit -m "feat: 初始化项目"
git branch -M main                  # 强制主分支叫 main
git remote add origin git@github.com:你的用户名/你的仓库.git
git push -u origin main             # -u 记住 upstream，以后直接 git push
```

### ⽅式 B：先在 GitHub 建仓库，再 clone 到本地 

```
git clone git@github.com:你的用户名/你的仓库.git
cd你的仓库
# 写代码...
git add .
git commit -m "feat: 第一次提交"
git push
```

## 4.2 SSH Key 配置（推荐，避免每次输密码） 

```
# 1. 生成密钥（一路回车）
ssh-keygen -t ed25519 -C "your@email.com"
# 2. 复制公钥
cat ~/.ssh/id_ed25519.pub          # macOS / Linux
# Windows: type %userprofile%\.ssh\id_ed25519.pub
# 3. 粘贴到 GitHub：Settings → SSH and GPG keys → New SSH key
```

```
# 4. 验证
ssh -T git@github.com
# 看到 "Hi xxx! You've successfully authenticated..." 就 OK
```

HTTPS vs SSH：新项⽬推荐 SSH；HTTPS 在某些代理下更稳，按需选。 

## 4.3 多账号管理：⼀台电脑多个 Git 平台 / 多个同 平台账号 

### 你很可能会遇到： 

   - 家⾥电脑既要推 GitHub 个⼈号、⼜要推 Gitee 公司号； ⼯作电脑同时有"个⼈ GitHub + 公司 GitHub（GitHub Enterprise）+ 公司 GitLab"三套账号； 

   - 帮客⼾改代码，⽤⾃⼰的 GitHub 推 PR，但客⼾ GitLab 也要能推。 

- ⼀台电脑 + ⼀个 GitHub 默认 key 不够⽤——这⼀节讲怎么⽤ `~/.ssh/config` + 多 key 解决，Windows / Linux 各给完整例⼦。 

### 4.3.1 3 种典型场景 

|场景<br>怎么配|
|---|
|A.⼀台电脑⼀个GitHub账号<br>4.2那节的内容就够了|
|B.⼀台电脑多个GitHub账号（个⼈+<br>公司Enterprise）<br>多key +`~/.ssh/config`+`Host` 别名|
|C.⼀台电脑多个平台账号（GitHub +<br>GitLab + Gitee）<br>多key +每个平台⼀个 `Host` 别名|
|D.同平台但想"按⽬录⾃动切"<br>`includeIf "gitdir:..."` ⾃动切<br>`user.email`（⻅ 2.3.4）|
|现实⾥ B + C + D经常同时存在：⼀台电脑、3个平台、4个账号、6把key——下<br>⾯的例⼦就是这种"复杂但真实"的场景。|



### 4.3.2 原理：SSH 怎么"认⼈" 

```
ssh -T git@github.com
   │
   ▼
```

```
客户端拿 ~/.ssh/config 找匹配 Host
   │
   ▼
用对应 Host 的 IdentityFile（密钥）发起认证
   │
   ▼
GitHub 收到公钥 → 查是哪个账号加的 → 允许 / 拒绝
```

### 关键点： 

⼀个 `~/.ssh/id_ed25519` 对应⼀对密钥（公钥 + 私钥）； ⼀个账号可以加多把公钥（公司电脑 / 家⾥电脑各⼀把）； ⼀台电脑可以有多把私钥，靠 `~/.ssh/config` 的 `Host` 别名切换。 

### 4.3.3 准备：⽣成 4 把 key（个⼈ / 公司 GitHub / GitLab / Gitee） 

⼀个平台⼀把 key，命名要⻅名知意。 

### Linux / macOS（bash / zsh）： 

```
# 1. 个人 GitHub
ssh-keygen -t ed25519 -C "your@personal.com" \
  -f ~/.ssh/id_ed25519_github_personal -N ""
# 2. 公司 GitHub Enterprise
ssh-keygen -t ed25519 -C "your@company.com" \
  -f ~/.ssh/id_ed25519_github_work -N ""
# 3. 公司 GitLab
ssh-keygen -t ed25519 -C "your@company.com" \
  -f ~/.ssh/id_ed25519_gitlab -N ""
# 4. Gitee
ssh-keygen -t ed25519 -C "your@gitee.com" \
  -f ~/.ssh/id_ed25519_gitee -N ""
# 验证 4 把都生成了
ls -la ~/.ssh/id_ed25519_*
```

### Windows（PowerShell）： 

```
# 同上 4 条命令，但路径用 $HOME\.ssh\
ssh-keygen-t ed25519 -C"your@personal.com" `
  -f "$HOME\.ssh\id_ed25519_github_personal"-N""
ssh-keygen-t ed25519 -C"your@company.com" `
  -f "$HOME\.ssh\id_ed25519_github_work"-N""
```

```
ssh-keygen-t ed25519 -C"your@company.com" `
```

```
  -f "$HOME\.ssh\id_ed25519_gitlab"-N""
ssh-keygen-t ed25519 -C"your@gitee.com" `
  -f "$HOME\.ssh\id_ed25519_gitee"-N""
dir$HOME\.ssh\id_ed25519_*
```

Windows ⽤ `dir` 看， `-N ""` 表⽰空密码（CI 友好）。私钥⽂件不要给别⼈看。 

### 4.3.4 把 4 个公钥分别贴到 4 个平台 

|平台|路径|
|---|---|
|GitHub个⼈|Settings→SSH and GPG keys→New SSH key，Title<br>写"personal mac"|
|GitHub<br>Enterprise|同上，但登录的是Enterprise账号|
|GitLab|Preferences→SSH Keys|
|Gitee|设置 →SSH公钥|



```
# Linux / macOS：分别打印公钥
cat ~/.ssh/id_ed25519_github_personal.pub
cat ~/.ssh/id_ed25519_github_work.pub
cat ~/.ssh/id_ed25519_gitlab.pub
cat ~/.ssh/id_ed25519_gitee.pub
```

```
# Windows：分别打印公钥
type$HOME\.ssh\id_ed25519_github_personal.pub
type$HOME\.ssh\id_ed25519_github_work.pub
type$HOME\.ssh\id_ed25519_gitlab.pub
type$HOME\.ssh\id_ed25519_gitee.pub
```

每条输出都是⼀串 `ssh-ed25519 AAAA...` ，整段复制贴到对应平台。 

### 4.3.5 写 **`~/.ssh/config`** ：核⼼步骤 

SSH `config` ⽂件是"按 Host 别名选不同 key"的唯⼀⼊⼝。没有它，4 把 key 没法 ⾃动切换。 

Linux / macOS： 

```
cat > ~/.ssh/config <<'EOF'
# ================== 个人 GitHub ==================
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_personal
    IdentitiesOnly yes
    AddKeysToAgent yes
# ================== 公司 GitHub Enterprise ==================
# 假设 Enterprise 域名是 github.acme.com
Host gh.acme
    HostName github.acme.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_work
    IdentitiesOnly yes
    AddKeysToAgent yes
# ================== 公司 GitLab ==================
# 假设 GitLab 域名是 gitlab.acme.com
Host gl.acme
    HostName gitlab.acme.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitlab
    IdentitiesOnly yes
    AddKeysToAgent yes
# ================== Gitee ==================
Host gitee.com
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitee
    IdentitiesOnly yes
    AddKeysToAgent yes
EOF
# 权限必须是 600
chmod 600 ~/.ssh/config
```

### Windows： 

```
# PowerShell
$configPath = "$HOME\.ssh\config"
@"
# ================== 个人 GitHub ==================
Host github.com
    HostName github.com
    User git
    IdentityFile $HOME\.ssh\id_ed25519_github_personal
    IdentitiesOnly yes
    AddKeysToAgent yes
```

```
# ================== 公司 GitHub Enterprise ==================
Host gh.acme
```

```
    HostName github.acme.com
    User git
    IdentityFile $HOME\.ssh\id_ed25519_github_work
    IdentitiesOnly yes
    AddKeysToAgent yes
# ================== 公司 GitLab ==================
Host gl.acme
    HostName gitlab.acme.com
    User git
    IdentityFile $HOME\.ssh\id_ed25519_gitlab
    IdentitiesOnly yes
    AddKeysToAgent yes
# ================== Gitee ==================
Host gitee.com
    HostName gitee.com
    User git
    IdentityFile $HOME\.ssh\id_ed25519_gitee
    IdentitiesOnly yes
    AddKeysToAgent yes
"@ | Out-File-FilePath$configPath-Encoding utf8 -NoNewline
```

### 关键参数： 

**`Host`** ：别名（不是域名，是"你想叫它什么"） **`HostName`** ：真正的域名 **`IdentityFile`** ：⽤哪把私钥 **`IdentitiesOnly yes`** ：只⽤ `IdentityFile` 指定的 key，不⾃动尝试 `~/.ssh/id_rsa` 等默认 key（避免和别的账号"撞⻋"） **`AddKeysToAgent yes`** （macOS 必需）：把 key 加到 ssh-agent，免去每次输 密码 

### 4.3.6 验证 4 把 key 都能连上 

### Linux / macOS： 

```
ssh -T git@github.com        # → Hi <个人用户名>! ...
ssh -T git@gh.acme            # → Hi <公司用户名>! ...
ssh -T git@gl.acme            # → Welcome to GitLab, @<公司用户名>!
ssh -T git@gitee.com          # → Hi <Gitee 用户名>! ...
```

### Windows（PowerShell）： 

```
ssh -T git@github.com
ssh -T git@gh.acme
```

```
ssh -T git@gl.acme
ssh -T git@gitee.com
```

第⼀次连会问 `Are you sure you want to continue connecting (yes/no)?` ——输⼊ yes 即可（确认公钥指纹）。 

### 4.3.7 clone / push 时怎么⽤别名 

```
# 拉公司项目
git clone git@gl.acme:my-team/my-project.git
#   └── 注意：HostName 是 gitlab.acme.com，但写时用别名 gl.acme
# 拉个人项目（直接用官方域名）
git clone git@github.com:your-name/your-repo.git
# 拉 GitHub Enterprise
git clone git@gh.acme:company-org/company-repo.git
# 拉 Gitee
git clone git@gitee.com:your-name/your-repo.git
```

关键：clone 命令⾥写 **`Host`** 别名，不是 `HostName` 。 

### 4.3.8 同⼀平台多个账号：必须⽤别名 Host 

最容易踩的坑：默认 `github.com` 只能绑⼀把 key。如果你有两个 GitHub 账号都 要⾛ `git@github.com:xxx/yyy.git` ，第⼆个⼀定失败。 

### 解法：给第⼆个账号起不同的 Host 别名。 

```
# 个人 GitHub：走官方域名
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_personal
    IdentitiesOnly yes
# 公司 GitHub（用别名 ghe 占住）
Host ghe
    HostName github.com            # ← 注意：和上面 HostName 相同，但 Host 别名不同
    User git
    IdentityFile ~/.ssh/id_ed25519_github_work
    IdentitiesOnly yes
```

使⽤： 

```
# 个人项目（直接用 github.com）
git clone git@github.com:tom/personal-repo.git
```

```
# 公司项目（用别名 ghe）
git clone git@ghe:acme-org/company-repo.git
```

看起来 `HostName` 都是 `github.com` ，但别名不同——SSH 会按别名匹配 IdentityFile，从⽽⽤不同的 key。这是同平台多账号的唯⼀正确解。 

4.3.9 Windows ⽤ Git Credential Manager（GCM）记 HTTPS 密码 如果你⽤ HTTPS ⽽不是 SSH，别再把 token 写在 **`~/.gitconfig`** ⾥——⽤ GCM 让系统帮你记。 

### 安装： 

### 已装 Git for Windows → 已有 GCM 

macOS： `brew install --cask git-credential-manager` 

Linux：⻅ https://github.com/git-ecosystems/git-credential-manager 

### 第⼀次 push 时会弹窗： 

```
Sign in with your browser to continue.
```

浏览器登录后 token ⾃动存到系统 Keychain / Credential Manager，以后免输⼊。 

```
# 检查 GCM 是否生效
git config --global credential.helper
# Windows / macOS 应该输出 manager / manager-core
```

### 4.3.10 常⻅坑速查 

|现象|原因|解决|
|---|---|---|
|`Permission denied`|key路径写错|检查 `~/.ssh/config` 的 `Host` 和|
|`(publickey)`|/没⽤别名|`IdentityFile`|
|`git@github.com:`<br>`Permission denied` ⼀直<br>提⽰|默认key还在<br>被尝试|确认 `IdentitiesOnly yes`|



|现象|原因|解决|
|---|---|---|
|同平台两个账号都连<br>`github.com` 失败|⼀域名只能绑<br>⼀把key|给第⼆个账号起别名（⻅4.3.8）|
|克隆公司仓库但 `git`<br>`config user.email` 仍是<br>个⼈邮箱|没⽤<br>`includeIf`|⻅ 2.3.4|
|Windows上<br>`~/.ssh/config` 不⽣效|路径不对/权<br>限太松|路径必须<br>`%USERPROFILE%\.ssh\config`，⽂件<br>不能有 `.txt` 后缀|
|改完 `~/.ssh/config` 没⽣<br>效|SSH缓存|跑 `ssh-add -D` 清空再 `ssh-add <key>`<br>重启agent|
|同⼀key在多个GitHub<br>账号上重复使⽤|平台不允许|每账号独⽴key|



### 4.3.11 验证清单（跑完下⾯ 4 ⾏才算"多账号配置完成"） 

```
# 1. 四把 key 都连得上
ssh -T git@github.com   && \
ssh -T git@gh.acme       && \
ssh -T git@gl.acme       && \
ssh -T git@gitee.com
# 2. clone 一个个人项目能 push
git clone git@github.com:tom/repo.git && cd repo && \
echo"hi" >> README.md && git add . && git commit -m "test" && git push
# 3. clone 一个公司项目能 push
git clone git@gl.acme:my-team/repo.git && cd repo && \
echo"hi" >> README.md && git add . && git commit -m "test" && git push
# 4. 检查 commit 显示的是"对应账号"的邮箱
git log -1 --format='%an <%ae>'
# 在 ~/work/ 下应该是 zhangsan@company.com
# 在 ~/personal/ 下应该是 tom@gmail.com
```

### 4.3.12 ⼀句话总结 

多账号的核⼼是"按⽬录分 key + 按别名选 key"—— `~/.ssh/config` 把别名和私 钥配对， `includeIf "gitdir:..."` 把⽬录和邮箱配对。两边配合，任何账号都 ⾃动选对 key、选对邮箱，不⽤每次⼿切。 

# 5. 分⽀策略：Trunk-Based / Git Flow / GitHub Flow 

"分⽀怎么管"是团队协作的核⼼问题。选错策略 = 每天在合并冲突⾥过⽇⼦。 

## 5.1 三⼤主流分⽀模型 

|模型|主分⽀|辅助分⽀|适合谁|节奏|
|---|---|---|---|---|
|Trunk-<br>Based|`main`|短期feature分⽀<br>（< 1天）|⾼频发版、<br>CI/CD成熟团队|每天合并到<br>main|
|GitHub<br>Flow|`main`|任意命名的feature<br>分⽀|中⼩团队/ SaaS<br>/互联⽹产品|PR→review<br>→merge|
|Git Flow|`main`+<br>`develop`|`feature/*`<br>`release/*`<br>`hotfix/*`|有"版本号"概念<br>的桌⾯/嵌⼊式|每周/每⽉⼀<br>版|



## 5.2 推荐：GitHub Flow（99% 的现代项⽬都⽤ 它） 

```
       feature/login ────────► PR ──► main ──► 部署
              │                  ▲
              └─ 短期分支（1~3 天）│
                                  │
              Code Review ────────┘
```

### 核⼼规则： 

1. `main` 分⽀永远可发布（CI 必须绿） 

2. 任何新功能 / 修复 = 从 `main` 拉新分⽀ 

3. 分⽀上写完提 PR → 团队评审 → 合并回 `main` 

4. 合并后⽴即部署 

## 5.3 Git Flow（适合"发版有节奏"的传统项⽬） 

```
main         ●─────────────●─────────────●─────►  v1.0    v1.1
              \           / \           /
develop        ●───●───●───●───●───●───●
                 \   /       \   /
feature/A         ●─●         \ /
                                 ●─●      hotfix
                                  X       X
```

`main` ：每个 tag 是⼀个发布版本 `develop` ：⽇常集成分⽀ `feature/*` ：从 develop 拉，合并回 develop `release/*` ：发版前准备，测完合并回 main + develop `hotfix/*` ：从 main 拉紧急修复 

初学者不要硬套 Git Flow——它流程重、合并分⽀多，对⼩项⽬是负担。 

## 5.4 命名约定 

```
# 推荐格式：<类型>/<简短描述>
feature/login             # 新功能
feature/user-profile
fix/header-404            # 修 bug
refactor/axios-instance   # 重构
docs/update-readme        # 文档
chore/bump-vite-6         # 杂项
```

类型对应 Conventional Commits 的 `feat / fix / refactor / docs / chore` 等。 

# 6. 团队协作：Pull Request 与 Code Review 

PR（Pull Request） 是 GitHub 协作的核⼼：不是"代码合并请求"，⽽是"团队评 审 + 讨论 + 留痕"的载体。 

## 6.1 ⼀次完整的 PR 流程 

```
1. 从 main 拉分支       git checkout -b feature/login
```

`2. 写代码 + 多次 commit 3. 推到远程 git push origin feature/login` 

`4. 在 GitHub 上开 PR     → 选 base = main, compare = feature/login` 

```
5. 写 PR 描述：改了什么 / 为什么改 / 截图 / 关联 Issue
6. 通知 reviewer         → 在 PR 右侧选 Reviewers
7. reviewer 看代码、写评论、approve / request changes
```

```
8. CI 自动跑测试        → 必须绿才能合并
9. 通过后 Squash and merge（推荐）
10. 删除远端 feature 分支
```

## 6.2 PR 描述模板 **`.github/pull_request_template.md`** 把下⾯这⽂件放到仓库根⽬录 `.github/` 下，下次开 PR 就会⾃动加载： 

**`##`** 改了什么 `<!-- 一句话讲清楚这一 PR 做了什么 -->` 

**`##`** 改动的类型 `- [ ] 新功能（ feature ） - [ ] Bug 修复（ fix ） - [ ] 重构（ refactor ） - [ ] 文档（ docs ） - [ ] 杂项（ chore ）` 

**`##`** 怎么测 `<!-- 步骤 + 截图 / 录屏 -->` **`##`** 关联 **`Issue`** `<!-- 关闭 / 关联 #xxx -->` **`##`** 自检清单 `- [ ] 跑过 `npm run dev` 验证 - [ ] 跑过 `npm run build` 没有报错 - [ ] 新增 / 修改的代码有注释 - [ ] 没把 `console.log` 留在生产代码里` 

## 6.3 Code Review 的三个原则 

原则 

含义 

就事论事 评论代码不评论⼈；⽤"这⾥可以……"代替"你怎么⼜……" 

⼩步快跑 PR 越短越好；200 ⾏以内的 PR ⽐ 2000 ⾏的 PR review 体验好 10 倍 

含义 

原则 

及时响应 作者提了 PR 24 ⼩时内⾄少回应；reviewer 收到通知尽快看 

## 6.4 ⾼频 review 关注点 

命名是否表意（ `btn1` / `data2` ❌； `submitButton` / `userList` ✅） 

- 是否引⼊了不必要的依赖 错误处理是否到位（有没有 `catch` 漏掉的） 

- 是否有 console.log / debugger 残留 类型是否完整（TS 项⽬） 

测试是否覆盖关键路径 

## 6.5 合并⽅式：merge / squash / rebase 

|⽅式|⾏为|适合|
|---|---|---|
|Merge commit|保留所有commit +⼀个merge<br>commit|想保留完整历史|
|Squash and<br>merge|把所有commit压成⼀个|最常⽤，main历史⼲净|
|Rebase and<br>merge|把分⽀commit接到main之后|想保留commit但不要merge<br>commit|
|推荐默认Squas<br>的"功能单元"。|h and merge，让 `main` 上每个com|mit都是⼀个独⽴可回滚|



# 7. 冲突处理：什么时候 rebase，什么时候 merge 

## 7.1 rebase vs merge 速记 

```
# 把当前分支"接到" main 最新提交之后
git rebase main
```

```
# 把 main 合并到当前分支（保留所有历史）
git merge main
```

|维度|rebase|merge|
|---|---|---|
|历史|⼲净的直线|有merge commit节点|
|冲突处理时机|⼀次性、容易|每个冲突commit都要处理|
|适合|本地整理未推送的commit|推送后、合并重要feature|



## 7.2 ⻩⾦规则 

没推上去的 commit：随便 rebase；已经推上去的 commit：永远别 rebase。 

否则你的同事 `git pull` 后会发现历史对不上号，全员崩溃。 

## 7.3 实战：解决合并冲突 

```
git pull origin main        # 或 git fetch + git rebase origin/main
# CONFLICT (content): Merge conflict in src/utils/request.ts
git status                  # 看哪些文件冲突
# 打开冲突文件，找到 <<<<<<< HEAD ======= >>>>>>> main 标记，手动选择保留谁
git add src/utils/request.ts
```

```
git commit                   # 合并冲突用 git commit（不是 git rebase --continue 那
种）
```

## 7.4 后悔药 

```
git rebase --abort          # rebase 过程中想撤销
git merge --abort           # merge 过程中想撤销
git reset --hard HEAD~1     # 撤销最近一次 commit（**未推送才用**）
```

# 8. 提交规范：Conventional Commits 

让 commit 信息机器可读，⾃动⽣成 CHANGELOG。 

## 8.1 格式 

```
<类型>(<范围>): <简短描述>
<body>（可选，72 字符换行）
<footer>（可选，关联 Issue）
```

## 8.2 常⽤类型 

|类型|含义|例⼦|
|---|---|---|
|`feat`|新功能|`feat(login):新增手机号登录`|
|`fix`|修bug|`fix(header):修复移动端导航错位`|
|`docs`|⽂档|`docs(readme):补充部署步骤`|
|`style`|格式（不改逻辑）|`style:统一代码缩进`|
|`refactor`|重构|`refactor(api):抽出 request工具方法`|
|`perf`|性能|`perf(list):虚拟滚动优化 10000项`|
|`test`|测试|`test(button):补点击事件单测`|
|`chore`|杂项|`chore:升级 vite到 6.0`|
|`ci`|CI配置|`ci:添加 GitHub Actions`|
|`revert`|回滚|`revert:撤回 feat(login)`|



## 8.3 实战 

```
git commit -m "feat(login): 新增手机号登录"
git commit -m "fix(header): 修复移动端导航错位
```

```
之前的 z-index 在 iOS Safari 失效
原因：没有 -webkit-overflow-scrolling: touch
Closes #123"
```

8.4 ⼯具推荐 

commitlint + husky：提交时⾃动校验格式 standard-version / release-please：⾃动⽣成 CHANGELOG 和版本号 

# 9. ⾼频救命命令：找回误删 / 回退版本 / 改 commit 历史 

这⼀节是"代码丢了怎么办"的速查表，建议收藏。 

## 9.1 误删⽂件 / 误删分⽀ 

```
# 误删文件：恢复成上次 commit 的样子
git checkout -- path/to/file
```

```
# 误删分支：reflog 里找
git reflog                          # 列出所有 HEAD 移动记录
# 找到那个分支最后一次的 commit hash
git checkout -b feature/login <hash>    # 重新拉回来
```

⻩⾦法则： `git reflog` 能救回 30 天内⼏乎所有"误操作"——只要你已经 commit 过。 

## 9.2 撤销最近⼀次 commit 

场景 

### 命令 

撤销 commit，保留改动在⼯作区 `git reset --soft HEAD~1` 撤销 commit，清空改动 `git reset --hard HEAD~1` ⚠ 改写最近⼀次 commit 信息 `git commit --amend` 已经推送？补⼀个"反向" commit `git revert HEAD` 然后 push 

- `--hard` 不可恢复！未推送时谨慎⽤，已推送时绝对不要⽤。 

## 9.3 改写历史：rebase -i 

```
git rebase -i HEAD~5   # 编辑最近 5 个 commit
```

### 弹出编辑器，把 `pick` 改成： 

|动作|含义|
|---|---|
|`pick`|保留|
|`reword`|改commit信息|
|`edit`|停下来改⽂件|
|`squash`|合并到上⼀个|
|`drop`|删掉|
|适合"准|备发版前把5个wip commit整理成1个⼲净的"。|



## 9.4 暂存当前⼯作 

```
git stash                  # 暂存所有未提交改动
git stash pop              # 恢复
git stash list             # 查看所有暂存
git stash apply stash@{0}  # 恢复指定一个
```

适⽤场景：正在改⼀半被叫去修紧急 bug，先把当前改动"藏"起来。 

## 9.5 GPG 签名 commit：公司项⽬ / 关键开源项⽬ 必备 

GitHub / GitLab 都⽀持"verified"绿⾊⼩标签——靠 GPG（PGP 的 GNU 实现）签 名。公司项⽬、Linux 内核等关键项⽬强制要求。 

### 9.5.1 ⼀次配置永久使⽤ 

```
# 1. 生成 GPG key（一直回车 = 默认 + 不设密码）
gpg --full-generate-key
# 选 RSA and RSA，4096 bits，0 = key 永不过期
```

```
# 2. 列出你的 GPG key 找到 ID
gpg --list-secret-keys --keyid-format=long
```

```
# 输出类似：
#   sec   rsa4096/ABC123DEF456 2026-01-01
#                       ^^^^^^^^^^  ← 这就是 GPG key ID
```

```
# 3. 告诉 git 用这个 key 签名
git config --global user.signingkey ABC123DEF456
git config --global commit.gpgsign true# 每次 commit 自动签
git config --global tag.gpgsign true# tag 也签
# 4. 导出公钥贴到 GitHub：Settings → SSH and GPG keys → New GPG key
gpg --armor --export ABC123DEF456
```

### 9.5.2 验证 commit 已签名 

```
git log --show-signature -1
# commit abc123 (HEAD -> main)
# Author: Tom <tom@example.com>
# Date:   ...
#
# commit abc123...
# gpg: Signature made ...
# gpg: Good signature from "Tom <tom@example.com>" [ultimate]
```

### 9.5.3 常⻅问题 

|现象|解决|
|---|---|
|commit后GitHub显<br>⽰"Unverified"|没把GPG公钥贴到GitHub，或email与<br>`user.email` 不⼀致|
|`gpg: signing failed:`<br>`Inappropriate ioctl for`<br>`device`|没启动 `gpg-agent`，先`echo"test"|
|想对单次commit签名|`git commit -S -m "msg"`（⼤写-S）|
|已经commit没签，怎么补签|`git commit --amend -S --no-edit`|
|想跳过签名|`git commit --no-gpg-sign -m "wip"` 或临<br>时 `git config commit.gpgsign false`|



### 何时必须开： 

### 任何商业 / 政企项⽬ 

- 开源项⽬想被 GitHub 标 "verified" 

监管 / 审计要求"能证明代码是某某⼈提交" 

何时可省：个⼈学习项⽬ / 内部⼩⼯具 / 与现有 GPG 体系冲突时。 

## 9.6 ssh-agent：免去每次输⼊ SSH 密码 

4.2 教你⽤ SSH key 推代码——但默认每次 push 都要输⼀次密钥密码。 `sshagent` 帮你"在系统⾥记住"，整个会话只输⼀次。 

### 9.6.1 启动 agent + 把私钥加进去 

```
# macOS（已自带）
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github_personal
# Linux 桌面
eval"$(ssh-agent -s)"# 启动 agent
ssh-add ~/.ssh/id_ed25519_github_personal
# Windows（PowerShell）
Start-Service ssh-agent            # 设为自动启动
Set-Service -Name ssh-agent -StartupType Automatic
ssh-add $HOME\.ssh\id_ed25519_github_personal
```

之后 `git push` 永远不再问密码，直到下次开机。 

### 9.6.2 验证 agent ⾥的 key 

```
ssh-add -l                          # 列出已加的 key 指纹
ssh-add -L                          # 列出完整公钥
```

### 9.6.3 macOS 把密码保存到 Keychain 

```
# 第一次 ssh-add 时加 --apple-use-keychain
# 之后重启也不用重输——密码在 Keychain
```

Windows 把密码存到 Credential Manager（第 4.3.9 节 Git Credential Manager 同⼀机制）。 

## 9.7 常⻅错误速查（10 ⼤经典报错） 

80% 的 Git 错误都在这 10 条⾥——收藏。 

|错误|原因|解决|
|---|---|---|
|`fatal: not a git`<br>`repository`|在⾮Git⽬录<br>⾥跑git命令|`cd` 到仓库，或 `git init`|
|`Permission denied`<br>`(publickey)`|SSH key没配/<br>没加到GitHub|⻅ 4.2 SSH Key配置|
|`fatal: refusing to merge`<br>`unrelated histories`|两个⽆关仓库<br>要合并|`git pull origin main --`<br>`allow-unrelated-histories`<br>（慎⽤）|
|`error: Your local changes`<br>`would be overwritten`|拉代码会覆盖<br>你的未提交改<br>动|先 `git stash` 再pull，最后 `git`<br>`stash pop`|
|`Updates were rejected`<br>`because the tip of your`<br>`current branch is behind`|远端有<br>commit，你本<br>地没有|先 `git pull`（或 `git pull --`<br>`rebase`）再push|
|`fatal: The current branch`<br>`main has no upstream`<br>`branch`|第⼀次push没<br>指定远端|`git push -u origin main`|
|`error: failed to push`<br>`some refs to ...`|远端有更新，<br>push被拒|先pull再push（⻅上）|
|`CONFLICT (content): Merge`<br>`conflict in xxx`|merge /<br>rebase时有冲<br>突|打开⽂件找 `<<<<<<<` 标记⼿动改<br>完，再 `git add`+ commit|
|`fatal: refusing to merge`<br>`unrelated histories`|同上|同上|
|`warning: LF will be`<br>`replaced by CRLF`|Windows上检<br>出时⾃动换⾏|加 `.gitattributes`（⻅ 10.4）|
|`error: src refspec xxx`<br>`does not match any`|本地没有这个<br>分⽀或tag|`git branch -a` 看有什么分⽀|
|`fatal: unable to access`<br>`... Could not resolve`<br>`host`|⽹络/ DNS /<br>VPN问题|切镜像（⻅ 10.7）|
|`fatal: refusing to accept`|push时远端不|`git pull --rebase` 后再|
|`non-linear history`|允许⾮快进合|push，或⽤ `git push --`|



解决 

错误 

原因 

并 `force-with-lease` （慎⽤） 

⻩⾦法则：报错信息第⼀⾏⼏乎永远告诉你原因——先读，再搜。 

# 10. **`.gitignore`** 模板速查 

新项⽬第⼀件事就是写好 `.gitignore` ： 

## 10.1 Node / 前端通⽤模板 

```
# 依赖
node_modules/
.pnpm-store/
# 构建产物
dist/
build/
*.tsbuildinfo
```

```
# 环境变量
.env
.env.local
.env.*.local
```

```
# 日志
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
```

```
# 编辑器
.vscode/
.idea/
*.swp
.DS_Store
```

```
# 测试
coverage/
.nyc_output/
```

```
# 缓存
.cache/
.parcel-cache/
.eslintcache
```

永远不要把 **`node_modules`** 提交到 Git——它能被 `package-lock.json` / `pnpmlock.yaml` 完整还原。 

## 10.2 怎么⽣成 

gitignore.io：选技术栈⾃动⽣成 VSCode 装 gitignore 插件：新建⽂件时模板⾃动出现 

## 10.3 提交了不该提交的⽂件怎么办 

```
# 从仓库里删掉，但保留本地文件
git rm --cached path/to/file
git commit -m "chore: 把 .env 从仓库移除"
```

重要：已经在历史⾥的敏感信息（如不⼩⼼提交了 `.env` ），单靠 `git rm -- cached` 不够——历史⾥还能看到。要⽤ git-filter-repo 真正擦除，⽽且必须⽴刻更 换密钥。 

## 10.4 **`.gitattributes`** ：跨平台换⾏符 / LFS 全靠 它 

很多⼈只听过 `.gitignore` ，但漏配 **`.gitattributes`** 是仓库"莫名其妙改⽂件"的 根因。 

常⻅⽤途 1：统⼀换⾏符（避免 Windows / macOS / Linux 互相污染） 

- `# 文本文件：检出时转 LF ，提交时转 LF * text=auto eol=lf` 

```
# 强制 CRLF（Windows 旧项目）
```

```
# *.bat text eol=crlf
# 排除二进制（不要去碰它）
*.png binary
*.jpg binary
*.zip binary
```

常⻅⽤途 2：导出时排除某些⽂件 

```
# 导出 zip / tar 时不包含测试和文档
/.github export-subst
/docs/      export-ignore
*.test.js  export-ignore
```

### 常⻅⽤途 3：搭配 Git LFS 存⼤⽂件（游戏 / 影视 / 机器学习） 

- `*.psd filter=lfs diff=lfs merge=lfs -text` 

- `*.zip filter=lfs diff=lfs merge=lfs -text` 

- `*.onnx filter=lfs diff=lfs merge=lfs -text` 

⼝诀：跨平台团队第⼀天就该有 **`.gitattributes`** ——能省下 50% 的"为啥这个⽂ 件状态变了"。 

## 10.5 Git Submodules：嵌套仓库 

当你需要"项⽬⾥嵌⼀个独⽴仓库"（如把共享库、第三⽅代码⽤ Git 跟踪）时，⽤ submodule。 

### 10.5.1 添加 submodule 

```
# 把 https://github.com/xxx/shared-lib 作为本仓库的 submodules/shared
git submodule add https://github.com/xxx/shared-lib submodules/shared
```

```
# 之后 git status 会看到：
# new file:   .gitmodules
# new file:   submodules/shared
```

`.gitmodules` 记录了"哪⾥有什么⼦仓库"： 

```
[submodule "submodules/shared"]
```

```
    path = submodules/shared
    url = https://github.com/xxx/shared-lib.git
```

### 10.5.2 clone 包含 submodule 的仓库 

```
# 一行：把父仓库 + 所有 submodule 一起拉下来
git clone --recurse-submodules https://github.com/xxx/main-app.git
```

```
# 已经 clone 了？补救：
git submodule init
git submodule update
```

### 10.5.3 更新 submodule 

```
# 1. 进 submodule 切到要用的分支 / 拉新代码
cd submodules/shared
git checkout main
git pull
# 2. 回到父仓库，commit 这个"子模块版本变更"
cd ../..
git add submodules/shared
git commit -m "chore: bump shared-lib to v1.2.3"
```

### 10.5.4 何时⽤ vs 何时不⽤ 

|场景|推荐|
|---|---|
|公共库多项⽬共享|✅monorepo（⻅ 第15章）|
|第三⽅代码想跟上游同步|✅submodule|
|需要精确控制⼦仓库版本|✅submodule|
|频繁改⼦仓库代码|❌ 太⿇烦|
|CI构建慢|❌submodule的clone经常很慢|



替代⽅案：现在⼤多数场景⽤ monorepo + pnpm workspace / Turborepo（⻅ 15 章），submodule ⽤得越来越少。 

## 10.6 浅克隆与部分克隆：⼤仓库的救星 

仓库动辄 1‒10 GB（chrome、vue-next、tensorflow …）？完整 clone ⼀次半⼩ 时——⽤浅克隆。 

10.6.1 **`--depth`** 浅克隆 

```
# 只拉最近 1 次 commit
git clone --depth 1 https://github.com/vuejs/core.git
```

```
# 拉最近 50 次
```

```
git clone --depth 50 https://github.com/vuejs/core.git
```

代价：拿不到完整历史， `git log` 只能看最近 N 条。但 99% 的项⽬"只看最新代 码"⾜够。 

### 10.6.2 **`--filter=blob:none`** 部分克隆（partial clone） 

```
# 拿到所有 commit 和 tree，**但不下载任何文件**
```

```
git clone --filter=blob:none https://github.com/vuejs/core.git
```

```
# 第一次 checkout 时才真正下载文件（按需）
```

⽐ **`--depth`** 更强：保留完整历史，但省下"⼤⽂件"的下载。 

### 10.6.3 **`--sparse`** 稀疏检出：只拉⼀个⼦⽬录 

```
# clone 后只检出 packages/react 一个目录
git clone --filter=blob:none --sparse https://github.com/facebook/react.git
cd react
```

```
git sparse-checkout set packages/react
```

```
# 想看全部？
```

```
git sparse-checkout disable
```

实战：公司 monorepo ⼏ GB 时，新员⼯只拉⾃⼰负责的包，clone 10 秒搞定。 

### 10.6.4 ⼀句话选型 

你想 

### ⽤这个 

拉代码就够，不看历史 `git clone --depth 1` 拉所有 commit 但不下载⼤⽂件 `git clone --filter=blob:none` 只看⼀个⼦⽬录 `--sparse` + `sparse-checkout set` 

真的需要完整历史 ⽼⽼实实 **`git clone`** 

## 10.7 国内镜像加速：clone GitHub 不再卡死 

国内 clone GitHub 平均 10‒50 KB/s，10 GB 仓库要等⼤半天。⽤镜像。 

### 10.7.1 GitHub 官⽅镜像 

```
# 把 github.com 换成 ghproxy.com（GitHub 官方反代镜像）
```

```
git clone https://ghproxy.com/https://github.com/vuejs/core.git
```

```
# 或用 gh-proxy.com
```

```
git clone https://gh-proxy.com/https://raw.githubusercontent.com/...
```

注意：镜像只是"代理"，权限控制和私密仓库还是⾛你⾃⼰电脑的 SSH key，不会 经镜像泄露。 

### 10.7.2 把镜像设成 git 全局配置（推荐） 

```
# 国内用户推荐：把 https 替换走镜像
```

```
git config --global url."https://ghproxy.com/https://github.com/".insteadOf
"https://github.com/"
```

之后 `git clone https://github.com/xxx/yyy.git` ⾃动⾛镜像。 

### 10.7.3 第三⽅镜像站速查 

|镜像|域名|适合|
|---|---|---|
|ghproxy|`https://ghproxy.com`|通⽤反代（最常⽤）|
|mirror.ghproxy.com|`https://mirror.ghproxy.com`|ghproxy镜像|
|kkgithub|`https://kkgithub.com`|通⽤|
|fastgit|`https://hub.fastgit.xyz`|通⽤|
|阿⾥云npm镜像|`https://registry.npmmirror.com`|只镜像npm（不是<br>git）|
|重要：别把镜像写进|push URL——push⾛镜像会被ban。|镜像只⽤于 clone。|



### 10.7.4 ⾃部署 git 镜像 

公司 / 学校内⽹常⽤ `git clone` 内⽹仓库速度慢，可在 CI / 内⽹ GitLab 上挂"⾃动同步 GitHub"： 

```
# GitLab 自带的"镜像仓库"功能
```

```
# 或用 gitea 做 GitHub 反代（mirror）
```

⼀句话：国内 clone 必配镜像—— `ghproxy.com` ⼏乎能解决 80% 的痛点。 

# 11. GitHub Issue / Project / Wiki 协作 

写代码只是软件开发的⼀半；另⼀半是"沟通、跟踪、复盘"。GitHub 提供⼀整套 ⼯具让这些事不必靠微信群。 

## 11.1 Issue：轻量任务管理 

典型⽤法： 

报 bug 

提需求 / 改进建议 提问题求帮助 

任务分派 

Issue 模板 `.github/ISSUE_TEMPLATE/bug.md` ： 

`--name: Bug 报告 about: 创建一个报告来帮助我们改进 title: '[BUG] '` **`labels: bug --##`** 复现步骤 `1. 打开 ... 2. 点击 ... 3. 看到 ...` 

**`##`** 期望行为 `<!-- 期望是什么 -->` **`##`** 实际行为 `<!-- 实际看到什么 -->` 

**`##`** 环境 

```
- OS: Windows 11 / macOS 14 / Ubuntu 22.04
-浏览器: Chrome 120
-版本: v1.2.3
```

关键字关联：在 PR 或 commit 信息⾥写 `Closes #123` / `Fixes #45` ，合并 PR 后 会⾃动关闭对应 Issue。 

## 11.2 Project：看板视图 

GitHub Project 是个看板——把 Issue / PR 拖到不同列： 

`┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐ │ Backlog │──►│ Todo    │──►│ In Prog │──►│ Done    │ └─────────┘   └─────────┘   └─────────┘   └─────────┘` 适合 3-10 ⼈⼩团队代替 Jira。 

## 11.3 Discussion：问答 / 公告 

⽐ Issue 更轻量——适合"我想问个问题"或"我们发了 v2.0 ⼤家看 README"。 

## 11.4 Wiki：⻓期⽂档 

适合放不会频繁变动的内容：架构图、API ⽂档、Onboarding 指南。本教程 README 也是类似⻆⾊。 

## 11.5 三者对⽐ 

⼯具 适合 寿命 Issue 任务、Bug、具体功能 完成后关闭 Project 团队看板上视图 ⻓期存在 Discussion 问答、公告、闲聊 ⻓期存在 Wiki ⻓期⽂档 ⻓期存在 

# 12. GitHub Actions ⾃动化⼊⻔ 

CI（持续集成）/ CD（持续部署） 听着⾼⼤上，本质就是"每次 push / PR，机器 ⾃动跑你写好的脚本"。GitHub Actions 是 GitHub 内置的 CI ⼯具。 

## 12.1 核⼼概念 

|名词|含义|
|---|---|
|Workflow|⼀个YAML⽂件，定义"什么事件触发+做什么事"|
|Job|⼀个⼯作单元（如"跑测试"/"打包"）|
|Step|Job⾥的⼀步命令|
|Action|别⼈写好的可复⽤步骤（`actions/checkout@v4` 等）|
|Runner|真正执⾏命令的虚拟机（GitHub免费提供Linux 2核7G）|



## 12.2 ⽂件位置 

```
.github/workflows/<任意名>.yml
```

## 12.3 最⼩可运⾏⽰例：跑测试 

```
# .github/workflows/test.yml
name: Test
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
- uses: actions/checkout@v4
- name: 装Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
- name: 装pnpm
        run: npminstall-gpnpm
```

```
- name: 装依赖
        run: pnpminstall--frozen-lockfile
- name: 跑测试+覆盖率
        run: pnpmtest----coverage
- name: 上传覆盖率到Codecov
        uses: codecov/codecov-action@v4
        with:
          token: ${{secrets.CODECOV_TOKEN}}
```

## 12.4 部署到服务器：SSH + rsync 

```
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]      # 推 main 就部署
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
- uses: actions/checkout@v4
- name: 用Node20构建
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'
- run: pnpminstall--frozen-lockfile
- run: pnpmbuild
- name: 把dist/拷到服务器
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{secrets.SERVER_HOST}}
          username: ${{secrets.SERVER_USER}}
          key: ${{secrets.SERVER_SSH_KEY}}
          source: "dist/*"
          target: "/var/www/my-app"
- name: 在服务器上重启
        uses: appleboy/ssh-action@v1
        with:
          host: ${{secrets.SERVER_HOST}}
          username: ${{secrets.SERVER_USER}}
          key: ${{secrets.SERVER_SSH_KEY}}
          script: |
            cd /var/www/my-app
            pm2 restart my-app
```

**`secrets`** 怎么加：仓库 → Settings → Secrets and variables → Actions → New repository secret。每个密钥独⽴使⽤、独⽴轮换。 

## 12.5 5 个最常⽤的现成 Action 

Action 

⽤途 拉代码 

```
actions/checkout@v4
actions/setup-node@v4
```

装 Node 

Action ⽤途 `actions/setup-python@v5` 装 Python `appleboy/ssh-action@v1` 远程执⾏命令 `docker/build-push-action@v5` 构建并推送 Docker 镜像 浏览更多：https://github.com/marketplace?type=actions 

## 12.6 实战节奏 

```
push / PR
   │
   ▼
[CI] install → lint → test → build
   │
   ▼ (全绿才能合并)
合并到 main
   │
   ▼
[CD] build → deploy
```

# 14. 其他 VCS 平台对⽐：GitHub / GitLab / Gitea / GitCode / Gitee / Bitbucket / SVN / Mercurial / Perforce 

前⾯所有章节都在讲 Git + GitHub——但 "Git" 是协议 / ⼯具，"GitHub" 只是其 中⼀个托管平台。它们不是⼀回事：你可以⽤ Git 把代码托管到任何地⽅。本节⼀ 次性把常⻅的 9 个平台 / 系统摆出来对⽐，让你知道"为什么公司不让我⽤ GitHub"、"Gitee 和 GitCode 区别在哪"、"SVN 真的要被淘汰了吗"。 

## 14.1 先分清两个概念 

概念 

含义 

### 关系 

VCS（Version 协议 / ⼯具（Git、SVN、 版本控制系统本⾝ Control System） Mercurial、Perforce …） 

概念 

含义 

关系 

Forge / Code 托管代码 + 提供 PR / Issue 平台（GitHub、GitLab、 Hosting / CI 等协作能⼒的⽹站 Gitee、Gitea …） 

Git 是⼯具，GitHub 是⽹站。你可以⽤ Git 把代码托管在 GitHub、GitLab、 Gitee、企业内⽹ Gitea，或者就放在⾃⼰的笔记本硬盘上——VCS 和"放在哪"是正 交的两件事。 

## 14.2 九⼤平台 / 系统速览 

|平台/系统|类<br>型|出⾝/持有<br>⽅|部署⽅式|主要特⾊|谁在⽤|
|---|---|---|---|---|---|
|GitHub|托<br>管<br>平<br>台|美国<br>Microsoft，<br>2008|SaaS（公<br>有云）|全球最⼤开源社<br>区、Copilot、<br>Codespaces、<br>Actions|全球开发<br>者/开源<br>项⽬|
|GitLab|平<br>台+<br>⾃<br>部<br>署|美国GitLab<br>Inc.（俄裔创<br>始⼈），<br>2011|SaaS +⾃<br>部署<br>（CE/EE）|内置CI/CD⼀<br>体化、安全合规<br>最强、可离线|企业/政<br>府/⾦融/<br>想"⼀套全<br>包"的团队|
|Gitea|⾃<br>部<br>署<br>平<br>台|开源社区<br>（中国开发<br>者发起），<br>2016|⾃托管（轻<br>量Go写）|极轻量、单⼆进<br>制、UI像<br>GitHub|中⼩企业/<br>学校/个<br>⼈⾃建|
|Gogs|⾃<br>部<br>署<br>平<br>台|开源（与<br>Gitea同作者<br>早期作品）|⾃托管（更<br>轻量）|⽐Gitea还轻、<br>但功能较少|极⼩团队/<br>个⼈|
|GitCode|托<br>管<br>平<br>台|CSDN旗<br>下，2023推<br>出|SaaS（公<br>有云，国<br>内）|整合CSDN流<br>量、⾯向中⽂开<br>发者、内置AI<br>助⼿|国内个⼈/<br>⾼校/国<br>产开源|



|平台/系统|类<br>型|出⾝/持有<br>⽅|部署⽅式|主要特⾊|谁在⽤|
|---|---|---|---|---|---|
|Gitee（码云）|托<br>管<br>平<br>台|OSChina旗<br>下，2013|SaaS +企<br>业版（私有<br>化）|国内访问快、对<br>中⽂名/邮箱友<br>好、政企客⼾多|国内中⼩<br>团队/政<br>企/学⽣|
|Bitbucket|托<br>管<br>平<br>台|澳⼤利亚<br>Atlassian，<br>2008|SaaS +⾃<br>部署（DC）|和Jira /<br>Confluence原<br>⽣打通、<br>Atlassian⽣态|公司全⽤<br>Atlassian<br>⼯具栈|
|SVN<br>（Subversion）|集<br>中<br>式<br>VCS|Apache基⾦<br>会，2000|⼯具（需⾃<br>配服务端）|集中式版本控<br>制、有"全局版<br>本号"、权限细<br>到⽬录|⽼牌国企/<br>银⾏/游<br>戏客⼾端/<br>Unity⽼<br>项⽬|
|Mercurial（hg）|分<br>布<br>式<br>VCS|Matt<br>Mackall，<br>2005|⼯具（命令<br>⾏）|命令和Git极<br>像、性能稳定，<br>已被Bitbucket<br>弃⽤|⼏乎只剩<br>历史项⽬|
|Perforce / Helix<br>Core|集<br>中<br>式<br>VCS|Perforce<br>Software|商业（⾃部<br>署为主）|巨型⼆进制⽂<br>件、美术资产、<br>游戏源码同时管|3A游戏、<br>影视特<br>效、汽⻋<br>ECU、芯<br>⽚|



### 2026 年现状速记： 

- 开源 / 个⼈ → GitHub + 偶尔 Gitee / GitCode 同步 国内企业 / ⾦融 / 政府 → GitLab ⾃部署 或 Gitee 企业版 想轻量⾃建 → Gitea（Gogs 已被 Gitea 超越） 

- ⽼项⽬ / ⾏业硬性要求 → SVN 或 Perforce Mercurial：基本只剩历史包袱，新项⽬别选 

## 14.3 三⼤平台功能对⽐ 

|维度|GitHub|GitLab|Gitea|Gitee|GitCode|
|---|---|---|---|---|---|
|免费私有仓<br>库|✅|✅|✅（⾃部<br>署）|✅|✅|
|PR / MR|Pull<br>Request|Merge<br>Request|Pull<br>Request|Pull<br>Request|Pull<br>Request|
|内置CI/CD|GitHub<br>Actions|GitLab CI<br>（⼀体化）|Gitea<br>Actions|Gitea<br>Actions<br>（兼容）|内置|
|Issue /<br>Wiki|✅|✅|✅|✅|✅|
|Package<br>Registry|npm /<br>Maven /<br>Docker|npm / Maven<br>/ Docker /<br>Generic|基础|基础|基础|
|AI辅助|Copilot|Duo|⽆|⽆|CodeGeeX /<br>内置AI|
|国内访问速<br>度|❌ 慢/被墙|⚠ ⾃部署可<br>控|✅ ⾃部<br>署可控|✅ 快|✅ 快|
|价格（企<br>业）|中|中-⾼|免费（⾃<br>部署）|中|中|
|适合规模|个⼈/中⼩<br>/开源|⼤企业|中⼩团队<br>⾃建|国内中⼩/<br>政企|国内个⼈/<br>⾼校|



## 14.4 Git vs SVN：两种 VCS 的根本分歧 

这是⾯试常考题。Git 是分布式（DVCS），SVN 是集中式（CVCS）——这个根本 差异决定⼀切。 

|维度|Git|SVN|
|---|---|---|
|架构|分布式：每⼈本地⼀份完整历|集中式：所有⼈共⽤⼀个中央服务|
||史|器|
|是否能离线提<br>交|✅ 强（本地有完整历史）|❌ 必须连服务器|
|提交速度|秒级|取决于⽹络|



|维度|Git|SVN|
|---|---|---|
|分⽀|极轻量（指针+⼏个对象）|笨重（拷⼀份⽬录）|
|全局版本号|❌ 哈希号|✅ 有 `r1234` 这样的递增版本号|
|权限控制|较粗（仓库/分⽀级）|细到⽬录/⽂件级|
|⼤⽂件/⼆进<br>制|友好（Git LFS）|原⽣⽀持|
|学习曲线|中（命令多）|低（命令少）|
|当前⽣态|主流（90%+新项⽬）|维护⽼项⽬/强权限场景|



### 4 个核⼼区别⽤⼀句话说清 

1. 离线：Git 能本地提交，SVN 不能； 

2. 分⽀：Git 分⽀是"瞬移"，SVN 分⽀是"复制⽬录"； 

3. 版本号：Git 是 `a1b2c3d` 哈希，SVN 是 `r1234` 递增； 

4. 权限：SVN 能精确到"某⼈能不能改这个⽂件"，Git ⼀般只能"能不能 push 这个仓 

   - 库"。 

### 为什么 SVN 还没死 

- ⾦融 / 政府 对审计要求⾼：SVN 的"全局版本号"在合同 / 法规⾥是证据，Git 的哈 希审计起来⿇烦； 

- 游戏客⼾端：Unity ⽼项⽬、美术资产动辄⼏个 G，SVN 集中式 + ⼤⽂件原⽣⽀持 更稳； 

- 嵌⼊式 / 芯⽚：某些⼯⼚流程系统只⽀持 SVN； 

- ⽼员⼯：⼤量"会 SVN 不会 Git"的技术⻣⼲。 

## 简单记：新项⽬⼏乎 100% 选 Git；遇到"公司明⽂规定⽤ SVN"再学不迟——基础 概念 2 ⼩时能上⼿。 14.5 Git vs Mercurial vs Perforce：三⼤特殊玩 家 

|维度|Git|Mercurial（hg）|Perforce / Helix Core|
|---|---|---|---|
|架构|分布式|分布式|集中式（客⼾端轻、服务端重）|
|性能|仓库⼤时变慢|⽐Git略快|巨型仓库（百万⽂件）依然秒级|



|维度|Git|Mercurial（hg）|Perforce / Helix Core|
|---|---|---|---|
|⼤⽂件|Git LFS（外挂）|不擅⻓|原⽣+强（游戏/影视⾏业标准）|
|当前地<br>位|主流|已式微|3A游戏/影视/汽⻋⾏业事实标<br>准|
|学习曲<br>线|中|易|难（服务端运维复杂）|



### 什么时候考虑⾮ Git ⼯具？ 

- 单仓库 > 10 万个⽂件 / 总⼤⼩ > 50 GB → 考虑 Perforce； 公司历史包袱或甲⽅要求 → 选 SVN； ⼏乎找不到理由选 Mercurial（2026 年的新项⽬⽤ hg 是反主流）。 

## 14.6 国内三⼤平台怎么选：Gitee / GitCode / GitHub 

同样"国内能访问的 Git 平台"，定位完全不同——别被名字迷惑。 

|维度|Gitee（码云）|GitCode|GitHub（国际）|
|---|---|---|---|
|出⾝|OSChina（2013）|CSDN（2023）|Microsoft（2008）|
|主战场|企业/政企|个⼈/学⽣/⾼校|全球开源/个⼈开发者|
|私有仓库|免费5⼈协作|免费|免费（⽆限）|
|访问速度|✅ 国内快|✅ 国内快|❌ 慢/不稳定|
|AI助⼿|基础|CodeGeeX +国产<br>AI|GitHub Copilot（付<br>费）|
|适合"放简历作<br>品"|较弱（社区⼩）|较强（CSDN流量）|最强（全球雇主都看）|
|适合"放公司项<br>⽬"|✅ 推荐|⼀般|❌ 涉密不能⽤|
|适合"放开源作<br>品"|⼀般|⼀般|✅ 强烈推荐|



### 经验法则： 

想"被看⻅" → GitHub（开源）+ 顺便 Gitee / GitCode 镜像 

公司在⽤ → 公司选啥你⽤啥 国内个⼈项⽬ + 偶尔不想翻墙 → Gitee 或 GitCode 别把鸡蛋全放国内：万⼀平台挂了 / 改政策，你的东西就找不回来了 

## 14.7 ⾃部署 vs SaaS：什么时候⾃⼰搭⼀套？ 

|场景|推荐|理由|
|---|---|---|
|10⼈以下⼩团队/个<br>⼈|SaaS（GitHub / Gitee）|0运维、免费、⾜够⽤|
|中⼤型企业/⾦融/政<br>府|⾃部署GitLab EE或Gitee企业<br>版|数据可控、等保合规|
|想要"轻量+完全⾃主"|Gitea⾃部署|单⼆进制、1G内存就能<br>跑|
|开源/跨地域协作|GitHub公有云|⽹络与⽣态最好|
|⾃部署最低成本⽅案：<br>10⼈团队完全够⽤。|⼀台1核2G的云服务器+ Docker，|⼀⾏命令跑起Gitea，|



## 14.8 选型决策表（⼀句话版） 

|你是谁|选|
|---|---|
|个⼈开发者/学⽣|GitHub + Gitee镜像（必要时翻墙）|
|国内个⼈作品/ AI时代开发者|GitCode（流量+ AI辅助）|
|国内中⼩团队/公司项⽬|Gitee企业版 或 ⾃部署GitLab / Gitea|
|⾦融/政府/强合规|⾃部署GitLab EE|
|⼤型游戏/影视/汽⻋|Perforce（或主⽤Git +美术资源Perforce）|
|⽼项⽬维护|看历史是啥就⽤啥|
|想"私有⼜不想运维"|GitHub Enterprise Cloud|



## 14.9 平台之间怎么迁移 

常⻅场景："公司在⽤ Gitee，想迁到 GitLab"。 

```
# 1. 从 Gitee clone（bare 仓库）
git clone --bare https://gitee.com/old/repo.git
# 2. 推到新平台
cd repo.git
git push --mirror https://gitlab.com/new/repo.git
# 3. 同步所有分支 + tag + commit 历史
# （--mirror 会推送 refs/heads/*、refs/tags/*、refs/remotes/*）
```

坑：PR / Issue / Wiki / Release 不会⾃动迁移——这些是平台数据，不在 Git ⾥。需要各家平台⾃⼰的"导⼊"功能（GitHub → GitLab 有 Import tool）。 

## 14.10 ⼀句话总结 

2026 年的事实标准 = Git ⼯具 + GitHub 平台。但你⼯作后⼤概率会遇到 Gitee / GitLab / 内部 Gitea——理解它们都是"Git + 协作 UI"就够了，剩下的只是换个 URL 和按钮位置。 

# 15. monorepo 是什么：单仓多包 / 多仓多 包 / ⼯具选型 

"monorepo" 这词在前端圈出现频率极⾼——Vue 3 / React / Vite / Next.js / Babel / Tauri 等"明星项⽬"⼏乎都⻓在 monorepo ⾥。但它不是 Git 的特性，⽽是"仓库 组织⽅式"的⼀种选择。这⼀节彻底讲清"monorepo 是什么 / 解决什么 / 怎么落 地"。 

## 15.1 是什么 

monorepo（monolithic repository）= 单个仓库⾥管理多个项⽬ / 包。 

```
my-company/
├── apps/                          ← "应用"
│   ├── web/                       ← 用户端 Vue 3 项目
│   ├── admin/                     ← 后台管理系统
│   └── mobile/                    ← H5 / RN / 小程序
├── packages/                      ← "包"
│   ├── ui/                        ← 共享 UI 组件库
│   ├── utils/                     ← 共享工具函数
```

```
│   ├── eslint-config/             ← 共享 lint 配置
│   └── tsconfig/                  ← 共享 tsconfig
├── pnpm-workspace.yaml            ← 一行配置开启 monorepo
├── package.json                   ← 根 package.json
└── README.md
```

### 对⽐另⼀种"传统"做法：每个项⽬⼀个独⽴仓库，叫 multi-repo。 

```
github.com/company/web         ← 一个 repo
github.com/company/admin       ← 另一个 repo
github.com/company/ui          ← 又是另一个 repo
github.com/company/utils       ← 又是另一个 repo
```

### 关键： 

- monorepo = 多个 package 在 1 个 git 仓库⾥； multi-repo = 每个 package 独⽴⼀个 git 仓库。 VCS 选型（Git / SVN）和 monorepo / multi-repo 选型正交——任⼀组合 都可能。 

## 15.2 解决了什么问题 

### 痛点 

### monorepo 的解法 

共享代码难：A 项⽬想⽤ B 项⽬的 `utils` 在仓库内 `import '@company/utils'` → 复制粘贴 / 提私有 npm 包 直接⽤，不⽤发布 

版本不同步：A 项⽬⽤ vue 3.4、B 项⽬⽤ vue 3.5，升级要挨个改 

⼀次 `pnpm update` ，所有 package 同步 

跨项⽬改⼀个东西：要修 8 个 PR / 8 个仓 库 / 8 个 CI 

1 个 PR / 1 个 CI 

新⼈上⼿成本：要 clone N 个仓库、装 N 遍依赖 

clone 1 个、装 1 遍 

依赖图不清晰：谁依赖谁靠⼝头约定 `pnpm why` / `nx graph` ⼀键可视化 

## 15.3 monorepo vs multi-repo 对⽐ 

|维度|monorepo|multi-repo|
|---|---|---|
|仓库数|1个|N个|
|共享代码|仓库内 `import` 即可|发npm私有包/ git submodule|
|跨项⽬重<br>构|⼀个PR|N个PR同步|
|CI速度|需要增量构建（只跑改动的）|天然独⽴|
|权限控制|较粗（谁能push整个仓库）|细（每个repo独⽴）|
|仓库体积|容易膨胀（⼏个G起步）|各⾃轻量|
|Git性能|巨型仓库时 `git log` 变慢|各⾃轻快|
|学习曲线|⼯具多（pnpm workspace /<br>Turborepo / Nx）|简单|
|适合|紧密耦合的项⽬群（同⼀公司/同⼀<br>产品）|完全独⽴的项⽬/跨组织|
|典型代表|Google / Meta / Microsoft / Vue 3 /<br>React|早期Google（pre-Piper）/⼤<br>量⼩公司|



### 决策⼀句话： 

- 项⽬之间共享代码 > 30% → monorepo 划算 

- 项⽬之间完全独⽴（⼀个 2C 端、⼀个 2B 端、跨业务线） → multi-repo 更清 爽 不确定 → 从 multi-repo 起，超过 3 个项⽬需要共享代码再合并 

## 15.4 monorepo 不是银弹：3 个真实坑 

|坑|后果|缓解|
|---|---|---|
|仓库体积爆炸|clone⼀次1G+，CI拉代<br>码5分钟|配 `.gitignore` 屏蔽产物；⽤<br>sparse checkout|
|权限粒度粗|实习⽣能push整个<br>monorepo|配CODEOWNERS + GitHub分⽀保<br>护|
|CI不知道"我该<br>跑哪些"|改⼀⾏代码跑全量测试30<br>分钟|引⼊ Turborepo / Nx 做增量构建|



## 15.5 ⼯具选型矩阵 

|⼯具|作⽤|适合|备注|
|---|---|---|---|
|pnpm<br>workspace|包管理+软链接|⼏乎所有前端<br>monorepo|⼊⻔⾸选，⻅ 03第<br>3.5.4节|
|Yarn<br>workspaces|包管理+软链接|⽼项⽬/已⽤yarn|经典⽅案|
|npm<br>workspaces|包管理+软链接|想"零额外依赖"|⾃npm 7起⽀持，<br>功能最简|
|Lerna|多包发布+版本管理|⽼牌monorepo<br>⼯具|2022年后已被Nx<br>收购|
|Turborepo|增量构建+任务编排|中⼤型monorepo|Vercel出品，最推荐|
|Nx|增量构建+任务编排<br>+代码⽣成|⼤型/ Angular背<br>景团队|功能最全、配置最重|
|Rush|⼤型monorepo编排|微软系（Office /<br>Teams）|适合100+包|
|Bun<br>workspaces|包管理+软链接|想"⽤bun⼀把梭"|bun内置，⽐pnpm<br>更快但⽣态新|



## 15.6 pnpm workspace 最⼩落地：4 步⾛起来 

承接 03 第 3.5.4 节。这⾥给⼀个完整的"从 0 到跑起来"流程。 

```
# 1. 准备目录结构
mkdir my-monorepo && cd my-monorepo
mkdir -p apps/web packages/ui
# 2. 根 pnpm-workspace.yaml
cat > pnpm-workspace.yaml <<'EOF'
packages:
  - 'apps/*'
  - 'packages/*'
EOF
# 3. 根 package.json
cat > package.json <<'EOF'
{
"name": "my-monorepo",
"private": true,
```

```
"scripts": {
"dev": "pnpm -r --parallel dev"
  }
}
EOF
# 4. 在 packages/ui 里写一个共享组件（具体略）
# 然后在 apps/web/package.json 引用
# { "dependencies": { "ui": "workspace:*" } }
```

装⼀次，跑全部： 

```
pnpm install              # 装所有
pnpm --filter web dev     # 只跑 web
pnpm -r --parallel dev    # 全部并行 dev
```

## 15.7 Turborepo：把"增量构建"做到极致 

假设 monorepo 有 50 个 package，改了 1 个⽂件——理想情况是只跑这 1 个⽂件 相关的测试。这就是 Turborepo ⼲的事。 

`# 装 Turborepo pnpm add -D turbo` **`turbo.json`** （项⽬根⽬录）： 

```
{
  "$schema": "https://turbo.build/schema.json",
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**"]
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": ["coverage/**"]
    },
    "lint": {},
    "dev": { "cache": false }
  }
}
```

```
# 自动找"哪些包要重跑"
turbo run build
```

```
# 看任务图
turbo run build --graph
```

### 实战收益：CI 时间从 30 分钟 → 3 分钟。 

## 15.8 Nx：更重、更全、更适合⼤企业 

|维度|Turborepo|Nx|
|---|---|---|
|增量构建|✅ 极简|✅ 功能更多|
|受影响分析|⼿动写 `dependsOn`|⾃动推断|
|代码⽣成|⽆|✅ `nx g component xxx`|
|远程缓存|Vercel平台|Nx Cloud|
|插件⽣态|少|丰富（React / Next / Node / Express…）|
|学习曲线|低|中|
|适合|5‒50个package|50+个package /⼤型monorepo|
|选哪个？|||
|5‒20<br>20+个<br>啥也不|个包+中⼩团队 →Tu<br>包+复杂依赖图+⼤<br>想管 →pnpm work|rborepo<br>公司 →Nx<br>space裸跑（够⽤就好）|



## 15.9 monorepo × Git 平台：坑与解 

|坑|表现|解决|
|---|---|---|
|GitHub仓库体积警<br>告|单仓> 5 GB时发邮件|sparse checkout / Git LFS /拆仓<br>库|
|PR改动太⼤|monorepo⼀个PR改30<br>个package|限制范围（⽤changeset / nx<br>affected）|
|CODEOWNERS粒|⼀⼈review整个|`.github/CODEOWNERS` 按路径分|
|度|monorepo|owner|
|CI拉仓库慢|每个job都要clone整个<br>monorepo|⽤Turborepo远程缓存/只拉需<br>要的⼦⽬录|



坑 

解决 

表现 

package 之间循环 依赖 

A 依赖 B、B ⼜依赖 A ⼯具检测（ `madge` / `nx graph` ） 

## 15.10 真实世界：哪些项⽬在⽤ monorepo 

|项⽬|monorepo⼯具|规模|
|---|---|---|
|Vue 3|pnpm workspace|~30个package|
|React|Yarn workspaces|~50个package|
|Vite|pnpm workspace|~20个package|
|Next.js|Turborepo（迁移后）|数百个package|
|Babel|Yarn workspaces + Lerna|100+个package|
|Google|⾃研Piper|⼏⼗亿⾏代码|
|Meta（Facebook）|⾃研|巨型|
|Microsoft|部分monorepo + Rush|Office / Teams等|
|看到没？全世界最顶<br>司玩具"，⽽是应对|尖的开源/⼤⼚项⽬80%在<br>复杂项⽬的标准答案。|⽤monorepo——它不是"⼩公|



## 15.11 ⼀句话总结 

monorepo = 把"多个相关项⽬"放⼀个 Git 仓库⾥，配 pnpm workspace + Turborepo 落地。它解决"共享代码 / 同步升级 / 跨项⽬重构"三⼤痛点；⻔槛是⼯ 具变多、CI 要做增量。如果你项⽬数 ≥ 3、共享代码 ≥ 30%，就该上 monorepo。 

# 13. ⼩结与⾃检 

想对⽐ GitHub / GitLab / Gitee / SVN 等 9 个平台、以及搞清楚 monorepo 是什 么，⻅ 第 14 章 和 第 15 章。 

## 13.1 ⾃检清单 

|#|题⽬|关键点|
|---|---|---|
|1|解释⼯作区/暂存区/版本库三个区|三区转换图|
|2|`git reset`/`git revert` 区别|reset改历史、revert加反向<br>commit|
|3|在GitHub创建⼀个repo，本地 `git push` 推<br>上去|全流程|
|4|拉⼀个feature分⽀、写代码、提PR、合并|完整PR流程|
|5|模拟⼀个冲突并解决|`<<<<<<<` 标记|
|6|写⼀个Conventional Commit|`feat(scope):` 格式|
|7|配SSH key并 `ssh -T git@github.com` 验证|免密推送|
|8|⽤ `git reflog` 找回误删分⽀|reflog是后悔药|
|9|在仓库⾥加GitHub Actions：push时跑 `pnpm`<br>`test`|CI⼊⻔|
|10|写⼀个 `.github/ISSUE_TEMPLATE/bug.md`|Issue模板|



## 13.2 推荐练习 

1. 把本教程的 README 仓库 fork 到⾃⼰账号，clone 下来，试着提⼀个 PR 改个错别 字（真实 PR 流程体验⼀次就忘不了）； 

2. 在 GitHub 上开⼀个私⼈项⽬，模拟"主分⽀ → feature 分⽀ → PR → 合并 → 删除 分⽀"的完整流程； 

3. 给⼀个项⽬配 GitHub Actions：每次 push ⾃动跑 `pnpm lint && pnpm test && pnpm build` ； 

4. 故意制造⼀次合并冲突，体验完整解决过程。 

## 13.3 必备速查表 

### 想知道 

### 跑这个 

|当前状态|`git status`|
|---|---|
|历史（简洁）|`git log --oneline --graph`|



|想知道|跑这|个|
|---|---|---|
|历史（详细）|`git`|`log --stat`|
|某⽂件改了什么|`git`|`diff path/to/file`|
|找包含某关键字的commit|`git`|`log -S "keyword"`|
|找某⾏是谁改的|`git`|`blame path/to/file`|
|看某次commit内容|`git`|`show <hash>`|
|远程地址|`git`|`remote -v`|
|改commit信息（最近⼀次）|`git`|`commit --amend`|
|救回误删|`git`|`reflog`|



## 推荐阅读 

- Pro Git 中⽂版：https://git-scm.com/book/zh/v2 

- Learn Git Branching（交互练习）：https://learngitbranching.js.org/? locale=zh_CN 

- Conventional Commits：https://www.conventionalcommits.org/zh-hans/ GitHub Actions 官⽅⽂档：https://docs.github.com/zh/actions Oh My Git（游戏化学习）：http://ohanigit.com/ 

