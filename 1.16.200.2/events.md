# Ver 1.16.200.2
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
在这里，您可以找到脚本可以监听和响应的事件列表。

## 客户端事件

### 监听事件
以下事件可通过脚本引擎监听，您可以在脚本中设置监听它们并对其作出响应。

#### 1. minecraft:client_entered_world

#### 2. minecraft:hit_result_changed

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 在指向实体时返回实体对象，离开（不指向）时返回null
position | 向量 [a, b, c] | 在指向实体时返回实体的坐标，离开（不指向）时返回null

##### 代码示例
响应“hit_result_changed”事件。
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  this.listenForEvent("minecraft:hit_result_changed", (eventData) => this.onHitChanged(eventData));
};

mySystem.onHitChanged = function(eventData) {
  if(eventData.position != null) {
    let chatEvent = this.createEventData("minecraft:display_chat_event");
    chatEvent.data.message = "正指向实体，坐标 x:" + eventData.position.x + " y:" + eventData.position.y + " z:" + eventData.position.z;
    this.broadcastEvent("minecraft:display_chat_event", chatEvent);
  }
};
```

#### 3. minecraft:hit_result_continuous

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 在指向实体时返回实体对象，离开（不指向）时返回null
position | 向量 [a, b, c] | 在指向实体时返回实体的坐标，离开（不指向）时返回null

##### 代码示例
响应“hit_result_continuous”事件。
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  this.listenForEvent("minecraft:hit_result_continuous", (eventData) => this.onHit(eventData));
};

mySystem.onHit = function(eventData) {
  if(eventData.position != null) {
    let chatEvent = this.createEventData("minecraft:display_chat_event");
    chatEvent.data.message = "正指向实体，坐标 x:" + eventData.position.x + " y:" + eventData.position.y + " z:" + eventData.position.z;
    this.broadcastEvent("minecraft:display_chat_event", chatEvent);
  }
};
```

#### 4. minecraft:pick_hit_result_changed

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。否则返回null
position | 向量 [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，否则返回null

##### 代码示例
响应“pick_hit_result_changed”事件。
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  this.listenForEvent("minecraft:pick_hit_result_changed", (eventData) => this.onPickChanged(eventData));
};

mySystem.onPickChanged = function(eventData) {
  if(eventData.position != null) {
    let chatEvent = this.createEventData("minecraft:display_chat_event");
    chatEvent.data.message = "正指向实体，坐标 x:" + eventData.position.x + " y:" + eventData.position.y + " z:" + eventData.position.z;
    this.broadcastEvent("minecraft:display_chat_event", chatEvent);
  }
};
```

#### 5. minecraft:pick_hit_result_continuous

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。否则返回null
position | 向量 [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，否则返回null

##### 代码示例
响应“pick_hit_result_continuous”事件。
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  this.listenForEvent("minecraft:pick_hit_result_continuous", (eventData) => this.onPick(eventData));
};

mySystem.onPick = function(eventData) {
  if(eventData.position != null) {
    let chatEvent = this.createEventData("minecraft:display_chat_event");
    chatEvent.data.message = "正指向实体，坐标 x:" + eventData.position.x + " y:" + eventData.position.y + " z:" + eventData.position.y + " z:" + eventData.position.z;
    this.broadcastEvent("minecraft:display_chat_event", chatEvent);
  }
};
```


### 可触发事件

#### 1. minecraft:display_chat_event

名称 | 类型 | 备注
-|-|-
message | 字符串 | 要发送的消息数据

##### 代码示例
触发消息事件
```javascript
const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
  let chatEvent = this.createEventData("minecraft:display_chat_event");
  chatEvent.data.message = "你好，世界！";
  this.broadcastEvent("minecraft:display_chat_event", chatEvent);
};
```

#### 2. minecraft:load_ui

##### 事件数据参数
名称 | 类型 | 备注
-|-|-
options | Json对象 | 您可以通过设置true或false来定义以下GUI选项：<br><br> **1. absorbs_input** <br> 如果为true，输入就不会再传递给在该界面下的其他界面 <br><br> **2. always_accepts_input** <br> 如果为true，则即使其他GUI显示在其顶部该GUI依然始终接收并处理输入（除非不在UI栈中了）。<br><br> **3. force_render_below** <br> 如果为true，则该GUI会被强制渲染，即使它的上层有其他GUI（甚至HUD）正在其上层渲染。<br><br> **4. is_showing_menu** <br> 如果为true，GUI将被视为暂停菜单，并且不允许在此GUI上层再显示暂停菜单。<br><br> **5. render_game_behind** <br> 如果为true，则游戏将继续在此GUI下方继续呈现。<br><br> **6. render_only_when_topmost**<br>如果为true，则此GUI只显示在UI栈顶层。<br><br> **7. should_steal_mouse**<br> 如果为true，则屏幕将捕获鼠标指针并将其移动范围限制在GUI上。
path | 字符串 | 定义GUI的HTML文件路径

#### 3. minecraft:send_ui_event
此事件用于将GUI事件发送到UI引擎。触发该事件后，将立即发送UI事件。
自定义GUI基于HTML5，请查看自定义GUI示例以获得参考。

