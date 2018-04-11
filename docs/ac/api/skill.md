# 技能
关于技能的说明见[这里][技能]

### 创建
创建/获取技能

* 参数
    + name (string) - 技能名
* 返回
    + skill (skill) - 技能数据表

需要在技能表中定义过名称为`name`的技能，才能在脚本中创建。

```lua
local mt = ac.skill['技能名']
```

使用[add_skill]来给单位添加技能。

```lua
local skill = unit:add_skill('技能名', '英雄')
```

### 属性
#### owner
拥有技能的单位（unit）

不能自定义，这个属性被设置为[add_skill]时的对象。

#### name
技能名

[get_name]方法会优先获取这个值。你可以利用这个属性来方便的制作一个技能的多个版本：

```lua
local mt1 = ac.skill['超电磁炮']

local mt2 = ac.skill['超电磁炮-改']
mt2.name = '超电磁炮'

-- 这样你在其他地方就不用关心到底是哪个技能
if skill:get_name() == '超电磁炮' then
    -- 超电磁炮 与 超电磁炮-改 都会成立
end
```

### 方法
#### active_cd
激活冷却

* 参数
    + *max_cd* (number) - 冷却上限（秒）
    + *ignore_cooldown_reduce* (boolean) - 是否无视冷却缩减

这个方法有3种用法
1. 激活冷却（如同使用了技能）

```lua
skill:active_cd()
```

2. 按照指定冷却上限来激活冷却

```lua
skill:active_cd(30)
```

3. 按照指定冷却上限来激活冷却，并且决定是否受冷却缩减的影响

```lua
-- 无视冷却缩减
skill:active_cd(30, true)
-- 计算冷却缩减
skill:active_cd(30, false)
```

#### add_cd
增加剩余冷却

* 参数
    * cd (number) - 冷却

只对已经在冷却状态的技能有效，单位为秒。这个方法在用于充能型技能时，可以正确的改变层数。

```lua
skill:add_cd(1)
```

#### add_level
增加等级

* 参数
    * lv (integer) - 等级

技能等级不能降低，且不能在技能的施法事件中改变技能等级。

```lua
skill:add_level(1)
```

#### add_stack
增加层数

* 参数
    * stack (integer) - 层数

```lua
skill:add_stack(1)
```

#### channel_finish
引导完成

立即完成技能的引导，使技能进入[施法出手]阶段。只能在[施法引导]阶段中使用。

```lua
skill:channel_finish()
```

#### add_damage
造成伤害

* 参数
    * data (table) - 伤害属性表
* 返回
    * success (boolean) - 是否成功

伤害属性等说明见[damage]。

```lua
skill:add_damage
{
    source = source,
    target = target,
    damage = 100,
}
```

#### disable
禁用

触发[on_disable]事件，技能禁用后将不再触发任何事件。

```lua
skill:disable()
```

#### enable
启用

触发[on_enable]事件。

```lua
skill:enable()
```

#### get
获取数据

* 参数
    * key (非nil) - 索引
* 返回
    * value - 值

读取技能的数据，对于技能来说，这个操作等价于`skill[key]`。对于施法来说，则是从它的源技能上读取数据。

```lua
local value = skill:get(key)
```

#### get_cd
获取冷却

* 返回
    * cd (number) - 冷却（秒）

```lua
local cd = skill:get_cd()
```

#### get_level
获取等级

* 返回
    * level (integer) - 技能等级

```lua
local level = skill:get_level()
```

#### get_name
获取技能名

* 返回
    * name (string) - 技能名

如果技能有[name]属性，则返回[name]，否则返回技能名。

```lua
local name = skill:get_name()
```

#### get_slot_id
获取格子ID

* 返回
    * id (integer) - 格子ID

```lua
local id = skill:get_slot_id()
```

#### get_stack
获取层数

* 返回
    * stack (integer) - 层数

```lua
local stack = skill:get_stack()
```

#### get_target
获取目标

* 返回
    * *target* (unit/point) - 技能目标

仅用于施法，返回值类型由技能的目标类型属性决定。

```lua
local target = skill:get_target()
```

#### get_type
获取类型

* 返回
    * type (string) - [技能类型]

```lua
local type = skill:get_type()
```

#### is
是否是相同技能

* 参数
    * dest (skill) - 另一个技能
* 返回
    * result (boolean) - 结果

对于施法来说会使用源技能进行比较，因此可以用来判断2次施法是不是来自同一个技能。

```lua
local result = skill:is(dest)
```

#### is_cast
是否是施法

* 返回
    * result (boolean) - 结果

```lua
local result = skill:is_cast()
```

#### is_common_attack
是否是攻击技能

* 返回
    * result (boolean) - 是否是攻击技能

攻击技能的说明见[这里][攻击技能]。区别于[attack:is_common_attack]。

```lua
local result = skill:is_common_attack()
```

#### is_enable
是否启用

* 返回
    * result (boolean) - 是否启用

```lua
local result = skill:is_enable()
```

#### is_skill
是否是技能

* 返回
    * result (boolean) - 结果

总是返回 `true` 。区别于[attack:is_skill]。

```lua
local result = skill:is_skill()
```

#### mover_line
创建直线运动

* 参数
    * data (table) - 运动属性
* 返回
    * mover (mover) - 运动

运动的说明见[这里][运动]。

```lua
local mover = skill:mover_line
{
    source = unit,
    mover = unit,
}
```

#### mover_target
创建追踪运动

* 参数
    * data (table) - 运动属性
* 返回
    * mover (mover) - 运动

运动的说明见[这里][运动]。

```lua
local mover = skill:mover_target
{
    source = unit,
    mover = unit,
    target = unit,
}
```

#### notify_damage
伤害通知

