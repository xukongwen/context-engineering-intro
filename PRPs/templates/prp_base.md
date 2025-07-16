name: "基础 PRP 模板 v2 - 富含上下文与验证循环"
description: |

## 目的
为 AI 代理优化的模板，通过足够的上下文和自我验证能力，通过迭代优化来实现工作代码。

## 核心原则
1. **上下文为王**：包含所有必要的文档、示例和注意事项
2. **验证循环**：提供 AI 可以运行和修复的可执行测试/代码检查
3. **信息密集**：使用代码库中的关键字和模式
4. **逐步成功**：从简单开始，验证，然后增强
5. **全局规则**：确保遵循 CLAUDE.md 中的所有规则

---

## 目标
[需要构建什么 - 具体说明最终状态和期望]

## 为什么
- [商业价值和用户影响]
- [与现有功能的集成]
- [这解决了什么问题以及为谁解决]

## 是什么
[用户可见的行为和技术要求]

### 成功标准
- [ ] [具体的可衡量结果]

## 所需的全部上下文

### 文档与参考（列出实现功能所需的所有上下文）
```yaml
# 必读 - 在你的上下文窗口中包含这些
- url: [官方 API 文档 URL]
  why: [你需要的特定部分/方法]
  
- file: [path/to/example.py]
  why: [要遵循的模式，要避免的陷阱]
  
- doc: [库文档 URL] 
  section: [关于常见陷阱的特定部分]
  critical: [防止常见错误的关键见解]

- docfile: [PRPs/ai_docs/file.md]
  why: [用户粘贴到项目中的文档]

```

### 当前代码库树（在项目根目录运行 `tree`）以获得代码库概览
```bash

```

### 期望的代码库树，包含要添加的文件和文件职责
```bash

```

### 我们代码库的已知陷阱和库怪癖
```python
# 关键：[库名] 需要 [特定设置]
# 示例：FastAPI 端点需要异步函数
# 示例：此 ORM 不支持超过 1000 条记录的批量插入
# 示例：我们使用 pydantic v2 并且
```

## 实施蓝图

### 数据模型和结构

创建核心数据模型，确保类型安全和一致性。
```python
示例：
 - orm 模型
 - pydantic 模型
 - pydantic 模式
 - pydantic 验证器

```

### 完成 PRP 所需的任务列表，按完成顺序排列

```yaml
任务 1：
修改 src/existing_module.py：
  - 查找模式："class OldImplementation"
  - 在包含 "def __init__" 的行后注入
  - 保留现有的方法签名

创建 src/new_feature.py：
  - 镜像模式来自：src/similar_feature.py
  - 修改类名和核心逻辑
  - 保持错误处理模式相同

...(...)

任务 N：
...

```


### 根据需要为每个任务添加的伪代码
```python

# 任务 1
# 带有关键细节的伪代码，不要编写完整代码
async def new_feature(param: str) -> Result:
    # 模式：始终先验证输入（参见 src/validators.py）
    validated = validate_input(param)  # 抛出 ValidationError
    
    # 陷阱：此库需要连接池
    async with get_connection() as conn:  # 参见 src/db/pool.py
        # 模式：使用现有的重试装饰器
        @retry(attempts=3, backoff=exponential)
        async def _inner():
            # 关键：如果 >10 req/sec，API 返回 429
            await rate_limiter.acquire()
            return await external_api.call(validated)
        
        result = await _inner()
    
    # 模式：标准化响应格式
    return format_response(result)  # 参见 src/utils/responses.py
```

### 集成点
```yaml
数据库：
  - 迁移："向 users 表添加列 'feature_enabled'"
  - 索引："CREATE INDEX idx_feature_lookup ON users(feature_id)"
  
配置：
  - 添加到：config/settings.py
  - 模式："FEATURE_TIMEOUT = int(os.getenv('FEATURE_TIMEOUT', '30'))"
  
路由：
  - 添加到：src/api/routes.py  
  - 模式："router.include_router(feature_router, prefix='/feature')"
```

## 验证循环

### 级别 1：语法和风格
```bash
# 首先运行这些 - 在继续之前修复任何错误
ruff check src/new_feature.py --fix  # 自动修复可能的问题
mypy src/new_feature.py              # 类型检查

# 预期：没有错误。如果有错误，阅读错误并修复。
```

### 级别 2：单元测试 每个新功能/文件/函数使用现有的测试模式
```python
# 创建 test_new_feature.py，包含这些测试用例：
def test_happy_path():
    """基本功能正常工作"""
    result = new_feature("valid_input")
    assert result.status == "success"

def test_validation_error():
    """无效输入引发 ValidationError"""
    with pytest.raises(ValidationError):
        new_feature("")

def test_external_api_timeout():
    """优雅地处理超时"""
    with mock.patch('external_api.call', side_effect=TimeoutError):
        result = new_feature("valid")
        assert result.status == "error"
        assert "timeout" in result.message
```

```bash
# 运行并迭代直到通过：
uv run pytest test_new_feature.py -v
# 如果失败：阅读错误，理解根本原因，修复代码，重新运行（永远不要通过模拟来通过）
```

### 级别 3：集成测试
```bash
# 启动服务
uv run python -m src.main --dev

# 测试端点
curl -X POST http://localhost:8000/feature \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'

# 预期：{"status": "success", "data": {...}}
# 如果错误：检查 logs/app.log 中的堆栈跟踪
```

## 最终验证清单
- [ ] 所有测试通过：`uv run pytest tests/ -v`
- [ ] 没有代码检查错误：`uv run ruff check src/`
- [ ] 没有类型错误：`uv run mypy src/`
- [ ] 手动测试成功：[特定的 curl/命令]
- [ ] 优雅地处理错误情况
- [ ] 日志信息丰富但不冗长
- [ ] 如果需要，文档已更新

---

## 要避免的反模式
- ❌ 当现有模式有效时，不要创建新模式
- ❌ 不要因为"应该可以工作"而跳过验证
- ❌ 不要忽略失败的测试 - 修复它们
- ❌ 不要在异步上下文中使用同步函数
- ❌ 不要硬编码应该是配置的值
- ❌ 不要捕获所有异常 - 要具体