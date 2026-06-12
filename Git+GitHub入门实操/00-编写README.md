<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [问题场景](#%E9%97%AE%E9%A2%98%E5%9C%BA%E6%99%AF)
- [核心语法](#%E6%A0%B8%E5%BF%83%E8%AF%AD%E6%B3%95)
- [实际应用示例](#%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8%E7%A4%BA%E4%BE%8B)
- [关键注意事项](#%E5%85%B3%E9%94%AE%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 问题场景

在 GitHub 仓库的根目录 `README.md` 中，希望创建一个可以点击并跳转到项目中其他子目录内 Markdown 文件的目录结构。

## 核心语法

在 Markdown 中，实现点击跳转需要结合**超链接语法**与**相对路径**，标准格式如下：

Markdown

```
[你想显示的文字](文件的相对路径)
```

## 实际应用示例

假设项目根目录下有 `README.md`，同级有 `Git+GitHub入门实操` 和 `MarkDown语法` 两个文件夹，存放了具体的文档。

在 `README.md` 中，对应的目录代码应如下编写：

Markdown

```
# 目录

## Git+GitHub从零入门实操手册

* [00-准备篇](Git+GitHub入门实操/00-准备篇.md)
* [01-推送篇](Git+GitHub入门实操/01-推送篇.md)
* [02-拉取篇](Git+GitHub入门实操/02-拉取篇.md)
* [03-简单推送与拉取](Git+GitHub入门实操/03-简单推送与拉取.md)
* [Git 与 GitHub 项目重命名指南](Git+GitHub入门实操/Git 与 GitHub 项目重命名指南.md)
* [修改他人开放源码注意事项](Git+GitHub入门实操/修改他人开放源码注意事项.md)
* [常见问题及解决方案](Git+GitHub入门实操/常见问题及解决方案.md)
* [版本迭代与回溯](Git+GitHub入门实操/版本迭代与回溯.md)

## MarkDown语法

* [HTML图片排版语法](MarkDown语法/HTML图片排版语法.md)
```

## 关键注意事项

- **使用相对路径**：因为 `README.md` 位于根目录，指向子目录文件时，直接写 `文件夹名/文件名.md` 即可。GitHub 会自动识别并跳转到该文件。
    
- **必须保留文件后缀**：路径中一定要带上 `.md` 后缀名。否则 GitHub 会把它当作没有后缀的文件去寻找，导致出现 404 错误。
    
- **兼容空格与特殊字符**：即使文件名中包含空格（如 `Git 与 GitHub 项目重命名指南.md`）或特殊字符（如加号 `+`），GitHub 的 Markdown 渲染器也能智能识别，直接在路径中原样填写即可。