* 参数
    * damage (damage) - 伤害

一般用于[伤害流程]，会产生以下效果：
+ 根据伤害来源、伤害目标、伤害角度与技能播放特效。
+ 根据伤害来源、伤害目标、伤害角度与技能播放音效。
+ 令伤害来源显形一段时间。
+ 令伤害来源与伤害目标的动画树切换为战斗状态。

```lua
skill:notify_damage(damage)
```

#### reload
重载

让技能重新加载脚本。不会触发任何事件。

```lua
skill:reload()
```

#### remove
移除

```lua
skill:remove()
```

#### set
设置数据

* 参数
    * key (非nil) - 索引
    * value - 值

设置技能的数据，对于技能来说，这个操作等价于`skill[key] = value`。对于施法来说，则是在它的源技能上设置数据。

```lua
skill:set(key, value)
```

#### set_animation
设置动画

* 参数
    * name (string) - 动画名称

设置本次施法使用的动画。

```lua
skill:set_animation(name)
```

#### set_cd
设置剩余冷却

* 参数
    * cd (number) - 冷却（秒）

只对已经在冷却状态的技能有效，最小值是0，最大值是当前冷却上限。

```lua
skill:set_cd(cd)
```

#### set_level
设置等级

* 参数
    * lv (integer) - 等级

技能等级不能降低，且不能在技能的施法事件中改变技能等级。

```lua
skill:set_level(2)
```

#### set_option
设置属性

* 参数
    * key (string) - 属性名
    * value (number) - 属性值

修改技能的属性，并通知给客户端。

#### simple_cast
发动效果

* 参数
    * effect (function) - 回调函数
* 回调参数
    * self (skill) - 施法

根据技能创建施法并执行回调函数，传入回调函数的技能是一个施法。

```lua
skill:simple_cast(function (cast)
    -- 这里的cast是源自skill的施法
end)
```

#### stop
停止施法

若在[施法开始]阶段，则进入[施法打断]阶段；否则进入[施法停止]阶段。

```lua
skill:stop()
```

### 事件

#### on_add
获得事件

每当单位从“没有该技能”变为“拥有该技能”时触发。注意，这里的“没有该技能”有2种情况：
1. 单位身上没有这个技能
2. 单位身上有这个技能，但等级为0

```lua
function mt:on_add()
    -- 你的代码
end
```

#### on_remove
失去事件

每当单位失去此技能时触发。

```lua
function mt:on_remove()
    -- 你的代码
end
```

#### on_upgrade
升级事件

每当此技能提升等级时触发

```lua
function mt:on_upgrade()
    -- 你的代码
end
```

#### on_cooldown
冷却完成事件

每当技能冷却完成时触发此事件。对于充能技能来说，每次充能完成一层也会触发此事件。

```lua
function mt:on_cooldown()
    -- 你的代码
end
```

#### on_enable
启用事件

每当使用[enable]，让技能从禁用状态变为启用状态时触发。

```lua
function mt:on_enable()
    -- 你的代码
end
```

#### on_disable
禁用事件

每当使用[disable]，让技能从启用状态变为禁用状态时触发。

```lua
function mt:on_disable()
    -- 你的代码
end
```

#### on_can_cast
即将施法事件

* 事件返回
    * *enbale* (boolean) - 是否允许发动技能
    * *error* (string) - 错误消息

每当技能即将发动时触发，此时的技能是施法。在事件中返回`false`来阻止技能的发动。在阻止技能发动后，会在客户端上显示`error`文本。

```lua
function mt:on_can_cast()
    return false, '不能发动此技能'
end
```

#### on_can_break
即将打断事件

* 事件参数
    * dest (skill) - 想要发动的技能
* 事件返回
    * *break* (boolean) - 是否打断施法

每当在此技能的施法过程中，想要发动另一个技能时触发。此时的技能和想要发动的另一个技能均为施法。在事件中返回`true`来打断当前施法。瞬发技能不会触发此事件（因为本来不会打断技能）。

```lua
function mt:on_can_break(dest)
    return true
end
```

#### on_cast_start
施法开始事件

每当技能技能进入[施法开始]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_start()
    -- 你的代码
end
```

#### on_cast_break
施法打断事件

每当技能进入[施法打断]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_break()
    -- 你的代码
end
```

#### on_cast_channel
施法引导事件

每当技能进入[施法引导]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_channel()
    -- 你的代码
end
```

#### on_cast_shot
施法出手事件

每当技能进入[施法出手]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_shot()
    -- 你的代码
end
```

#### on_cast_finish
施法完成事件

每当技能进入[施法完成]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_finish()
    -- 你的代码
end
```

#### on_cast_stop
施法停止事件

每当技能进入[施法停止]阶段时触发。此时的技能为施法。

```lua
function mt:on_cast_stop()
    -- 你的代码
end
```

[技能]: /ac/skill/
[施法开始]: 404
[施法打断]: 404
[施法引导]: 404
[施法出手]: 404
[施法完成]: 404
[施法停止]: 404
[攻击技能]: /ac/skill/攻击技能
[技能类型]: /ac/skill/技能类型
[运动]: /ac/api/mover
[伤害流程]: ac/damage/伤害流程
[add_skill]: /ac/api/unit?id=add_skill
[damage]: /ac/api/damage
[name]: /ac/api/skill?id=name
[get_name]: /ac/api/skill?id=get_name
[on_disable]: /ac/api/skill?id=on_disable
[on_enable]: /ac/api/skill?id=on_enable
[enable]: /ac/api/skill?id=enable
[disable]: /ac/api/skill?id=disable
[attack:is_common_attack]: /ac/api/attack?id=is_common_attack
[attack:is_skill]: /ac/api/attack?id=is_skill
