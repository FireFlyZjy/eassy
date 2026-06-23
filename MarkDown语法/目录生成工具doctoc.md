<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [🚀 快速上手：生成目录](#-%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B%E7%94%9F%E6%88%90%E7%9B%AE%E5%BD%95)
- [⚙️ 常用选项与参数](#-%E5%B8%B8%E7%94%A8%E9%80%89%E9%A1%B9%E4%B8%8E%E5%8F%82%E6%95%B0)
- [详细示例](#%E8%AF%A6%E7%BB%86%E7%A4%BA%E4%BE%8B)
- [⚠️ 常见问题](#-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
- [💎 总结](#-%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

`doctoc` 能帮你在 Markdown 文件的标题处自动生成并维护一个目录（Table of Contents, TOC）。它的命令结构如下：

```bash
doctoc <文件或目录路径> [选项]
```

下面是它最常用的几种用法。

### 🚀 快速上手：生成目录

**1. 为单个文件生成目录**

```bash
doctoc README.md
```
目录会默认插入在文件的最顶部。
> **注意**：`doctoc` 是通过 `<!-- START doctoc -->` 和 `<!-- END doctoc -->` 这两个注释标签来定位并更新目录的，所以不要改动它们，也不要手动修改二者之间的内容。

**2. 为当前文件夹及子文件夹下所有 Markdown 文件生成目录**

```bash
doctoc .
```
该命令会递归地更新当前目录下所有的 `.md` 文件。如果想**忽略**某个文件，可以在该文件的最顶部加入 `<!-- DOCTOC SKIP -->` 作为标记。

**3. 自定义目录位置**

目录的默认位置是在文件开头。如果需要自定义，可以在 Markdown 文件中你想放置目录的位置，手动加上这两个注释标签，之后再运行 `doctoc` 命令，目录就会生成在那里了。

### ⚙️ 常用选项与参数

`doctoc` 提供了很多选项，方便你生成更符合需求的目录。

| 功能           | 参数示例                                | 作用                                                       |
| :----------- | :---------------------------------- | :------------------------------------------------------- |
| **自定义标题**    | `doctoc --title '**目录**' README.md` | 将默认的 `**Table of Contents**` 标题改为 `**目录**` (Markdown 语法) |
| **隐藏标题**     | `doctoc --notitle README.md`        | 不显示目录标题，只展示链接列表                                          |
| **控制目录层级**   | `doctoc --maxlevel 3 README.md`     | 目录只包含 1 到 3 级标题，默认没有上限                                   |
| **指定平台兼容模式** | `doctoc README.md --bitbucket`      | 生成兼容 Bitbucket 的锚点链接                                     |
| **仅更新现有目录**  | `doctoc --update-only README.md`    | 只更新已有目录的文件，不添加新目录                                        |
| **打印到标准输出**  | `doctoc README.md --stdout`         | 将结果打印到终端而不修改原文件                                          |

### 详细示例

**自定义标题**

在Windows下使用双引号，MacOS和Linux下使用单引号。

```bash
doctoc --title "# 目录" .          # 一级标题
doctoc --title "**内容导航**" .    # 加粗文本（无标题级别）
doctoc --notitle .                 # 无标题
```

### ⚠️ 常见问题

在你的 Windows 环境下如果遇到权限错误，可以这样解决：
以**管理员身份**打开 PowerShell，运行 `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`，根据提示选择 `Y`（是）即可。

### 💎 总结

简单来说，`doctoc` 能**自动化**地帮你解决 Markdown 目录的生成和同步问题。它的核心工作流就是：先在你需要的位置（通常是文件开头）放好定位注释标签，然后运行 `doctoc <文件名>` 命令。之后，当你修改了标题，只需要再次运行同样的命令，`doctoc` 就会为你自动更新目录了。