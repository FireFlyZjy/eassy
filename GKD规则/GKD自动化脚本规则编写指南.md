<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. 整体结构 (App Structure)](#1-%E6%95%B4%E4%BD%93%E7%BB%93%E6%9E%84-app-structure)
- [2. 规则组核心属性 (Group Properties)](#2-%E8%A7%84%E5%88%99%E7%BB%84%E6%A0%B8%E5%BF%83%E5%B1%9E%E6%80%A7-group-properties)
- [3. 核心语法：选择器 (Selectors)](#3-%E6%A0%B8%E5%BF%83%E8%AF%AD%E6%B3%95%E9%80%89%E6%8B%A9%E5%99%A8-selectors)
  - [3.1 属性匹配 (Attribute Matching)](#31-%E5%B1%9E%E6%80%A7%E5%8C%B9%E9%85%8D-attribute-matching)
  - [3.2 层级与兄弟关系 (Combinators)](#32-%E5%B1%82%E7%BA%A7%E4%B8%8E%E5%85%84%E5%BC%9F%E5%85%B3%E7%B3%BB-combinators)
- [4. 动作逻辑与目标定位 (Action & Targeting)](#4-%E5%8A%A8%E4%BD%9C%E9%80%BB%E8%BE%91%E4%B8%8E%E7%9B%AE%E6%A0%87%E5%AE%9A%E4%BD%8D-action--targeting)
  - [4.1 目标指示符 `@` (Target Modifier)](#41-%E7%9B%AE%E6%A0%87%E6%8C%87%E7%A4%BA%E7%AC%A6--target-modifier)
  - [4.2 多步连贯操作 (PreKeys)](#42-%E5%A4%9A%E6%AD%A5%E8%BF%9E%E8%B4%AF%E6%93%8D%E4%BD%9C-prekeys)
- [5. 经典实战实例 (Practical Examples)](#5-%E7%BB%8F%E5%85%B8%E5%AE%9E%E6%88%98%E5%AE%9E%E4%BE%8B-practical-examples)
  - [实例 1：标准开屏跳过 (利用 text)](#%E5%AE%9E%E4%BE%8B-1%E6%A0%87%E5%87%86%E5%BC%80%E5%B1%8F%E8%B7%B3%E8%BF%87-%E5%88%A9%E7%94%A8-text)
  - [实例 2：利用特征元素定位关闭按钮 (利用兄弟关系与 `@`)](#%E5%AE%9E%E4%BE%8B-2%E5%88%A9%E7%94%A8%E7%89%B9%E5%BE%81%E5%85%83%E7%B4%A0%E5%AE%9A%E4%BD%8D%E5%85%B3%E9%97%AD%E6%8C%89%E9%92%AE-%E5%88%A9%E7%94%A8%E5%85%84%E5%BC%9F%E5%85%B3%E7%B3%BB%E4%B8%8E-)
  - [实例 3：复杂的层级推断](#%E5%AE%9E%E4%BE%8B-3%E5%A4%8D%E6%9D%82%E7%9A%84%E5%B1%82%E7%BA%A7%E6%8E%A8%E6%96%AD)
  - [给 Agent 的行动指南：](#%E7%BB%99-agent-%E7%9A%84%E8%A1%8C%E5%8A%A8%E6%8C%87%E5%8D%97)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


GKD 规则基于 JSON (或 JSON5) 格式，其核心思想是：**定位应用包名 -> 指定生效页面 (Activity) -> 通过层级与属性选择器匹配节点 -> 执行点击或其他操作。**

## 1. 整体结构 (App Structure)

每个应用的规则定义在一个独立的对象中，最外层包含应用的基础信息和规则组（`groups`）。

代码段

```
{
    "id": "com.example.app", // 必须：应用的唯一包名 (Package Name)
    "name": "示例应用",        // 可选：应用名称
    "groups": [              // 必须：规则组数组，按场景分类
        // ... 具体的规则组
    ]
}
```

## 2. 规则组核心属性 (Group Properties)

每个规则组（Group）代表一个特定的场景（如：开屏广告、首页弹窗）。

代码段

```
{
    "key": 1,                           // 必须：当前应用的唯一标识整数
    "name": "开屏广告",                 // 必须：规则组的名称描述
    "activityIds": "com.example.MainActivity", // 可选：限制规则生效的 Activity 页面。不填则全局生效
    "fastQuery": true,                  // 可选：布尔值。若节点具有 id/vid/text 等明确属性，开启可大幅提升查询速度
    "actionMaximum": 1,                 // 可选：限制该组规则在全局/特定周期内最多触发的次数
    "resetMatch": "app",                // 可选：配合 actionMaximum 使用，重置次数的周期（如 app 启动时重置）
    "rules": [                          // 必须：具体的匹配逻辑和动作数组
        // ... 具体的匹配规则
    ]
}
```

## 3. 核心语法：选择器 (Selectors)

这是 GKD 规则的灵魂，类似于 CSS 选择器，用于在复杂的 UI 树中精准定位目标节点。

### 3.1 属性匹配 (Attribute Matching)

使用 `[属性名=属性值]` 的格式。常用的节点属性包括 `text`, `desc`, `vid` (Resource ID 的后半部分), `id` (完整 Resource ID), `clickable`, `visibleToUser` 等。

- **完全匹配:** `[text="跳过"]` (匹配文本完全为“跳过”的节点)
    
- **包含匹配 (`*=`):** `[text*="跳过"]` (匹配文本包含“跳过”的节点，如“跳过广告”)
    
- **前缀匹配 (`^=`):** `[desc^="dislike"]` (匹配 desc 以 dislike 开头的节点)
    
- **布尔值匹配:** `[clickable=true]` (匹配可点击的节点)
    
- **多属性组合:** `[text="跳过"][clickable=true]` (匹配既是“跳过”又可点击的节点)
    

### 3.2 层级与兄弟关系 (Combinators)

用于描述节点在 UI 树中的相互位置关系。

- `>` : **直接子节点**。例如 `ViewGroup > ImageView` (查找父节点为 ViewGroup 的 ImageView)。
    
- `<` : **直接父节点**。例如 `TextView[text="广告"] < ViewGroup` (查找包含“广告”文本的父容器)。
    
- `+` : **下一个兄弟节点**。
    
- `-` : **上一个兄弟节点**。
    
- `>n` 或 `<n` : **指定层级的祖先/子孙**。例如 `>2` 表示往下两层。
    
- `+n` 或 `-n` : **后续/前面的任意兄弟节点**。
    

## 4. 动作逻辑与目标定位 (Action & Targeting)

### 4.1 目标指示符 `@` (Target Modifier)

**这是极其重要的一点！** 当你使用复杂的层级选择器时，表达式匹配的是整个路径，你需要用 `@` 告诉 GKD 最终执行点击动作的是哪一个具体节点。

**实例对比：**

- **场景：** “广告”字样本身不可点击，需要点击它旁边的“关闭图标 (ImageView)”。
    
- **错误写法：** `ImageView - TextView[text="广告"]` （这会默认点击整个表达式最外层的节点，或者导致逻辑混乱）。
    
- **正确写法：** `@ImageView - TextView[text="广告"]` （明确指示：寻找一个前面是 ImageView 的 “广告”文本节点，但**点击动作落在 ImageView 上**）。
    

### 4.2 多步连贯操作 (PreKeys)

遇到需要分两步关闭的广告（如：先点击“不感兴趣”，再点击“关闭”），使用 `preKeys` 将规则串联。

代码段

```
"rules": [
    {
        "key": 0,
        "name": "第一步：点击不感兴趣",
        "matches": "TextView[text=\"不感兴趣\"]"
    },
    {
        "key": 1,
        "preKeys": [0], // 声明此规则必须在 key=0 的规则触发后才执行
        "name": "第二步：点击确认关闭",
        "matches": "TextView[text=\"确认关闭\"]"
    }
]
```

## 5. 经典实战实例 (Practical Examples)

阅读以下实例，体会如何通过节点特征寻找突破口：

### 实例 1：标准开屏跳过 (利用 text)

最简单的场景，直接匹配特定文字且可点击的节点。

代码段

```
{
    "matches": "@[clickable=true] > [text=\"跳过\"]"
    // 解释：找到文本为“跳过”的节点，并点击它具备可点击属性(clickable=true)的直接父节点。
    // 这在很多应用中很常见，因为文字本身可能不可点，而它的父级按钮容器可点。
}
```

### 实例 2：利用特征元素定位关闭按钮 (利用兄弟关系与 `@`)

遇到没有明确 ID，且关闭按钮只是一个没有任何 text 的 `ImageView` 的卡片广告。

代码段

```
{
    "matches": "@* - [vid=\"native_express_ad_logo_tv\"]"
    // 解释：目标应用广告卡片底部有一个特定的标志节点 vid 为 native_express_ad_logo_tv。
    // 规则寻找这个标志节点的上一个兄弟节点（通常就是那个叉号图标），并点击它（@*）。
}
```

### 实例 3：复杂的层级推断

遇到 WiFi 列表里的穿插广告，广告标签和关闭按钮在不同的容器层级里。

代码段

```
{
    "matches": "@Image[text=\"\"] < View +n View > View > TextView[text=\"广告\"]"
    // 解释：
    // 1. 先定位到带有“广告”字样的 TextView：TextView[text="广告"]
    // 2. 向上找两层父级：> View > TextView... 的逆向思考，意思是它的爷爷节点
    // 3. 找爷爷节点前面的某个兄弟节点：< View +n View...
    // 4. 在那个兄弟节点内部找一个没有任何文字的 Image 节点并点击它：@Image[text=""]
    // （这类规则通常通过 GKD 的界面审查器一键生成层级关系，然后手动删减优化而成）
}
```

### 给 Agent 的行动指南：

当你接收到用户发来的**应用快照 (Snapshot)** 或节点树描述时：

1. **寻找唯一性：** 优先寻找目标关闭按钮是否有独一无二的 `vid` 或 `id`。如果有，直接 `[vid="xxx"]`，这最稳定。
    
2. **文本锚点：** 如果关闭按钮没有特征，寻找它附近是否有带明确文本的节点（如“广告”、“跳过”、“Ad”），通过兄弟 `+`/`-` 或父子 `<`/`>` 关系倒推过去。
    
3. **加 `@` 标记：** 永远记住在推导出的层级关系中，用 `@` 标记最终要被点击的那个节点。
    
4. **属性过滤：** 适当加上 `[clickable=true]` 或 `[visibleToUser=true]` 以防止误触后台隐藏节点。