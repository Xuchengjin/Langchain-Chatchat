# 电商客服与运营知识库智能体

基于 `Langchain-Chatchat` 的二次开发项目，面向电商客服与运营场景，构建一个可演示的知识库问答系统。系统支持业务文档导入、知识库构建、检索增强问答与来源引用展示，可用于退款规则、发货时效、售后流程、客服标准话术等场景。

## 项目背景

在电商客服与运营场景中，常见问题主要集中在以下几类：

- 退款多久到账
- 现货和预售商品多久发货
- 是否支持 7 天无理由退货
- 质量问题退货的运费由谁承担
- 物流长时间未更新时客服应如何处理

传统人工查阅文档效率较低，且不同客服人员回答口径容易不一致。  
本项目基于 `RAG`（Retrieval-Augmented Generation，检索增强生成）思路，让模型先检索业务文档，再结合检索结果生成回答，从而提升回答的相关性、稳定性与可解释性。

## 项目目标

- 完成电商业务文档的知识库构建
- 支持基于知识库内容的智能问答
- 支持答案引用来源展示，降低幻觉风险
- 支持快速部署和在线演示
- 用于 `AI 应用开发工程师` 方向的项目展示与面试说明

## 原始项目与二次开发说明

本项目基于开源项目 `Langchain-Chatchat` 进行二次开发。  
原始项目已具备知识库问答、模型接入和 WebUI 等基础能力，本项目的重点不在于重复造轮子，而在于结合真实业务场景完成以下二次开发与实践：

- 选择电商客服与运营场景作为知识库问答目标
- 接入 `OpenAI-compatible` 聊天模型与 embedding 模型
- 构建中文测试知识库，覆盖退款、发货、售后等业务文档
- 调整 `chunk size`、`chunk overlap`、`top-k` 等参数，验证检索效果
- 整理固定测试问题，便于复现问答结果
- 完成演示截图、README 整理和项目交付说明

## 项目功能

### 1. 知识库构建

- 支持导入 `PDF`、`Markdown`、`TXT` 等文档
- 支持对业务文档进行文本切分与向量化
- 支持构建可检索的知识库索引

### 2. 检索增强问答

- 用户提问后，系统先从知识库中检索相关文档片段
- 将问题与检索结果拼接为 Prompt
- 调用大模型基于上下文生成回答

### 3. 来源引用展示

- 返回答案时可显示来源文档或相关片段
- 方便人工核对，降低模型幻觉风险

### 4. 场景化测试

- 基于电商客服知识库进行固定问题测试
- 验证退款、发货、售后规则类问题的问答效果

## 核心流程

```text
文档上传 -> 文本切分 -> 向量化 -> 向量检索 -> 拼接 Prompt -> 大模型回答

技术栈
Python
Langchain-Chatchat
LangChain
FastAPI
OpenAI-compatible LLM API
OpenAI-compatible Embedding API
Linux
Nginx（可选，用于反向代理部署）

目录结构
.
├─ data/
│  └─ test_docs_cn/
│     ├─ refund_policy_cn.md
│     ├─ shipping_rules_cn.md
│     └─ after_sales_process_cn.txt
├─ screenshots/
│  ├─ kb_created.png
│  ├─ docs_uploaded.png
│  └─ qa_result.png
├─ README.md

示例文档说明
当前测试知识库使用 3 份中文业务文档：
refund_policy_cn.md
退款到账时效、退款适用范围、客服回复口径

shipping_rules_cn.md
现货发货、预售商品发货、延迟发货规则

after_sales_process_cn.txt
7 天无理由退货、质量问题处理、运费承担规则

文档目录示例：
data/
└─ test_docs_cn/
   ├─ refund_policy_cn.md
   ├─ shipping_rules_cn.md
   └─ after_sales_process_cn.txt

固定测试问题
建议使用以下问题验证知识库问答效果：
退款一般多久到账？
预售商品多久发货？
现货商品多久发货？
签收后还能申请 7 天无理由退货吗？
质量问题退货，运费谁承担？
物流 48 小时未更新，客服应该怎么处理？

快速启动
以下为最小启动流程示例：
export CHATCHAT_ROOT=$HOME/chatchat_data
chatchat init
chatchat kb -r
chatchat start -a

如果在远程机器或 GitHub Codespaces 中运行，需要确认服务监听地址可被外部访问，例如将默认绑定地址设置为 0.0.0.0。
启动前需在配置文件中正确填写：
聊天模型名称
embedding 模型名称
OpenAI-compatible API 地址
API Key

我的工作
在本项目中，我主要完成了以下工作：
基于 Langchain-Chatchat 完成知识库问答系统的部署与配置
接入兼容 OpenAI 协议的聊天模型与 embedding 模型
构建电商客服中文知识库测试文档
上传文档并验证知识库问答链路
基于固定问题测试检索与回答效果
整理项目文档、目录结构与演示截图
形成可用于简历与面试展示的项目版本

演示截图
在此放置项目运行截图，例如：
![知识库创建页面](./screenshots/kb_created.png)
![文档上传页面](./screenshots/docs_uploaded.png)
![问答结果页面](./screenshots/qa_result.png)

项目亮点
基于成熟开源框架进行二次开发，能够快速完成业务场景落地
聚焦电商客服场景，问题和文档更贴近真实业务
项目结构清晰，适合初学者理解 RAG 应用开发流程
可以清楚讲解文档切分、向量化、检索、Prompt 拼接和回答生成的完整链路
适合作为 AI 应用开发工程师 面试项目进行展示

后续优化方向
增加订单查询、库存查询等业务工具调用能力
增加多轮会话上下文管理
增加混合检索和重排机制，提升召回质量
增加日志与问答效果评测能力
增加更完整的部署与权限控制能力

说明
本项目为基于开源框架的学习与实践项目，重点在于理解 RAG 应用开发的核心流程，并结合真实业务场景完成最小可演示交付。
