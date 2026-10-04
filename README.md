如果喜欢这个插件请点个star非常感谢(｡◕◡◕｡)ﾉ

# astrbot_plugin_iamthinking

别吵，我在思考！
还在因为思考型模型反应太慢而烦恼？这个插件不能解决问题，但可以提升一点体验。
收到用户消息并准备调用 LLM 时，为这条用户消息贴一个表情表示处理中；发送回复后，再贴一个表情表示处理完成。
注意，贴表情是群聊专属功能，私信场景下不会生效。

## 配置

插件配置通过 `_conf_schema.json` 提供，默认仅在 QQ OneBot (aiocqhttp) 平台生效：

- enabled：是否启用
- thinking_emoji_ids：处理中表情 ID 列表
- random_thinking_emoji：处理中是否从 `thinking_emoji_ids` 中随机选择 1 个（默认开启）
- done_emoji_ids：完成表情 ID 列表
- random_done_emoji：完成时是否从 `done_emoji_ids` 中随机选择 1 个（默认开启）
- remove_thinking_on_done：完成后是否移除处理中表情

### 配置示例

```json
{
  "enabled": true,
  "thinking_emoji_ids": [66],
  "random_thinking_emoji": true,
  "done_emoji_ids": [74, 75, 76],
  "random_done_emoji": true,
  "remove_thinking_on_done": true
}
```

### 行为说明

- 当 `random_thinking_emoji=true` 且 `thinking_emoji_ids` 有多个值时，进入处理中阶段会随机贴其中一个。
- 当 `random_thinking_emoji=false` 时，处理中会按列表（去重后）逐个贴上。
- 当 `done_emoji_ids` 为空列表 `[]` 时，完成阶段会**正常跳过贴完成表情**，这是合法配置，不会产生 warning。
- 当 `random_done_emoji=true` 且 `done_emoji_ids` 有多个值时，每次完成会随机贴其中一个。
- 当 `random_done_emoji=false` 时，会按列表（去重后）逐个贴上，保持旧行为。

## 说明

插件只在 AstrBot 准备调用 LLM 时触发，不会对普通消息、指令消息贴表情。
