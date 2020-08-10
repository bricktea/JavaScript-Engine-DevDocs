# Ver 1.16.0.2 
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
在这里，你可以找到一些Script API关于对象和返回类型的定义

## 脚本API对象


### 1. JavaScript 方块对象
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>__identifier__</td>
<td>字符串</td>
<td>[只读] 这是命名空间对于指定方块的对象标识符。例如基岩(bedrock)方块，标识符为 minecraft:bedrock。<br></td></tr><tr>
<td>__type__</td>
<td>字符串</td>
<td>[只读] 这是对象的类型，为“block”。<br></td></tr><tr>
<td>block_position</td>
<td>JavaScript对象</td>
<td>[只读] 这是方块的位置。<br>
<!-- Markdown表格不支持嵌套就离谱 -->

**参数**
<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th></tr><tr>
<td>x</td>
<td>整数</td>
<td>x 坐标<br></td></tr><tr>
<td>y</td>
<td>整数</td>
<td>y 坐标<br></td></tr><tr>
<td>z</td>
<td>整数</td>
<td>z 坐标<br></td></tr></tbody></table>
</td></tr><tr>
<td>ticking_area</td>
<td>JavaScript 对象</td>
<td>[只读] 这用于常加载区块对象获取此方块。<br></td></tr></tbody></table>

### 2. JavaScript 组件对象
名称 | 类型 | 备注
-|-|-
__type__ | 字符串 | [只读] 这是对象的类型，为“component”。
data | JavaScript对象 | 这是组件的数据。

### 3. JavaScript 物品堆对象
名称 | 类型 | 备注
-|-|-
__identifier__ | 字符串 | [只读] 这是命名空间对于指定实体的对象标识符。例如牛(cow)，标识符为minecraft:cow。
__type__ | 字符串 | [只读] 这是对象的类型，为“item_stack”。
count | 字符串 | [只读] 这是物品堆叠的数量。
item | 字符串 | [只读] 这是物品的唯一标识符。

### 4. JavaScript 实体对象
名称 | 类型 | 备注
-|-|-
__identifier__ | 字符串 | [只读] 这是命名空间对于指定实体的对象标识符。例如牛(cow)，标识符为minecraft:cow。
__type__ | 字符串 | [只读] 这是对象的类型，可以是“entity”或“item_entity”。
id | 正整数 | [只读] 这是实体的唯一标识符。

### 5. JavaScript 存档对象
名称 | 类型 | 备注
-|-|-
__type__ | 字符串 | [只读] 这是对象的类型，为“level”。
level_id | 整数 | [只读] 这是level的唯一标识符。

### 6. JavaScript 实体常加载区域对象
名称 | 类型 | 备注
-|-|-
__type__ | 字符串 | [只读] 这是对象的类型，为 "entity_ticking_area"。
entity_ticking_area_id | 整数 | [只读] 这是实体常加载区域的唯一标识符。

### 7. JavaScript 存档常加载区域对象
名称 | 类型 | 备注
-|-|-
__type__ | 字符串 | [只读] 这是对象的类型，为“level_ticking_area”。
level_ticking_area_id | 字符串 | [只读] 这是ticking area的唯一标识符。

### 8. JavaScript 查询器对象
名称 | 类型 | 备注
-|-|-
__type__ | 字符串 | [只读] 这是对象的类型，为“query”。
query_id | 整数 | [只读] 这是Query的唯一标识符。

### Notes
**[常加载区域对象]** 有两种：实体（Entity）和存档（Level）。当您需要常加载区域时您可以选择任意一种作为参数。<br>
**[内容绑定]** 是脚本引擎提供的修改游戏中内容的功能。<br>
**[方块绑定]** 定义玩家如何与方块交互


## 获取方块信息


### 1. getBlock()
获取方块的信息，指定方块需要在常加载区域内。
#### 参数
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

#### 返回值

类型 | 值（内容）
-|-
JavaScript方块对象 | 具有指定方块的对象

### 2. getBlock(常加载区域, x, y, z)
提供方块位置时，你可以在存档中获取指定方块的信息，但指定方块必须在常加载区域内。

名称 | 类型 | 备注
-|-|-
Ticking Area | JavaScript常加载区域对象 | 方块所在的常加载区域
x | 整数 | 指定方块的x坐标值
y | 整数 | 指定方块的y坐标值
z | 整数 | 指定方块的z坐标值

#### 返回值

