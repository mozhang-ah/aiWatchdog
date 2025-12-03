[English](readme_en.md) | 中文

### 2025.12.3更新-我们发布了aiwatchdog的桌面版本软件，普通用户可以快速上手而不是从0开始安装， [下载地址](https://www.aiwatchdog.cn/ "download") 
### 关注我们的B站账号，我们的使用视频教程和案例都会在上面发布：[b站教程地址](https://www.bilibili.com/video/BV1SpSaBhE2i "b站教程地址")

# 智能视频监控预警系统

## 项目概述

本项目是一个基于多模态视觉模型的智能视频监控预警系统。系统能够实时分析监控视频画面，自动检测异常行为并生成预警信息。通过结合视频处理、异常检测和实时预警推送等技术，为安防监控提供智能化解决方案。
同时支持自定义的提示词，可以覆盖不同场景的异常情况。

## 系统流程

### 1. 视频处理流程
```mermaid
graph LR
    A[视频输入源] --> B[视频处理器]
    B -->|视频帧| C[帧缓冲区]
    C -->|批量帧| D[多模态分析器]
    D -->|分析| E[内容描述生成]
    E -->|描述文本| F[异常行为检测]
    F -->|检测结果| G{异常判断}
```

### 2. 预警服务流程
```mermaid
graph TB
    G{异常判断} -->|是| H[预警服务]
    G -->|否| I[历史记录存储]
    
    H -->|WebSocket| J[客户端推送]
    H -->|触发| K[异常视频保存]
    H -->|触发| L[异常截图保存]
    
    I -->|定期| M[历史内容总结]
```

### 3. RAG系统流程（可选）
```mermaid
graph TB
    E[内容描述生成] -->|文本向量化| V[向量数据库]
    H[预警服务] -->|预警信息| V
    M[历史总结] -->|总结文本| V
    
    X[用户查询] -->|向量化| Y[相似度检索]
    Y -->|结果| Z[智能问答]
```

## 主要功能

1. **实时视频分析**
   - 支持本地视频文件和RTSP视频流
   - 自动分析视频内容并生成场景描述

2. **异常行为检测**
   - 智能识别可疑行为和异常情况
   - 实时生成预警信息

3. **实时预警推送**
   - 通过WebSocket推送预警信息
   - 支持视频流实时推送

4. **异常记录存档**
   - 自动保存异常视频片段
   - 记录详细的异常描述

## 使用说明

### 1. 环境配置

```bash
# 安装必要的Python库
pip install -r requirements.txt

```
配置config.py中的参数，包括qwen-vl和语言模型的api地址和API_KEY等
修改提示词，提示词需要根据实际场景进行修改


### 2. 运行系统

```bash
# 使用本地视频文件
python video_server.py --video_source "./测试视频/xxx.mp4"

# 或使用RTSP视频流
python video_server.py --video_source "rtsp://xxx.xxx.xxx.xxx/xxx"
```

### 3. WebSocket推送服务访问

- 预警消息WebSocket：`ws://localhost:16532/alerts`
- 视频流WebSocket：`ws://localhost:16532/video_feed`

### 4. 输出文件

- 异常视频片段：`video_warning/`目录
- 视频内容描述：`video_histroy_info.txt`
- 系统日志：`code.log`

## RAG知识库配置（可选）

如需启用RAG本地知识库功能，需要：

1. 安装向量数据库（如Milvus）
2. 配置数据库连接
3. 配置数据库插入api等
3. 修改系统配置，启用RAG模式
4. 测试utils.py中的insert_txt插入代码

启用后，视频内容描述将存储在向量数据库中，支持智能问答功能。

注意：本开源版本仅包含智能监控报警的核心功能，RAG知识库和大模型api需要自行配置。
