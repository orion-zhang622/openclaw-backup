# ERRORS.md - 错误记录

记录命令失败、API 错误、工具使用错误。

## 格式模板

```markdown
## [ERR-YYYYMMDD-XXX] 错误来源

**Logged**: ISO-8601 时间戳
**Priority**: high
**Status**: pending | in_progress | resolved
**Area**: frontend | backend | infra | tests | docs | config | memory

### Summary
错误简述

### Error
```
实际错误信息
```

### Context
- 尝试的命令/操作
- 使用的参数
- 环境信息

### Suggested Fix
可能的解决方案

### Metadata
- Reproducible: yes | no | unknown
- Related Files: 相关文件

---
```

## 错误列表

