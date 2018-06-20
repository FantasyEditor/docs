# 单位
单位的说明见[这里][单位]。

#### add
增加属性

* 参数
    * state (string) - [单位属性]
    * value (number) - 数值

```lua
-- 攻击增加50点
unit:add('攻击', 50)
-- 攻击增加50%
unit:add('攻击%', 50)
```

#### add_ai
添加AI

* 参数1
    * name (string) - AI名称
* 参数2
    * data (table) - AI数据

AI的说明见[这里][ai]。

```lua
unit:add_ai '小兵'
{
    -- 自定义数据
}
```

#### add_animation
添加动画

* 参数1
    * name (string) - 动画名称
* 参数2
    * *speed* (number) - 动画播放速度，默认为`1.0`
    * *loop* (boolean) - 循环播放，默认为`false`

```lua
unit:add_animation '跳舞'
{
    speed = 1.0,
    loop = false,
}
```

#### add_buff
添加状态

* 参数1
    * name (string) - 状态名称
    * *delay* (number) - 生效延迟（秒）
* 参数2
    * data (table) - [状态属性]
* 返回
    * buff (buff) - 状态

状态的详细说明见[这里][buff]。指定生效延迟后，状态会在延迟时间后生效，但会立即返回状态，因此可以操作这个还未生效的状态，包括移除它。

```lua
local buff = unit:add_buff('状态名称', 1000)
{
    skill = skill,
}
```

#### add_exp
增加经验

* 参数
    * exp (number) - 经验值

用于[英雄升级]库。

```lua
unit:add_exp(100)
```

#### add_height
增加高度

* 参数
    * height (name) - 高度

指的是单位距离地面的高度

```lua
unit:add_height(100)
```

#### add_level
提升等级

* 参数
    * level (interger) - 等级

用于[英雄升级]库。等级不能降低。

```lua
unit:add_level(1)
```

#### add_provide_sight
提供视野

* 参数
    * team (integer) - 队伍ID

令单位的视野提供给指定队伍。视野的说明见[这里][视野]。

```lua
unit:add_provide_sight(1)
```

#### add_resource
增加能量

* 参数
    * type (string) - [能量类型][能量]
    * value (number) - 数值

```lua
unit:add_resource('怒气', 100)
```

#### add_restriction
增加行为限制

* 参数
    * type (string) - [行为限制]

```lua
unit:add_restriction '无敌'
```

#### add_sight
添加可见形状

* 参数
    * sight (sight) - [可见形状]
* 返回
    * *sight_handle* (sight_handle) - [可见形状对象][可见形状]

```lua
local sight_handle = unit:add_sight(sight)
```

> 有个体型巨大的单位，希望能在它的轮廓进入视野时就能看到他。

```lua
-- 在单位位置创建一个半径500的圆形可见形状
local sight = ac.sight_range(unit:get_point(), 500)
-- 将可见形状添加给单位
local sight_handle = unit:add_sight(sight)
```

> 之后不需要这个可见形状了

```lua
if sight_handle then
    sight_handle:remove()
end
```

#### add_skill
添加技能

* 参数
    * name (string) - 技能名称
    * type (string) - [技能类型]
    * *slot* （integer) - 技能格子
* 返回
    * *skill* (skill) - 技能

每个类型的技能格子从0开始，最大为99。指定格子时，这个格子上已经有技能的话会添加失败；不指定格子时，会找一个最小的空格。

```lua
unit:add_skill('技能名称', '英雄', 1)
```

#### attack
攻击

* 参数
    * target (unit) - 攻击目标
* 返回
    * valid (boolean) - 是否有效

```lua
local valid = unit:attack(target)
```

#### attack_skill
获取攻击技能

* 返回
    * attack (attack/skill) - 当前[攻击技能]

```lua
local attack = unit:attack_skill()
```

#### blink
传送

* 参数
    * target (point) - 目标位置
* 返回
    * result (boolean) - 是否成功

若目标位置有[静态碰撞]或[动态碰撞]且该单位不能无视它，则会将该单位传送至离目标位置最近的合法位置。

```lua
local result = unit:blink(target)
```

#### can_attack
是否可以攻击目标

* 参数
    * target (unit) - 目标单位
* 返回
    * result (boolean) - 结果

```lua
local result = unit:can_attack(target)
```

#### capturer
创建弹道捕获器

* 参数
    * data (table) - [弹道捕获器属性]
* 返回
    * capturer (capturer) - [弹道捕获器]

