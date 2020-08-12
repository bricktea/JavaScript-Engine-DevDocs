# Ver 1.16.0.2 
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
这里是Minecraft脚本引擎中可用的属性、附加属性和组件的文档<br>
有两种组件：服务端组件和客户端组件。我们将在后面的部分详细介绍它们的含义。<br>
组件在实体中可以被添加、检索、更新和删除，它们不是独立存在的，当前，只有自定义的“附加组件”可以被从实体中删除。组件需要附加到实体才能被检索或更新。<br>
查看“组件绑定”部分以了解如何对组件进行增、删、改、查。本节介绍每个组件的特定API。<br>


## 方块组件
这些组件仅存在于方块对象上。

### 1. minecraft:blockstate
该组件包含Block对象上的所有方块状态。方块状态涵盖了方块方方面面的不同的属性，从方块的摆放方向到它们的类型。方块状态由数字，布尔或字符串表示。请参考“Blockstates”文档以查看每个状态的有效值。该组件允许获取和设置这些状态。
#### 示例代码
```
let blockstateComponent = this.getComponent(block, "minecraft:blockstate");
blockstateComponent.data.coral_color = "blue";
this.applyComponentChanges(block, blockstateComponent);
```
## 客户端组件
这些组件仅在启动脚本引擎的客户端上运行，并且只能在客户端脚本中使用。

### 1. minecraft:molang
使用MoLang组件可以访问实体中的MoLang变量。想要了解有关MoLang变量的更多信息，请查看其他附加文档。 在脚本中，您可以获取并设置在实体的Json文件中定义的这些变量。由于MoLang变量的格式（例如，variable.isgrazing）因此必须在对象上使用“[]”运算符来访问变量。下面的示例说明如何使用[]运算符访问变量。
#### 示例代码
```
let molangComponent = this.createComponent(entity, "minecraft:molang"); 
molangComponent["variable.molangexample"] = 1.0; 
this.applyComponentChanges(molangComponent); 
```


## 存档组件
这些是属于存档的组件。它们只在level对象中存在，并且不能从level对象中删除。您可以通过全局server对象获取该组件并更改其数据。

### 1. minecraft:ticking_areas
该组件可以访问存档中的静态常加载区域。该组件数组包含存档中的常加载区域，可以通过名称或UUID进行访问。

### 2. minecraft:weather
该组件用于更改存档中的天气，可以单独更改雷、雨级别，甚至能完全禁用默认的天气周期。
#### 参数

名称 | 类型 | 备注
-|-|-
do_weather_cycle | 布尔 | 决定是否使用默认的天气周期
lightning_level | 小数 | 一个介于0-1的值，决定有多少雷电
lightning_time | 整数 | 闪电和雷声持续多长时间，以Tick为单位
rain_level | 小数 | 一个介于0-1的值，决定降雨的强度
rain_time | 整数 | 雨将持续多长事件，以Tick为单位

## 服务器组件
这里是在服务器上运行并与客户端状态同步的组件。
每个组件的字段基本匹配其Json对应项（但也有一些区别需要注意）。

### 1.minecraft:armor_container
此组件表示实体的装甲物品栏的内容。该组件包含一个JavaScript物品堆叠对象数组，这些数组代表装甲栏里的插槽。<br>
Note:目前对于脚本来说装甲栏是只读的；物品堆叠数组对应装甲插槽从头到脚排序。
```
//此示例为在玩家攻击实体后检查玩家头盔盔甲插槽中是否有特定物品。
system.listenForEvent("minecraft:player_attacked_entity", function(eventData) {
    //获取玩家的装甲栏信息
    let playerArmor = system.getComponent(eventData.data.player, "minecraft:armor_container");
    //获取头盔栏信息
    let playerHelmet = playerArmor.data[0];
    //如果玩家装备了金头盔，就删除被攻击实体。
    if (playerHelmet.item == "minecraft:golden_helmet") {
        system.destroyEntity(eventData.data.attacked_entity);
    }
});
```

### 2. minecraft:attack
该组件控制实体的“攻击伤害”属性。可以通过该组件修改当前实体攻击伤害的最大值和最小值。一旦更改，立即生效。任何附加的攻击伤害（无论增加或减少）不受影响。
#### 参数

