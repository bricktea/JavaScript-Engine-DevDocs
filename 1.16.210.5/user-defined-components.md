# Ver 1.16.210.5
[原文档链接](https://bedrock.dev/docs/stable/Scripting)<br>
自定义组件是一种特殊的组件，可以在脚本中进行定义。<br>
该组件需要通过提供名称和一组名称为"value:value"的字段向脚本引擎注册。应用后，该组件的行为就像任何内置组件一样：您可以从实体中获取它，修改其值并应用更改。<br>
当前，自定义组件是唯一可以使用脚本从实体动态添加和删除的组件。无需在实体的Json文件中预先定义它们。在当前版本中，这些组件不会被保存：它们仅在实体存在时存在，并且在世界重新加载后需要重新添加。<br>
### 示例代码 {docsify-ignore}
注册自定义组件
```javascript
this.registerComponent("myNamespace:myComponent", { myString: "TamerJeison", myInt: 42, myFloat: 1.0, myArray: [1, 2, 3] });
```