## 目录

 1. Git 下载 & 安装（`Windows`）
 2. Git 全局一次性配置（用户名、邮箱）
 3. 永久把 Git 默认分支从 `master` 改为 `main`
 4. 验证所有全局配置是否生效
 5. 使用`Git Bash`将本地仓库推送到`GitHub`
 6. 将代码`push`到`GitHub`以后本地项目文件夹中项目文件的变化
 7. 日常后续修改代码、提交、推送固定流程
 8. 克隆别人`GitHub`项目到本地完整步骤
 9. 个人学习修改他人代码并上传`GitHub` 合规规范（版权 / `LICENSE` / `README`）
 10. 常见报错 `&` 警告解决方案（`LF` / `CRLF`、分支名错误、连接重置）

## 一、Git 下载与安装（`Windows`）

### 1. 下载官网安装包

官网地址：[https://git-scm.com/download/win](https://git-scm.com/download/win)

下载 **64-bit Git for Windows Setup**

### 2. 安装步骤

全程**默认下一步即可**，无需修改任何配置：

- 安装路径默认即可
- 默认编辑器选 Vim 不用改
- 调整 PATH 环境变量默认
- 换行符配置默认 **Checkout Windows-style, commit Unix-style line endings**
- 其余全部下一步，直到安装完成

### 3. 打开 Git 终端

任意文件夹空白处右键 → **Git Bash Here**，弹出黑色命令行窗口，后续所有命令都在这里执行。

## 二、Git 全局一次性配置（只做一次，终身有效）

> 作用：告诉 Git 你的身份，每次提交代码自动带上，**不用每次 push 都输**。

在 Git Bash 中依次执行两条命令，替换成你自己的信息：

```
# 配置 GitHub 用户名（自定义英文名/昵称） 
git config --global user.name "你的GitHub用户名" 

# 配置 GitHub 注册邮箱 
git config --global user.email "你的GitHub注册邮箱"
```

## 三、永久修改 `Git `默认分支：从 `master` 改成 `main`

> 作用：以后任意文件夹执行 `git init`，**默认直接是 `main` 分支**，再也不会生成`master`。

执行这条全局配置命令：

```
git config --global init.defaultBranch main
```

❗️❗️❗️这里要对**⚠️⚠️⚠️必须⚠️⚠️⚠️**将分支命名为`main`做一下解释：❗️❗️❗️

1、**`GitHub` 规定**：2020 年之后，所有新建仓库的**默认主分支统一叫 `main`**（不再是以前的 `master`）；

2、**老版本 `Git` 规定**：你在本地执行 `git init` 初始化仓库时，**默认生成的主分支叫 `master`**。
这就出现了**名字冲突**：
- 本地：`master`
- `GitHub` 远程：`main`

名字不一样，`Git` 根本不知道你要把代码推到哪个分支上，**直接推送会失败**！

## 四、验证所有全局配置是否生效

### 1. 查看全部全局配置

```
git config --global --list
```

正常能看到三行：

- `user.name=xxx`
- `user.email=xxx`
- `init.defaultbranch=main`

### 2. 测试新建仓库默认分支是不是 main

随便新建一个空文件夹，右键 Git Bash：

```
git init
```

输出提示：`Using 'main' as the name of the initial branch.`

代表当前的默认主分支为`main`，如果不是`main`，需要执行命令强制修改为`main`（详细操作见三、永久修改 `Git `默认分支：从 `master` 改成 `main`）。

### 3. 查看当前所在分支命令（随时可用）

```
git branch --show-current
```

## 五、使用`Git Bash`将本地仓库推送到`GitHub`

#### 第一步：在`GitHub`上新建项目

1. 登录 `GitHub` 官网：[https://github.com](https://github.com)

2. 右上角点击 **+** 号 → **New repository**

3. 填写配置：

- `Repository name`：填你的仓库名（英文无空格）
- `Description`：可选填项目描述
- 选 `Public`（公开仓库）
- ❌ **不勾选** `Add a README file`
- ❌ **不勾选** `Add .gitignore`
- ❌ **不勾选** `Choose a license`

4. 点击最下方 `Create repository`

5. 进入仓库页面，复制 **`HTTPS` 地址**（备用）

![[img1.png|552]]

#### 第二步：配置邮箱+用户名

##### 方式 1：命令行

首先在`Git Bash`中执行这条命令查看是否配置过邮箱+用户名

```
git config --global --list
```

如果之前没配置过，或者要提交到其他账户的`GitHub`仓库，依次在`Git Bash`中执行下面的命令

```
git config --global user.email "yourmail@XXX.com"
git config --global user.name "yourname"
```

##### 方式 2：修改config文件

打开`.git`目录下的`config`文件

![[img2.png|480]]

在文件末尾加上下面的命令并保存。

```
[user]
	name = yourname
	email = yourmail@XXX.com
```

#### 第三步：`Push`本地代码到`GitHub`

##### Tips

**注意**：其实在创建完`GitHub`项目以后，这里就会给出你相应的提示。

![[img3.png|624]]

这里主要对下面两块区域作解释：
##### 第一块：适合从零开始

```
...or create a new repository on the command line
```

**本地还没有项目，想从零开始创建仓库**的场景，命令解释：

```
# 0. 创建 README.md 文件（可省略） 
echo "# YOLO11-RGBT-zjy" >> README.md 

# 1. 初始化本地 Git 仓库 
git init 

# 2. 把文件添加到暂存区 
git add README.md 

# 3. 提交第一次代码（备注是 first commit） 
git commit -m "first commit" 

# 4. 把默认分支重命名为 main（GitHub 默认主分支名） 
git branch -M main 

# 5. 把本地仓库和远程 GitHub 仓库关联（就是上面的地址） 
git remote add origin https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git 

# 6. 把本地 main 分支推送到远程仓库 
git push -u origin main
```

##### 第二块：适合上传本地已有项目

```
...or push an existing repository from the command line
```

**本地已经有 Git 仓库，想推送到这个新建的 GitHub 仓库**的场景，命令解释：

```
# 1. 把本地仓库和远程 GitHub 仓库关联 
git remote add origin https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git 

# 2. 确保本地分支是 main 
git branch -M main 

# 3. 把本地分支推送到远程仓库 
git push -u origin main
```

如果GitHub仓库不是空仓库，需要先合并GitHub和本地仓库再push

```
# 拉取远程代码，允许合并不相关的历史 
git pull origin main --allow-unrelated-histories -m "合并远程仓库初始化内容"

# 合并完成后，再推送到远程 
git push -u origin main
```

##### 下面详细展开讲一下涉及到的两个场景
###### 场景1 ：本地已有代码文件夹，推送到 `GitHub` 空仓库

**步骤 1**：初始化本地仓库

进入到要Push的本地目录，在你的代码根目录空白处 → 右键 **Git Bash Here**

输入`git init`命令进行初始化仓库

![[img4.png]]

初始化完成后会在本地目录生成一个`.git`文件夹

![[img5.png|585]]

**步骤 2**：检查主分支是否为`main`

输入这条命来检验主分支到底是`main`还是`master`

` git branch --show-current`

因为前面已经配置过，所以这里一定会输出`main`

如果不是`main`，执行命令强制设置为`main`

```
git branch -M main
```

修改完后验证

```
git branch --show-current
```

如果输出`main`则表示修改成功。

**步骤 3**：把所有文件加入暂存区

```
git add .
```

**步骤 4**：本地第一次提交

运行

```
git commit -m "初始提交：上传完整项目代码"
```

`-m`后面是这次提交代码的说明，可以是中文，也可以是英文。

执行完这条命令后输出下面这种类似的内容就代表成功了。

![[img6.png|608]]

**步骤 5**：关联远程 `GitHub` 仓库

运行下面的命令，把后面地址换成你刚才复制的 `GitHub HTTPS` 地址：

```
git remote add origin [你的GitHub仓库HTTPS地址]
```

==验证关联是否成功==，运行

```
git remote -v
```

能看到`origin`（远程仓库的名字默认叫`origin`，不用改）后面跟着你的`GitHub`仓库`HTTPS`地址，就是绑定成功。

如果没有任何输出，就执行下面的命令进行关联

```
git remote add origin [GitHub地址]
```

==使用`HTTPS`地址关联可能会出现网络问题，因此这里推荐使用SSH进行关联==

1. 先在 GitHub 上配置 SSH 密钥（如果还没配置），参考[GitHub 官方教程](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh)。

2. 把远程仓库地址换成 SSH 格式：

```
# 先删除原来的HTTPS远程地址 
git remote remove origin 

# 添加SSH地址（替换成你的仓库SSH地址，一般是 git@github.com:FireFlyZjy/YOLO11-RGBT-zjy.git） 
git remote add origin git@github.com:FireFlyZjy/YOLO11-RGBT-zjy.git
```

测试连接：

```
ssh -T git@github.com
```

成功会显示 `Hi [你的GitHub用户名]! You've successfully authenticated...`。

**步骤 6**：第一次推送到远程并绑定分支

运行

```
git push -u origin main
```

###### 场景2 ：本地无代码，从零新建项目推送到 `GitHub`

1. 电脑新建空文件夹，进入文件夹打开 `Git Bash`

2. 依次执行：

```
# 初始化本地仓库
git init 

# 修改主分支为main
git branch -M main 

# 添加本地仓库的文件到暂存区
git add . 

# 提交项目并写说明
git commit -m "初始提交" 

# 连接远程仓库地址（推荐使用SSH）
git remote add origin [仓库HTTPS/SSH地址] 

# 将本地仓库的文件push到GitHub
git push -u origin main
```

==最后，验证是否推送成功==

a. 终端输出没有报错，最后显示类似：  

```
$ git push -u origin main
Enumerating objects: 1186, done.
Counting objects: 100% (1186/1186), done.
Delta compression using up to 12 threads
Compressing objects: 100% (814/814), done.
Writing objects: 100% (1186/1186), 7.29 MiB | 344.00 KiB/s, done.
Total 1186 (delta 358), reused 1186 (delta 358), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (358/358), done.
To github.com:FireFlyZjy/YOLO11-RGBT-zjy.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

b. 打开你的` GitHub `仓库页面 `https://github.com/FireFlyZjy/YOLO11-RGBT-zjy`，就能看到你上传的所有代码文件了。

#### 将代码`push`到`GitHub`以后本地项目文件夹中项目文件的变化

![[img7.png]]

可以看到上传项目到`GitHub`上以后，有的目录/文件颜色发生了改变

**【黄色 / 橙色的文件夹】：未被 `Git` 跟踪的文件（`Untracked`）**

- `runs`文件夹（包括里面的训练日志、模型子文件夹）显示橙色 / 黄色，说明：
    
    - 它们**没有被`.gitignore`忽略**
    - 也没有被你执行过`git add`命令，Git 还没把它们加入版本控制
    
- 这意味着：
    
    这些文件在你的电脑上存在，但 `Git` 不会自动把它们提交到 `GitHub`，除非你手动执行`git add runs/`把它们加入暂存区。

⚠️注意：

**训练日志、模型权重**这类大文件（通常几 GB），**不适合提交到 GitHub**，可以将对应目录/文件加入`.gitignore`，让它被忽略。

打开项目根目录的`.gitignore`文件（如果没有，新建一个同名文件）
加入一行配置：`runs/`


补充：`VS Code/PyCharm` 等`IDE`中其他 `Git` 状态颜色含义（以后遇到能快速识别）

![[img8.png]]

## 六、使用`Git Bash`拉取`GitHub`仓库到本地

### 场景 1：从 0 开始，空文件夹拉取 GitHub 项目（无本地代码，无覆盖风险）

步骤：

1. 创建并进入存放项目的父目录

打开终端，执行命令创建一个空的父文件夹（比如 `code`），并进入：

```
# 创建并进入 ~/zjy/code 目录（你可以改成自己的路径）
mkdir -p ~/zjy/code && cd ~/zjy/code
```

2. 执行 `git clone` 拉取项目（两种方式，二选一）

##### 方式 1：自动创建同名项目文件夹（推荐，最省事）

```
# 拉取你的项目，自动创建 YOLO11-RGBT-zjy 文件夹
git clone https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git
```

执行后，会自动生成 `YOLO11-RGBT-zjy` 文件夹，里面就是完整的项目代码。

##### 方式 2：拉取到**已存在的空文件夹**（如果你已经手动创建了项目文件夹）

```
# 先进入你手动创建的空文件夹 
cd ~/zjy/code/YOLO11-RGBT-zjy 

# 拉取代码到当前目录（注意结尾的 `.` 不能少！） 
git clone https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git .
```

3. 验证拉取成功

```
# 进入项目文件夹（如果用了方式1） 
cd YOLO11-RGBT-zjy 

# 查看项目文件 
ls
```

能看到项目文件（比如 `README.md`、代码文件夹），说明拉取成功！

### 场景 2：已有本地项目，拉取 GitHub 更新（分「不覆盖」和「覆盖」两种）

#### （一）不覆盖本地：合并更新（保留本地修改，把远程更新合并进来）

⚠️ 适用场景：你本地有修改，不想丢失，只想把 GitHub 上的新代码合并进来，不会删除你的本地文件。

步骤：

1. 进入项目目录

```
cd ~/zjy/code/YOLO11-RGBT-zjy
```

2. 检查本地修改状态

```
git status
```

如果有未提交的修改，先**暂存或提交**，避免合并冲突：

```
# 方式1：暂存修改（安全保存，后续可恢复） 
git stash 

# 方式2：提交修改（推荐，永久保存你的修改） 
git add . git commit -m "保存本地临时修改"
```

3. 拉取远程更新（自动合并）

```
# 分支名换成你的主分支（比如 main 或 master） 
git pull origin main
```

4. （可选）恢复暂存的修改（如果用了 `git stash`）

```
git stash pop
```

#### （二）覆盖本地：强制同步为远程最新（删除所有本地修改 / 文件，完全和 GitHub 一致）

⚠️ 适用场景：你本地的代码已经没用了，只想完全替换成 GitHub 上的最新版本，**所有本地修改 / 新增文件会被永久删除，不可恢复！**

步骤：

1. 进入项目目录

```
cd ~/zjy/code/YOLO11-RGBT-zjy
```

2. （可选）先绑定远程仓库（如果没绑定）

```
git remote add origin https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git
```

3. **关键！拉取远程分支信息**（解决 `origin/main` 不存在的问题）

```
git fetch origin
```

4. 强制重置本地代码为远程最新版（核心命令）

```
# 先确认远程分支名 
git ls-remote --heads origin 

# 比如分支名是 main，执行： 
git reset --hard origin/main
```

5. （可选）清理本地多余文件（删除你本地新增的、远程没有的文件）

```
# 慎用！会删除所有未被Git跟踪的文件/文件夹 
git clean -df
```

## 七、日常后续修改代码、提交、推送固定流程

==只需要 3 条命令，以后每次都这么用，不用再配置任何东西==

```
# 1. 把修改的所有文件加入暂存 
git add . 

# 2. 提交并写备注 
git commit -m "本次修改做了什么" 

# 3. 直接推送到 
GitHub git push
```

在本地修改代码经常会遇到bug，有的时候出现bug后无法回到修改前的样子，会给日常工作带来很大的麻烦，这时候就需要学会使用Git的原生功能回溯代码。

在日常修改、调试代码的过程中要养成频繁“本地提交”的习惯，不必等到整个项目完美无缺时才存档 。

### 精细化存档方案


## 八、克隆别人`GitHub`项目到本地完整步骤

1. 打开别人项目主页，复制 `HTTPS` 地址
2. 电脑找一个空文件夹，打开`Git Bash`
3. 执行：

```
git clone 别人项目的HTTPS地址
```

自动下载完整代码、保留原提交记录、默认分支为`main/master`。

## 九、个人学习修改他人代码并上传`GitHub` 合规规范（版权 / `LICENSE` / `README`）

1. **必须保留**原项目自带的 `LICENSE` 文件，**绝不删除、绝不修改内容**

2. 原项目 `README` 可以完整保留，**在 `README` 最顶部加一段溯源说明**

	```
	# 个人学习修改版
	本项目仅用于个人学习研究，基于【原项目链接】修改而来， 遵循原项目开源协议，不用于商业用途，不冒充原创。
	```

3. 不要删除原代码里的作者版权注释

4. 不用 Fork，直接下载修改上传，**只要做好上面两点，完全不侵权**

## 十、常见报错 `&` 警告解决方案

**1. 出现` LF will be replaced by CRLF `警告**

执行`git add .`命令时可能会报出这样的警告。

![[img9.png]]

**这个警告到底在说什么？**

- `LF`（`\n`）：`Linux/macOS` 系统的标准换行符
- `CRLF`（`\r\n`）：`Windows` 系统的标准换行符
- 你的项目文件里用的是 `LF`，但你现在在 Windows 系统操作，Git 会自动把 `LF` 转换成 Windows 兼容的 `CRLF`，所以弹出这个提示。

**核心结论**：这个转换是 Git 的跨平台兼容功能，是正常的，不用紧张。

**解决方法**：

保留自动转换，关闭警告。直接关闭提示，但 Git 依然会帮你处理换行符兼容，不影响仓库里的文件格式。
```
git config --global core.safecrlf false
```


**2. 报错 `remote origin already exists`**

已经关联过远程仓库，先删除旧的再重新关联：

```
git remote remove origin 
git remote add origin 新仓库地址
```

**3. 推送提示分支不匹配**

执行分支重命名再推送：

```
git branch -M main
git push -u origin main
```

**4. GitHub 连接超时、Connection was reset**

可后续改用 `SSH` 协议，或换网络；日常学习也可改用 `Gitee/GitCode`，**所有 Git 命令完全通用**。

如果提示

`error: failed to push some refs to 'https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git'

说明网络连接不稳定

执行命令检查你现在的 `origin` 还是 `HTTPS` 格式，还是 `SSH` 地址

```
git remote -v
```

如果输出如下，则表示当前使用的是 `HTTPS` 方式连接，会遇到网络问题。需要修改为 `SSH` 地址。

```
origin  https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git (fetch)
origin  https://github.com/FireFlyZjy/YOLO11-RGBT-zjy.git (push)
```

修改命令为

```
git remote set-url origin git@github.com:FireFlyZjy/YOLO11-RGBT-zjy.git
```

如果没有任何输出，代表修改完成。执行`git remote -v`验证，输出下面的命令代表修改成功。

```
origin  git@github.com:FireFlyZjy/YOLO11-RGBT-zjy.git (fetch)
origin  git@github.com:FireFlyZjy/YOLO11-RGBT-zjy.git (push)
```

## 十一、迭代与回溯

使用 Git 的原生功能来替代繁琐的压缩包下载

我们可以把日常开发想象成打游戏，**Commit 是你的常规存档点**，而 **Branch 是你的平行宇宙**。

### 1. 进阶使用 Commit（你的精细化存档系统）

前面提到，养成频繁“本地提交”的好处在于，你不必等到整个项目完美无缺时才存档 。下面是标准的操作步骤：

**1. 确认修改状态 (Status)**

在准备存档前，先看看自己目前改动了哪些东西。

Bash

```
git status
```

_这会用红色显示你修改过、但还没准备放入存档箱的文件。_

**2. 挑选要存档的文件 (Add)**

你可以把所有修改打包存档，也可以只挑一部分。

- 把所有变动放入存档箱：`git add .`
    
- 只放入特定文件（推荐，能让存档更精准）：`git add 你的文件名`
    

**3. 写好存档说明 (Commit)** 一个好的说明能让你在未来回溯时一眼看懂，而不是一头雾水 。

Bash

```
git commit -m "修复了主页面的导航栏错位问题"
```

**4. 查看时光机记录 (Log)**

当你搞砸了，想找回之前的代码时，先查看你的历史存档记录：

Bash

```
git log --oneline
```

_你会看到类似 `a1b2c3d 修复了主页面的导航栏错位问题` 的列表。前面那串字符（哈希值）就是你的“存档编号”。_

**5. 真正的“时光倒流” (Reset/Checkout)**

- **当前代码没提交，只想清空重来：** `git restore .`（丢弃所有未提交的修改，回到上一次 Commit 的状态） 。
    
- **想强行回到更早的某个历史存档（慎用，会丢失该存档之后的修改）：** `git reset --hard 你的存档编号`。
    

---

### 2. 掌握 Branch 分支（你的试错平行宇宙）

当你要开发一个容易出 bug 的新功能，或者准备进行大规模修改时，永远不要直接在主分支（`main` 或 `master`）上直接改 。

**1. 创建并穿越到新宇宙** 现代版本的 Git 推荐使用 `switch` 命令（和老版本的 `checkout -b` 效果一样 ），语义更直观：

Bash

```
git switch -c try-new-feature
```

这相当于基于你当前的代码克隆了一个一模一样的平行世界，并且你现在已经在这个新世界里了 。

**2. 在新宇宙里放飞自我** 在这个分支里，你可以随便怎么改、怎么搞出 bug 都没关系，因为主分支的代码依然被安全隔离着 。像平时一样修改代码、进行 `git add` 和 `git commit`。

**3. 试错失败（放弃修改）**

如果这个分支的代码彻底改崩了，思路行不通：

1. 直接切回主干：`git switch main`
    
2. 强制销毁那个失败的宇宙：`git branch -D try-new-feature` _一切就像没发生过一样。_
    

**4. 试错成功（合并修改）** 如果代码写好了，且测试没问题 ：

1. 先回到主干：`git switch main`
    
2. 把新宇宙的成果吸收合并过来：`git merge try-new-feature`
    
3. 此时新功能已经加入主干，你可以安全地把试验分支删除了：`git branch -d try-new-feature`


