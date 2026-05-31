# Test Scenario: Simple Calculator API

Use this to verify the phased-subagent-orchestration skill works correctly.

## Project: Simple Calculator

A REST API calculator with two independent modules:

### Module A: Basic Operations (+, -, *, /)
- No dependencies
- Phase 1

### Module B: History Logging
- Depends on Module A
- Phase 2

## project-overview.md (for testing)

```markdown
# Simple Calculator API

## 项目目标
A REST API that performs basic math operations and logs history.

## 架构概述
Express.js REST API with two modules: operations and history.

## 模块划分
### 模块A: Basic Operations
- 职责: Handle +, -, *, / operations
- 依赖: 无
- 对应子任务: task-operations.md

### 模块B: History Logging
- 职责: Log all operations to file
- 依赖: 模块A
- 对应子任务: task-history.md

## 依赖关系图
Basic Operations (Phase 1) → History Logging (Phase 2)

## 统一规范
- 代码风格: 2空格缩进
- 命名约定: camelCase
- 文件组织: routes/, services/, utils/
- 错误处理: 返回 { error: string } 格式
```

## task-operations.md (for testing)

```markdown
# Task: Basic Operations

## 所属模块
模块A: Basic Operations

## 任务目标
Implement addition, subtraction, multiplication, and division endpoints.

## 详细需求
### 功能点1: POST /api/calculate
- 验收标准: Accepts { op: string, a: number, b: number }, returns { result: number }
- 涉及文件: routes/calculate.js, services/calculator.js

### 功能点2: Division by zero
- 验收标准: Returns { error: "Division by zero" } when dividing by 0
- 涉及文件: services/calculator.js

## 约束条件
- 只能修改: routes/, services/
- 不能修改: 历史模块相关文件
- 依赖: 无

## 技术提示
Use a switch statement for operation routing.
```

## task-history.md (for testing)

```markdown
# Task: History Logging

## 所属模块
模块B: History Logging

## 任务目标
Log all calculation operations to a JSON file.

## 详细需求
### 功能点1: History service
- 验收标准: appendToHistory(op, a, b, result) writes to history.json
- 涉及文件: services/history.js

### 功能点2: GET /api/history
- 验收标准: Returns array of past operations
- 涉及文件: routes/history.js

## 约束条件
- 只能修改: routes/history.js, services/history.js
- 不能修改: 计算模块相关文件
- 依赖: 模块A (需要知道操作格式)

## 技术提示
Read existing history before appending.
```

## Expected Execution Flow

1. Main agent creates project-overview.md and two task docs
2. Phase 1: Dispatch sub-agent for task-operations
3. Sub-agent implements operations module
4. Sub-agent creates reviewer → reviewer checks → passes
5. Main agent checks consistency → passes
6. Phase 2: Dispatch sub-agent for task-history
7. Sub-agent implements history module
8. Sub-agent creates reviewer → reviewer checks → passes
9. Main agent final check → passes
10. Project complete

## Test Criteria

- [ ] Main agent never reads/writes code files
- [ ] Sub-agent reads both documents before coding
- [ ] Sub-agent checks skill library
- [ ] Sub-agent creates reviewer after implementation
- [ ] Reviewer reads both documents
- [ ] Reviewer outputs structured review result
- [ ] Main agent performs incremental check after Phase 1
- [ ] Main agent performs final check after Phase 2
- [ ] All code follows unified standards
- [ ] History module correctly depends on operations format