```lua
local capturer = unit:capturer
{
    radius = 500,
}
```

#### cast
使用技能

* 参数
    * name (string) - 技能名
    * *target* - 目标
    * *data* (table) - 数据
* 返回
    * valid (boolean) - 是否合法

`target`由技能属性决定，如果使用了错误的目标（比如无目标技能却传入了单位作为目标）会导致技能使用失败。`data`中的数据会被设置到施法中。

```lua
local valid = unit:cast('技能名', target, data)
```

#### clean_command
清空命令队列

```lua
unit:clean_command()
```

#### create_illusion
创建幻象

* 参数
    * where (point) - 创建位置
    * face (number) - 朝向
    * *dest* (unit) - 镜像复制目标
* 返回
    * *illusion* (unit) - 幻象

镜像复制目标默认为对象自己。复制单位的流程如下：
1. 触发[单位-初始化]事件
2. 复制技能
3. 复制单位属性
4. 触发[单位-创建]事件

也可以用[create_unit]实现。

```lua
local illusion = unit:create_illusion(where, face, dest)
```

#### create_unit
创建单位

* 参数
    * name (string/unit) - 单位名字/要复制的单位
    * where (point) - 创建位置
    * face (number) - 面朝方向
    * *on_init* (function) - 初始化函数
* 返回
    * unit (unit) - 单位

创建出来的单位属于`unit`。如果`name`是一个字符串，那么会创建名称为`name`的单位。如果`name`是一个单位，那么会复制该单位，目前该行为等同于[create_illusion]。单位创建后会最先执行`on_init`，你可以在这里给单位初始化一些数据。创建单位的流程如下：
+ 执行`on_init`
+ 触发[单位-初始化]事件
+ 添加技能
+ 触发[单位-创建]事件

```lua
local unit = player:create_unit('鹿目圆香', where, face, function (unit)
    -- 对unit进行初始化设置
end)
```

#### current_skill
获取当前施放的技能

* 返回
    * *skill* (skill) - 施法

获取到的技能是施法。如果当前不在放技能，则返回`nil`。

```lua
local skill = unit:current_skill()
```

#### disable_ai
禁用AI

令单位不再执行[AI][ai]。

```lua
unit:disable_ai()
```

#### each_buff
遍历状态

* 参数
    * *name* (string) - 状态名
* 遍历
    * buff (buff) - 遍历到的状态

当指定了`name`后只会遍历到该名称的状态，否则遍历所有状态。

```lua
for buff in unit:each_buff '状态名' do
    -- buff为遍历到的状态
end
```

#### each_mover
遍历运动

* 遍历
    * mover (mover) - 遍历到的运动

```lua
for mover in unit:each_mover() do
    -- mover为遍历到的运动
end
```

#### each_skill
遍历技能

* 参数
    * *type* (string) - [技能类型]
* 遍历
    * skill (skill) - 遍历到的技能

如果指定了`type`，则会遍历到所有技能类型为`type`的技能，不包括0级技能；否则会遍历到所有技能，包括0级技能。

```lua
for skill in unit:each_skill '英雄' do
    -- skill为遍历到的技能
end
```

#### enable_ai
启用AI

```lua
unit:enable_ai()
```

#### event
注册事件

* 参数
    * name (string) - 事件名
    * callback (function) - 事件函数
* 返回
    * trigger (trigger) - 触发器
* 事件参数
    * trigger (trigger) - 触发器
    * ... (...) - 自定义数据

这是对`ac.event_register`方法的封装，你可以在[这里][event]看到详细说明。

```lua
local trigger = unit:event('单位-死亡', function (trigger, unit, name)
    -- 你的代码
end)
```

#### event_dispatch
触发事件

* 参数
    * name (string) - 事件名
    * ... (...) - 自定义数据

这是对`ac.event_dispatch`方法的封装，你可以在[这里][event]看到详细说明。

```lua
unit:event_dispatch('自定义事件', ...)
```

#### event_has
是否订阅事件

* 参数
    * name (string) - 事件名
* 返回
    * result (boolean) - 结果

```lua
local result = unit:event_has(name)
```

#### event_notify
触发事件

* 参数
    * name (string) - 事件名
    * ... (...) - 自定义数据

这是对`ac.event_notify`方法的封装，你可以在[这里][event]看到详细说明。

```lua
unit:event_notify('自定义事件', ...)
```

#### event_subscribe
订阅事件

* 参数
    * name (string) - 事件名

