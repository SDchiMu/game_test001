# Project Template

> 使用此模板快速创建新项目，自带 VS Code + GitHub Copilot 配置。

## 包含的配置

### 🤖 GitHub Copilot

| 文件 | 作用 |
|------|------|
| `.github/copilot-instructions.md` | 全局 Copilot 行为规范 |
| `.github/instructions/python.instructions.md` | Python 后端开发规范 |
| `.github/instructions/react.instructions.md` | React 前端开发规范 |
| `.github/prompts/code-review.prompt.md` | `/code-review` 自定义 prompt |
| `.github/prompts/write-tests.prompt.md` | `/write-tests` 自定义 prompt |

### ⚙️ VS Code

| 文件 | 作用 |
|------|------|
| `.vscode/settings.json` | 工作区设置（Python + React + 通用） |

## 使用方法

### 方式一：GitHub 模板（推荐）

1. 点击绿色 **"Use this template"** 按钮
2. 输入新仓库名称
3. 克隆到本地开始开发

### 方式二：手动复制

```bash
# 将模板文件复制到新项目
cp -r .github/ 新项目/.github/
cp -r .vscode/ 新项目/.vscode/
```

## 技术栈

- **后端**: Python / FastAPI / SQLAlchemy
- **前端**: TypeScript / React 18+
- **测试**: pytest（Python）/ Vitest（前端）
- **样式**: Tailwind CSS