<table><tbody><tr><th>名称</th> <th>类型</th><th>备注</th></tr><tr>
<td>damage</td>
<td>范围 [a, b]</td>
<td>攻击时（仅近战）对被攻击实体造成的伤害范围。若设置为负值会增加被攻击实体的生命值。<br><br><table>
<tbody><tr> <th>名称</th> <th>类型</th><th>默认值</th> <th>备注</th> </tr><tr>
<td>range_max</td>
<td>小数</td>
<td>0.0</td>
<td>被攻击实体的最大被伤害值<br></td></tr><tr>
<td>range_min</td>
<td>小数</td>
<td>0.0</td>
<td>被攻击实体的最小被伤害值<br></td></tr></tbody></table></td></tr></tbody></table>

### 3. minecraft:collision_box
该组件控制实体的碰撞盒。碰撞盒规定实体的“实际的”占用空间。一旦更改，立即生效。<BR>
**警告：**更改碰撞盒可能导致实体卡在方块里并窒息。
#### 参数

名称 | 类型 | 默认值 | 备注
-|-|-|-
height | 小数 | 1.0	碰撞盒高度（以块为单位），若设置为负值会当0处理。
width | 小数 | 1.0	碰撞盒的宽度（以块为单位），若设置为负值会当0处理。

### 4. minecraft:container
该组件表示方块容器信息。该组件包含一个JavaScript物品堆叠对象数组，这些数组代表容器物品栏里的插槽。该组件仅用于方块，有关实体的相似组件请参考“inventory”组件。
**Note:**目前对于脚本来说装甲栏是只读的；物品堆叠数组对应装甲插槽从头到脚排序。
```
//本示例将检查指定方块是否是容器方块（是否具有容器属性）。
let block; //通过 getBlock()获取。
let has_container = system.hasComponent(block, "minecraft:container");
if (has_container === true) {
  let container = system.getComponent(block, "minecraft:container");
  //现在可以读取容器内的物品信息了。
```

### 5. minecraft:damage_sensor
该组件定义当特定的实体被损害时该实体时该实体一系列需要调用的事件。
#### 参数

<table><tbody><tr> <th>名称</th> <th>类型</th><th>备注</th> </tr><tr>
<td>triggers</td>
<td>列表</td>
<td>实体受到特定类型损害时要被触发事件的触发器列表。<br><br><table>
<tbody><tr> <th>名称</th> <th>类型</th><th>默认值</th> <th>备注</th> </tr><tr>
<td>cause</td>
<td>字符串</td>
<td>none</td>
<td>伤害类型<br></td></tr><tr>
<td>damage_multiplier</td>
<td>小数</td>
<td>1.0</td>
<td>一个乘数。基本损害乘该乘数等于实际损害。如果deals_damage为true，则该值只能将实体受到的伤害降低到最低1。<br></td></tr><tr>
<td>deals_damage</td>
<td>布尔</td>
<td>true</td>
<td>如果为true，将从其生命值中扣除其受到的伤害值，将其设置为false可以使实体忽略该伤害。<br></td></tr><tr>
<td>on_damage</td>
<td>Json对象</td>
<td></td>
<td>指定事件的过滤器。<br></td></tr><tr>
<td>on_damage_sound_event</td>
<td>字符串</td>
<td></td>
<td>定义在满足on_damage过滤器时播放的声音（如果有）。<br></td></tr></tbody></table></td></tr></tbody></table>

```
//下面的示例为让一个实体（本例为苦力怕）在被玩家攻击时启动爆炸。
//注意：实体必须在其Json中定义有“damage_sensor”组件和与之相关的事件
this.listenForEvent("minecraft:player_attacked_entity", function(eventData) {
  let damageSensorComponent = serverSystem.getComponent(eventData.attacked_entity, "minecraft:damage_sensor");
  damageSensorComponent.data[0].on_damage = { event:"minecraft:start_exploding", filters:[{test:"has_component", operator:"==", value:"minecraft:breathable"}] };
  serverSystem.applyComponentChanges(eventData.attacked_entity, damageSensorComponent);
});
```

