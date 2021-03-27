# Ver 1.16.210.5
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
绑定是脚本引擎提供的、可以让您修改游戏中的各种内容的功能。

## 日志绑定

### 1. log(消息)
日志可以通过服务端或客户端来查看，并且日志可以被记录在“ContentLog”文件中。在Win10设备上，它位于`%APPDATA%\..\Local\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\logs`。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
Message | 字符串 | 希望日志记录的内容。

#### 示例代码 {docsify-ignore}
日志
```javascript
system.exampleFunction = function() {
  client.log("示例日志") 
}; 
```


## 实体绑定

### 1. createEntity()
**Note:** 实体应该先在服务器上创建，随后发给客户端。如果您立即将刚创建的实体对象发送给客户端，则实体可能不会存在于客户端中。。
#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript实体对象 | 为新创建实体的对象

### 2. createEntity(类型, 模板标识符)
**Note:** 实体应该先在服务器上创建，随后发给客户端。如果您立即将刚创建的实体对象发送给客户端，则实体可能不会存在于客户端上。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
TemplateIdentifier | 字符串 | 这可以是已启用行为包中的任意实体标识符。例如，使用“minecraft:cow”将使您的实体成为一只母牛。
Type | 字符串 | 创建的实体的类型，只能是“entity”或“item_entity”。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript实体对象 | 新创建实体的对象

### 3. destroyEntity(实体)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值 {docsify-ignore}
类型 | 值（内容）
-|-
布尔 | 实体是否删除成功？

### 4. isValidEntity(EntityObject)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 该实体是否存在于脚本引擎的实体数据库中？


## 组件绑定


### 1. registerComponent(组件标识符, 组件数据)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentData | JavaScript对象 | 该对象定义字段的名称和每个字段在组件内部保存的数据。
ComponentIdentifier | 字符串 | 自定义组件的标识符。必须在命名空间中使用，既方便以后只有您才能引用它，又避免了内置组件重叠：例如“myPack：myCustomComponent”

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 组件是否注册成功？

#### 示例代码 {docsify-ignore}
注册自定义组件
```javascript
const mySystem = server.registerSystem(0, 0);

 mySystem.initialize = function() {
   this.registerComponent("myPack:myCustomComponent", { myString: "string", myNumber: 0, myBool: true, myArray: [1, 2, 3] });
 };
```

### 2. createComponent(实体, 组件标识符)
**[待求证]** 由于原文档语言过于迷惑或译者水平有限，本条目可能有错误的地方。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲添加到实体的组件的标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript组件对象 | 包含以下字段的对象，以及组件中定义的所有字段 ~~我草，以下字段在哪儿啊afk?!~~

#### 示例代码 {docsify-ignore}
创建自定义组件
```javascript
let globals = {
  ready: false
};

const mySystem = server.registerSystem(0, 0);

mySystem.initialize = function() {
  this.registerComponent("myPack:myCustomComponent", { myString: "string", myNumber: 0, myBool: true, myArray: [1, 2, 3] });
}

mySystem.update = function() {
  if(globals.ready == false) {
    globals.ready = true;
    let myEntity = this.createEntity();
    if(myEntity != null) {
      let myComponent = this.createComponent(myEntity, "myPack:myCustomComponent");
    }
  }
};
```

### 3. hasComponent(实体, 组件标识符)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲在实体中检索的组件的标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值 {docsify-ignore}
类型 | 值（内容）
-|-
布尔 | 实体中是否存在指定组件？

#### 示例代码 {docsify-ignore}
检索指定实体中是否存在指定组件。
```javascript
let globals = {
   ready: false
};

const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
   if(globals.ready == false) {
   globals.ready = true;
    let entity = this.createEntity("entity", "minecraft:pig");
    if(this.hasComponent(entity, "minecraft:nameable")) {
      // Do some work
    }
  }
```

### 4. getComponent(实体, 组件标识符)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲检索的组件标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或在事件（event）中取得。

#### 返回值 {docsify-ignore}
<table><tbody><tr> <th>类型</th><th>值（内容）</th></tr><tr>
<td>JavaScript组件对象</td>
<td>包含以下字段的对象。此外，还包括组件中定义的所有字段<br>

**JavaScript组件对象**
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>__type__</td>
<td>字符串</td>
<td>[只读] 这是对象的类型，为“component”。<br></td></tr><tr>
<td>data</td>
<td>JavaScript对象</td>
<td>这是组件的数据。<br></td></tr></tbody></table></td></tr></tbody></table>

