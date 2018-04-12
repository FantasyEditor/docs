# mover
运动

运动可以让弹道或拥有移动能力的生物按照一定的规则进行持续移动。

> 令单位`unit`向其面朝方向运动1000距离：

```lua
skill:mover_line
{
    source = unit,
    mover = target,
    angle = target:get_facing(),
    distance = 1000,
    speed = 1000,
}
```

> 发射一个追踪弹道追踪`unit`：

```lua
skill:mover_target
{
    source = unit,
    model = '弹道单位名称',
    target = unit,
    speed = 1000,
}
```

运动分为直线运动与追踪运动2种。直线运动会使单位沿着固定直线轨迹移动；而追踪运动则需要设置一个单位作为追踪目标，使单位不停的靠近目标。

+ 一个单位同一时间只能进行一个运动，当单位已有运动却获得新的运动时，将根据[优先级]决定哪个运动有效：若新的运动优先级大于等于当前运动的优先级，则移除当前运动获得新的运动；否则新的运动添加失败。
+ 单位可以同时进行运动与行走，不过目前客户端对这种情况的表现不正确。
+ 若单位处于[跟随]状态，则无法添加运动。
+ 通过[passive]可以设定是否是主动运动：主动运动会使运动单位的朝向始终朝向运动方向，且当运动单位拥有[定身]行为限制时将无法进行主动运动。
+ 通过[animation_point]可以修正运动起点，让弹道看起来是从“武器”或“手部”发射，而不是从身体中央发射。这个值支持2种格式：
    + 字符串`skill`：若[skill]使用[set_animation]设置过动画，则会从动画表中找到该动画的[ProjectilePoint]，使用这个值来修正起点。
    + 表`{x, y, z}`：使用指定的坐标修正起点。
+ 运动可能会创建失败，你需要判空后再使用它。

### 创建
#### mover_line
直线运动

* 参数
    * data (table) - 运动属性
* 返回
    * mover (mover) - 运动对象

```lua
local mover = skill:mover_line
{
    source = unit,
    mover = unit,
}
```

#### mover_target
追踪运动

* 参数
    * data (table) - 运动属性
* 返回
    * mover (mover) - 运动对象

```lua
local mover = skill:mover_target
{
    source = unit,
    mover = unit,
    target = unit,
}
```

### 属性
运动的属性需要在[创建]时作为参数传入。

#### source
运动来源（unit）

#### skill
关联技能（skill）

会被设置为[创建]时使用的技能对象，不需要再次设置。

#### target
目标（point/unit）

如果是[追踪运动]，那么这个值的类型必须是（unit）。如果是[直线运动]，那么这是个可选参数，会在[angle]与[distance]中用到这个值。

#### *mover*
运动单位（unit）

进行运动的单位。[mover]与[model]这2个参数需要设置其一，运动才能创建成功。

#### *model*
单位名（string）

创建一个临时单位作为运动单位，当运动结束后该单位会被移除。[mover]与[model]这2个参数需要设置其一，运动才能创建成功。

#### *start*
起点（point）

如果不填，则使用[mover]的位置或[source]的位置作为起点。

#### *angle*
方向（number）

运动的初始方向，若不填则为运动单位到[target]的方向。

#### *distance*
距离（number)

[直线运动]的运动距离。

#### *max_distance*
最大距离（number）

[追踪运动]的最大距离。默认为运动单位到[target]距离的2倍。

#### *speed*
速度（number）

#### *accel*
加速度（number）

#### *min_speed*
速度下限（number）

一般配合加速度使用，使运动速度不会小于该值。默认为0.0。

#### *max_speed*
速度上线（number）

一般配合加速度使用，使运动速度不会大于该值。

#### *height*
起点高度（number）

默认为0。

#### *target_height*
终点高度（number）

[直线运动]的默认值为[height]，[追踪运动]的默认值为[target]的[受击高度]。

#### *parabola_height*
抛物线高度（number）

抛物线的顶点高度，默认为0。