### 6. minecraft:equipment
定义实体使用的战利品表以定义它的装备。在更改被应用后，备选装备会重新滚动，然后新的实体装备会被选中。
#### 参数

名称 | 类型 | 备注
-|-|-
slot_drop_chance | 列表 | 一个可以从中掉落装备物品的插槽列表
table | 字符串 | 装备表文件的路径，相对于行为包根目录

### 7. minecraft:equippable
定义实体的装备行为
#### slots
带有物品槽和可装备物品的列表
##### 参数

名称 | 类型 | 默认值 | 备注
-|-|-|-
accepted_items | 列表 |  | 可以装备进该槽位的物品的列表
interact_text | 字符串 |  | 使用触摸屏控制时，实体可以配备此物品时要显示的文本。
item | 字符串 |  | 	可以装备进该槽位的物品的标识符
on_equip | 字符串 |  | 当此实体装备此物品时触发的事件。
on_unequip | 字符串 |  | 当此实体取消装备此物品时触发的事件。
slot | 整数 | 0 | 该槽位的槽位编号

### 8. minecraft:explode
定义实体爆炸的方式
#### 参数

名称 | 类型 | 默认值 | 备注
-|-|-|-
breaks_blocks | 布尔 | 真 | 如果为真，则实体爆炸时将破坏爆炸半径内的方块。
causes_fire | 布尔 | 假 | 如果为真，则爆炸半径内将产生火。
destroy_affected_by_griefing | 布尔 | 假 | 如果为真，爆炸是否破坏方块将受游戏规则“mobGriefing”控制。
fire_affected_by_griefing | 布尔 | 假 | 如果为真，爆炸是否产生火将受游戏规则“mobGriefing”控制。
fuse_length | 范围 [a, b] | [0.0, 0.0] | 爆炸启动到爆炸 其中间时间的范围。若设置为负值，爆炸将立即发生。
fuse_lit | 布尔 | 假 | 如果为真，实体生成后将立即启动爆炸
max_resistance | 小数 | 3.40282e+38 | 爆炸发生时，方块的最高爆炸抗性
power | 小数 | 3 | 爆炸半径（以方块为单位）和爆炸的破坏程度。

### 9. minecraft:hand_container
此组件代表实体手中的物品。该组件包含一个JavaScript物品堆对象数组，这些数组代表容器中的每个插槽。<br>
**Notes：**现在的物品和容器是只读的。插槽0为主用插槽，1为副手插槽。
```
// 此示例将在玩家攻击实体后检查玩家的副手插槽中是否有指定物品。
system.listenForEvent("minecraft:player_attacked_entity", function(eventData) {
    // 获取玩家的“手”容器
    let handContainer = system.getComponent(eventData.data.player, "minecraft:hand_container");
    // 获取玩家副手中的物品信息
    let offhandItem = handContainer.data[1];
    // 如果玩家副手插槽是不死图腾，则删除被攻击实体
    if (offhandItem.item == "minecraft:totem") {
        system.destroyEntity(eventData.data.attacked_entity);
    }
});
```

### 10. minecraft:healable
定义此实体能否被玩家交互活动所治愈。
#### 参数

<table><tbody><tr> <th>名称</th> <th>类型</th> <th>默认值</th> <th>备注</th> </tr><tr>
<td>filters</td>
<td>Minecraft过滤器</td><td></td>
<td>定义治愈实体的条件的过滤器组。<br></td></tr><tr>
<td>force_use</td>
<td>布尔</td>
<td>假</td>
<td>确定可以使用项，无论实体是否处于活跃状态。<br></td></tr><tr>
<td>items</td>
<td>数组</td><td></td>
<td>可以用来治愈该实体的物品<br><table><tbody><tr>
<th>名称</th> <th>类型</th><th>默认值</th> <th>备注</th> </tr><tr>
<td>heal_amount</td>
<td>整数</td>
<td>1</td>
<td>当玩家给实体喂食该物品时，该实体将获得生命值（不会超过限制的生命值）。<br></td></tr><tr>
<td>item</td>
<td>字符串</td><td></td>
<td>可以用于治愈该实体的物品标识符。<br></td></tr></tbody></table></td></tr></tbody></table>

