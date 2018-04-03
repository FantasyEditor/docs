# 状态
在逻辑上，状态可以用来管理单位的[属性]与[限制]变化；在表现上，状态可以用来给单位绑定特效、音效甚至改变单位的模型属性。

当你制作一个复杂的技能效果时，它应该是有许多状态组合而成的。

### 构造
创建/获取状态

* 参数
    + name (string) - 状态名
* 返回
    + buff (buff) - 状态数据表

如果[ClientBuff]中有同名的状态定义，则会包含定义的属性。使用[unit:add_buff]来给单位添加状态。

```lua
    local mt = ac.buff[name]
```

### 设置
只能在[构造]时设置：

#### cover_global
全局覆盖类型

决定如何视为同名状态，可以是以下的值（integer）：
* `0`：必须名字和来源都相同才视为同名状态,触发覆盖。这是默认值。
* `1`：只要名字相同就会视为同名状态,触发覆盖

#### cover_max
最大生效数量

当单位身上有多个同名状态时，最多可以同时生效的状态数量（integer）。这个值默认为0，表示无限制。只有当[cover_type]为共存模式时才有意义。关于同名状态的排序方式见[on_cover]。

#### cover_type
共存模式

决定了单位获得多个同名状态时的行为，可以是以下的值（integer）：
* `0`：独占模式，单位只能同时保留一个同名状态。[on_cover]可以决定哪个状态保留下来。
* `1`：共存模式，单位可以同时保留多个同名状态。[on_cover]可以决定这些状态的排序，以便通过[cover_max]来只让部分状态生效。

#### keep
死亡后保留

决定了单位死亡后，状态是否继续保留；以及单位死亡时是否能添加该状态。可以是以下的值（boolean）：
* `true`：单位死亡时保留状态，状态也可以添加给死亡的单位。
* `false`：单位死亡时移除状态，状态无法添加给死亡的单位。这是默认值。

#### sync
同步方式

这个值决定了状态可以被哪些人看见，见[同步方式]，默认值为`none`。同步方式的参照单位为状态的[source]，而不是状态的[target]。

### 属性
可以在[构造]或[unit:add_buff]时设置：

#### pulse
心跳

触发[on_pulse]事件的频率（number），单位为秒。这个值的精度受到[逻辑帧]的影响。默认值为1帧。

#### skill
技能

在[unit:add_buff]时设置。

#### source
来源

状态的来源，在[unit:add_buff]时设置。这会影响状态的[同步方式]。如果不设置，则为[unit:add_buff]时的对象。

#### target
目标

状态的目标。该属性不需要设置，为[unit:add_buff]时的对象。

#### time
持续时间

状态会在经过此时间后自动移除。这个值类型为（number），默认为一个非常巨大的值，表示持续无限时间。

[同步方式]: /ac/API/buff/属性/sync
[逻辑帧]: /ac/API/main?id=逻辑帧
[构造]: /ac/API/buff/构造
[ClientBuff]: 404
[unit:add_buff]: /ac/API/unit/add_buff
[on_pulse]: /ac/API/buff/事件/on_pulse
[on_cover]: /ac/API/buff/事件/on_cover
[cover_type]: /ac/API/buff/属性/cover_type
[cover_max]: /ac/API/buff/属性/cover_max
[source]: /ac/API/buff/属性/source
[target]: /ac/API/buff/属性/target