#### 示例代码 {docsify-ignore}
从实体中获取指定的组件的数据
```javascript
let globals = {
   ready: false
};

const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
  if(globals.ready == false) {
    globals.ready = true;
    let entity = this.createEntity("entity", "minecraft:pig");
	let positionComponent = this.getComponent(entity, "minecraft:position");
    if (positionComponent != null) {
      positionComponent.data.x = 0;
      positionComponent.data.y = 0;
      positionComponent.data.z = 0;
    }
  }
};
```

### 5. applyComponentChanges(实体, 组件)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentObject | JavaScript组件对象 | 组件对象由“createComponent()”或“getComponent()”得到。
EntityObject | JavaScript实体对象 | 希望修改的实体

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 是否更改成功？

#### 示例代码 {docsify-ignore}
更新实体的组件
```javascript
let globals = {
  pig: null
};

const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
  if(globals.pig == null) {
    globals.pig = this.createEntity("entity", "minecraft:pig");
  }
  else {
    let positionComponent = this.getComponent(globals.pig, "minecraft:position");
    positionComponent.data.y += 0.1;
    this.applyComponentChanges(globals.pig, positionComponent);
  }
};
```

### 6. destroyComponent(实体, 组件标识符)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲从实体中销毁的组件的标识符。只能是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或在事件（event）中取得。

#### 返回值 {docsify-ignore}
类型 | 值（内容）
-|-
布尔 | 是否销毁成功

#### 示例代码 {docsify-ignore}
销毁实体中的组件
```javascript
let globals = {
   myEntity: null
 };

const mySystem = server.registerSystem(0, 0);
 
mySystem.initialize = function() {
  this.registerComponent("myPack:myCustomComponent", { myString: "string", myNumber: 0, myBool: true, myArray: [1, 2, 3] });
};
 
mySystem.update = function() {
  if(globals.myEntity == null) {
    globals.myEntity = this.createEntity();
  }
  else {
    if(this.hasComponent(globals.myEntity, "myPack:myCustomComponent")) {
      this.destroyComponent(globals.myEntity, "myPack:myCustomComponent");
    }
    else {
      this.createComponent(globals.myEntity, "myPack:myCustomComponent");
     }
  }
};
```


## 事件绑定
进行事件绑定后，在被绑定事件发生后脚本引擎会通知脚本以便脚本进行相应的反应。有关可以反应或触发的事件的列表详见文档的“事件”部分。

### 1. registerEventData(事件标识符, 事件数据)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
EventData | JavaScript对象 | 提供所有必须字段和事件数据的JS对象
EventIdentifier | 字符串 | 要注册的自定义组件标识符。必须使用命名空间，但不能是“minecraft”。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 是否成功注册事件？

### 2. createEventData(事件标识符)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
EventIdentifier | 字符串 | 要创建的自定义组件标识符。必须使用命名空间，但不能是“minecraft”。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript对象 | 包含事件数据（eventData）的JS对象。

### 3. listenForEvent(事件标识符, 回调函数)

#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
CallbackObject | JavaScript对象 | 该对象将被注册为回调函数。
EventIdentifier | 字符串 | 欲监听的事件的标识符，可以是内置的事件，也可以是自定义的事件。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 是否成功注册监听事件？

#### 示例代码 {docsify-ignore}
注册监听事件
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  this.listenForEvent("minecraft:client_entered_world", (eventData) => this.onClientEnteredWorld(eventData));
};

mySystem.onClientEnteredWorld = function(eventData) {
  let messageData = this.createEventData("minecraft:display_chat_event");
  messageData.data.message = "玩家进入地图";
  this.broadcastEvent("minecraft:display_chat_event", messageData);
};
```

### 4. broadcastEvent(事件标识符, 事件数据)
**[待求证]** 由于原文档语言过于迷惑或译者水平有限，本条目可能有错误的地方。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
EventData | JavaScript对象 | 要传递的事件数据，引擎负责将事件数据传递给每个监听器。
EventIdentifier | 字符串 | 要进行数据传递的事件标识符，可以是内置的事件，也可以是脚本自定义的事件。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
布尔 | 是否成功广播事件？

#### 示例代码 {docsify-ignore}
广播一个事件
```javascript
const mySystem = client.registerSystem(0, 0);

