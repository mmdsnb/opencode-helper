# OpenCode Config Bootstrap

Let opencode configure itself automatically.

**[中文文档](README.zh-CN.md)**

## Usage

### Option 1: Clone the project (Recommended, no crawling needed)

```bash
git clone https://github.com/mmdsnb/opencode-helper.git && cd opencode-helper && cp -r ./docs ~/.config/opencode/docs && cp ./helper.md ~/.config/opencode/agent/helper.md
```

opencode will use the pre-crawled docs directly, no crawling needed.

### Option 2: Only get bootstrap.md (crawling required)

In opencode, enter:

```
Read this file and execute the instructions:
https://raw.githubusercontent.com/mmdsnb/opencode-helper/main/bootstrap.md
```

opencode will automatically crawl the docs (~2 min), then create the helper agent.

## After Setup

Use `@helper` to configure opencode:

- "What built-in agents does opencode have?"
- "Check my config for issues"
- "Help me create a code review agent"
- "Change build agent's model to haiku"

## Files

- `bootstrap.md` - Bootstrap instructions
- `docs/` - Pre-crawled opencode docs (33 pages)
- `README.md` - This file
- `README.zh-CN.md` - Chinese documentation
