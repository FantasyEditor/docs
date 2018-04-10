# 伤害

伤害结算的说明见[这里][伤害结算]。

### 创建
使用[attack:add_damage]或[skill:add_damage]来创建一个伤害对象，并立即结算此伤害。

```lua
skill:add_damage
{
    source = source,
    target = target,
    damage = 100,
}
```

### 属性
属性可以在[创建]时传入。

#### source
来源

伤害的来源（unit）。

#### target
目标

伤害的目标（unit）。

#### skill
关联技能

如果伤害是通过[attack:add_damage]创建的，那么关联技能即为`attack`。如果伤害是通过[skill:add_damage]创建的，那么关联技能即为`skill`。

#### damage
伤害

类型为（number）。这个属性应该在[创建]时传入。

#### *angle*
伤害方向

类型为（number）。客户端会根据伤害方向决定受击特效的方向。如果不填，则会使用[来源]到[目标]的方向。

### 自定义属性
你可以在[创建]时给伤害添加任何自定义的属性（当然不能重名），之后便可以从伤害对象中将其读出，以便实现自定义功能。

### 事件
一次伤害流程会触发若干[全局事件]，这些事件按照以下顺序依次触发：

#### 伤害初始化

* 事件对象
    * damage (damage) - 伤害对象
* 事件参数
    * damage (damage) - 伤害对象

用于实现[法球]。使用触发器接收事件：

```lua
damage:event('伤害初始化', function (trg, damage)
    -- 你的代码
end)
```

#### 造成伤害开始

* 事件对象
    * source (unit) - 伤害来源
* 事件参数
    * damage (damage) - 伤害对象

在事件中返回`false`可以阻止本次伤害。使用触发器接收事件：

```lua
source:event('造成伤害开始', function (trg, damage)
    -- 返回false以阻止伤害
    return false
end)
```

#### 受到伤害开始

* 事件对象
    * target (unit) - 伤害目标
* 事件参数
    * damage (damage) - 伤害对象

在事件中返回`false`可以阻止本次伤害。使用触发器接收事件：

```lua
target:event('受到伤害开始', function (trg, damage)
    -- 返回false以阻止伤害
    return false
end)
```

#### 造成伤害

* 事件对象
    * source (unit) - 伤害来源
* 事件参数
    * damage (damage) - 伤害对象

此时[护甲]等效果已经结算完毕。使用触发器接收事件：

```lua
target:event('造成伤害', function (trg, damage)
    -- 你的代码
end)
```

#### 受到伤害

* 事件对象
    * target (unit) - 伤害目标
* 事件参数
    * damage (damage) - 伤害对象

此时[护甲]等效果已经结算完毕。使用触发器接收事件：

```lua
target:event('受到伤害', function (trg, damage)
    -- 你的代码
end)
```

#### 伤害前效果

* 事件对象
    * damage (damage) - 伤害对象
* 事件参数
    * damage (damage) - 伤害对象

用于实现[法球]。使用触发器接收事件：

```lua
damage:event('伤害前效果', function (trg, damage)
    -- 你的代码
end)
```

### 方法

#### get_angle
获取伤害方向

如果[创建]时传入了[angle]属性，则返回该值。否则，返回当前[来源]的位置到[目标]位置的方向。

#### get_damage
获取伤害值

返回伤害值。[护甲]或用户可以在[伤害事件]中修改伤害值。

[attack:add_damage]: /ac/api/attack?id=add_damage
[skill:add_damage]: /ac/api/skill?id=add_damage
[angle]: /ac/api/damage?id=angle
[创建]: /ac/api/damage?id=创建
[来源]: /ac/api/damage?id=source
[目标]: /ac/api/damage?id=target
[全局事件]: 404
[伤害事件]: /ac/api/damage?id=事件
[伤害结算]: /ac/damage/伤害结算
[护甲]: 404
[护盾]: /ac/damage/护盾