类型 | 值（内容）
-|-
 | 对象
 | NULL

### 3. getBlocks(常加载区域, 最小坐标, 最大坐标)
提供最小和最大坐标时，您可以从世界上批量获得许多方块的信息。这些方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
#### 参数
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

#### 返回值

类型 | 值（内容）
-|-
数组 | 一个方块对象的3D数组，索引是相对于给定最小坐标的方块位置
 | NULL

### 4. getBlocks(常加载区域, 最小x坐标, 最小y坐标, 最小z坐标, 最大x坐标, 最大y坐标, 最大z坐标)
提供最小和最大坐标时，您可以从世界上批量获得许多方块的信息。这些方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
#### 参数

名称 | 类型 | 备注
-|-|-
Ticking Area | JavaScript常加载区域对象 | 指定方块所在的常加载区域
x max | 整数 | 最大x的坐标值
x min | 整数 | 最小x的坐标值
y max | 整数 | 最大y的坐标值
y min | 整数 | 最小y的坐标值
z max | 整数 | 最大z的坐标值
z min | 整数 | 最小z的坐标值

#### 返回值

类型 | 值（内容）
-|-
 | 数组
 | NULL


## 组件绑定


### 1. registerComponent(组件标识符, 组件数据)
使用此函数创建的组件仅在脚本引擎运行并且脚本加载时存在，反之则不存在。存在的组件可以在实体中被添加，删除和更新。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentData | JavaScript对象 | 该对象定义字段的名称和每个字段在组件内部保存的数据。
ComponentIdentifier | 字符串 | 自定义组件的标识符。必须在命名空间中使用，既方便以后只有您才能引用它，又避免了内置组件重叠：例如“myPack：myCustomComponent”

#### 返回值

类型 | 值（内容）
-|-
布尔 | 组件是否注册成功？

#### 示例代码
注册自定义组件
```
const mySystem = server.registerSystem(0, 0);

 mySystem.initialize = function() {
   this.registerComponent("myPack:myCustomComponent", { myString: "string", myNumber: 0, myBool: true, myArray: [1, 2, 3] });
 };
```

### 2. createComponent(实体, 组件标识符)
将组件添加到指定实体。在此之前请确保指定组件已注册，如果组件在此之前已添加到实体，则返回数据是那个已经存在的组件。<br><br>
**[待求证]** 由于原文档语言过于迷惑或译者水平有限，本条目可能有错误的地方。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲添加到实体的组件的标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值

类型 | 值（内容）
-|-
JavaScript组件对象 | 包含以下字段的对象，以及组件中定义的所有字段 ~~我草，以下字段在哪儿啊afk?!~~

#### 示例代码
创建自定义组件
```
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

### 3. destroyComponent(实体, 组件标识符)
从给定的实体中删除指定的组件。目前这仅适用于自定义组件，不能删除为Json配置中的实体定义的组件。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲从实体中销毁的组件的标识符。只能是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或在事件（event）中取得。

#### 返回值
类型 | 值（内容）
-|-
布尔 | 是否销毁成功

#### 示例代码
销毁实体中的组件
```
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

### 4. getComponent(实体, 组件标识符)
在实体中检索指定的组件。如果组件的确存在于实体中，则获得实体中该组件的数据。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲检索的组件标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或在事件（event）中取得。

#### 返回值
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

#### 示例代码
从实体中获取指定的组件的数据
```
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
将对组件做的更改应用回实体，并且立即生效。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentObject | JavaScript组件对象 | 组件对象由“createComponent()”或“getComponent()”得到。
EntityObject | JavaScript实体对象 | 希望修改的实体

#### 返回值

类型 | 值（内容）
-|-
布尔 | 是否更改成功？

#### 示例代码
更新实体的组件
```
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

### 6. hasComponent(实体, 组件标识符)
检索指定实体中是否存在指定组件。
#### 参数

名称 | 类型 | 备注
-|-|-
ComponentIdentifier | 字符串 | 欲在实体中检索的组件的标识符。可以是内置组件的标识符（请看“组件与Script”部分），也可以是通过调用“registerComponent()”创建的自定义组件。
EntityObject | JavaScript实体对象 | 可以通过创建一个实体“createEntity()”或从事件（event）中取得。

#### 返回值
类型 | 值（内容）
-|-
布尔 | 实体中是否存在指定组件？

#### 示例代码
检索指定实体中是否存在指定组件。
```
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