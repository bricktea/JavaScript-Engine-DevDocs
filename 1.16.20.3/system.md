# Ver 1.16.20.3
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
Minecraft脚本引擎使用JavaScript语言。
您可以编写JavaScript脚本并将其与行为包联系在一起，以监听和响应游戏事件，获取和修改实体所具有组件中的数据以及影响游戏的各种部分。


## 重大变化
在我们完善Minecraft 脚本引擎的时候，我们会对脚本API做出一些更改。这可能会导致您的脚本无法正常工作，如果您存在此问题，请看此部分。

类别 | 变化
-|-
组件 | 现在，调用getComponent() 返回组件相关的参数将返回到返回对象的“data”参数中。
事件 | 现在，eventData对象将事件的参数放在了事件数据对象的'data'参数中。
UI | engine.on 现在取一个方面名称。方便检查脚本引擎是否启动并将其连接到UI，您将需要以下内容：<br> `engine.on("facet:updated:core.scripting", ...);` <br> 完成初始化后，继续... <br> `engine.trigger("facet:request", ["core.scripting"]);`

## 调试
您的脚本无法正常工作？不用担心！我们已在脚本引擎中内置了一些调试功能，可能帮助您了解脚本无法工作的基本原因。<br>
如果您不熟悉编程或想了解有关调试的更多信息，建议您查看以下有关使用VisualStudio进行调试的文档：https://docs.microsoft.com/visualstudio/debugger<br><br>
有两种方法可以“调试”脚本：在游戏内或高级，我们将在下面进行介绍。您只需要游戏和脚本即可在游戏中进行调试，如果想使用高级调试，您需要一台装有Windows 10和Visual Studio的电脑进行高级调试。
### 在游戏内 {docsify-ignore}
当您在游戏中运行脚本时，只要出现问题，脚本引擎就会输出错误消息。例如，如果您尝试获取脚本引擎不知道的组件，就会出现错误。<br>
要查看这些消息，您可以打开聊天屏幕，该屏幕上将显示所有脚本引擎输出的错误信息。或者您可以打开游戏生成的日志文件。日志文件的位置因平台而异。在Windows 10上，您可以在`%APPDATA%\..\Local\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\logs`中找到日志文件<br>
我们强烈建议您在处理脚本时在脚本中构建更多的调试消息和工具。这将帮助您辨别什么时候工作不正常。如果您需要其他帮助，请访问Discord官方频道：https://discord.gg/Minecraft
### 实时（高级） {docsify-ignore}
如果您有一台装有Visual Studio的Windows10电脑，则可以使用Visual Studio调试器实时调试脚本。<br>
如果您使用本文档“推荐”部分中提到的组件安装了Visual Studio，则将安装并启用即时调试器。每当脚本中发生异常时，此工具都会从Visual Studio弹出一条消息，并在脚本的异常处打开。<br><br>

此外，您可以手动连接到脚本引擎并调试代码。您可以使用远程调试来连接和调试在其他设备上运行的Minecraft。请参考上面的Visual Studio调试器文档，以获取有关如何使用远程调试的说明。
首先，您需要启动Visual Studio。如果这是您第一次启动Visual Studio，建议您设置环境以进行JavaScript开发并在出现提示时登录到Microsoft帐户。这将使用您需要的最重要的工具来定义Visual Studio的主界面。
打开Visual Studio后，应该启动Minecraft。然后创建一个启用了实验性游戏的新世界，并应用包含脚本的行为包。
创建完世界后，返回Visual Studio并单击“调试”菜单。然后单击“附加到处理”。在打开的窗口中，将出现一个名为“筛选进程”的搜索框。单击它，然后键入Minecraft。
将列表选定到Minecraft，您可以通过查看“类型”列来验证脚本引擎是否正在运行。<br><br>