### 11. minecraft:health
定义实体当前的生命值与最大的生命值。当组件被应用回实体，实体生命值立即发生变化，如果生命值≤0，实体将死亡。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
max | Integer | 10 | 实体的最大生命值
value | Integer | 1 | 实体当前的生命值

### 12. minecraft:hotbar_container
此组件表示玩家的常备物品栏。该组件包含一个物品堆JavaScript对象数组，表示常备物品栏中的每个插槽。<br>
**注意：**当前物品和容器只读。插槽从左到右排列。
```
// 此示例将在玩家攻击实体后检查玩家的第一个常备物品栏插槽中是否有特定物品。
system.listenForEvent("minecraft:player_attacked_entity", function(eventData) {
    // 获取玩家的常备物品栏
    let playerHotbar = system.getComponent(eventData.data.player, "minecraft:hotbar_container");
    // 获取玩家常备物品栏第一个物品的信息
    let firstHotbarSlot = playerHotbar.data[0];
    // 如果为苹果，删除被攻击实体
    if (firstHotbarSlot.item == "minecraft:apple") {
        system.destroyEntity(eventData.data.attacked_entity);
    }
});
```

### 13. minecraft:interact
定义对于应用了该组件的实体，玩家如何与之交互。
#### 参数

<table><tbody><tr><th>名称</th><th>类型</th><th>默认值</th><th>备注</th></tr><tr>
<td>add_items</td>
<td>Json对象</td><td></td>
<td>交互成功后会被添加到背包的物品的列表<br><table>
<tbody><tr><th>名称</th> <th>类型</th><th>默认值</th><th>备注</th></tr><tr>
<td>table</td>
<td>字符串</td><td></td>
<td>物品列表文件路径，相对于行为包根目录<br></td></tr></tbody></table></td></tr><tr>
<td>cooldown</td>
<td>小数</td>
<td>0.0</td>
<td>互动冷却时间，以秒为单位<br></td></tr><tr>
<td>hurt_item</td>
<td>整数</td>
<td>0</td>
<td>使用物品与该实体互动时物品将损失的耐久量，0表示不会失去耐久<br></td></tr><tr>
<td>interact_text</td>
<td>字符串</td><td></td>
<td>使用触摸屏控制时，玩家可以以这种方式与实体互动时要显示的文本。<br></td></tr><tr>
<td>on_interact</td>
<td>字符串</td><td></td>
<td>互动发生时要触发的事件的标识符。<br></td></tr><tr>
<td>particle_on_start</td>
<td>Json对象</td><td></td>
<td>互动开始时触发的粒子效果<br><table>
<tbody><tr><th>名称</th><th>类型</th><th>默认值</th><th>备注</th> </tr><tr>
<td>particle_offset_towards_interactor</td>
<td>布尔</td>
<td>假</td>
<td>粒子是否在离互动者较近的位置生成<br></td></tr><tr>
<td>particle_type</td>
<td>字符串</td><td></td>
<td>要产生的粒子的类型<br></td></tr><tr>
<td>particle_y_offset</td>
<td>小数</td>
<td>0.0</td>
<td>粒子生成位置在y轴上的偏移量<br></td></tr></tbody></table></td></tr><tr>
<td>play_sounds</td>
<td>数组</td><td></td>
<td>互动发生时要播放的声音的标识符数组<br></td></tr><tr>
<td>spawn_entities</td>
<td>数组</td><td></td>
<td>互动发生时产生的实体的标识符数组<br></td></tr><tr>
<td>spawn_items</td>
<td>Json对象</td><td></td>
<td>交互成功后会掉落到地面的物品的列表<br><table>
<tbody><tr><th>名称</th> <th>类型</th><th>默认值</th> <th>备注</th> </tr><tr>
<td>table</td>
<td>字符串</td><td></td>
<td>物品列表文件路径，相对于行为包根目录<br></td></tr></tbody></table></td></tr><tr>
<td>swing</td>
<td>布尔</td>
<td>假</td>
<td>如果为真，则玩家与该实体互动时将播放“swing”动画<br></td></tr><tr>
<td>transform_to_item</td>
<td>字符串</td><td></td>
<td>成功交互后要将已有物品转变的物品，格式“itemName:auxValue”<br></td></tr><tr>
<td>use_item</td>
<td>布尔</td>
<td>假</td>
<td>如果为真，互动将消耗一个物品<br></td></tr></tbody></table>

