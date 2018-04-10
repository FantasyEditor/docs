# 技能
关于技能的说明见[这里][技能]

### 创建
创建/获取技能

* 参数
    + name (string) - 技能名
* 返回
    + skill (skill) - 技能数据表

需要在技能表中定义过名称为`name`的技能，才能在脚本中创建。使用[add_skill]来给单位添加技能。

### 属性
#### name
待补充

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

读取技能的数据，对于技能对象来说，这个操作等价于`skill[key]`。对于施法对象来说，则是从它的源技能上读取数据。

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
获取技能目标

* 返回

[技能]: /ac/skill/
[施法引导]: 404
[施法出手]: 404
[add_skill]: /ac/api/unit?id=add_skill
[damage]: /ac/api/damage
[name]: /ac/api/skill?id=name
[on_disable]: /ac/api/skill?id=on_disable
[on_enable]: /ac/api/skill?id=on_enable