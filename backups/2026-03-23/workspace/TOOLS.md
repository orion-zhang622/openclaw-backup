# TOOLS.md - Local Notes

## 工具优先级速查

### 搜索工具
| 优先级 | 工具 | 适用场景 |
|--------|------|----------|
| 1 | tavily | AI优化搜索，首选 |
| 2 | baidu-search | Tavily 不可用时 |
| 3 | kimi_search | 备选/中文内容 |
| 4 | agent-reach Exa | 语义搜索、研究 |

### 内容抓取
| 优先级 | 工具 | 适用场景 |
|--------|------|----------|
| 1 | **Chrome MCP/CDP** (本地) | 首选 - 快速、稳定、支持复杂交互 |
| 2 | agent-reach 平台模块 | 社交媒体专用 |
| 3 | kimi_fetch / web_fetch | 简单静态页面 |
| 4 | playwright-mcp | 复杂浏览器自动化流程 |

### 浏览器工具详情
| 工具 | 类型 | 适用场景 |
|------|------|----------|
| `browser` (MCP) | 本地 Chrome | 首选，支持 snapshot/act/click/type |
| `canvas` + `browser` | CDP Docker | 需要截图/画面同步时使用 |
| `playwright-mcp` | 独立服务 | 复杂自动化、录制回放 |

### 文档/表格生成
| 需求 | 工具 | 备注 |
|------|------|------|
| Word 文档 | feishu_create_doc | 上传到 openclaw 文件夹 |
| Excel 表格 | feishu_sheet | 上传到 openclaw 文件夹 |
| PPT (9:16) | ppt-generator skill | 手机友好 |
| PPT (16:9) | python-pptx | 标准演示文稿 |

### 图像生成
| 优先级 | 工具 | 适用场景 |
|--------|------|----------|
| 1 | coze-image-gen | 快速生成，效果稳定 |
| 2 | image-gen (Fal) | 高质量/特定风格 |
