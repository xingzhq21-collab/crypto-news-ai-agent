# 加密新闻 AI 自动摘要工作流

用 n8n + DeepSeek API 搭建的 AI Agent 工作流，每天定时抓取加密新闻，
自动生成中文摘要并推送到飞书群。

## 工作流结构

RSS Read → Loop Over Items → DeepSeek API → Wait (2s) → 飞书推送

![工作流截图](n8n.png)

## 技术栈

- **n8n** — 工作流自动化平台
- **DeepSeek API** — 大模型摘要生成
- **飞书 Webhook** — 消息推送
- **Cointelegraph RSS** — 新闻数据源

## 遇到的关键问题

1. **表达式未解析**：JSON Body 需要切换到表达式模式，才能让 `{{ $json.title }}` 生效
2. **飞书限流**：短时间内推送太多消息会触发限流，加入 Wait 节点做 2 秒延迟解决
3. **循环闭环**：Loop Over Items 需要把结果连回输入端才能持续循环

## 成果

- 每天自动处理 30 条新闻
- 每条新闻生成 80 字以内中文摘要
- 全程零代码，人工只需几分钟复核

![飞书推送截图](飞书.png)

## 如何使用

### 前置条件
- n8n 账号（Cloud 或自建）
- DeepSeek API Key
- 飞书群自定义机器人 Webhook

### 导入与配置
1. 下载本仓库的 `crypto-news-ai-agent.json`
2. 在 n8n 中导入该文件
3. 配置 DeepSeek 节点：将 Authorization 头替换为 `Bearer 你的API Key`
4. 配置飞书节点：将 URL 替换为你的飞书 Webhook 地址
5. 点击 Execute workflow 即可运行