mySystem.initialize = function() {
  let eventData = this.createEventData("minecraft:display_chat_event");
  eventData.data.message = "Hello, World!";
  this.broadcastEvent("minecraft:display_chat_event", eventData);
};
```


## 实体查询
实体查询是一种用于依据实体的组件筛选实体的方法。注册查询器后，您可以调用函数取得所有过滤器捕获的实体。实体查询将仅支持查询当前在世界（level）中处于活跃状态的实体<br>
**Note:**未加载区块中的实体均为不活跃状态。

### 1. registerQuery()
注册一个查询器。查询器将包含所有符合过滤器要求的实体。<br>
这样注册的查询器不包含任何过滤器，因此它将捕获所有实体。

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript查询器对象 | 提供查询ID的对象

#### 示例代码 {docsify-ignore}
注册一个查询器
```javascript
const mySystem = server.registerSystem(0, 0);

mySystem.initialize = function() {
  let myQuery = this.registerQuery();
};
```

### 2. registerQuery(组件标识符, 组件字段1, 组件字段2, 组件字段3)
注册一个带过滤器的查询器，并定义组件中的那些字段作为过滤器以获取实体。您可以只提供一个组件标识符，也可以提供组件标识符上三个字段共同作为过滤器（如果你选择提供字段，就必须提供3个）。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
Component | 字符串 | 用作过滤器的指定组件的标识符。
ComponentField1 | 字符串 | "x"	组件中用作过滤器的第一个字段，默认为“x”
ComponentField2 | 字符串 | "y"	组件中用作过滤器的第二个字段，默认为“y”
ComponentField3 | 字符串 | "z"	组件中用作过滤器的第三个字段，默认为“z”

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript查询器对象 | 提供查询器ID的对象

#### 示例代码 {docsify-ignore}
注册一个带过滤器的查询器
```javascript
const mySystem = server.registerSystem(0, 0);

mySystem.initialize = function() {
  let spatialQuery = this.registerQuery("minecraft:position", "x", "y", "z");
};
```

### 3. addFilterToQuery(查询器, 组件标识符)
向已存在的查询器中添加过滤器（也就是组件）
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 要添加到查询器的指定组件的标识符。查询器将过滤出具有该组件的所有实体。
Query | JavaScript查询器对象 | 提供查询器ID的对象

#### 示例代码 {docsify-ignore}
向查询器添加过滤器（组件）
```javascript
let globals = {
  simpleQuery: null
};
const mySystem = server.registerSystem(0, 0);

mySystem.initialize = function() {
  globals.simpleQuery = this.registerQuery();
};

mySystem.update = function() {
  globals.simpleQuery = this.registerQuery();
  this.addFilterToQuery(globals.simpleQuery, "minecraft:explode");  let explodingEntities = this.getEntitiesFromQuery(globals.simpleQuery);
  for(var entity in explodingEntities) {
    server.log(JSON.stringify(entity));
  }
};
```

### 4. getEntitiesFromQuery(查询器)
获取指定查询器捕获的所有实体。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
Query | JavaScript查询器对象 | 使用 registerQuery()注册的查询器

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
数组 | 包含实体对象的数组，为查询器过滤得到的所有实体。

#### 示例代码 {docsify-ignore}
从查询器中获取实体
```javascript
const mySystem = server.registerSystem(0, 0);

mySystem.update = function() {
  let simpleQuery = this.registerQuery();
  let allEntities = this.getEntitiesFromQuery(simpleQuery);
  for(var entity in allEntities) {
    server.log(JSON.stringify(entity));
  }
};
```

### 5. getEntitiesFromQuery(查询器, 组件字段1_最小, 组件字段2_最小, 组件字段3_最小, 组件字段1_最大, 组件字段2_最大, 组件字段3_最大)
获取带组件（过滤器）的查询器所捕获的实体。通过该函数获取将只返回在过滤器（组件）指定的三个字段中具有要求值的实体。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
ComponentField1_Max | 小数 | 组件字段1在实体中的最大值
ComponentField1_Min | 小数 | 组件字段1在实体中的最小值
ComponentField2_Max | 小数 | 组件字段2在实体中的最大值
ComponentField2_Min | 小数 | 组件字段2在实体中的最小值
ComponentField3_Max | 小数 | 组件字段3在实体中的最大值
ComponentField3_Min | 小数 | 组件字段3在实体中的最小值
Query | JavaScript查询器对象 | 使用 registerQuery(...)注册的查询器

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
数组 | 包含实体对象的数组，为查询器过滤得到的所有实体。

#### 示例代码 {docsify-ignore}
从查询器中获取实体
```javascript
let globals = {
  spatialQuery: null
};

const mySystem = server.registerSystem(0, 0);

mySystem.initialize = function() {
  globals.spatialQuery = this.registerQuery("minecraft:position", "x", "y", "z");
};

