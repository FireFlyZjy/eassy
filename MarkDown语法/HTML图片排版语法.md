<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. 基础 Markdown 语法](#1-%E5%9F%BA%E7%A1%80-markdown-%E8%AF%AD%E6%B3%95)
- [2. 基础 HTML 语法（控制尺寸）](#2-%E5%9F%BA%E7%A1%80-html-%E8%AF%AD%E6%B3%95%E6%8E%A7%E5%88%B6%E5%B0%BA%E5%AF%B8)
- [3. 图片居中对齐](#3-%E5%9B%BE%E7%89%87%E5%B1%85%E4%B8%AD%E5%AF%B9%E9%BD%90)
- [4. 多张图片并排显示](#4-%E5%A4%9A%E5%BC%A0%E5%9B%BE%E7%89%87%E5%B9%B6%E6%8E%92%E6%98%BE%E7%A4%BA)
- [5. 图片带底部标题/注释](#5-%E5%9B%BE%E7%89%87%E5%B8%A6%E5%BA%95%E9%83%A8%E6%A0%87%E9%A2%98%E6%B3%A8%E9%87%8A)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### 1. 基础 Markdown 语法

最标准、最基础的写法。支持所有平台，但无法控制尺寸和位置。

Markdown

```
![图片描述文字](images/图片名称.png)
```

### 2. 基础 HTML 语法（控制尺寸）

使用 HTML 标签，可以完美控制图片的宽度（`width`），Obsidian 和 GitHub 均支持。

HTML

```
<img src="images/图片名称.png" width="600" alt="图片描述文字">
```

### 3. 图片居中对齐

使用 `<div>` 标签包裹图片并设置居中属性。

HTML

```
<div align="center">
  <img src="images/图片名称.png" width="600">
</div>
```

### 4. 多张图片并排显示

将多张图片放在同一个居中的 `<div>` 中，并使用百分比控制宽度，防止图片过大换行。

HTML

```
<div align="center">
  <img src="images/图片1.png" width="45%">
  <img src="images/图片2.png" width="45%">
</div>
```

### 5. 图片带底部标题/注释

使用 `<figure>` 和 `<figcaption>` 标签，让图片看起来像专业文档中的插图。

HTML

```
<figure align="center">
  <img src="images/图片名称.png" width="600">
  <figcaption>这里填写你的图片标题或注释</figcaption>
</figure>
```