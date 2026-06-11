Pandoc的命令行功能非常强大，可以用一个命令完成复杂的文档转换任务。我把它整理成了一份速查表，涵盖了最常用的命令：

### 🎯 基础用法：语法与常用命令
这是 Pandoc 最核心的命令格式和基础示例，可以快速上手。

*   **命令语法**：`pandoc [OPTIONS] [输入文件]`。其中 `[OPTIONS]` 是参数选项，`[输入文件]` 是源文件，未指定则从标准输入读取。
*   **查看帮助**：`pandoc --help` 或 `man pandoc`。
*   **查看版本**：`pandoc --version`。
*   **格式支持**：`pandoc --list-input-formats` / `--list-output-formats`。
*   **合并文件**：`pandoc 第一章.md 第二章.md -o 全文.docx`。Pandoc 会将多个输入文件按顺序合并为一个。
*   **从标准输入读取**：在终端中按 `Ctrl+D` 结束输入，Pandoc 会处理你粘贴的内容并输出为 `out.docx`。

### ⚙️ 核心参数：详解与示例
通过组合这些参数，可以精确控制转换细节与最终文档效果。

#### 📁 指定格式与输出
这些是最基本的文件操作参数。

| 参数 | 说明 |
| :--- | :--- |
| `-f FORMAT`, `--from=FORMAT` | 指定源文件格式（如 `markdown`, `html`, `docx`, `latex`）。 |
| `-t FORMAT`, `--to=FORMAT` | 指定目标文件格式（如 `docx`, `html`, `latex`, `pdf`）。 |
| `-o FILE`, `--output=FILE` | 指定输出文件名。 |

**基础示例**：
```bash
pandoc 论文.md -f markdown -t docx -o 论文.docx  # 等价于 pandoc 论文.md -o 论文.docx
```

#### 🖌️ 文档结构与样式控制
这些参数用于优化文档结构、排版和外部资源管理。

| 参数 | 说明 |
| :--- | :--- |
| `-s`, `--standalone` | 生成独立文档，确保文件包含完整头部，而非文档片段。 |
| `--toc`, `--toc-depth=NUMBER` | 生成目录，并可指定提取深度（默认为3级）。 |
| `--reference-doc=FILE` | 使用指定Word/PPTX文件作为样式模板。 |
| `--extract-media=PATH` | 将文档中的图片、音视频等媒体文件提取到指定目录。 |
| `--wrap=auto\|none\|preserve` | 控制文本换行方式。 |
| `-V KEY[=VAL]`, `--variable=KEY[:VAL]` | 设置模板变量值，在模板中使用 `$KEY$` 引用。 |
| `-M KEY[=VAL]`, `--metadata=KEY[:VAL]` | 设置文档元数据信息。 |

**进阶示例：使用样式模板**
```bash
pandoc --print-default-data-file reference.docx > 我的模板.docx  # 导出默认模板
# 然后手动修改模板中的样式库，保存后使用
pandoc 论文.md --reference-doc=我的模板.docx -o 精排版论文.docx  # 应用自定义模板
```

#### 💡 其他实用参数
除了文档结构，Pandoc 还提供了许多实用参数。

| 参数 | 说明 |
| :--- | :--- |
| `--filter=PROGRAM` | 通过外部程序处理文档的AST，实现自定义功能。 |
| `--bibliography=FILE` | 指定文献数据库文件（BibTeX等），用于文献引用。 |
| `--csl=FILE` | 指定引文格式（CSL）文件，控制参考文献的样式。 |
| `--mathml` / `--webtex` | 将LaTeX数学公式转换为MathML或通过Web服务渲染为图片。 |
| `-H FILE`, `--include-in-header=FILE` | 将指定文件内容插入到文档头部（HTML的`<head>`或LaTeX导言区）。 |
| `-B FILE`, `--include-before-body=FILE` | 将指定文件内容插入到文档正文之前。 |
| `-A FILE`, `--include-after-body=FILE` | 将指定文件内容插入到文档正文之后。 |

### 🛠️ 实战场景命令速查
以下是一些常见场景下的 Pandoc 命令示例。

| 场景 | 示例命令 |
| :--- | :--- |
| **Markdown 转 Word** | `pandoc 论文.md -o 论文.docx` |
| **Markdown 转 PDF** | `pandoc 论文.md -o 论文.pdf`（需系统安装LaTeX） |
| **HTML 转 Markdown** | `pandoc 网页.html -t markdown -o 笔记.md` |
| **Word 转 Markdown** | `pandoc 文档.docx -t markdown -o 文档.md` |
| **生成带目录的Word文档** | `pandoc 论文.md --toc --toc-depth=2 -o 论文.docx` |
| **生成带参考文献的PDF** | `pandoc 论文.md --bibliography=文献.bib --csl=apa.csl -o 论文.pdf` |
| **批量转换** | `for f in *.md; do pandoc "$f" -o "${f%.md}.docx"; done` |
| **在线HTML转Word并携带请求头** | `pandoc -f html -t docx -o article.docx --request-header User-Agent:"Mozilla/5.0" https://example.com/article` |

### 🚀 高级技巧与生态扩展
熟练掌握以下高级技巧和生态扩展，可以满足更复杂的需求。

#### 1. 利用元数据 (Metadata) 管理文档信息
你可以在 Markdown 文件开头通过 YAML 代码块定义文档信息，Pandoc 会自动应用到生成的文件中。例如：
```yaml
---
title: 论文标题
author: [作者1, 作者2]
date: 2026年6月11日
abstract: 这是论文摘要...
keywords: [关键词1, 关键词2]
---
```
#### 2. 使用过滤器 (Filters) 进行深度定制
Pandoc 先将文档解析成 JSON 格式的抽象语法树 (AST)，你可以编写脚本处理这个 JSON，实现任何修改（如特定格式转换）。
*   **运行过滤器**：`pandoc 论文.md --filter ./我的过滤器.py -o 论文.docx`。
#### 3. 利用模板 (Templates) 实现格式统一
通过 `--template` 指定自定义模板，获得完全控制输出的能力。
```bash
pandoc --print-default-template=html > 我的网页模板.html  # 导出默认HTML模板
pandoc 论文.md --template=我的网页模板.html -o 论文.html  # 使用自定义模板
```
#### 4. 与生态工具无缝集成
Pandoc 能与许多优秀的工具无缝协作，扩展性极强：
*   **文献管理：Zotero**：从 Zotero 导出 `Better BibTeX` 格式的文献库，配合 `--bibliography` 使用。
*   **学术写作：R Markdown**：R Markdown 内部集成了 Pandoc，用于生成动态报告。
*   **博客系统：Jekyll/Hugo**：许多静态网站生成器可选用 Pandoc 作为 Markdown 渲染器。
*   **笔记应用：Joplin/Typora**：这些工具利用 Pandoc 实现 Markdown 的导入导出功能。

这份速查表涵盖了最常用的场景。如果想了解某个特定场景（比如从 Markdown 生成幻灯片）的详细用法，或者对某个命令有疑问，随时可以继续提问。