### 14. minecraft:inventory
定义实体的背包（大小，限制等），当前不允许更改实体背包的内容
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
additional_slots_per_strength | 整数 | 0 | 每个额外的运输力度会给实体的背包增加的槽位的数量
can_be_siphoned_from | 布尔 | 假 | 如果为真，漏斗可以漏走背包中的物品
container_type | 字符串 | None | 实体背包容器的类型，可以是horse（马），minecart_chest（箱子矿车），minecart_hopper（漏斗矿车），inventory，container或hopper（漏斗）
inventory_size | 整数 | 5 | 背包容器的插槽数量
private | 布尔 | 假 | 如果为真，只有实体可以访问背包
restrict_to_owner | 布尔 | 假 | 如果为真，只有实体与其主人可以访问其背包

### 15. minecraft:inventory_container
此组件代表实体的物品栏。该组件包含一个JavaScript物品堆叠对象数组，这些数组代表背包中的每个插槽。<br>
**注意：**目前背包和物品是只读的。0-8是常备栏，9-16是背包第一行，17-24是中间行，25-32是最后一行。
```
// 此示例将在玩家攻击实体后检查玩家的第三个背包物品栏插槽中是否有特定物品。
system.listenForEvent("minecraft:player_attacked_entity", function(eventData) {
    // 获取玩家的背包
    let playerInventory = system.getComponent(eventData.data.player, "minecraft:inventory_container");
    // 获取背包第三个插槽中的物品信息
    let thirdItemSlot = playerInventory.data[2];
    // 如果是苹果，删除被攻击的实体
    if (thirdItemSlot.item == "minecraft:apple") {
        system.destroyEntity(eventData.data.attacked_entity);
    }
});
```

### 16. minecraft:lookat
使一个实体看着另一个实体。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
allow_invulnerable | 布尔 | 假 | 如果为真。无敌实体（如在创造模式的玩家）将被视为有效目标
filters | Minecraft过滤器 |  | 定义可以触发此组件的实体。
look_cooldown | 范围 [a, b] | [0, 0] | 实体“冷静下来”并且不会生气或寻找目标的随机时间范围。
look_event | 字符串 |  | 当过滤器中指定的实体看向该实体时会运行的事件。
search_radius | 小数 | 10 | 可理解为实体的视力。即实体与最远可寻找实体的距离。
set_target | 布尔 | 真 | 如果为真，则此实体会将攻击目标设置为正在看着它的实体。

### 17. minecraft:nameable
该组件定义一个实体被应用该组件后能否被命名牌命名以及名字显示与否。另外，脚本引擎允许脚本直接通过属性“name”来命名实体。
#### 参数

<table><tbody><tr><th>名称</th><th>类型</th> <th>默认值</th><th>备注</th></tr><tr>
<td>allow_name_tag_renaming</td>
<td>布尔</td>
<td>真</td>
<td>如果为真，可以使用命名牌重命名此实体<br></td></tr><tr>
<td>always_show</td>
<td>布尔</td>
<td>假</td>
<td>如果为真，名字将始终显示。<br></td></tr><tr>
<td>default_trigger</td>
<td>字符串</td><td></td>
<td>实体被命名时触发...<br></td></tr><tr>
<td>name</td>
<td>字符串</td><td></td>
<td>实体的当前名称，如果尚未命名实体，则为空，如果改为非空将命名实体。<br></td></tr><tr>
<td>name_actions</td>
<td>Json对象</td><td></td>
<td>设定该实体的特殊候选名以及当这个实体被设置名称时会调用的事件<br><table>
<tbody><tr> <th>名称</th> <th>类型</th><th>备注</th></tr><tr>
<td>name_filter</td>
<td>列表</td>
<td>特殊名称列表，特殊名称会导致“ on_named”中定义的事件被触发<br></td></tr><tr>
<td>on_named</td>
<td>字符串</td>
<td>当此实体获取“name_filter”中指定的名称时要调用的事件。<br></td></tr></tbody></table></td></tr></tbody></table>

