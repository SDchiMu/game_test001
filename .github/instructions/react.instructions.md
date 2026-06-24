---
description: React 前端开发规范，适用于 TypeScript + React 18+ 项目
applyTo: "**/*.{tsx,jsx}"
---

# React Development Instructions

## 组件规范

### 当创建新组件时
- 使用函数组件和 Hooks，不使用 Class 组件
- 组件文件使用 PascalCase 命名，例如 `UserProfile.tsx`
- 每个组件文件只导出一个默认组件，辅助类型和工具函数放在单独文件

### 当编写组件逻辑时
- 将业务逻辑提取到自定义 Hooks 中，不在组件体内写过多副作用
- 组件 props 使用 TypeScript 接口定义，接口名以 `Props` 结尾
- 对于可选 props，提供合理的默认值

### 当处理组件渲染时
- 避免内联函数和对象字面量作为 props，使用 `useCallback` 和 `useMemo` 优化
- 列表渲染时必须提供稳定的 `key`，不使用索引作为 key
- 条件渲染使用三元运算符或早期返回，避免深层嵌套的三元表达式

## Hooks 使用规范

### 当使用 useState 时
- 状态按职责拆分，不把多个无关状态放在同一个对象中
- 如果状态逻辑复杂，使用 `useReducer` 替代

### 当使用 useEffect 时
- 明确指定依赖数组，避免遗漏或过度包含
- 清理函数必须清除副作用（定时器、订阅、事件监听）
- 尽可能将副作用提取到自定义 Hook 中

### 当使用 useRef 时
- 用于 DOM 引用时，类型为 `React.RefObject<HTMLDivElement>`
- 用于保存可变值时，不用于渲染逻辑

### 当使用 useContext 时
- Context 的值尽量稳定，避免频繁变更导致消费组件大面积重渲染
- 将 Context 拆分为多个独立 context，避免单一 context 承载过多数据

## 状态管理规范

### 当选择状态管理方案时
- 组件间共享状态优先使用 React Context + useReducer
- 跨页面/跨路由的全局状态使用 Zustand 或 Jotai，不使用 Redux（除非项目已有）
- 服务端状态（数据获取）使用 React Query（TanStack Query），不手动管理缓存

### 当使用 React Query 时
- 为每个查询定义唯一的 queryKey，结构为 `['resource', params]`
- 使用 `staleTime` 和 `gcTime` 控制缓存策略，避免不必要的网络请求
- 突变操作使用 `useMutation` 并在 `onSuccess` 中失效相关查询

## 样式规范

### 当编写样式时
- 使用 Tailwind CSS 或 CSS Modules，不使用全局 CSS 类名
- 样式与组件文件放在同级目录，CSS Module 文件名为 `ComponentName.module.css`
- 主题色、间距等设计令牌从统一的 tokens 文件中引用

### 当处理响应式时
- 使用 Tailwind 的断点前缀（`sm:` `md:` `lg:` `xl:`）实现响应式
- 移动端优先，基础样式针对手机，断点向上覆盖

## 测试规范

### 当编写组件测试时
- 使用 Vitest + Testing Library，不使用 Enzyme
- 测试文件与被测组件同目录，命名为 `ComponentName.test.tsx`
- 优先测试用户交互行为，不测试内部实现细节
- 使用 `screen.getByRole`、`findByText` 等查询方法，避免使用 test ID

## 性能优化

### 当发现性能瓶颈时
- 使用 React DevTools Profiler 定位重渲染原因
- 对大型列表使用虚拟滚动（`react-window` 或 `@tanstack/react-virtual`）
- 图片懒加载使用 `loading="lazy"` 属性或 `next/image`（如果使用 Next.js）

## 错误处理

### 当处理组件错误时
- 使用 Error Boundary 包裹可能出错的组件层级
- 异步操作错误在组件内使用 try-catch 捕获，并用状态显示错误提示
- 全局未捕获错误通过 `window.onerror` 或 Sentry 上报