mySystem.update = function() {
  let closeEntities = this.getEntitiesFromQuery(globals.spatialQuery, 0, 10, 0, 5, 0, 10);
  for(var entity in closeEntities) {
    server.log(JSON.stringify(entity));
  }
};
```


## 方块绑定
这些函数可以定义如何与方块进行交互。

### 1. getBlock(常加载区域, x, y, z)
提供方块位置时，你可以在存档中获取指定方块的信息，但指定方块必须在常加载区域内。

名称 | 类型 | 备注
-|-|-
Ticking Area | JavaScript常加载区域对象 | 方块所在的常加载区域
x | 整数 | 指定方块的x坐标值
y | 整数 | 指定方块的y坐标值
z | 整数 | 指定方块的z坐标值

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
 | 对象
 | NULL

 ### 2. getBlock(常加载区域, 坐标对象) 
获取方块的信息，指定方块需要在常加载区域内。
#### 参数 {docsify-ignore}
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>PositionObject</td>
<td>JavaScript对象</td>
<td>提供方块位置(x,y与z)的JavaScript对象<br>

**参数**
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>x</td>
<td>整数</td>
<td>x坐标<br></td></tr><tr>
<td>y</td>
<td>整数</td>
<td>y坐标<br></td></tr><tr>
<td>z</td>
<td>整数</td>
<td>z坐标<br></td></tr></tbody></table></td></tr><tr>
<td>Ticking Area</td>
<td>JavaScript常加载区域对象</td>
<td>方块所在的常加载区域<br></td></tr></tbody></table>

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
JavaScript方块对象 | 具有指定方块的对象


### 3. getBlocks(常加载区域, 最小x坐标, 最小y坐标, 最小z坐标, 最大x坐标, 最大y坐标, 最大z坐标)
提供最小和最大坐标时，您可以从世界上批量获得许多方块的信息。这些方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
Ticking Area | JavaScript常加载区域对象 | 指定方块所在的常加载区域
x max | 整数 | 最大x的坐标值
x min | 整数 | 最小x的坐标值
y max | 整数 | 最大y的坐标值
y min | 整数 | 最小y的坐标值
z max | 整数 | 最大z的坐标值
z min | 整数 | 最小z的坐标值

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
 | 数组
 | NULL

### 4. getBlocks(常加载区域, 最小坐标对象, 最大坐标对象)
提供最小和最大坐标时，您可以从世界上批量获得许多方块的信息。这些方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
#### 参数 {docsify-ignore}
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>Maximum PositionObject</td>
<td>JavaScript对象</td>
<td>具有所需最小坐标(x,y与z)的坐标对象<br>

**参数**
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>x</td>
<td>整数</td>
<td>x坐标<br></td></tr><tr>
<td>y</td>
<td>整数</td>
<td>y坐标<br></td>
</tr><tr><td>z</td>
<td>整数</td>
<td>z坐标<br></td></tr></tbody></table></td></tr><tr>
<td>Minimum PositionObject</td>
<td>JavaScript对象</td>
<td>具有所需最大坐标(x,y与z)的坐标对象<br>

**参数**
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>x</td>
<td>整数</td>
<td>x坐标<br></td></tr><tr>
<td>y</td>
<td>整数</td>
<td>y坐标<br></td></tr><tr>
<td>z</td>
<td>整数</td>
<td>z坐标<br></td></tr></tbody></table></td></tr><tr>
<td>Ticking Area</td>
<td>JavaScript常加载区域对象</td>
<td>指定方块所在的常加载区域<br></td></tr></tbody></table>

#### 返回值 {docsify-ignore}

类型 | 值（内容）
-|-
数组 | 一个方块对象的3D数组，索引是相对于给定最小坐标的方块位置
 | NULL


## 斜杠命令

### 1. executeCommand(命令, 回调)

名称 | 类型 | 备注
-|-|-
Callback | Json对象 | 命令执行后的回调对象
Command | 字符串 | 欲执行的命令

#### 示例代码 {docsify-ignore}
```javascript
system.executeCommand("/fill ~ ~ ~ ~100 ~5 ~50 stone", (commandResultData) => this.commandCallback(commandResultData));

system.commandCallback = function (commandResultData) {
  let eventData = this.createEventData("minecraft:display_chat_event");
  if (eventData) {
    eventData.data.message = message;
    this.broadcastEvent("minecraft:display_chat_event", "回调！命令： " + commandResultData.command + " 数据: " + JSON.stringify(commandResultData.data, null, "    ") );
  }
};
```