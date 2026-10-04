如果喜欢这个插件请点个star非常感谢(｡◕◡◕｡)ﾉ

# astrbot_plugin_iamthinking_fix

别吵，我在思考！
还在因为思考型模型反应太慢而烦恼？这个插件不能解决问题，但可以提升一点体验。
收到用户消息并准备调用 LLM 时，为这条用户消息贴一个表情表示处理中；发送回复后，再贴一个表情表示处理完成。
注意，贴表情是群聊专属功能，私信场景下不会生效。

## 功能

- **思考状态**：收到消息并调用 LLM 时，贴上"处理中"表情
- **完成状态**：LLM 回复发送后，贴上"完成"表情，可选移除"处理中"表情
- **随机表情**：处理中与完成阶段可从表情 ID 列表中随机贴 1 个
- **超时检测**：LLM 长时间未响应时，自动贴上"失败"表情，避免表情一直挂着
- **超时恢复**：超时后如果 LLM 最终响应了，自动移除失败表情，贴上完成表情
- **工具调用状态**：LLM 调用工具（如搜索、查资料）时，切换为"工具调用"表情，工具完成后切回"思考中"表情

## 配置

插件配置通过 `_conf_schema.json` 提供，默认仅在 QQ OneBot (aiocqhttp) 平台生效：

### 基础配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| enabled | bool | true | 是否启用 |
| thinking_emoji_ids | list[int] | [66] | 处理中表情 ID 列表 |
| random_thinking_emoji | bool | true | 处理中是否从 `thinking_emoji_ids` 中随机选择 1 个 |
| done_emoji_ids | list[int] | [74] | 完成表情 ID 列表 |
| random_done_emoji | bool | true | 完成时是否从 `done_emoji_ids` 中随机选择 1 个 |
| remove_thinking_on_done | bool | true | 完成后是否移除处理中表情 |

### 超时检测配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| timeout_seconds | int | 120 | LLM 超时阈值（秒），设为 0 禁用超时检测 |
| error_emoji_ids | list[int] | [264] | 超时/失败表情 ID 列表（默认：捂脸） |
| remove_thinking_on_error | bool | true | 超时后是否移除处理中表情 |

### 工具调用配置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| using_tool_emoji_ids | list[int] | [270] | 工具调用表情 ID 列表（默认：emm） |
| remove_thinking_on_tool | bool | true | 切换到工具调用状态时是否移除处理中表情 |

### 配置示例

```json
{
  "enabled": true,
  "thinking_emoji_ids": [66],
  "random_thinking_emoji": true,
  "done_emoji_ids": [74, 75, 76],
  "random_done_emoji": true,
  "remove_thinking_on_done": true,
  "timeout_seconds": 120,
  "error_emoji_ids": [264],
  "remove_thinking_on_error": true,
  "using_tool_emoji_ids": [270],
  "remove_thinking_on_tool": true
}
```

### 行为说明

- 当 `random_thinking_emoji=true` 且 `thinking_emoji_ids` 有多个值时，进入处理中阶段会随机贴其中一个。
- 当 `random_thinking_emoji=false` 时，处理中会按列表（去重后）逐个贴上。
- 当 `done_emoji_ids` 为空列表 `[]` 时，完成阶段会**正常跳过贴完成表情**，这是合法配置，不会产生 warning。
- 当 `random_done_emoji=true` 且 `done_emoji_ids` 有多个值时，每次完成会随机贴其中一个。
- 当 `random_done_emoji=false` 时，会按列表（去重后）逐个贴上，保持旧行为。
- 移除表情（超时、工具调用切换、完成时移除处理中表情）始终按列表全量执行，不受随机开关影响。
- `timeout_seconds=0` 时禁用超时检测；超时后如果 LLM 最终响应，会移除失败表情并贴上完成表情。

## 说明

- 插件只在 AstrBot 准备调用 LLM 时触发，不会对普通消息、指令消息贴表情
- 超时检测基于 asyncio 定时器，不会阻塞其他操作
- 工具调用状态支持 LLM 多次调用工具的场景
- 所有表情 ID 使用 OneBot/QQ 表情 ID
