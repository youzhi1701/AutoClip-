<p align="center">
  <img src="src-tauri/icons/128x128.png" alt="AutoClip" width="80" height="80">
</p>

# AutoClip · 二次开发版

> 基于 AutoClip 的二次开发仓库：输入视频或链接，自动完成内容分析、高光筛选、字幕、画面适配、封面与发布文案生成。

| 项目 | 信息 |
| --- | --- |
| 当前代码版本 | **v1.5.6** |
| 项目类型 | AI 视频剪辑 / 桌面端 / CLI / MCP |
| 桌面框架 | Tauri + React |
| 当前状态 | 二次开发 / 独立维护 |

---

## 项目概览

本仓库基于上游 **zhouxiaoka/autoclip** 继续开发，用于在自己的项目空间中进行功能调整、界面优化和后续发布。

核心目标是把长视频处理压缩成一条相对完整的自动化流程：

**输入素材 → 分析内容 → 选择高光 → 生成字幕 → 画面重构 → 渲染 → 生成封面与发布文案**

桌面剪辑与渲染主要在本机完成；云端模型是否参与分析、转写或生图，取决于用户所选择的模型与服务商。

---

## 核心能力

### 一键出片
- 支持本地视频与链接输入
- 自动分析长视频内容
- 自动筛选高光片段
- 按平台时长与画幅输出
- 生成字幕、标题、简介、话题和发布包

### 多平台适配
可面向以下平台生成内容：

- 抖音
- 小红书
- TikTok
- Instagram Reels
- YouTube Shorts
- B 站
- YouTube

### 画面与字幕
- 访谈式竖版
- 满屏播客式竖版
- 原画幅横版
- 自动字幕分页
- 说话人物取景与跟随
- 无人物镜头保留完整画面

### 模型选择
支持：
- 云端 API
- OpenAI 兼容接口
- Ollama
- LM Studio
- 本地 Whisper / SenseVoice
- 已有字幕直接跳过转写

### 多入口
- Windows / macOS 桌面端
- Docker / Web
- CLI
- MCP Server

CLI / MCP 与桌面端尽量复用同一条核心处理链路。

---

## 快速开始

### 桌面版

普通用户优先从本仓库 GitHub Releases 下载正式安装包。

当前桌面版本号：

```text
1.5.6
```

Windows 使用 x64 安装包；macOS 构建能力取决于对应发布流水线。

### CLI / MCP

环境建议：

```text
Python 3.10+
FFmpeg / FFprobe
```

安装依赖：

```bash
python -m pip install -r requirements.txt
python -m pip install -e .
```

检查版本：

```bash
autoclip --version
```

示例：

```bash
autoclip produce talk.mp4 --srt talk.srt --platform douyin --portrait-style podcast --json
```

启动 MCP：

```bash
autoclip mcp
```

### Docker / Web

```bash
cp env.example .env
mkdir -p data logs uploads
docker compose up -d --build
```

---

## 模型配置

### 云端 API
在设置中选择服务商、填写 API Key，并选择可用模型。

### Ollama
默认 OpenAI 兼容地址：

```text
http://localhost:11434/v1
```

### LM Studio
默认本地服务地址：

```text
http://localhost:1234/v1
```

Docker 访问宿主机模型服务时，`localhost` 指向容器自身，需要改成容器可以访问的宿主机地址。

---

## 构建与发布

### 桌面技术栈
- Tauri
- Rust
- React
- Vite
- Python portable runtime
- FFmpeg

### Windows x64

仓库保留正式 Windows 构建脚本：

```bash
bash scripts/build_windows_x64.sh
```

构建流程会：
- 准备 portable Python
- 安装后端依赖
- 打包后端源码
- 准备 FFmpeg / FFprobe
- 构建前端
- 使用 Tauri 生成 NSIS 安装包

### 版本来源

当前以下位置应保持版本一致：

- `pyproject.toml`
- `src-tauri/tauri.conf.json`
- `src-tauri/Cargo.toml`

当前版本均为：

```text
1.5.6
```

正式发布前必须核对安装包版本、Release Tag、应用内版本与更新通道一致。

---

## 项目结构

```text
AutoClip-/
├─ backend/                 # Python 后端与核心处理逻辑
├─ frontend/                # React / Vite 前端
├─ src-tauri/               # Tauri 桌面壳与打包配置
├─ scripts/                 # 构建、校验与辅助脚本
├─ docs/                    # 使用与开发文档
├─ skills/                  # Agent / MCP 相关能力
├─ requirements.txt
├─ pyproject.toml
└─ README.md
```

---

## 数据、安全与隐私

- 剪辑与渲染主要在本机完成
- 使用云端文本模型时，会发送处理所需的字幕或文案
- 使用云端转写时，会发送对应音频
- 使用画面理解或生图时，可能发送抽样画面
- 使用本地模型时，可避免对应的云端 API 调用
- API Key、账号凭据与签名密钥不应提交到仓库

详细隐私说明请查看：

`docs/PRIVACY.md`

---

## 使用边界

- 自动高光与标题结果仍需人工复核
- 不同模型的输出质量与成本差异较大
- 第三方视频、音乐、字幕与平台内容应遵守对应版权和平台规则
- 云端服务费用、速度与可用性由对应服务商决定

---

## 上游与许可证

- 上游项目：`zhouxiaoka/autoclip`
- 二次开发仓库：`youzhi1701/AutoClip-`
- 许可证：MIT

本仓库应继续保留上游许可证与必要版权声明。

---

## 发布与维护

当前二次开发版本：**v1.5.6**

本仓库的 Release 与上游 Release 分开记录。后续正式版本应保证：

- 版本号统一
- Windows 安装包可直接安装
- Release 下载地址属于本仓库
- 自动更新地址不会误指向其他发行仓库
- 构建脚本在干净环境可重复执行
- 发布前完成基础安装与运行验证
