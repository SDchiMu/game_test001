---
description: Python 后端开发规范，适用于 FastAPI / SQLAlchemy 项目
applyTo: "**/*.py"
---

# Python Development Instructions

## 项目结构与命名

### 当新建文件时
- 文件名使用小写蛇形命名，例如 `user_service.py`、`data_models.py`
- 模块内部类名使用大写驼峰，函数和变量使用小写蛇形
- 私有函数和方法以单下划线 `_` 开头

### 当组织目录结构时
- 按业务模块划分包，每个包包含 `__init__.py`
- 路由层放在 `routers/` 目录，服务层放在 `services/` 目录
- 数据模型放在 `models/` 目录，Schema 放在 `schemas/` 目录

## 编码规范

### 当编写函数时
- 所有函数必须有类型注解（参数和返回值）
- 函数长度控制在 50 行以内，超过应考虑拆分
- 复杂逻辑必须写 docstring，说明输入、输出、异常

### 当处理异常时
- 使用自定义异常类继承 `Exception`，不使用裸 `raise Exception`
- 在服务层捕获底层异常，转换为业务异常再抛给路由层
- 路由层使用 HTTPException 返回给客户端

### 当使用异步时
- 所有 I/O 操作使用 async/await
- 数据库查询使用异步 Session，配合 `async with`
- 耗时计算任务使用 `asyncio.to_thread` 或在 Celery 中执行

## 数据库操作规范

### 当定义 SQLAlchemy 模型时
- 模型继承 `DeclarativeBase`，不直接使用 `declarative_base()`
- 每个模型包含 `id` 主键（UUID 或自增整数）、`created_at`、`updated_at` 时间戳
- 关系定义使用 `relationship()` 并指定 `back_populates`

### 当编写查询时
- 使用 `select()` 语句，不写原生 SQL
- 查询结果使用 `scalars().all()` 或 `scalar_one_or_none()`
- 批量操作使用 `bulk_insert` 或 `bulk_update`

## 测试规范

### 当编写测试时
- 使用 pytest 框架，测试文件以 `test_` 开头
- 每个测试函数独立，不依赖其他测试的执行顺序
- 外部依赖使用 pytest-mock 模拟，不连接真实数据库或外部 API
- 测试数据库使用内存 SQLite 或测试容器

## 依赖管理

### 当引入新依赖时
- 优先使用项目已有的依赖，不重复引入功能相似的库
- 新依赖必须在 `requirements.txt` 或 `pyproject.toml` 中声明
- 避免使用过时或不再维护的库