选择进程，然后单击“附加”以将调试器附加到游戏。现在，当下一行脚本代码运行时，您将可以按“暂停”按钮暂停脚本引擎。这使您可以检查脚本中变量的值，并在代码中发生错误时进入Visual Studio。<br>
**警告：**当您到达断点以使用调试器单步执行代码时，客户端可能会超时并断开连接，服务器可能会断开与所有玩家的连接。

## 示例
以下一些Minecraft爱好者提供的脚本演示，可以帮助您编写脚本。解压它们就可以获得js源代码，也可以将它们导入到游戏中游玩。

示例 | 上次更新 | 下载
-|-|-
怪物竞技场 | 2018/10/24 | https://aka.ms/minecraftscripting_mobarena
回合制RPG | 2018/10/24 | https://aka.ms/minecraftscripting_turnbased

## 快速开始
首先，您需要下载最新的Vanilla Behavior Pack。您可以从这里获取：https://aka.ms/behaviorpacktemplate<br>
下载行为包后，将其解压缩到文件夹中。在行为包中，您将找到scripts文件夹，其中包含要运行的所有脚本文件。<br>
在脚本文件夹中，您将找到两个文件夹：一个用于客户端脚本，一个用于服务器脚本。<br>
**Server 脚本** 这些脚本在游戏的服务器端运行。这包括在实体上生成新实体，添加组件或修改组件。<br>
**Client 脚本** 这些脚本在游戏的每个玩家的设备上运行。这是响应事件并管理玩家特定内容的好地方。<br>
一旦选择要制作客户端脚本还是服务器脚本，只需将扩展名为.js的新空白文本文件添加到适当的文件夹中，然后在代码编辑器中将其打开。您可以在此处添加任意数量的JavaScript文件（文件名无关紧要），它们将彼此独立运行！<br><br>

**Note:** 对于要由客户端运行的脚本，您需要在要运行脚本的世界上启用“实验性”选项。目前脚本引擎仍处于测试阶段，这将是必要的。<br>
当进入一个包含客户端脚本的世界时，游戏将提示您接受在设备上运行脚本（单机和多人游戏都会显示）。<br>
另外，如果您的包中包含客户端脚本，则需要在Manifest中包含一个client_data模块。这告诉游戏脚本/客户端文件夹中的所有内容都需要发送到客户端。请参阅“附加文档”页面以获取有关包装清单内容的更多信息。

### 文件夹结构 {docsify-ignore}
客户端脚本所需模块的Manifest示例
```json
{
    "description": "Example client scripts module",
    "type": "client_data",
    "uuid": "c05a992e-482a-455f-898c-58bbb4975e47",
    "version": [0, 0, 1]
}
```
vanilla_behavior_pack
```
|-scripts
|--client
|---myClientScript.js
|--server
|---myServerScript.js
|-manifest.json
|-pack_icon.png
```

## 已知问题
这是Minecraft脚本引擎当前已知问题的列表

问题 | 解决方法
-|-
实体处于死亡时立即调用removeEntity删除实体可能会导致游戏崩溃 | 请勿在将实体的伤害降低到0的的同一时钟周期上调用removeEntity。删除实体会立即将其删除。您可以保存所有死亡的实体在下一时钟周期清理（有关示例，请参见RPG回合示例）
自定义GUI在游戏暂停和恢复后不会被保留 | 暂时没有解决方法
自定义GUI在VR或MR模式下不可用 | 只在常规模式中使用自定义GUI
退出没有脚本的世界并进入有脚本的世界可能会导致加载错误的世界 | 在新使用脚本加载世界之前重启Minecraft
无法从压缩包中正确加载脚本 | 在导入脚本之前自行解压，如果你选择了一个.mcpack后缀的脚本包，那么游戏会自动为你解压

## 先决条件
您想制作自己的脚本？棒极了！在本节中，您将找到所需的最低和推荐软件列表。<br>
**Note：**目前，仅Minecraft for Windows 10支持脚本。如果您尝试在不支持脚本的设备上使用脚本打开世界，则会看到一条错误消息，提示您无法进入世界。（译者注：EZ可无视）

