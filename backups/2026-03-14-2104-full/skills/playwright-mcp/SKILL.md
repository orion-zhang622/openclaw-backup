---
name: playwright-mcp
description: Browser automation via Playwright MCP server. Navigate websites, click elements, fill forms, extract data, take screenshots, and perform full browser automation workflows.
metadata: {"openclaw":{"emoji":"🎭","os":["linux","darwin","win32"],"requires":{"bins":["playwright-mcp","npx"]},"install":[{"id":"npm-playwright-mcp","kind":"npm","package":"@playwright/mcp","bins":["playwright-mcp"],"label":"Install Playwright MCP"}]}}
---

# Playwright MCP Skill

Browser automation powered by Playwright MCP server.

## Usage

Playwright MCP runs as a separate server process. Start it with:

```bash
npx @playwright/mcp
```

Then use the MCP tools: browser_navigate, browser_click, browser_type, etc.

## vs openclaw-agent-browser

| Feature | Playwright MCP | openclaw-agent-browser |
|---------|----------------|------------------------|
| Element selection | CSS/XPath selectors | @e1, @e2 dynamic refs |
| Best for | Complex automation, testing | Quick AI agent tasks |
| Browser support | Chromium/Firefox/WebKit | Chromium |
