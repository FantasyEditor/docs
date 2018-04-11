# 状态
在逻辑上，状态可以用来管理单位的[属性]与[限制]变化；在表现上，状态可以用来给单位绑定特效、音效甚至改变单位的模型属性。

当你制作一个复杂的技能效果时，它应该是有许多状态组合而成的。

> 制作一个状态，单位在获得该状态5秒后失去该状态。

```lua
-- 给状态一个合适的名字，以便在其他地方添加这个状态
local mt = ac.buff['持续5秒的状态']
-- 设置属性[持续时间]为5秒
mt.time = 5
```

> 你可以在合适的时候给单位添加状态，例如我们有个单位`u`：

```lua
-- 添加状态，将状态对象保存在变量[buff]中
local buff = u:add_buff '持续5秒的状态'
{
    -- 关联技能
    skill = skill,
}
```

> 你可以再次修改这个状态的属性：

```lua
-- 将刚刚添加的状态的剩余时间改为10秒
buff:set_remaining(10)
```

### 创建
创建/获取状态

* 参数
    + name (string) - 状态名
* 返回
    + buff (buff) - 状态数据表

如果[ClientBuff]中有同名的状态定义，则会包含定义的属性。使用[add_buff]来给单位添加状态。

```lua
-- 将创建的状态保存下来，你之后需要为它进行设置，以及注册事件
local mt = ac.buff[name]
```

### 设置
只能在[创建]时设置：

#### *cover_global*
全局覆盖类型

决定如何视为同名状态，可以是以下的值（integer）：
* `0`：必须名字和来源都相同才视为同名状态,触发覆盖。这是默认值。
* `1`：只要名字相同就会视为同名状态,触发覆盖

#### *cover_max*
最大生效数量

当单位身上有多个同名状态时，最多可以同时生效的状态数量（integer）。这个值默认为0，表示无限制。只有当[cover_type]为共存模式时才有意义。关于同名状态的排序方式见[on_cover]。

#### *cover_type*
共存模式

决定了单位获得多个同名状态时的行为，可以是以下的值（integer）：
* `0`：独占模式，单位只能同时保留一个同名状态。[on_cover]可以决定哪个状态保留下来。这是默认行为。
* `1`：共存模式，单位可以同时保留多个同名状态。[on_cover]可以决定这些状态的排序，以便通过[cover_max]来只让部分状态生效。

#### *keep*
死亡后保留

决定了单位死亡后，状态是否继续保留；以及单位死亡时是否能添加该状态。可以是以下的值（boolean）：
* `true`：单位死亡时保留状态，状态也可以添加给死亡的单位。
* `false`：单位死亡时移除状态，状态无法添加给死亡的单位。这是默认值。

#### *sync*
同步方式

这个值决定了状态可以被哪些人看见，见[同步方式]，默认值为`none`。同步方式的参照单位为状态的[source]，而不是状态的[target]。

### 属性
可以在[创建]或[unit:add_buff]时设置：

#### *pulse*
心跳

触发[on_pulse]事件的频率（number），单位为秒。这个值的精度受到[逻辑帧]的影响。默认值为1帧。

#### skill
技能

在[unit:add_buff]时设置。

#### *source*
来源

状态的来源（unit)，在[unit:add_buff]时设置。这会影响状态的[sync]属性的参照物。如果不设置，则为[unit:add_buff]时的对象。

#### target
目标

状态的目标（unit)。该属性不需要设置，为[unit:add_buff]时的对象。

#### *time*
持续时间

状态会在经过此时间后自动移除。这个值类型为（number），默认为一个非常巨大的值，表示持续无限时间。

### 自定义属性
你可以在[创建]或[unit:add_buff]时给状态添加任何自定义的属性（当然不能重名），之后便可以从状态对象中将其读出，以便实现自定义功能。

### 方法

#### add_stack
增加层数

* 参数
    * count (integer) - 状态的层数

如果状态的属性允许，层数会显示在状态图标上。

```lua
buff:add_stack(count)
```

#### get_pulse
获取心跳

* 返回
    * pulse (number) - 触发[on_pulse]事件的周期

```lua
local pulse = buff:get_pulse()
```

#### get_remaining
获取剩余时间

* 返回
    * time (number) - 状态的剩余持续时间

```lua
local time = buff:get_remaining()
```

#### get_stack
获取层数

* 返回
    * count (integer) - 状态的层数

```lua
local count = buff:get_stack()
```

#### remove
移除状态

```lua
buff:remove()
```

#### set_pulse
设置周期

* 参数
    * pulse (number) - 触发[on_pulse]事件的周期

```lua
buff:set_pulse(pulse)
```

#### set_remaining
设置剩余时间

* 参数
    * time (number) - 状态的剩余持续时间

```lua
buff:set_remaining(time)
```

#### set_stack
设置层数

* 参数
    * count (integer) - 状态的层数

```lua
buff:set_stack(count)
```

### 事件
事件需要在[创建]状态时注册。事件中的`self`表示状态对象。

#### on_add
获得事件

每当单位获得该状态时触发此事件。

```lua
function mt:on_add()
    -- 你的代码
end
```

#### on_cover
叠加事件

* 事件参数
    * new (buff) - 新的状态
* 事件返回
    * cover (boolean) - 叠加方式。根据状态的[cover_type]，它有不同的处理。
        * 若[cover_type]为独占模式：
            * true: 当前状态被移除，新的状态被添加
            * false: 阻止新的状态添加
        * 若[cover_type]为共存模式：
            * true: 新的状态排序到当前状态之前
            * false: 新的状态排序到当前状态之后

每当有新的同名状态添加到单位身上时触发此事件。若状态没有注册此事件，则发生叠加时按照返回`true`的情况处理。

```lua
function mt:on_cover(new)
    return true
end
```

#### on_finish
完成事件

每当状态因持续时间耗尽而被移除时触发此事件，会比[on_remove]事件先触发。

```lua
function mt:on_finish()
    -- 你的代码
end
```

#### on_pulse
心跳事件

根据[pulse]的设置，周期性触发的事件。

```lua
function mt:on_pulse()
    -- 你的代码
end
```

#### on_remove
失去事件

* 事件参数
    * *new* (buff) - 新的状态

每当状态被移除时触发此事件。若状态的[cover_type]为共存模式，且该状态被移除后有一个同名状态即将生效，那么那个状态就会被作为参数传入。你可以利用这个特性为即将生效的状态进行初始化或数据继承等操作。

```lua
function mt:on_remove(new)
    -- 你的代码
end
```

[同步方式]: /ac/game/同步方式
[逻辑帧]: /ac/api/main?id=逻辑帧
[创建]: /ac/api/buff?id=创建
[ClientBuff]: 404
[add_buff]: /ac/api/unit?id=add_buff
[on_pulse]: /ac/api/buff?id=on_pulse
[on_cover]: /ac/api/buff?id=on_cover
[cover_type]: /ac/api/buff?id=cover_type
[cover_max]: /ac/api/buff?id=cover_max
[source]: /ac/api/buff?id=source
[target]: /ac/api/buff?id=target
[sync]: /ac/api/buff?id=sync
[pulse]: /ac/api/buff?id=pulse
