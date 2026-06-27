<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [一、简介](#%E4%B8%80%E7%AE%80%E4%BB%8B)
- [二、在线使用（KIE 模块）](#%E4%BA%8C%E5%9C%A8%E7%BA%BF%E4%BD%BF%E7%94%A8kie-%E6%A8%A1%E5%9D%97)
  - [2.1 访问与登录](#21-%E8%AE%BF%E9%97%AE%E4%B8%8E%E7%99%BB%E5%BD%95)
  - [2.2 界面概览](#22-%E7%95%8C%E9%9D%A2%E6%A6%82%E8%A7%88)
  - [2.3 新建流程](#23-%E6%96%B0%E5%BB%BA%E6%B5%81%E7%A8%8B)
  - [2.4 上传文档](#24-%E4%B8%8A%E4%BC%A0%E6%96%87%E6%A1%A3)
  - [2.5 解析 (Parse)](#25-%E8%A7%A3%E6%9E%90-parse)
- [三、API 使用](#%E4%B8%89api-%E4%BD%BF%E7%94%A8)
  - [3.1 精准解析 API](#31-%E7%B2%BE%E5%87%86%E8%A7%A3%E6%9E%90-api)
    - [单个文件解析](#%E5%8D%95%E4%B8%AA%E6%96%87%E4%BB%B6%E8%A7%A3%E6%9E%90)
    - [批量文件解析](#%E6%89%B9%E9%87%8F%E6%96%87%E4%BB%B6%E8%A7%A3%E6%9E%90)
  - [3.2 Agent 轻量解析 API](#32-agent-%E8%BD%BB%E9%87%8F%E8%A7%A3%E6%9E%90-api)
- [四、私有化部署](#%E5%9B%9B%E7%A7%81%E6%9C%89%E5%8C%96%E9%83%A8%E7%BD%B2)
  - [4.1 硬件配置建议](#41-%E7%A1%AC%E4%BB%B6%E9%85%8D%E7%BD%AE%E5%BB%BA%E8%AE%AE)
  - [4.2 软件依赖](#42-%E8%BD%AF%E4%BB%B6%E4%BE%9D%E8%B5%96)
  - [4.3 安装步骤](#43-%E5%AE%89%E8%A3%85%E6%AD%A5%E9%AA%A4)
  - [4.4 启动服务](#44-%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1)
- [五、性能优化建议](#%E4%BA%94%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E5%BB%BA%E8%AE%AE)
- [六、常见问题](#%E5%85%AD%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
  - [Q1：镜像拉取失败？](#q1%E9%95%9C%E5%83%8F%E6%8B%89%E5%8F%96%E5%A4%B1%E8%B4%A5)
  - [Q2：CUDA 版本不兼容？](#q2cuda-%E7%89%88%E6%9C%AC%E4%B8%8D%E5%85%BC%E5%AE%B9)
  - [Q3：解析结果不准确？](#q3%E8%A7%A3%E6%9E%90%E7%BB%93%E6%9E%9C%E4%B8%8D%E5%87%86%E7%A1%AE)
  - [Q4：文件上传失败？](#q4%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%A4%B1%E8%B4%A5)
  - [Q5：国外 URL 请求超时？](#q5%E5%9B%BD%E5%A4%96-url-%E8%AF%B7%E6%B1%82%E8%B6%85%E6%97%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 一、简介

MinerU 是一款开源的文档解析工具，能够将 PDF、图片、Office 文档等非结构化文件转换为机器可读的结构化格式（如 Markdown、JSON）。它能够智能识别并处理各类复杂版式、多模态内容（如表格、数学公式、图表、图片、多栏布局等），适用于 RAG、智能体等下游 LLM 应用场景。

MinerU 提供两种文档解析 API，满足不同场景需求：

| 对比维度 | 精准解析 API | Agent 轻量解析 API |
|---|---|---|
| 是否需要 Token | ✅ 需要 | ❌ 无需（IP 限频） |
| 接口地址 | `/api/v4/extract/task` 或 `/api/v4/file-urls/batch` | `/api/v1/agent/parse/url` 或 `/api/v1/agent/parse/file` |
| 模型版本 | pipeline（默认）/ vlm（推荐）/ MinerU-HTML | 固定 pipeline 轻量模型 |
| 文件大小限制 | ≤ 200MB | ≤ 10MB |
| 页数限制 | ≤ 600 页 | ≤ 20 页 |
| 批量支持 | ✅ 支持（≤ 200 个） | ❌ 单文件 |
| 输出格式 | Zip 包，含 Markdown、JSON，可导出为 docx/html/latex | 仅 Markdown（CDN 链接） |

## 二、在线使用（KIE 模块）

### 2.1 访问与登录

登录 MinerU 主站后，进入“在线使用”页面，在顶部找到“智能抽取火爆内测中”卡片，点击 **立即体验** 即可进入文档智能抽取独立页面。也可以在侧边栏底部找到工具箱，点开即可看到“文档智能抽取”模块。

文档智能抽取与 MinerU 共用同一账户体系，无需重复登录。

### 2.2 界面概览

- **底部导航**：用户头像、API 文档、使用指引、平台工具箱
- **左侧面板**：展示“我的流程”列表
- **右侧工作区**：创建新流程的核心入口及流程配置区域

### 2.3 新建流程

首次进入时，点击页面顶部或中央的 **“开始流程”** 按钮，系统将引导进入流程创建向导页面。

在新建流程页面，您将看到三个核心处理节点：

- **解析**：将非结构化文档转换为机器可读的结构化格式
- **分割**：对长文档进行内容分割
- **抽取**：从文档中提取关键字段

**新手建议**：
- 初次体验：选择“解析 → 抽取”（点击“抽取”卡片）
- 文档较长且内容多样：选择“解析 → 分割 → 抽取”（依次点击三个卡片）

完成配置后点击确认按钮，系统将自动创建流程并跳转到配置界面。

### 2.4 上传文档

**支持格式**：PDF、JPG、PNG

**文件限制**：
- 单文件最大 10MB
- PDF 最多 10 页
- 批量上传最多 10 个文件

**上传方式**：
- 直接上传：点击上传区域，拖拽或选择文件
- 批量处理：可同时上传多个相关文件，系统按顺序处理，支持在任务运行中随时添加新文件

**状态反馈**：
- 上传中：显示进度条和百分比
- 上传成功：绿色对勾标识
- 格式错误/大小超限：红色感叹号提示

### 2.5 解析 (Parse)

解析模块是流程的基础步骤，核心功能包括：
- 识别文本、表格、图片
- 分析版面结构
- 输出带位置信息的结构化数据

**处理流程**：文档上传 → 版面分析 → 元素识别 → 结构组装 → 结果输出

**默认配置**：解析所有页面，标准 OCR 精度，包含所有元素类型。**输出格式**：Markdown 和 JSON。

点击“运行”按钮即可开始解析，完成后可查看结果。

## 三、API 使用

### 3.1 精准解析 API

需先申请 Token。

#### 单个文件解析

**接口说明**：适用于通过 API 创建解析任务的场景。

**注意事项**：
- 单个文件大小不超过 200MB，页数不超过 600 页
- 每个账号每天享有 2000 页最高优先级解析额度，超过部分优先级降低
- 因网络限制，GitHub、AWS 等国外 URL 可能请求超时
- 该接口不支持文件直接上传
- Header 中需包含 `Authorization` 字段，格式为 `Bearer + 空格 + Token`

**Python 请求示例（适用于 PDF、Doc、PPT、图片文件）**：

```python
import requests

token = "官网申请的api token"
url = "https://mineru.net/api/v4/extract/task"
header = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {token}"
}
data = {
    "url": "https://cdn-mineru.openxlab.org.cn/demo/example.pdf",
    "model_version": "vlm"
}
res = requests.post(url, headers=header, json=data)
print(res.status_code)
print(res.json())
print(res.json()["data"])
```

**Python 请求示例（适用于 HTML 文件）**：

```python
import requests

token = "官网申请的api token"
url = "https://mineru.net/api/v4/extract/task"
header = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {token}"
}
data = {
    "url": "https://****",
    "model_version": "MinerU-HTML"
}
res = requests.post(url, headers=header, json=data)
print(res.status_code)
```

#### 批量文件解析

精准解析 API 支持批量处理，最多支持 200 个文件。接口地址为 `/api/v4/file-urls/batch`。

### 3.2 Agent 轻量解析 API

无需登录，采用 IP 限频防止滥用，专为 AI Agent 工作流设计。

**接口地址**：
- `/api/v1/agent/parse/url`（通过 URL 提交）
- `/api/v1/agent/parse/file`（通过文件提交）

**限制**：单文件 ≤ 10MB，≤ 20 页。输出格式仅为 Markdown（CDN 链接）。

## 四、私有化部署

### 4.1 硬件配置建议

对于中小型企业，建议采用以下配置：
- **CPU**：4 核以上（支持多线程解析）
- **内存**：16GB 及以上（处理大文档时需更高内存）
- **存储**：SSD 固态硬盘（提升 I/O 性能）
- **GPU**（可选）：NVIDIA 显卡（加速 OCR 识别）

### 4.2 软件依赖

MinerU 支持 Windows、Linux 和 macOS 三大主流操作系统。推荐使用 Python 3.10-3.13 版本。

**核心依赖**：
- 操作系统：Ubuntu 20.04/CentOS 7+（推荐 Linux 环境）
- Python 环境：Python 3.8+
- 关键库：opencv-python、pytesseract、pdf2image
- OCR 引擎：Tesseract 5.0+（需单独安装）

**避坑指南**：避免使用 Python 3.11+，因部分依赖库可能存在兼容性问题。

### 4.3 安装步骤

**1. 获取代码**：

```bash
git clone https://github.com/dsr-lab/MinerU.git
cd MinerU
git checkout v1.2.0  # 推荐使用LTS版本
```

**2. 配置 Python 虚拟环境**：

```bash
# 使用 conda（推荐）
conda create -n mineru_env python=3.10
conda activate mineru_env

# 或使用 venv
python -m venv mineru_env
source mineru_env/bin/activate  # Linux/Mac
# mineru_env\Scripts\activate  # Windows
```

**3. 安装依赖**：

```bash
pip install -r requirements.txt
```

**4. 安装 Tesseract OCR**：

```bash
# Ubuntu示例
sudo apt update
sudo apt install tesseract-ocr libtesseract-dev
```

**5. GPU 加速（可选）**：

需确保 CUDA 版本 ≥ 12.1，并安装对应版本的 PyTorch：

```bash
pip install torch==2.3.1 torchvision==0.18.1
```

**6. 下载模型**：

```bash
# 使用 ModelScope 源下载（国内网络更优）
export MINERU_MODEL_SOURCE=modelscope
mineru-models-download
```

该过程会生成 `magic-pdf.json` 配置文件，关键参数包括：
- `models-dir`：指向模型存储路径
- `device-mode`：设置为 `"cuda"` 以启用 GPU 加速
- `formula-config`：控制公式识别模块的启用状态

### 4.4 启动服务

**方式一：命令行直接运行**（适用于快速验证）：

```bash
mineru -p input.pdf -o output_dir \
  --formula-enable true \
  --table-enable true \
  --lang ch
```

**方式二：FastAPI 服务化部署**（生产环境推荐）：

```bash
mineru-api --host 0.0.0.0 --port 8000
```

服务启动后，可通过 `http://<服务器IP>:8000/docs` 访问 Swagger 界面，实时测试 `/file_parse` 端点。该接口支持批量文件上传，单次请求最多可处理 20 个 PDF 文件。

**方式三：Docker 容器化部署**（环境隔离）：

```bash
docker build -t mineru-sglang:latest .
docker run --gpus all \
  --shm-size 32g \
  -p 30000:30000 \
  mineru-sglang:latest
```

该配置启用了 GPU 加速，并设置 32GB 共享内存，可稳定处理超长文档。

## 五、性能优化建议

1. **批处理配置**：批量处理时建议每组文件数量控制在 10-20 个，避免内存溢出
2. **超时设置**：复杂文档解析需调整 `--timeout` 参数，默认 120 秒可覆盖 90% 的文档类型
3. **资源分配**：GPU 环境建议设置 `--vram 8000` 限制显存占用，防止单个进程占用全部资源
4. **批处理大小**：调整 `batch_size` 参数（默认 8，建议测试 16-32）
5. **异步处理**：启用 `--async-mode` 实现请求队列管理

## 六、常见问题

### Q1：镜像拉取失败？
建议配置国内镜像源加速，或在 `/etc/docker/daemon.json` 中添加国内镜像源配置。

### Q2：CUDA 版本不兼容？
确保 CUDA 版本 ≥ 12.1。如遇兼容性问题，可参考版本兼容矩阵进行驱动升级或降级。

### Q3：解析结果不准确？
可尝试切换模型版本：`pipeline`（默认）、`vlm`（推荐）或 `MinerU-HTML`。对于复杂文档，`vlm` 模型通常效果更好。

### Q4：文件上传失败？
检查文件大小和页数是否超出限制。精准解析 API 单文件 ≤ 200MB、≤ 600 页；Agent 轻量 API 单文件 ≤ 10MB、≤ 20 页。

### Q5：国外 URL 请求超时？
因网络限制，GitHub、AWS 等国外 URL 可能请求超时。建议将文件上传至国内可访问的存储服务后使用 URL 提交。