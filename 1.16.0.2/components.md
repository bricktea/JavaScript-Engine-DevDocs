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
该组件定义当特定的实体被损害时该实体时该实体调用的事件。
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