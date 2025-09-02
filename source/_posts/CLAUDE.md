# _posts 模块文档

[根目录](../CLAUDE.md) > **_posts**

## 模块职责

存放已发布的博客文章，按主题分类组织。

## 入口与启动

- 入口文件：各 Markdown 文件
- 启动方式：通过 Hexo 构建生成静态网站

## 对外接口

- 接口形式：Markdown 文件，包含 front-matter 和内容
- 访问方式：通过生成的静态网站 URL 访问

## 关键依赖与配置

- 依赖：Hexo、Stellar 主题
- 配置：_config.yml, _config.stellar.yml

## 数据模型

- 数据格式：Markdown 文件
- front-matter 包含：title, date, tags, topic, categories 等元数据

## 测试与质量

- 测试策略：手动预览和验证
- 质量工具：Markdown 语法检查

## 常见问题 (FAQ)

Q: 如何创建新文章？
A: 使用 `hexo new "文章标题"` 命令

Q: 如何修改已发布的文章？
A: 直接编辑对应的 Markdown 文件

## 相关文件清单

- 所有 .md 文件
- 各文章对应的资源文件夹

## 变更记录 (Changelog)

- 2025-09-02: 初始化模块文档