软件 | 至少需要 | 推荐
-|-|-
代码编辑器 | VSCode或纯文本编辑器 | Visual Studio Community 2017 并安装“Node.js开发”组件
调试器 | N/A | 	Visual Studio Community 2017
Minecraft | Win10版MC | Win10版MC
其他 | 获得Vanilla Behavior Pack：https://aka.ms/behaviorpacktemplate | 获得Vanilla Behavior Pack：https://aka.ms/behaviorpacktemplate
存储空间 | 1GB（包括MC，文本编辑器和您的脚本） | 3GB（包括MC，文本编辑器和您的脚本）

## 脚本结构
在本部分中，您可以快速了解可运行在Minecraft脚本引擎的JavaScript文件的基本结构。如果您想了解有关JavaScript的更多信息或希望学习基础教程，可以在以下Mozilla开发人员网络上查看官方文档：https://developer.mozilla.org/docs/Web/JavaScript<br>
从某种意义上讲，它们是脚本的必需部分，也不是脚本唯一的部分。您可以根据喜好来写脚本，但在必须在合适的地方调用它们！（看下面）
### 1. System 注册 
首先，我们需要为脚本注册系统。这定义了我们的脚本是要在游戏的客户端还是服务端上使用。总的来说，你要根据你是把脚本文件放到了client还是server文件夹里了来选择函数。你只需要在client或者server对象上调用registerSystem并传参你需要的API的版本就可以完成它。
#### 参数 {docsify-ignore}

名称 | 类型 | 备注
-|-|-
majorVersion | 整数 | 这是脚本应工作在的Minecraft脚本引擎的版本号
minorVersion | 整数 | 这是脚本应工作在的Minecraft脚本引擎的子版本号

#### 示例代码 {docsify-ignore}
Client
```javascript
let sampleClientSystem = client.registerSystem(0, 0);
```
Server
```javascript
let sampleServerSystem = server.registerSystem(0, 0);
```

### 2. System 初始化
这是系统注册后立即调用的第一个方法。 一旦脚本在世界上加载，该方法将立即被执行。
您可以使用它来设置脚本的环境：注册自定义组件和事件，注册事件侦听器等。此操作将在世界准备好并将播放器添加到其中之前运行。 此函数应用于初始化变量和设置事件侦听器。此时，您不应该在此处生成任何实体或与之交互！您还应该避免在此处将GUI元素或将消息发送到聊天窗口，因为这是在玩家准备就绪之前调用的。
#### 示例代码 {docsify-ignore}
```javascript
sampleSystem.initialize = function() {
  // 在此处注册事件数据，组件，查询器与事件监听器。
};
```
### 3. System 更新
游戏的每个Tick都会调用该方法一次，服务端每秒会“Tick”20次。可以在这里获取，检查组件变化并对之做出响应的好地方。
```javascript
sampleSystem.update = function() {
  // 在这里执行事件更新
};
```

### 4. System 关闭
当Minecraft脚本引擎关闭时，将调用此方法。对于客户端来说，这就是玩家退出世界的时候。对于服务器，这是最后一个玩家退出世界的时候。
## 自定义组件
自定义组件是一种特殊的组件，可以在脚本中进行定义。<br>
该组件需要通过提供名称和一组名称为"value:value"的字段向脚本引擎注册。应用后，该组件的行为就像任何内置组件一样：您可以从实体中获取它，修改其值并应用更改。<br>
当前，自定义组件是唯一可以使用脚本从实体动态添加和删除的组件。无需在实体的Json文件中预先定义它们。在当前版本中，这些组件不会被保存：它们仅在实体存在时存在，并且在世界重新加载后需要重新添加。<br>
### 示例代码 {docsify-ignore}
注册自定义组件
```javascript
this.registerComponent("myNamespace:myComponent", { myString: "TamerJeison", myInt: 42, myFloat: 1.0, myArray: [1, 2, 3] });
```