### 18. minecraft:position
该组件可以控制实体在世界上的当前位置。应用后，实体将被传送到指定的新位置。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
x | 小数 | 0.0 | 沿实体的X轴（东西方向）位置
y | 小数 | 0.0 | 沿实体的Y轴（高度）位置
z | 小数 | 0.0 | 沿实体的Z轴（南北方向）位置

### 19. minecraft:rotation
该组件使您可以控制实体的头部旋转。应用后，实体将按照指定的方向旋转。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
x | 小数 | 0.0 | 控制头部上下旋转
y | 小数 | 0.0 | 控制实体身体平行于地面旋转

### 20. minecraft:shooter
定义实体的远程攻击。注意，这不使实体可以使用远程攻击：它仅定义了设计弹射物的类型。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
auxVal | 整数 | -1 | 将要应用到目标上的药水效果的 ID
def | 字符串 |  | 用作远程攻击的弹射物的实体标识符。实体必须具有弹射物组件才射击弹射物

### 21. minecraft:spawn_entity
添加一个计时器，之后该实体将会生成另一个实体或物品（类似于鸡的生蛋行为）。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
filters | Minecraft 过滤器 |  | 指定的实体只会在过滤器结果为真时生成。
max_wait_time | 整数 | 600 | 产生另一个实体需要随机等待的最长时间（单位为秒）。
min_wait_time | 整数 | 300 | 产生另一个实体需要随机等待的最短时间（单位为秒）。
num_to_spawn | 整数 | 1 | 每次触发时产生的实体数。
should_leash | 布尔 | 假 | 如果为真，出生的实体将会被父实体拴住。
single_use | 布尔 | 假 | 如果为真，应用该组件的实体将只产生一次实体。
spawn_entity | 字符串 |  | 要产生的实体的标识符，留空则产生定义的物品。
spawn_event | 字符串 |  | 实体出生后要调用的事件。minecraft:entity_born
spawn_item | 字符串 | egg | 要生成的物品的物品标识符。
spawn_method | 字符串 | born | 产生实体的方法
spawn_sound | 字符串 | plop | 产生实体时播放的音效果的标识符。

### 22. minecraft:tag
标签组件用于访问实体标签数组中可能具有的实体标签。使用此组件可以添加和删除实体标签。

### 23. minecraft:teleport
此组件可以控制实体的自身传送能力（类似于末影人）。如果您希望传送该实体，请使用“Position”组件。
#### 参数
名称 | 类型 | 默认值 | 备注
-|-|-|-
dark_teleport_chance | 小数 | 0.01 | 实体在黑暗中启动传送的几率
light_teleport_chance | 小数 | 0.01 | 实体在白天启动传送的几率
max_random_teleport_time | 小数 | 20 | 随机传送之间的最长时间（单位为秒）
min_random_teleport_time | 小数 | 0 | 随机传送之间的最短时间（单位为秒）
random_teleport_cube | 向量 [a, b, c] | [32, 16, 32] | 实体将传送到此立方体定义的区域内的随机位置
random_teleports | 布尔 | 真 | 如果为真，则实体将会随机传送
target_distance | 小数 | 16 | 实体在追逐目标时会传送的最远距离
target_teleport_chance | 小数 | 1 | 实体启动传送的几率。1.0表示100％

### 24. minecraft:tick_world
该组件是一个只读组件，允许用户访问常加载区域上的实体以及数据。
#### 参数
名称 | 类型 | 备注
-|-|-
distance_to_players | 小数 | 实体与玩家的距离
never_despawn | 布尔 | 当玩家超出范围时此常加载区域是否会消失
radius | 整数 | 常加载区块的半径
ticking_area | JavaScript实体常加载区域对象 | 附加到该实体的常加载区域的实体

### 25. minecraft:ticking_area_description
常加载区域的备注
#### 参数
名称 | 类型 | 备注
-|-|-
is_circle | 布尔 | 如果为真，该区域的一个圆形。 否则为矩形。
max | 向量 [a, b, c] | （如果区域是正方形）区域的边缘。
name | 字符串 | 区域的名称。
origin | 向量 [a, b, c] | 区域的原点位置。
radius | 向量 [a, b, c] | （如果区域是正方形）区域的半径。