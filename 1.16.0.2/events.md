<script>window.$docsify={subMaxLevel: 4}</script>
# Ver 1.16.0.2 
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
在这里，您可以找到脚本可以监听和响应的事件列表。


## 客户端事件


### 监听事件
以下事件可通过脚本引擎监听，您可以在脚本中设置监听它们并对其作出响应。

#### 1. minecraft:client_entered_world
每当玩家加入世界时就会触发此事件。事件数据为玩家的实体对象。

#### 2. minecraft:hit_result_changed
每当玩家十字准星（或触屏模式的圆圈）从其他指向其他地方改为指向实体时，都会触发此事件（最大支持范围为1000格）。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 在指向实体时返回实体对象，离开（不指向）时返回null
position | 向量 [a, b, c] | 在指向实体时返回实体的坐标，离开（不指向）时返回null

##### 代码示例
响应“hit_result_changed”事件。
```
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
（参考上个条目），每次指向更新都会触发此事件，支持范围在1000格以上。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 在指向实体时返回实体对象，离开（不指向）时返回null
position | 向量 [a, b, c] | 在指向实体时返回实体的坐标，离开（不指向）时返回null

##### 代码示例
响应“hit_result_continuous”事件。
```
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
每当玩家十字准星（或触屏模式的圆圈）指向物类型（如方块、空气、实体或其他事物）改变时触发。最大支持范围是1000格。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。否则返回null
position | 向量 [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，否则返回null

##### 代码示例
响应“pick_hit_result_changed”事件。
```
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
每次时钟循环更新都会触发此事件，并告诉您玩家的十字准星（或触屏模式的圆圈）在世界中（最多1000格）正指向哪个实体。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。否则返回null
position | 向量 [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，否则返回null

##### 代码示例
响应“pick_hit_result_continuous”事件。
```
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
可以通过脚本引擎使脚本触发以下事件，并令游戏做出相应的响应。

#### 1. minecraft:display_chat_event
此事件用于向特定玩家发送聊天消息。事件数据就是纯文本消息。支持特殊格式，就像玩家发送的消息一样。
##### 参数
名称 | 类型 | 备注
-|-|-
message | 字符串 | 要发送的消息数据

##### 代码示例
触发消息事件
```
const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
  let chatEvent = this.createEventData("minecraft:display_chat_event");
  chatEvent.data.message = "你好，世界！";
  this.broadcastEvent("minecraft:display_chat_event", chatEvent);
};
```

#### 2. minecraft:load_ui
此事件用于向特定玩家显示GUI。此事件会将指定的GUI放置到UI界面栈的顶层。触发事件后，将立即显示该GUI。使用此事件只能显示HTML文件中定义的GUI。
##### 事件数据参数
名称 | 类型 | 备注
-|-|-
options | Json对象 | 您可以通过设置true或false来定义以下GUI选项：<br><br> **1. absorbs_input** <br> 如果为true，输入就不会再传递给在该界面下的其他界面 <br><br> **2. always_accepts_input** <br> 如果为true，则即使其他GUI显示在其顶部该GUI依然始终接收并处理输入（除非不在UI栈中了）。<br><br> **3. force_render_below** <br> 如果为true，则该GUI会被强制渲染，即使它的上层有其他GUI（甚至HUD）正在其上层渲染。<br><br> **4. is_showing_menu** <br> 如果为true，GUI将被视为暂停菜单，并且不允许在此GUI上层再显示暂停菜单。<br><br> **5. render_game_behind** <br> 如果为true，则游戏将继续在此GUI下方继续呈现。<br><br> **6. render_only_when_topmost**<br>如果为true，则此GUI只显示在UI栈顶层。<br><br> **7. should_steal_mouse**<br> 如果为true，则屏幕将捕获鼠标指针并将其移动范围限制在GUI上。
path | 字符串 | 定义GUI的HTML文件路径

#### 3. minecraft:script_logger_config
此事件用于开关客户端各种级别的日志。<br>
**Note:** 打开/关闭日志记录级别操作并不仅限于影响广播该事件的脚本，它将影响所有的脚本，比如在其他行为包中的脚本。有关日志记录的更多信息，请参见“调试”部分。
##### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
log_errors | 布尔 | 假 | 设置为真以记录客户端上所有的脚本错误信息
log_information | 布尔 | 假 | 设置为真以记录客户端上所有的常规脚本信息。比如使用client.log() 进行的日志记录
log_warnings | 布尔 | 假 | 设置为真以记录客户端上所有的脚本警告信息

#### 4. minecraft:send_ui_event
此事件用于将GUI事件发送到UI引擎。触发该事件后，将立即发送UI事件。
自定义GUI基于HTML5，请查看自定义GUI示例以获得参考。
##### 参数
名称 | 类型 | 备注
-|-|-
data | 字符串 | 将被触发的事件的数据
eventIdentifier | 字符串 | 事件的标识符

#### 5. minecraft:spawn_particle_attached_entity
此事件用于创建将跟随在实体周围的粒子效果。此事件所创建粒子效果仅对指定玩家可见。可以在Json文件中定义效果（在资源包中）。然后，可以通过在效果的Json中定义的MoLang变量在其所附加的实体中进行更改来控制该效果。
##### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
effect | 字符串 |  | 您要附加到实体的粒子效果的标识符。这是您在其Json文件中设置的效果的名称
entity | JavaScript实体对象 |  | 要附加粒子效果的实体
offset | 向量 [a, b, c] | [0, 0, 0] | 效果产生位置较实体位置的偏移量

#### 6. minecraft:spawn_particle_in_world
此事件用于在世界上创建静态粒子效果。此事件所创建粒子效果仅对指定玩家可见。可以在Json文件中定义效果（在资源包中）。产生效果后，您无法进一步控制它。与该事件的服务器版本不同，客户端版本将在玩家当前所在的维度中产生粒子效果。
##### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
effect | 字符串 |  | 要生成的粒子效果的标识符。这是您在其Json文件中设置效果的名称
position | 向量 [a, b, c] | [0, 0, 0] | 您想要产生粒子效果的位置

#### 7. minecraft:unload_ui
此事件用于从的指定玩家的UI栈中移除GUI。事件数据包含要删除的GUI名称（字符串类型）。触发事件后，界面将被安排到下一次UI引擎可以这么做的时候被移除。只有通过HTML文件定义的界面可以通过该事件被移除。



## 服务端事件


### 监听事件
以下事件可通过脚本引擎监听，您可以在脚本中设置监听它们并对其作出相应。

#### 1. minecraft:block_destruction_started
每当玩家开始破坏方块时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 正在被破坏的方块的坐标
player | JavaScript实体对象 | 正在破坏这个方块的玩家

#### 2. minecraft:block_destruction_stopped
每当玩家停止破坏方块时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 被破坏的方块的位置
destruction_progress | 小数 | 方块破坏的进度（在0-1之间）
player | JavaScript实体对象 | 停止破坏方块的玩家

#### 3. minecraft:block_exploded
方块因爆炸被炸毁会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
block_identifier | 字符串 | 被破坏的方块的标识符
block_position | JavaScript对象 | 因爆炸被破坏的方块的位置
cause | 字符串 | 方块被破坏的原因
entity | JavaScript实体对象 | 爆炸的实体

#### 4. minecraft:block_interacted_with
每当玩家与方块互动时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 参与互动的方块坐标
player | Entity JS API Object | 参与互动的玩家的实体对象

#### 5. minecraft:entity_acquired_item
每当实体获取物品时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
acquired_amount | 整数 | 实体在此事件中获取的物品的总数
acquisition_method | 字符串 | 实体获取物品的方式
entity | JavaScript实体对象 | 获取物品的实体
item_stack | JavaScript物品堆对象 | 获取的物品
secondary_entity | JavaScript实体对象 | 如果存在，此实体在另一个实体获取物品前影响了获取。如：玩家完成了与村民的交易。“entity”属性是玩家，“secondary_entity”为村民

#### 6. minecraft:entity_attack
只要一个实体受到另一个实体的攻击，就会触发此事件。但这不能保证被攻击实体的确受到了攻击的伤害。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 发起攻击的实体
target | JavaScript实体对象 | 被攻击的实体

#### 7. minecraft:entity_carried_item_changed
每当实体切换手中的物品时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
carried_item | JavaScript物品堆对象 | 现在实体手中的物品
entity | JavaScript实体对象 | 正在切换手中物品的实体
hand | 字符串 | 正在切换物品的手。主手或副手。
previous_carried_item | JavaScript物品堆对象 | 在切换前实体手中的物品

#### 8. minecraft:entity_created
每当将实体在世界中生成时，都会触发此事件。 
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 刚刚生成的实体

#### 9. minecraft:entity_death
每当实体死亡时，都会触发此事件。移除实体（例如使用destroyEntity）不会触发此事件。并非每个实体死亡都存在以下所有的字段与值。
##### 参数
名称 | 类型 | 备注
-|-|-
block_position | JavaScript对象 | 实体死亡的位置
cause | 字符串 | 死亡的原因
entity | JavaScript实体对象 | 死亡的实体
killer | JavaScript实体对象 | 杀手，导致实体死亡的实体
projectile_type | 字符串 | 杀死实体的弹射物类型

#### 10. minecraft:entity_definition_event
每当脚本或实体触发实体在Json中的定义事件时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 被影响的实体
event | 字符串 | 被触发的事件

#### 11. minecraft:entity_dropped_item
每当实体放置物品时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 放置物品的实体
item_stack | JavaScript物品堆对象 | 被放置的方块

#### 12. minecraft:entity_equipped_armor
只要实体在装甲插槽中新装备了盔甲，就会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 新装备了盔甲的实体
item_stack | JavaScript物品堆对象 | 装备的盔甲
slot | 字符串 | 盔甲被配备到的插槽

#### 13. minecraft:entity_hurt
每当实体受到伤害时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
absorbed_damage | 整数 | 实体实际吸收的伤害
attacker | JavaScript实体对象 | 攻击并造成实体伤害的实体。只在直接受到实体伤害和通过弹射物伤害时存在
block_position | 向量 [a, b, c] | 仅在被方块伤害时存在。这是命中实体的方块的位置
cause | 字符串 | 实体遭受伤害的方式。有关伤害方式的完整列表，请参阅伤害源文档。
damage | 整数 | 实体在获得免疫力和防具后受到的伤害
entity | JavaScript实体对象 | 被伤害的实体
projectile_type | 字符串 | 仅在被弹射物伤害时出现。这是命中实体的弹射物的标识符

#### 14. minecraft:entity_sneak
每当实体的行走模式更改时，都会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 正在改变其行走状态的实体
sneaking | 布尔 | 如果为真，则实体正在潜行状态，否则实体退出了潜行状态。

#### 15. minecraft:entity_start_riding
每当一个实体成为另一个实体的骑手时，就会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 骑手
ride | JavaScript实体对象 | 正在被骑的实体

#### 16. minecraft:entity_stop_riding
每当一个实体停止骑行另一个实体时，就会触发此事件。
##### 参数
名称 | 类型 | 备注
-|-|-
entity | JavaScript实体对象 | 骑手
entity_is_being_destroyed | 布尔 | 如果为真，则骑手停止骑行是因为他（们）已经死亡
exit_from_rider | 布尔 | 如果为真，则骑手自行决定停止骑行
switching_rides | 布尔 | 如果为真，则该骑手停止骑行是因为他正在骑另一个实体