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
<td>[只读] 这是方块的位置。是它唯一标识符的一部分。<br>
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
item | 字符串 | [只读] 这是Item的唯一标识符。

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
获取方块的NBT信息，指定方块需要在常加载区域内。
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
提供方块位置时，你可以在存档中获取指定方块的NBT信息，但指定方块必须在常加载区域内。

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
提供最小和最大坐标时，您可以从世界上获得许多方块的NBT信息。方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
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
提供最小和最大坐标时，您可以从世界上获得许多方块的NBT信息。方块必须在常加载区域内。 如果最终选定了很多块，此调用可能会很慢，因此应不经常使用。
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