# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

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
| 1 | agent-reach 平台模块 | 社交媒体（小红书/抖音/YouTube等） |
| 2 | kimi_fetch | 单网页提取 |
| 3 | web_fetch | 备用网页提取 |
| 4 | browser | 复杂交互/动态页面 |

### 文档/表格生成
| 需求 | 工具 | 备注 |
|------|------|------|
| Word 文档 | feishu_create_doc | 上传到 openclaw 文件夹 |
| Excel 表格 | feishu_sheet | 上传到 openclaw 文件夹 |
| PPT (9:16 竖屏) | ppt-generator skill | 手机友好，乔布斯风格 |
| PPT (16:9 横屏) | python-pptx | 标准演示文稿 |

### 图像生成
| 优先级 | 工具 | 适用场景 |
|--------|------|----------|
| 1 | coze-image-gen | 快速生成，效果稳定 |
| 2 | image-gen (Fal) | 高质量/特定风格 |

---

Add whatever helps you do your job. This is your cheat sheet.