订阅拥有计数，使用[event][unit:event]会自动订阅事件。

```lua
unit:event_subscribe(name)
```

#### event_unsubscribe
取消订阅事件

* 参数
    * name (string) - 事件名

订阅拥有计数，删除触发器时会自动取消订阅相关事件。

```lua
unit:event_unsubscribe(name)
```

#### execute_ai
执行AI

```lua
unit:event_subscribe '单位-死亡'
```

#### follow
跟随

* 参数
    * data (table) - [跟随属性]
* 返回
    * *mover* (mover) - 运动

```lua
local mover = unit:follow
{
    source = unit,
    skill = skill,
    mover = target,
}
```

#### find_buff
寻找状态

* 参数
    * name (string) - 状态名称
* 返回
    * *buff* (buff) - 找到的状态

如果有多个同名状态，则返回其中一个状态。

```lua
local buff = unit:find_buff(name)
```

#### find_skill
寻找技能

* 参数
    * name (string/integer) - 技能名/格子
    * *type* (string) - [技能类型]
* 返回
    * *skill* (skill) - 技能

指定`type`后只在此类型中寻找技能。如果`name`使用格子，那么必须要指定`type`。不会找到0级技能。

```lua
local skill = unit:find_skill('技能名')
local skill = unit:find_skill(0, '英雄')
```

#### get
获取属性

* 参数
    * state (string) - [单位属性]
* 返回
    * value (number) - 数值

```lua
local value = unit:get '生命'
```

#### get_class
获取单位类别

* 返回
    * class (string) - [单位类别]

```lua
local class = unit:get_class()
```

#### get_data
获取数据

* 返回
    * data (table) - 数据表

```lua
local data = unit:get_data()
```

#### get_exp
获取经验

* 返回
    * exp (number) - 经验

用于[英雄升级]库。

```lua
local exp = unit:get_exp()
```

#### get_facing
获取朝向

* 返回
    * face (number) - 朝向

朝向的取值范围为(-180, 180]。

```lua
local face = unit:get_facing()
```

#### get_height
获取高度

* 返回
    * height (number) - 高度

```lua
local height = unit:get_height()
```

#### get_level
获取等级

* 返回
    * level (interger) - 等级

用于[英雄升级]。

```lua
local level = unit:get_level()
```

#### get_max_exp
获取经验上限

* 返回
    * exp (number) - 经验上限

用于[英雄升级]库。

```lua
local exp = unit:get_max_exp()
```

#### get_name
获取名字

* 返回
    * name (string) - 单位名字

```lua
local name = unit:get_name()
```

#### get_owner
获取控制单位的玩家

* 返回
    * player (player) - 控制单位的玩家

```lua
local player = unit:get_owner()
```

#### get_point
获取位置

* 返回
    * point (point) - 位置

```lua
local point = unit:get_point()
```

#### get_resource
获取能量

* 参数
    * type (stirng) - [能量类型][能量]
* 返回
    * value (number) - 数值

```lua
local value = unit:get_resource '怒气'
```

#### get_resource_type
获取能量类型

* 返回
    * type (string) - [能量类型][能量]

```lua
local type = unit:get_resource_type()
```

#### get_restriction
获取行为限制计数

* 参数
    * type (string) - [行为限制]
* 返回
    * count (integer) - 计数

```lua
local count = unit:get_restriction(type)
```

#### get_selected_radius
获取选取半径

* 返回
    * radius (number) - 选取半径

```lua
local radius = unit:get_selected_radius()
```

#### get_team_id
获取队伍ID

* 返回
    * team (integer) - 队伍ID

```lua
local team = unit:get_team_id()
```

#### get_type
获取单位类型

* 返回
    * type (string) - [单位类型]

```lua
local type = unit:get_type()
```

#### get_walk_command
获取移动命令

* 返回
    * *command* (string) - 命令
    * *target* - 目标

可以通过该方法了解单位为什么在移动，返回`walk`表示只是在移动，`attack`表示是为了进行攻击，技能名表示是为了使用技能，`target`为移动目标（如果有的话）。如果不在移动则返回`nil`。

```lua
local command, target = unit:get_walk_command()
```

#### get_xy
获取坐标

* 返回
    * x (number) - X坐标
    * y (number) - Y坐标

```lua
local x, y = unit:get_xy()
```

#### has_restriction
是否存在限制

* 参数
    * type (string) - [行为限制]
* 返回
    * result (boolean) - 结果

```lua
local result = unit:has_restriction(type)
```

