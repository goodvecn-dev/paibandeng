# 排版邓（paibandeng）

纯离线、单文件的 Markdown → 微信公众号富文本排版工具。

在线演示：[https://goodvecn-dev.github.io/paibandeng/](https://goodvecn-dev.github.io/paibandeng/)

## 项目定位

排版邓面向公众号作者与内容运营，把 Markdown 源稿转换为带内联样式的 HTML，并通过系统剪贴板以「富文本 + 纯文本」双格式复制，尽量减少粘贴到公众号后台后的二次排版成本。

设计约束：

- **离线优先**：不依赖后端服务，打开 `index.html` 即可使用。
- **单文件交付**：样式、解析引擎与交互逻辑集中在一个 HTML 文件中，便于分发与托管。
- **复制即所得**：预览区与剪贴板输出共用同一份内联样式结果。

## 快速开始

### 本地打开

1. 克隆或下载本仓库。
2. 用现代浏览器直接打开 `index.html`（建议通过本地静态服务以启用安全上下文下的 Clipboard API）。
3. 输入或粘贴 Markdown，选择版式，点击「一键复制富文本」，粘贴到公众号编辑器。

### GitHub Pages

上游仓库已配置 Pages。也可将本仓库启用 Pages，并将发布目录指向仓库根目录。

## 功能概览

| 能力 | 说明 |
|------|------|
| Markdown 预览 | 标题、段落、加粗/斜体/删除线、高亮、行内代码、链接、图片 |
| 结构块 | 引用、有序/无序列表、任务列表、分隔线、表格 |
| 代码块 | 普通围栏代码；`graph` / `flow` / `mermaid` 简化流程图；`chart` / `bar` 简易条形图 |
| 版式主题 | 朱砂、湖蓝、松烟、琥珀、胭脂、松绿、群青、藤黄共 8 套 |
| 草稿持久化 | `localStorage` 保存文稿与主题选择 |
| 导入导出入口 | 剪贴板粘贴、本地 `.md` 导入、示例稿、清空 |
| 富文本复制 | 优先 `ClipboardItem`；失败时回退 `document.execCommand('copy')` |

## 架构说明

`index.html` 内含三个逻辑分区：

1. **styles**：页面壳层与编辑/预览布局样式（不影响最终复制到公众号的内联样式）。
2. **MarkdownEngine**：自研轻量解析与渲染，输出带 theme 内联样式的 HTML；同时暴露 `toPlainText`。
3. **app**：编辑器绑定、主题切换、草稿读写、复制与提示。

关键数据流：

```text
Markdown 输入 → renderMarkdown(theme) → 预览 DOM
                                 ↘ Clipboard（text/html + text/plain）
```

## 浏览器兼容性

- 推荐：最新 Chromium / Edge / Safari / Firefox。
- 富文本复制在 HTTPS 或 `localhost` 等安全上下文中体验最佳。
- 部分移动浏览器可能限制剪贴板写入，此时请按提示改用长按预览区复制。

## 安全说明

- 链接与图片地址仅允许 `http://` / `https://` 协议。
- 文本内容经 `escapeHtml` 处理后再进入预览 DOM。
- 本工具在本地浏览器运行；草稿保存在本机 `localStorage`，不会主动上传。

## 开发与贡献

当前以单文件为主。欢迎提交 Issue 与 Pull Request，建议优先关注：

- CommonMark / GFM 兼容缺口
- 公众号后台粘贴兼容性
- 无障碍与键盘操作
- 自动化测试与工程化基线

贡献流程建议：Fork → 特性分支 → Pull Request，并在描述中说明动机、复现步骤与影响面。

## 版本

参见 [CHANGELOG.md](./CHANGELOG.md)。当前工程化基线版本为 **v0.1.0**。

## 许可证

[MIT License](./LICENSE) © 2026 goodvecn-dev
