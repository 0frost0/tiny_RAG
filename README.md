# 🤖 Tiny RAG：个人知识库问答助手

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.30%2B-FF4B4B)](https://streamlit.io/)
[![OpenAI](https://img.shields.io/badge/OpenAI-SDK-green)](https://openai.com/)

Tiny RAG 是一个基于 **RAG (Retrieval-Augmented Generation)** 技术构建的轻量级个人知识库助手。它允许用户上传私有文档（PDF、Markdown、TXT），系统会自动进行智能切片、向量化存储，并基于文档内容回答用户提问。

本项目完全本地化运行向量数据库（无需安装 Chroma/Milvus），代码结构清晰，非常适合用于**课程作业**、**毕业设计**或**RAG 原理学习**。

---

## ✨ 项目亮点

* **⚡️ 完全本地化向量库**: 不依赖重型数据库，利用 `NumPy` + `JSON` 手写实现轻量级向量检索，易于理解 RAG 核心原理。
* **📄 强力 PDF 解析**: 集成 `pdfplumber`，显著解决了中文教材、论文的乱码和排版错乱问题。
* **🖥️ 可视化交互**: 基于 Streamlit 构建的 Web 界面，支持实时构建知识库、查看对话历史及折叠展示参考来源（Citation）。
* **🛡️ 防幻觉设计**: 通过精心设计的 Prompt Template，强制模型“只根据参考资料回答”，减少大模型的胡编乱造。

---

## 🛠️ 技术栈 (Technology Stack)

* **LLM & Embedding**: OpenAI SDK (支持 GPT-3.5/4, Qwen, DeepSeek 等)
* **前端界面**: Streamlit
* **文档处理**: pdfplumber (PDF), BeautifulSoup4 (HTML/XML)
* **向量计算**: NumPy (余弦相似度计算), tiktoken (Token 计数)
* **进度管理**: tqdm

---

## 🧩 模块架构

项目采用模块化设计，各组件职责如下：

| 模块文件 | 功能描述 |
| :--- | :--- |
| **`app.py`** | **Web 主程序**。串联所有模块，提供图形界面，处理用户输入与会话状态。 |
| **`ReadFiles.py`** | **数据读取与切片**。实现了滑动窗口切片算法（Sliding Window），支持自定义 Token 长度和重叠窗口。 |
| **`Embeddings.py`** | **向量化接口**。封装 OpenAI Embedding API，实现了基于 NumPy 的余弦相似度计算。 |
| **`VectorBase.py`** | **向量数据库**。实现了简易向量库的增删改查，支持将向量和文档持久化保存到 JSON。 |
| **`LLM.py`** | **大模型接口**。负责构建 RAG Prompt，将检索到的上下文与用户问题组装发送给 LLM。 |

---

## 📂 项目目录结构

```text
TinyRAG/
├── .env                  # [配置] API 密钥配置文件
├── requirements.txt      # [依赖] 项目依赖库列表
├── app.py                # [入口] Streamlit 启动入口
├── ReadFiles.py          # [模块] 文档读取与切片
├── Embeddings.py         # [模块] 向量模型包装
├── VectorBase.py         # [模块] 向量数据库管理
├── LLM.py                # [模块] 大模型对话包装
├── data/                 # [输入] 存放你的 PDF/TXT 参考文档
│   └── test.txt
└── storage/              # [输出] 自动生成的向量数据库（请勿手动修改）
    ├── document.json     # 存储切片后的文本内容
    └── vectors.json      # 存储对应的向量数据
