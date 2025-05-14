<div align="center">
  
![Python version](https://img.shields.io/badge/python-3.9+-blue)
[![web ui](https://img.shields.io/badge/WebUI-Gradio-important)](https://www.gradio.app/)

</div>

法律AI助手
=========

法律AI助手，法律RAG系统，通过导入全部200+本法律手册结合LLM进行专业检索回答，基于langchain、openai、chroma和Gradio构建

## Demo
[https://law.vmaig.com/](https://law.vmaig.com/)

**用户名**: username  
**密码**:  password  

## 核心原理

基于langchain链式调用框架，采用RAG（Retrieval-Augmented Generation）架构：
1. 法律条文预处理：按章节切分法律文本并构建向量数据库
2. 智能检索：通过语义相似度搜索匹配相关法律条文
3. 生成回答：结合检索到的法律条文生成专业解答

**初始化流程**
```mermaid
flowchart LR
    A[法律文件加载LawLoader] --> B[MarkdownHeaderTextSplitter]
    subgraph 文件切分LawSplitter
    B --> C[RecursiveCharacterTextSplitter]
    end
    C --> E[Embedding]
    E --> F[向量数据库Chroma]




**问答流程**
```mermaid
flowchart LR
    A[用户提问] --> B[问题校验];
    B -- 否 --> C1[请提问法律相关问题]
    B -- 是 --> D[法律检索链];
    subgraph Law Retrieval Chain 
    D --> E[LLM生成检索指令]
    E --> F[MultiQuery Retriever]
    subgraph 向量检索
    F --> G1[相似问题1]
    F --> G2[相似问题2]
    F --> G3[相似问题3]
    G1 --> H[Chroma向量库]
    G2 --> H
    G3 --> H
    H --> I[相关法律条文]
    end
    I --> J[法律条文合并]
    J --> K[LLM生成回答]
    K --> L[流式输出响应]
    end
```


## 初始化运行环境

```
# 创建.env 文件
cp .env.example .env

# 修改.env 中的内容
vim .env

# 安装venv环境
python -m venv ~/.venv/law
. ~/.venv/law
pip install -r requirements.txt
```

## 初始化向量数据库

```
# 加载和切分法律手册，初始化向量数据库
python manager.py --init
```

## 运行web ui

```
python manager.py --web
```

默认用户名/密码: username / password

<a href="https://sm.ms/image/DbP3TiHZConUFe7" target="_blank"><img src="https://s2.loli.net/2023/10/20/DbP3TiHZConUFe7.png" ></a>

## 运行对话

```
python manager.py --shell
```

<a href="https://sm.ms/image/7E4zMpbafCPvNxX" target="_blank"><img src="https://s2.loli.net/2023/10/19/7E4zMpbafCPvNxX.png"></a>

## 配置修改

如果你想修改回答中的法律条数，可以修改config.py
- 法律条数: LAW_VS_SEARCH_K
- web ui地址: WEB_HOST
- web ui端口: WEB_PORT
- web ui登录用户: WEB_USERNAME
- web ui登录密码: WEB_PASSWORD
