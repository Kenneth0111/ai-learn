需求分析文档（Deep Research版）

1. 功能需求 (Functional Requirements)
1.1 核心捕获系统 (Capture Engine)智能浏览器插件： 支持全平台浏览器，具备一键抓取、高亮标注功能 。开发者专用抓取： 自动解析 GitHub 仓库元数据、Stack Overflow 提问及 Discord 讨论链接 。离线全文存储： 自动为每个网页生成永久的干净副本（Markdown/HTML），防止链接失效 。
1.2 AI 处理中枢 (AI Hub)语义向量搜索： 实现基于 Embeddings 的模糊意图检索，支持自然语言对话式找寻 。自动内容摘要： 在入库时自动生成 skimmable（可略读）的文章概要和关键点提取 。智能分类与标签： AI 根据用户已有的分类习惯，自动建议标签并进行文件夹聚类 。
1.3 知识集成与同步 (Integration)双向 Notion/Obsidian 同步： 不仅导出 URL，还要将全文高亮及 AI 总结以结构化块（Blocks）的形式同步至笔记库 。开发者流插件： 提供 Raycast 插件、CLI 命令行工具及 LSP 支持 。MCP 协议实现： 作为标准 MCP Server 运行，支持 Amazon Q、Claude Desktop 等 AI Agent 直接访问 。
1.4 回顾与激活 (Retention)间隔重复队列： 自动将标记为“待学”的内容加入复习流 。失效链接清理： 定期巡检收藏夹，识别 404 页面并提示用户处理 。

2. 非功能需求 (Non-Functional Requirements)
2.1 性能要求 (Performance)搜索实时性： 在存储 10,000 条书签的情况下，关键词及语义检索响应时间须小于 300 毫秒 。同步延迟： 跨设备同步应在 5 秒内完成 。
2.2 隐私与安全 (Security)本地优先存储： 敏感数据默认存储在本地，仅在用户授权下进行端到端加密同步 。完全导出权： 用户可随时以标准 JSON/Markdown 格式导出其所有数据。
2.3 兼容性 (Compatibility)跨平台支持： 必须覆盖 macOS, Windows, Linux, iOS 和 Android 。语言支持： 首发版本必须完美支持中英文语义检索 。

3. 技术架构设计方案 (Technical Architecture)
基于 2026 年的技术栈，建议采用以下架构以保证系统的可扩展性与高效性：
数据采集层 (Scraping): 采用 Firecrawl 作为核心解析引擎。对于需要渲染的复杂页面，结合 Playwright 或 Selenium 模拟真实用户行为 。
模型层 (AI Models):
    摘要与基础分类： 使用 MiMo-V2-Flash 或 DeepSeek V4 (低延迟、高性价比) 。
    深度推理与综合： 使用 Claude Opus 4.6 (通过 API 接入) 负责高质量摘要和合成简报 。
索引与搜索层 (Search):
    结构化数据库： 使用 SQLite (本地) / PostgreSQL (云端)。
    向量数据库： 采用 ChromaDB 或本地 FAISS 存储语义嵌入 。
集成层 (Interface):
    API 协议： 实现 Model Context Protocol (MCP)。
    插件技术： 使用 React/TypeScript 构建跨浏览器扩展程序 。

4. 商业路径与路线图 (Roadmap)
阶段 1 (MVP): 聚焦极致的“开发者捕获”体验，实现CLI搜索和基础的Obsidian Markdown同步。
阶段 2 (AI Pro): 引入语义搜索、自动分类及摘要功能，上线 Pro 订阅服务。
阶段 3 (Ecosystem): 发布 MCP Server，打通主流 AI 编码助手，建立“私有知识计算中心”。