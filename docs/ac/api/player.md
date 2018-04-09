# 玩家

#### 创建
* 参数
    * id (integer) - 玩家ID
* 返回
    * player (player) - 玩家

```lua
local player = ac.player(id)
```

#### add
增加属性

* 参数
    * state (string) - 属性名称
    * value (number) - 数值

玩家属性说明见[这里][玩家属性]

```lua
player:add('金钱', 100)
```

#### controller
获取玩家控制者

* 返回
    * controller (string) - 会是以下值中的一个
        + `human` - 用户
        + `none` - 空位
        + `computer` - 电脑
        + `ai` - AI
        + `human-ai` - 像人AI

```lua
local controller = player:controller()
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

创建出来的单位由`player`控制。如果`name`是一个字符串，那么会创建名称为`name`的单位。如果`name`是一个单位，那么会复制该单位，目前该行为等同于[create_illusion]。单位创建后会最先执行`on_init`，你可以在这里给单位初始化一些数据。创建单位的流程如下：
+ 执行`on_init`
+ 触发[单位-初始化]事件
+ 添加技能
+ 触发[单位-创建]事件

```lua
local unit = player:create_unit('鹿目圆香', where, face, function (unit)
    -- 对unit进行初始化设置
end)
```

#### each_player
遍历玩家

* 参数
    * *type* (string) - 玩家类型
* 返回
    * player (player) - 玩家

当设置了`type`后，会遍历所有该类型的玩家（根据[config]中的玩家定义）；否则会遍历所有玩家。按照玩家ID从小到大的顺序遍历。

```lua
for player in ac.each_player 'user' do
    -- 你的代码
end
```

#### effect
创建特效

* 参数
    * data (table) - 属性表
* 返回
    * effect (effect) - 特效

特效的属性与相关信息见[这里][特效]

```lua
local effect = player:effect
{
    model = '特效名',
    target = point,
}
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
local trigger = player:event('玩家-选择英雄', function (trigger, player, name)
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
player:event_dispatch('自定义事件', ...)
```

#### event_notify
触发事件

* 参数
    * name (string) - 事件名
    * ... (...) - 自定义数据

这是对`ac.event_notify`方法的封装，你可以在[这里][event]看到详细说明。

```lua
player:event_notify('自定义事件', ...)
```

#### get
获取属性

* 参数
    * state (string) - 属性名称
* 返回
    * value (number) - 数值

玩家属性说明见[这里][玩家属性]

```lua
local value = player:get '金钱'
```

#### get_hero
获取英雄

* 返回
    * hero (unit) - [玩家的英雄]

```lua
local hero = player:get_hero()
```

#### get_slot_id
获取玩家ID

* 返回
    * id (integer) - 玩家ID

```lua
local id = player:get_slot_id()
```

#### get_team_id
获取队伍ID

* 返回
    * id (integer) - 队伍ID

```lua
local id = player:get_team_id()
```

#### input_mouse
获取鼠标位置

* 返回
    * mouse (point) - 鼠标位置

这个属性是由客户端上传的，需要客户端支持鼠标才有意义。

```lua
local point = player:input_mouse()
```

#### input_rocker
获取摇杆方向

* 返回
    * rocker (number) - 摇杆方向

这个属性是由客户端上传的，需要客户端支持摇杆才有意义。如果摇杆是松开状态，则返回`nil`。

```lua
local rocker = player:input_rocker()
```

#### kick
踢出游戏

* 参数
    * backend (string) - 服务器记录的原因
    * frontend (string) - 客户端显示的原因

```lua
player:kick('挂机', '你由于离开时间过长而被踢出游戏')
```

#### leave_reason
获取退出原因

* 返回
    * leave_reason (string) - 原因

```lua
local reason = player:leave_reason()
```

#### lightning
创建闪电

* 参数
    * data (table) - 属性表
* 返回
    * lightning (lightning) - 闪电

闪电的属性与相关信息见[这里][lightning]

```lua
local lightning = player:lightning
{
    model = '闪电名',
    start = start,
    target = target,
}
```

#### lock_camera
锁定镜头

令玩家无法通过客户端操作改变镜头状态。使用[unlock_camera]解锁镜头。

```lua
player:lock_camera()
```

#### message
发送消息

* 参数
    * data (table) - 属性表，可用属性如下：
        + text (string) - 消息内容
        + type (string) - 消息类型
            - `ann` 公告
            - `chat` 聊天
            - `error` 错误
        + *time* (integer) - 持续时间（毫秒）
        + *sound* (string) - 音效
        + *effect* (string) - 特效

```lua
player:message
{
    text = '消息内容',
    type = 'chat',
}
```

消息内容可以通过特殊标志进行格式化：
+ `$player[x]$` 格式化为玩家[X]的名字。
+ `&[X][Y]&` 格式化为单位[Y]的头像，边框颜色为[X]。

> 显示公告“[玩家1的名字]选择了英雄[鹿目圆香的头像，蓝色边框]”

```lua
player:message
{
    text = '$player1$选择了英雄&#0000ff鹿目圆香&',
    type = 'ann',
}
```

[玩家属性]: /ac/player/attribute
[create_illusion]: /ac/api/unit?id=create_illusion
[单位-初始化]: 404
[config]: 404
[特效]: /ac/api/effect
[闪电]: /ac/api/lightning
[event]: /ac/api/event
[玩家的英雄]: /ac/player/玩家的英雄
[lock_camera]: /ac/api/player?id=lock_camera
[unlock_camera]: /ac/api/player?id=unlock_camera
[消息格式化]: /ac/player/消息格式化