名称 | 类型 | 备注
-|-|-
data | 字符串 | 将被触发的事件的数据
eventIdentifier | 字符串 | 事件的标识符

#### 4. minecraft:spawn_particle_attached_entity

名称 | 类型 | 默认值 | 备注
-|-|-|-
effect | 字符串 |  | 您要附加到实体的粒子效果的标识符。这是您在其Json文件中设置的效果的名称
entity | JavaScript实体对象 |  | 要附加粒子效果的实体
offset | 向量 [a, b, c] | [0, 0, 0] | 效果产生位置较实体位置的偏移量

#### 5. minecraft:spawn_particle_in_world

名称 | 类型 | 默认值 | 备注
-|-|-|-
effect | 字符串 |  | 要生成的粒子效果的标识符。这是您在其Json文件中设置效果的名称
position | 向量 [a, b, c] | [0, 0, 0] | 您想要产生粒子效果的位置

#### 6. minecraft:unload_ui

#### 7. minecraft:script_logger_config

名称 | 类型 | 默认值 | 备注
-|-|-|-
log_errors | 布尔 | 假 | 设置为真以记录客户端上所有的脚本错误信息
log_information | 布尔 | 假 | 设置为真以记录客户端上所有的常规脚本信息。比如使用client.log() 进行的日志记录
log_warnings | 布尔 | 假 | 设置为真以记录客户端上所有的脚本警告信息


## 服务端事件

### 监听事件

#### 1. minecraft:entity_attack

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 发起攻击的实体
target | JavaScript实体对象 | 被攻击的实体

#### 2. minecraft:player_attacked_entity

名称 | 类型 | 备注
-|-|-
attacked_entity | JavaScript实体对象 | 被玩家攻击的实体
player | JavaScript实体对象 | 攻击实体的玩家

#### 3. minecraft:entity_acquired_item

名称 | 类型 | 备注
-|-|-
acquired_amount | 整数 | 实体在此事件中获取的物品的总数
acquisition_method | 字符串 | 实体获取物品的方式
entity | JavaScript实体对象 | 获取物品的实体
item_stack | JavaScript物品堆对象 | 获取的物品
secondary_entity | JavaScript实体对象 | 如果存在，此实体在另一个实体获取物品前影响了获取。如：玩家完成了与村民的交易。“entity”是玩家，“secondary_entity”为村民

#### 4. minecraft:entity_carried_item_changed

名称 | 类型 | 备注
-|-|-
carried_item | JavaScript物品堆对象 | 现在实体手中的物品
entity | JavaScript实体对象 | 正在切换手中物品的实体
hand | 字符串 | 正在切换物品的手。主手或副手。
previous_carried_item | JavaScript物品堆对象 | 在切换前实体手中的物品

#### 5. minecraft:entity_created

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 刚刚生成的实体

#### 6. minecraft:entity_definition_event

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 被影响的实体
event | 字符串 | 被触发的事件

#### 7. minecraft:entity_death

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 实体死亡的位置
cause | 字符串 | 死亡的原因
entity | JavaScript实体对象 | 死亡的实体
killer | JavaScript实体对象 | 杀手，导致实体死亡的实体
projectile_type | 字符串 | 杀死实体的弹射物类型

#### 8. minecraft:entity_dropped_item

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 放置物品的实体
item_stack | JavaScript物品堆对象 | 被放置的方块

#### 9. minecraft:entity_equipped_armor

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 新装备了盔甲的实体
item_stack | JavaScript物品堆对象 | 装备的盔甲
slot | 字符串 | 盔甲被配备到的插槽

#### 10. minecraft:entity_hurt

名称 | 类型 | 备注
-|-|-
absorbed_damage | 整数 | 实体实际吸收的伤害
attacker | JavaScript实体对象 | 攻击并造成实体伤害的实体。只在直接受到实体伤害和通过弹射物伤害时存在
block_position | 向量 [a, b, c] | 仅在被方块伤害时存在。这是命中实体的方块的位置
cause | 字符串 | 实体遭受伤害的方式。有关伤害方式的完整列表，请参阅伤害源文档。
damage | 整数 | 实体在获得免疫力和防具后受到的伤害
entity | JavaScript实体对象 | 被伤害的实体
projectile_type | 字符串 | 仅在被弹射物伤害时出现。这是命中实体的弹射物的标识符

## 11. minecraft:entity_move

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 移动的实体

#### 12. minecraft:entity_sneak

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 正在改变其行走状态的实体
sneaking | 布尔 | 如果为真，则实体正在潜行状态，否则实体退出了潜行状态。

#### 13. minecraft:entity_start_riding

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 骑手
ride | JavaScript实体对象 | 正在被骑的实体

#### 14. minecraft:entity_stop_riding

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 骑手
entity_is_being_destroyed | 布尔 | 如果为真，则骑手停止骑行是因为他（们）已经死亡
exit_from_rider | 布尔 | 如果为真，则骑手自行决定停止骑行
switching_rides | 布尔 | 如果为真，则该骑手停止骑行是因为他正在骑另一个实体