#### is_alive
是否存活

* 返回
    * result (boolean) - 是否存活

```lua
local result = unit:is_alive()
```

#### is_ally
是否是友方

* 参数
    * dest (unit/player) - 目标单位/目标玩家
* 返回
    * result (boolean) - 是否是友方

```lua
local result = unit:is_ally(dest)
```

#### is_enemy
是否是敌人

* 参数
    * dest (unit/player) - 目标单位/目标玩家
* 返回
    * result (boolean) - 是否是敌人

```lua
local result = unit:is_enemy(dest)
```

#### is_illusion
是否是幻象

* 返回
    * result (boolean) - 是否是幻象

```lua
local result = unit:is_illusion()
```

#### is_in_range
是否在范围内

* 参数
    * target (point/unit) - 目标位置
    * radius (number) - 范围
* 返回
    * result (boolean) - 结果

会算上自己的[选取半径]。

```lua
local result = unit:is_in_range(target, radius)
```

#### is_visible
是否可见

* 参数
    * dest (unit/player) - 单位/玩家
* 结果
    * result (boolean) - 结果

判断单位能否被`dest`看到。如果`dest`是单位，则会使用控制`dest`的玩家来计算[视野]。

```lua
local result = unit:is_visible(dest)
```

#### is_walking
是否在移动

* 返回
    * result (boolean) - 结果

```lua
local result = unit:is_walking()
```

#### kill
杀死

* 参数
    * *killer* (unit) - 凶手
* 返回
    * result (boolean) - 是否成功

没有凶手时，凶手视为自己。

```lua
local result = unit:kill(killer)
```

#### learn_skill
学技能

* 参数
    * name (string) - 技能名

用于[学习技能]库。

```lua
unit:learn_skill(name)
```

#### lightning
创建闪电

* 参数
    * data (table) - [闪电属性]
* 返回
    * *lightning* (lightning) - 闪电

闪电相关的描述见[这里][闪电]。

```lua
local lightning = unit:lightning
{
    model = '闪电名称',
    source = {unit, 'socket_hit'},
    target = {point, 100},
}
```

#### loop
启动循环计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

等价于`unit:timer(timeout, 0, on_timer)`。只要有可能，就应该使用此方法来启动计时器，而不是使用`ac.loop`。

```lua
local timer = unit:loop(1000, function (timer)
    -- 每1000毫秒执行一次，直到手动移除触发器或单位被移除
end)
```

#### reborn
复活

* 参数
    * where (point) - 复活位置

触发[单位-复活]事件。

```lua
unit:reborn(where)
```

#### remove
移除

```lua
unit:remove()
```

#### remove_animation
移除动画

* 参数
    * name (string) - 动画名称

```lua
unit:remove_animation '动画名称'
```

#### remove_buff
移除状态

* 参数
    * name (string) - 状态名称

会移除所有该名称的状态。

```lua
unit:remove_buff(name)
```

#### remove_provide_sight
不提供视野

* 参数
    * team (integer) - 队伍ID

令单位的视野不提供给指定队伍。视野的说明见[这里][视野]。

```lua
unit:remove_provide_sight(1)
```

#### remove_restriction
移除行为限制

* 参数
    * type (string) - [行为限制]

```lua
unit:remove_restriction '无敌'
```

#### replace_attack
切换攻击技能

* 参数
    * name (string) - [攻击技能]名称

```lua
unit:replace_attack(name)
```

#### replace_skill
切换技能

* 参数
    * old (string) - 旧技能
    * new (string) - 新技能
* 返回
    * result (boolean) - 是否成功

旧技能不能是隐藏类型，如果新技能还没在单位上，则会先添加为隐藏类型，再替换。

```lua
unit:replace_skill(old, new)
```

#### set
设置属性

* 参数
    * state (string) - [单位属性]
    * value (number) - 数值

会清除属性的百分比部分。

```lua
unit:set('生命', 100)
```

#### set_attribute_max
设置属性上限

* 参数
    * state (string) - [单位属性]
    * value (number) - 上限值

```lua
unit:set_attribute_max('攻击速度', 10.0)
```

#### set_attribute_min
设置属性下限

* 参数
    * state (string) - [单位属性]
    * value (number) - 下限值

```lua
unit:set_attribute_min('攻击速度', 1.0)
```

#### set_attribute_sync
设置属性同步方式

* 参数
    * name (string) - [单位属性]
    * sync (string) - [同步方式]

默认同步方式为`none`。

