# CLAUDE.md — 项目全局指令

> 本文件将原 `.github/copilot-instructions.md`、`.github/instructions/` 和 `.github/prompts/` 中的规则整合至此，Claude Code 每次会话自动加载，不会因上下文滚动而丢失。

---

## 通用行为规范

### 回答问题
- 先给出结论（一句话总结），再展开细节
- 如果涉及代码，提供完整的可运行示例
- 解释时注明适用的语言/框架版本

### 代码审查
- 按此顺序输出：**明确错误 → 边界情况 → 性能/可读性建议**
- 每条建议附带修改前后的代码对比
- 如果发现安全隐患，优先高亮
- 问题分级：🔴 严重 / 🟡 中等 / 🔵 建议

### 编写测试
- 使用 **pytest** 框架（Python）或 **Vitest**（TypeScript/React）
- 覆盖：正常路径 + 至少两个边界路径 + 一个异常路径
- 对外部依赖使用 mock，不产生真实副作用

### 需求不明确时
- 优先推荐当前项目已使用的技术栈
- 如果无法推断，询问用户偏好后再给出方案

### 代码有语法错误时
- 自动修复并展示修正后的代码
- 在注释中说明修改原因

### 报告错误时
- 先复现问题（如果可以）
- 根据实际代码分析原因，不猜测
- 给出修正后的代码并解释修改理由

---

## Python 开发规范（FastAPI / SQLAlchemy / pytest）

### 项目结构与命名
- 文件名：`user_service.py` 小写蛇形
- 类名：`PascalCase` / 函数变量：`snake_case`
- 私有成员以 `_` 开头
- 按业务模块划分包，每个包含 `__init__.py`
- 分层：`routers/` → `services/` → `models/` + `schemas/`

### 编码规范
- 函数必须带类型注解（参数 + 返回值）
- 函数控制在 50 行以内
- 复杂逻辑写 docstring（输入、输出、异常）
- 使用自定义异常继承 `Exception`，不抛裸异常
- 服务层捕获底层异常 → 转换为业务异常 → 路由层用 `HTTPException`

### 异步
- I/O 操作使用 async/await
- 数据库使用异步 Session（`async with`）
- 耗时计算用 `asyncio.to_thread` 或 Celery

### 数据库
- 模型继承 `DeclarativeBase`
- 每个模型含 `id`（UUID 或自增）、`created_at`、`updated_at`
- 关系用 `relationship()` + `back_populates`
- 查询用 `select()`，不写原生 SQL

### 测试
- pytest 框架，文件以 `test_` 开头
- 每个测试独立，不依赖执行顺序
- 用 `pytest-mock` 模拟外部依赖
- 测试数据库用内存 SQLite 或测试容器

### 依赖管理
- 优先使用已有依赖
- 新依赖在 `requirements.txt` 或 `pyproject.toml` 中声明
- 避免使用过时库

---

## React 开发规范（TypeScript / Tailwind CSS / Vitest）

### 组件
- 函数组件 + Hooks，不使用 Class 组件
- 文件 `UserProfile.tsx` PascalCase 命名
- 每文件只导出一个默认组件
- 业务逻辑提取到自定义 Hooks
- Props 用 TypeScript 接口定义（`XxxProps`）
- 列表渲染用稳定 `key`，不使用索引

### Hooks
- `useState` 按职责拆分，复杂逻辑用 `useReducer`
- `useEffect` 明确依赖数组，清理副作用
- `useRef` DOM 引用类型 `React.RefObject<HTMLElement>`
- `useContext` 值尽量稳定，拆分过大的 Context

### 状态管理
- 组件共享状态 → React Context + useReducer
- 全局状态 → Zustand 或 Jotai（不用 Redux，除非已有）
- 服务端状态 → TanStack Query
- Query key 结构：`['resource', params]`

### 样式
- Tailwind CSS 或 CSS Modules，不用全局 CSS
- CSS Module 文件：`ComponentName.module.css`
- 移动端优先，断点向上覆盖

### 测试
- Vitest + Testing Library
- 文件位置：`ComponentName.test.tsx` 与被测组件同目录
- 优先测试用户交互行为
- 使用 `screen.getByRole`、`findByText`，避免 test ID

### 性能
- 大型列表用虚拟滚动（`react-window` 或 `@tanstack/react-virtual`）
- 图片懒加载使用 `loading="lazy"`

### 错误处理
- 使用 Error Boundary 包裹可能出错的层级
- 异步错误用 try-catch + 状态显示错误提示
- 全局未捕获上报 Sentry

---

## 项目技术栈速查

| 领域 | 技术 |
|------|------|
| Python 后端 | FastAPI / SQLAlchemy / pytest |
| Python 格式化 | Ruff（autoFormat + organizeImports）|
| 前端 | React 18+ / TypeScript |
| 样式 | Tailwind CSS |
| 前端测试 | Vitest + Testing Library |
| Python 解释器 | `.venv/Scripts/python.exe` |
| 编辑器缩进 | 2 空格 |

> **注意**：本文件是 `.github/copilot-instructions.md`、`.github/instructions/python.instructions.md`、`.github/instructions/react.instructions.md`、`.github/prompts/code-review.prompt.md` 和 `.github/prompts/write-tests.prompt.md` 的整合。这些原始文件保留给 GitHub Copilot 使用，但本文件是 Claude Code 的权威来源。
