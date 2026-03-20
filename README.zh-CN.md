# OpenCode Config Bootstrap

让 opencode 自动配置自己。

**[English](README.md)**

## 使用方法

### 方式一：Clone 项目（推荐，无需爬取）

```bash
git clone https://github.com/mmdsnb/opencode-helper.git && cd opencode-helper && cp -r ./docs ~/.config/opencode/docs && cp ./helper.md ~/.config/opencode/agent/helper.md
```

opencode 会直接使用项目中已有的文档，无需爬取。

### 方式二：只获取 bootstrap.md（需要爬取）

在 opencode 中输入：

```
请读取这个文件并执行其中的指令：
https://raw.githubusercontent.com/mmdsnb/opencode-helper/main/bootstrap.md
```

opencode 会自动爬取文档（约 2 分钟），然后创建 helper agent。

## 完成后

使用 `@helper` 来配置 opencode：

- "opencode 有哪些内置 agent？"
- "检查我的配置有什么问题"
- "帮我创建一个代码审查 agent"
- "把 build agent 的模型换成 haiku"

## 文件说明

- `bootstrap.md` - 引导文件
- `docs/` - 预爬取的 opencode 文档（33 个页面）
- `README.md` - 英文文档
- `README.zh-CN.md` - 本文件