```lua
unit:set_attribute_sync('攻击速度', 'all')
```

#### set_facing
设置朝向

* 参数
    * angle (number) - 朝向
    * *time* (integer) - 转身时间（毫秒）

若不填`time`则按照单位转身速度转身，否则按照`time`指定的时间转身，若`time`为0则立即转身。

```lua
unit:set_facing(90.0, 0)
```

#### set_height
设置高度

* 参数
    * height (number) - 高度

```lua
unit:set_height(100)
```

#### set_level
设置等级

* 参数
    * level (interger) - 等级

用于[英雄升级]库。等级不能降低。

```lua
unit:set_level(2)
```

#### set_model
设置模型

* 参数
    * name (string) - 单位名称

修改为指定单位的模型。

```lua
unit:set_model(name)
```

#### set_resource
设置能量

* 参数
    * type (string) - [能量类型][能量]
    * value (number) - 数值

```lua
unit:set_resource('怒气', 100)
```

#### set_selected_radius
设置选取半径

* 参数
    * radius (numnber) - 选取半径

在使用[选取器]/[搜敌器]/计算施法距离等操作时会考虑选取半径。

```lua
unit:set_selected_radius(radius)
```

#### stop
打断

打断攻击，施法和移动。

```lua
unit:stop()
```

#### stop_attack
打断攻击

```lua
unit:stop_attack()
```

#### stop_cast
打断攻击和施法

```lua
unit:stop_cast()
```

#### stop_skill
打断施法

```lua
unit:stop_skill()
```

#### texttag
漂浮文字

* 参数
    * target (unit) - 创建位置
    * text (string) - 文本内容
    * type (string) - [漂浮文字类型]
    * sync (string) - [同步方式]
    * *data* (table) - 属性
        + r (integer) - RGB的红色[0,255]，只有“其他”支持。
        + g (integer) - RGB的绿色[0,255]，只有“其他”支持。
        + b (integer) - RGB的蓝色[0,255]，只有“其他”支持。
        + *size* (integer) - 漂浮文字大小，默认为10。

同步方式的参考单位为对象自己。

```lua
unit:texttag(target, '文本内容', '其他', 'self', { size = 20 })
```

#### timer
启动计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * count (integer) - 循环次数
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

`count`设置为0表示永久循环。只要有可能，就应该使用此方法来启动计时器，而不是使用`ac.timer`。

```lua
local timer = unit:timer(1000, 10, function (timer)
    -- 每1000毫秒执行一次，执行10次
end)
```

#### wait
启动单次计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

等价于`unit:timer(timeout, 1, on_timer)`。只要有可能，就应该使用此方法来启动计时器，而不是使用`ac.wait`。

```lua
local timer = unit:wait(1000, function (timer)
    -- 在1000毫秒后执行一次
end)
```

#### walk
移动

* 参数
    * target (point) - 目标地点

令单位移动到目标地点。

```lua
unit:walk(target)
```

[单位]: /ac/unit
[单位属性]: /ac/unit/attribute
[行为限制]: /ac/unit/restriction
[可见形状]: /ac/game/可见形状
[技能类型]: /ac/skill/技能类型
[攻击技能]: /ac/skill/攻击技能
[视野]: /ac/game/视野
[能量]: /ac/unit/能量
[状态属性]: /ac/api/buff?id=属性
[ai]: /ac/api/ai
[buff]: /ac/api/buff
[attack]: /ac/api/attack
[event]: /ac/api/event
[弹道捕获器]: /ac/api/capturer
[弹道捕获器属性]: /ac/api/capturer?id=属性
[create_unit]: /ac/api/unit?id=create_unit
[create_illusion]: /ac/api/unit?id=create_illusion
[unit:event]: /ac/api/unit?id=event
[跟随属性]: /ac/api/follow?id=属性
[跟随]: /ac/api/follow
[选取半径]: /ac/api/unit?id=get_selected_radius
[闪电属性]: /ac/api/lightning?id=属性
[闪电]: /ac/api/lightning
[同步方式]: /ac/game/同步方式
[单位类别]: /ac/unit/单位类别
[单位类型]: /ac/unit/单位类型

[单位-初始化]: 404
[单位-创建]: 404
[单位-学习技能]: 404
[单位-复活]: 404
[漂浮文字类型]: 404
[动态碰撞]: /ac/unit/动态碰撞
[静态碰撞]: /ac/unit/静态碰撞
[英雄升级]: /ac/game/英雄升级
[学习技能]: 404
