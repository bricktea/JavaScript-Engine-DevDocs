<script>window.$docsify={subMaxLevel: 4}</script>
# Ver 1.16.0.2 
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
在这里，您可以找到脚本可以监听和响应的事件列表。


## 客户端事件


### 监听事件
以下事件可通过脚本引擎监听，您可以在脚本中设置监听它们并对其作出相应。

#### 1. minecraft:client_entered_world
每当玩家加入世界时就会触发此事件。事件数据为玩家的实体对象。

#### 2. minecraft:hit_result_changed
每当玩家十字准星（或触屏模式的圆圈）从其他指向其他地方改为指向实体时，都会触发此事件，最大支持范围是1000个格。
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
（参考上个条目），每次指向更新都会触发此事件，并且范围支持1000格以上。
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
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。移开时返回null
position | Vector [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，移开时返回null

##### 代码示例
相应“pick_hit_result_changed”事件。
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
entity | JavaScript实体对象 | 当指向实体时，返回该实体对象。移开时返回null
position | Vector [a, b, c] | 当指向一个实体或方块时返回该实体或方块的坐标，移开时返回null

##### 代码示例
相应“pick_hit_result_continuous”事件。
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
此事件用于向特定玩家发送聊天消息。事件数据就是纯文本消息。支持特殊格式，就像玩家发送一样。
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
options | Json对象 | 您可以通过设置true或false来定义以下GUI选项：<br><br> **1. absorbs_input** <br> 如果为true，则输入就不会再传递给在该界面下的其他界面 <br><br> **2. always_accepts_input** <br> 如果为true，则即使其他GUI显示在其顶部该GUI依然始终接收并处理输入（除非它不在UI栈中了）。<br><br> **3. force_render_below** <br> 如果为true，则该GUI会被强制渲染，即使它的上层有其他GUI（甚至HUD）正在其上层渲染。<br><br> **4. is_showing_menu** <br> 如果为true，GUI将被视为暂停菜单，并且不允许在此GUI上层再显示暂停菜单。<br><br> **5. render_game_behind** <br> 如果为true，则游戏将继续在此GUI下方呈现。<br><br> **6. render_only_when_topmost**<br>如果为true，则此GUI只显示在UI栈顶层。<br><br> **7. should_steal_mouse**<br> 如果为true，则屏幕将捕获鼠标指针并将其移动范围限制在GUI上。
path | 字符串 | 定义GUI的HTML文件路径

#### 3. minecraft:script_logger_config
