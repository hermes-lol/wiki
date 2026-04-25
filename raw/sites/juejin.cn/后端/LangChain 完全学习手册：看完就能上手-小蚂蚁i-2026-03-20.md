---
---

# LangChain 完全学习手册：核心摘要

## 一、LangChain 概述

### 解决的问题
- 将 AI 应用开发中 80% 的基础设施工作抽象为可复用组件
- 开发者只需关注 20% 的业务逻辑

### 核心能力
| 功能 | 说明 |
|------|------|
| 统一模型接口 | 切换模型只需改一行配置 |
| Prompt 模板系统 | 支持变量、Few-shot |
| LCEL 链式调用 | 用 `|` 管道符串联组件 |
| 内置记忆管理 | 自动管理对话历史 |
| RAG 全套工具 | 文档加载→切割→向量化→检索 |
| Agent 框架 | AI 自主调用工具、搜索、执行代码 |

### 生态产品
- **LangChain**：核心框架，用于快速构建 LLM 应用、RAG、简单 Agent
- **LangGraph**：Agent 编排引擎，用于复杂多步骤工作流
- **LangSmith**：调试与监控平台

---

## 二、环境安装与配置

### 安装命令
```bash
# 核心框架
pip install langchain

# 模型（按需选一）
pip install langchain-openai       # GPT
pip install langchain-anthropic    # Claude
pip install langchain-google-genai # Gemini
pip install langchain-ollama       # 本地模型

# RAG 相关
pip install langchain-community    # 社区扩展
pip install chromadb               # 本地向量数据库
pip install python-dotenv          # 环境变量
```

### 配置 API Key
创建 `.env` 文件（**务必加入 `.gitignore`**）：
```
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxxxxx
```

### 第一个程序
```python
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage

load_dotenv()
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.7)
response = llm.invoke([HumanMessage(content="用一句话解释什么是 LangChain")])
print(response.content)
```

**切换模型只需改两行**：
```python
from langchain_anthropic import ChatAnthropic
llm = ChatAnthropic(model="claude-opus-4-5")
# 后面代码完全不变
```

---

## 三、六大核心概念

### 3.1 模型（Models）
```python
from langchain_openai import ChatOpenAI
from langchain_anthropic import ChatAnthropic
from langchain_ollama import ChatOllama

gpt = ChatOpenAI(model="gpt-4o-mini", temperature=0.7, max_tokens=1000)
claude = ChatAnthropic(model="claude-opus-4-5", temperature=0)
local = ChatOllama(model="llama3.2")

# 三个用法完全一样
result = gpt.invoke("你好")
print(result.content)  # .content 拿到文本
```

### 3.2 Prompt 模板
```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "你是一名专业的 {language} 开发专家。"),
    ("human", "请解释 {topic} 的概念，并给出代码示例。"),
])

messages = prompt.invoke({"language": "Python", "topic": "装饰器"})
response = llm.invoke(messages)
```

### 3.3 LCEL 链式调用（核心）
```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()  # 把 AIMessage 转成纯字符串
chain = prompt | llm | parser

# 单次调用
result = chain.invoke({"topic": "量子计算"})

# 批量处理
results = chain.batch([
    {"topic": "机器学习"},
    {"topic": "深度学习"},
])

# 流式输出（打字机效果）
for chunk in chain.stream({"topic": "神经网络"}):
    print(chunk, end="", flush=True)
```

### 3.4 输出解析器
```python
from langchain_core.pydantic_v1 import BaseModel, Field
from langchain.output_parsers import PydanticOutputParser

class MovieReview(BaseModel):
    title: str = Field(description="电影名")
    score: int = Field(description="评分 1-10")
    summary: str = Field(description="一句话评价")

parser = PydanticOutputParser(pydantic_object=MovieReview)
chain = prompt | llm | parser
review = chain.invoke({"movie": "星际穿越", "format_instructions": parser.get_format_instructions()})
print(review.title)  # 星际穿越
print(review.score)  # 9（整数）
```

### 3.5 记忆（Memory）
```python
from langchain_core.prompts import MessagesPlaceholder

prompt = ChatPromptTemplate.from_messages([
    ("system", "你是一个友好的 AI 助手。"),
    MessagesPlaceholder(variable_name="history"),
    ("human", "{input}"),
])

chain = prompt | llm
history = []

def chat(user_input):
    response = chain.invoke({"input": user_input, "history": history})
    history.append(HumanMessage(content=user_input))
    history.append(AIMessage(content=response.content))
    return response.content
```

### 3.6 检索器（Retrievers）
```python
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

vectorstore = Chroma.from_documents(docs, OpenAIEmbeddings())
retriever = vectorstore.as_retriever(search_kwargs={"k": 2})
results = retriever.invoke("什么是管道符？")
```

---

## 四、RAG 实战

### 完整实现
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_core.runnables import RunnablePassthrough

# 1. 准备文档
documents = [Document(page_content="...", metadata={"source": "hr_policy.txt"})]

# 2. 切割文档
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(documents)

# 3. 向量化存储
vectorstore = Chroma.from_documents(chunks, OpenAIEmbeddings())
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# 4. 构建 RAG Chain
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt | llm | StrOutputParser()
)

# 5. 查询
print(rag_chain.invoke("入职满两年有几天年假？"))
```

### 常用文档加载器
| 加载器 | 用途 |
|--------|------|
| `PyPDFLoader` | PDF 文件（需 `pip install pypdf`） |
| `WebBaseLoader` | 网页内容 |
| `TextLoader` | 纯文本 .txt |
| `CSVLoader` | CSV 数据文件 |
| `DirectoryLoader` | 批量加载整个文件夹 |
| `UnstructuredWordDocumentLoader` | Word 文档 |
| `GitLoader` | Git 仓库

[... summary truncated for context management ...]