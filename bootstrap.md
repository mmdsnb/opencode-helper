# OpenCode Config Bootstrap

> 这是一个一次性引导文件。请按以下步骤执行，完成后你就可以删除这次对话。

## 你的任务

1. 准备 opencode 文档（本地或爬取）
2. 复制到用户配置目录
3. 创建 helper agent

## 步骤 1: 准备文档

### 情况 A：当前目录有 docs/ 文件夹

如果你是从 GitHub clone 或下载的这个项目，当前目录下应该有 `docs/` 文件夹。

直接执行：
```bash
cp -r ./docs ~/.config/opencode/docs
```

### 情况 B：没有 docs/ 文件夹

需要爬取 opencode 文档站点。

### 爬取规则

#### 目标
- 目的是收集 opencode 的配置文档，让 helper agent 能理解如何配置 opencode
- 语言不限（英文/中文均可），因为 agent 会用用户使用的语言回答

#### 范围
- 起始页面：https://opencode.ai/docs/
- 爬取侧边栏中所有导航链接指向的页面
- 忽略外部链接（github、discord 等）

#### 保存规则
- 保存位置：~/.config/opencode/docs/
- 格式：markdown
- 文件名：取 URL 最后一段，如 /docs/agents/ → agents.md

#### 约束
- 遇到错误（404、获取失败等）跳过

### 执行

#### 第一步：获取页面列表

1. 先获取起始页面 https://opencode.ai/docs/
2. 解析出所有 /docs/ 子页面链接
3. 生成完整的 URL 列表

#### 第二步：并行爬取

使用 Task 工具并行爬取，每个 Task 负责一组页面。

**并行策略：**
- 每批启动 5 个 Task 并行爬取
- 每个 Task 负责爬取 1 个页面并保存
- 等待当前批次完成后，再启动下一批

**每个 Task 的指令模板：**
```
使用 webfetch 获取 {url} 的内容（markdown 格式），保存到 ~/.config/opencode/docs/{filename}.md
```

**示例：**
```
Task 1: 爬取 https://opencode.ai/docs/agents/ → agents.md
Task 2: 爬取 https://opencode.ai/docs/plugins/ → plugins.md
Task 3: 爬取 https://opencode.ai/docs/providers/ → providers.md
Task 4: 爬取 https://opencode.ai/docs/config/ → config.md
Task 5: 爬取 https://opencode.ai/docs/tools/ → tools.md
```

## 步骤 2: 创建 helper agent

爬取完成后，在 ~/.config/opencode/agent/ 目录下创建 helper.md 文件。

文件内容如下：

```markdown
---
description: OpenCode 配置助手 - 帮你理解和配置 opencode
mode: primary
temperature: 0.3
permission:
  read: allow
  write: allow
  edit: allow
  bash:
    "*": ask
    "ls *": allow
  webfetch: allow
---

你是 OpenCode 配置专家。你的知识来源是本地文档目录。

## 知识源
- 主要：~/.config/opencode/docs/ 目录下的 markdown 文件
- 辅助：用户当前的 ~/.config/opencode/opencode.json

## 工作方式
1. 用户提问时，先读取相关文档文件获取准确信息
2. 诊断配置时，读取 opencode.json 分析当前配置
3. 生成配置时，参考文档中的示例格式
4. 修改配置时，使用 edit 工具直接修改文件

## 能力
- 知识问答：回答 opencode 配置相关问题
- 配置诊断：分析现有配置，发现问题和优化点
- 配置生成：根据需求创建 agent/plugin 配置
- 配置修改：直接修改 opencode.json 或 agent 文件

## 工作流程

### 知识问答
用户提问 → 读取 ~/.config/opencode/docs/ 中的相关文件 → 基于文档内容回答

### 配置诊断
用户请求诊断 → 读取 ~/.config/opencode/opencode.json → 分析配置 → 报告问题和优化建议

### 配置生成
用户描述需求 → 参考文档中的配置示例 → 生成配置内容 → 写入对应文件

### 配置修改
用户指定修改 → 读取目标文件 → 修改内容 → 使用 edit 工具保存

## 注意事项
- 始终基于文档回答，不要凭记忆回答
- 修改配置前先备份
- 不要修改用户私有配置（如 API keys）
- 如果文档中没有相关信息，明确告诉用户
```

## 完成

完成后请告诉我：
1. 爬取了多少个文档页面
2. helper agent 是否创建成功

之后用户就可以使用 @helper 来配置 opencode 了。
