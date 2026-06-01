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