# _drafts 模块文档

[根目录](../CLAUDE.md) > **_drafts**

## 模块职责

存放未发布的博客草稿，供作者撰写和预览。

## 入口与启动

- 入口文件：各 Markdown 文件
- 启动方式：通过 `hexo server --draft` 命令预览草稿

## 对外接口

- 接口形式：Markdown 文件，包含 front-matter 和内容
- 访问方式：本地开发服务器预览

## 关键依赖与配置

- 依赖：Hexo
- 配置：_config.yml

## 数据模型

- 数据格式：Markdown 文件
- front-matter 包含：title, date, tags 等元数据

## 测试与质量

- 测试策略：本地预览验证
- 质量工具：Markdown 语法检查

## 常见问题 (FAQ)

Q: 如何创建新草稿？
A: 使用 `hexo new draft "草稿标题"` 命令

Q: 如何发布草稿？
A: 使用 `hexo publish "草稿文件名"` 命令

## 相关文件清单

- 所有 .md 文件

## 变更记录 (Changelog)

- 2025-09-02: 初始化模块文档