#### 15. minecraft:entity_tick

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 被更新的实体

#### 16. minecraft:entity_use_item

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 使用物品的实体
item_stack | JavaScript物品堆对象 | 被使用的实体
use_method | 字符串 | 使用的方式

#### 17. minecraft:block_destruction_started

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 正在被破坏的方块的坐标
player | JavaScript实体对象 | 正在破坏这个方块的玩家

#### 18. minecraft:block_destruction_stopped

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 被破坏的方块的位置
destruction_progress | 小数 | 方块破坏的进度（在0-1之间）
player | JavaScript实体对象 | 停止破坏方块的玩家

#### 19. minecraft:block_exploded

名称 | 类型 | 备注
-|-|-
block_identifier | 字符串 | 被破坏的方块的标识符
block_position | JavaScript对象 | 因爆炸被破坏的方块的位置
cause | 字符串 | 方块被破坏的原因
entity | JavaScript实体对象 | 爆炸的实体

#### 20. minecraft:block_interacted_with

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 参与互动的方块坐标
player | JavaScript实体对象 | 参与互动的玩家的实体对象

#### 21. minecraft:piston_moved_block

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 被移动的方块的位置
piston_action | 字符串 | 活塞执行的动作，“extended”（伸）或“retracted”（缩）
piston_position | JavaScript对象 | 活塞的位置

#### 22. minecraft:player_destroyed_block

名称 | 类型 | 备注
-|-|-
block_identifier | 字符串 | 被破坏的方块的标识符
block_position | JavaScript对象 | 被破坏的方块的位置
player | JavaScript实体对象 | 破坏方块的玩家

#### 23. minecraft:player_placed_block

名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 被放置方块的位置
player | JavaScript实体对象 | 放置方块的玩家

#### 24. minecraft:play_sound

名称 | 类型 | 默认值 | 备注
-|-|-|-
pitch | 小数 | 1.0 | 音效的音高。1.0将以常规音高播放
position | 向量 [a, b, c] | [0, 0, 0]	| 播放声音的位置
sound | 字符串 |  | 要播放的音效的标识符，只能播放已应用资源包中的音效
volume | 小数 | 1.0 | 音效的音量。值为1.0将以原音效的音量播放

#### 25. minecraft:projectile_hit

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 被击中的实体（如果有）
owner | JavaScript实体对象 | 发射弹射物的实体
position | 向量 [a, b, c] | 弹射物击中的位置
projectile | JavaScript实体对象 | 弹射物

#### 26. minecraft:weather_changed

名称 | 类型 | 备注
-|-|-
dimension | 字符串 | 天气发生变化的维度的名称
lightning | 布尔 | 新天气是否有闪电
raining | 布尔 | 新天气是否下雨


### 可触发事件

#### 1. minecraft:entity_definition_event

名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 想要触发事件的实体（如苦力怕）
event | 字符串 | 要在实体上触发的事件的标识符。支持内置事件（minecraft:) 和自定义事件

#### 2. minecraft:display_chat_event

名称 | 类型 | 备注
-|-|-
message | 字符串 | 要发送的消息数据

#### 3. minecraft:execute_command

名称 | 类型 | 备注
-|-|-
command | 字符串 | 想要执行的命令

#### 4. minecraft:play_sound

名称 | 类型 | 默认值 | 备注
-|-|-|-
pitch | 小数 | 1.0 | 音效的音高。1.0将以常规音高播放
position | 向量 [a, b, c] | [0, 0, 0]	| 播放声音的位置
sound | 字符串 |  | 要播放的音效的标识符，只能播放已应用资源包中的音效
volume | 小数 | 1.0 | 音效的音量。值为1.0将以原音效的音量播放

#### 5. minecraft:spawn_particle_attached_entity

名称 | 类型 | 默认值 | 备注
-|-|-|-
effect | 字符串 |  | 您要附加到实体的粒子效果的标识符。这是您在其Json文件中设置的效果的名称
entity | JavaScript实体对象 |  | 要附加粒子效果的实体
offset | 向量 [a, b, c] | [0, 0, 0] | 效果产生位置较实体位置的偏移量

#### 6. minecraft:spawn_particle_in_world

名称 | 类型 | 默认值 | 备注
-|-|-|-
dimension | 字符串 | overworld | 产生粒子效果的维度，可以是“overworld”（主世界），“nether”（下界），或“the end”（末地）
effect | 字符串 |  | 要生成的粒子效果的标识符。这是您在其Json文件中设置效果的名称
position | 向量 [a, b, c] | [0, 0, 0] | 您想要产生粒子效果的位置

#### 7. minecraft:script_logger_config

名称 | 类型 | 默认值 | 备注
-|-|-|-
log_errors | 布尔 | 假 | 设置为真以记录服务器上所有的脚本错误信息
log_information | 布尔 | 假 | 设置为真以记录服务器上所有的常规脚本信息。比如使用server.log() 进行的日志记录
log_warnings | 布尔 | 假 | 设置为真以记录服务器上所有的脚本警告信息