#### *turn_speed*
转身速度（number）

[追踪运动]每秒改变朝向的速度限制。不填表示无限制。

#### *hit_type*
碰撞类型（string）

设置此值后，运动单位与附近单位碰撞时将触发[on_hit]事件。敌我判断的参考单位为正在运动的单位。不会碰撞到运动单位自己。可以是以下值：
+ `敌方` 碰撞敌方单位
+ `友方` 碰撞友方单位
+ `全部` 碰撞所有单位

#### *hit_area*
碰撞范围（number）

#### *hit_same*
碰撞同一个单位（boolean）

默认为`false`。

#### *hit_target*
碰撞追踪目标（boolean）

对于[追踪运动]，当为`true`时，运动单位与[target]的距离小于[hit_area]时运动完成；当为`false`时，运动单位需要到达[target]的位置时运动完成。默认为`true`。

#### *block*
碰撞地形（boolean）

当为`true`时，当运动单位到达地形阻挡时会触发[on_block]事件。若没有注册此事件，则会移除运动。

#### *priority*
优先级（number）

#### *passive*
被动运动（boolean）

默认为`false`。

#### *animation_point*
动画点

可以是以下的值：
+ 字符串`skill`：若[skill]使用[set_animation]设置过动画，则会从动画表中找到该动画的[ProjectilePoint]，使用这个值来修正起点。
+ 表`{x, y, z}`：使用指定的坐标修正起点。

### 事件
#### on_block
碰撞地形事件

* 事件返回
    * remove (boolean) - 是否移除运动

只有当[block]设置为`true`时才可能触发此事件。在事件中返回`true`可以移除运动。

```lua
function mover:on_block()
    return true
end
```

#### on_finish
完成事件

当[直线运动]将[distance]跑完，或是[追踪运动]追上[target]后触发。比[on_remove]先触发。

```lua
function mover:on_finish()
    -- 你的代码
end
```

#### on_hit
碰撞单位事件

* 事件参数
    * unit (unit) - 碰撞到的单位
* **回调返回**
    * remove (boolean) - 是否移除运动

只有设置了[hit_type]后才可能触发此事件。在事件中返回`true`可以移除运动。

```lua
function mover:on_hit(unit)
    return true
end
```

#### on_remove
移除事件

运动被移除时触发此事件。

```lua
function mover:on_remove()
    -- 你的代码
end
```

### 方法
#### batch_update
批量更新

立即更新运动，直到运动完成。

> 利用直线运动来“探路”：

```lua
local mover = skill:mover_line
{
    source = unit,
    model = '马甲单位',
    start = start,
    target = target,
    speed = 100,
    block = true,
}

if mover then
    function mover:on_block()
        -- start与target的之间有地形阻挡
    end

    mover:batch_update()
end
```

#### remove
移除运动

```lua
mover:remove()
```

[优先级]: /ac/api/mover?id=priority
[直线运动]: /ac/api/mover?id=mover_line
[追踪运动]: /ac/api/mover?id=mover_target
[创建]: /ac/api/mover?id=创建
[mover]: /ac/api/mover?id=mover
[model]: /ac/api/mover?id=model
[source]: /ac/api/mover?id=source
[angle]: /ac/api/mover?id=angle
[distance]: /ac/api/mover?id=distance
[target]: /ac/api/mover?id=target
[height]: /ac/api/mover?id=height
[passive]: /ac/api/mover?id=passive
[block]: /ac/api/mover?id=block
[hit_type]: /ac/api/mover?id=hit_type
[animation_point]: /ac/api/mover?id=animation_point

[on_hit]: /ac/api/mover?id=on_hit
[on_block]: /ac/api/mover?id=on_block
[on_remove]: /ac/api/mover?id=on_remove

[跟随]: /ac/api/unit?id=follow
[受击高度]: 404
[定身]: /ac/unit/attribute?id=定身
[set_animation]: /ac/api/skill?id=set_animation
[ProjectilePoint]: 404
