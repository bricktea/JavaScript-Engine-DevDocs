# Ver 1.16.210.5
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
在这里，你可以找到一些Script API关于对象和返回类型的定义

## 脚本API对象


### 1. JavaScript 实体对象
名称 | 类型 | 备注
-|-|-
\_\_identifier\_\_ | 字符串 | [只读] 这是命名空间对于指定实体的对象标识符。例如牛(cow)，标识符为minecraft:cow。
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，可以是“entity”或“item_entity”。
id | 正整数 | [只读] 这是实体的唯一标识符。

### 2. JavaScript 存档对象
名称 | 类型 | 备注
-|-|-
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为“level”。
level_id | 整数 | [只读] 这是level的唯一标识符。

### 3. JavaScript 组件对象
名称 | 类型 | 备注
-|-|-
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为“component”。
data | JavaScript对象 | 这是组件的数据。

### 4. JavaScript 查询器对象
名称 | 类型 | 备注
-|-|-
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为“query”。
query_id | 整数 | [只读] 这是Query的唯一标识符。

### 5. JavaScript 物品堆对象
名称 | 类型 | 备注
-|-|-
\_\_identifier\_\_ | 字符串 | [只读] 这是命名空间对于指定实体的对象标识符。例如牛(cow)，标识符为minecraft:cow。
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为“item_stack”。
count | 字符串 | [只读] 这是物品堆叠的数量。
item | 字符串 | [只读] 这是物品的唯一标识符。

### 6. JavaScript 方块对象
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

### 7. JavaScript 实体常加载区域对象
名称 | 类型 | 备注
-|-|-
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为 "entity_ticking_area"。
entity_ticking_area_id | 整数 | [只读] 这是实体常加载区域的唯一标识符。

### 8. JavaScript 存档常加载区域对象
名称 | 类型 | 备注
-|-|-
\_\_type\_\_ | 字符串 | [只读] 这是对象的类型，为“level_ticking_area”。
level_ticking_area_id | 字符串 | [只读] 这是ticking area的唯一标识符。