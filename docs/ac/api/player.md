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
    model = '闪电名称',
    source = {unit, 'socket_hit'},
    target = {point, 100},
}
```

#### lock_camera
锁定镜头

锁定镜头后，玩家无法通过客户端操作改变镜头状态，[move_camera]无效。使用[unlock_camera]解锁镜头。

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

#### message_box
弹框消息

* 参数
    * text (string) - 消息内容

```lua
player:message_box('消息内容')
```

#### move_camera
移动镜头

* 参数
    * target (point) - 镜头目标

```lua
player:move_camera(point)
```

#### play_music
播放音乐

* 参数
    * path (string) - 音乐路径

```lua
player:play_music(path)
```

#### play_sound
播放音效

* 参数
    * path (string) - 音效路径

```lua
player:play_sound(path)
```

#### set
设置属性

* 参数
    * state (string) - 属性名称
    * value (number) - 数值

玩家属性说明见[这里][玩家属性]

```lua
player:set('金钱', 100)
```

#### set_afk
设置为挂机

```lua
player:set_afk()
```

#### set_camera
设置镜头

* 参数
    * data (table) - 镜头状态
        + position (point/table) - 镜头目标位置，可以直接使用`point`，也可以使用`{x, y, z}`来表示一个三维的位置。
        + *rotation* (table) - 镜头旋转，使用`{rx, ry, rz}`来表示镜头在3个坐标轴上的旋转，默认为`{0, 0, 0}`。
        + *focus_distance* (number) - 摄像机与镜头目标之间的距离，默认为0。
        + *time* (integer) - 变换时间，默认为0，单位为毫秒。

当变换时间为0时会镜头立即变换到新的状态，否则镜头会在变换时间内平滑运动至新的状态。使用该方法会立即中断上次的镜头变换。

```lua
player:set_camera
{
    position = {x, y, z},
    rotation = {rx, ry, rz},
    focus_distance = 1000,
    time = 2000,
}
```

#### set_hero
设置英雄

* 参数
    * hero (unit) - [玩家的英雄]

```lua
player:set_hero(hero)
```

#### set_team_id
设置队伍ID

* 参数
    * id (integer) - 队伍ID

```lua
player:set_team_id(1)
```

#### shake_camera
屏幕震动

* 参数
    * type (integer) - 震动类型

目前支持的震动类型为`0`，`1`，`2`，`3`。

```lua
player:shake_camera(0)
```

#### unlock_camera
解锁镜头

```lua
player:unlock_camera()
```

#### user_agent
用户客户端

* 返回
    * agent (string) - 客户端

用于分辨用户使用的客户端，该值由客户端上传。

```lua
local agent = player:user_agent()
```

#### user_id
用户ID

* 返回
    * id (integer) - 用户ID

```lua
local id = player:user_id()
```

#### user_info
用户信息

* 参数
    * type (string) - 类型
* 返回
    * list (table) - 信息列表

类型为`英雄`时，返回用户可用英雄的名单；类型`免费英雄`时，返回用户免费英雄的名单；类型为`养成`时，返回用户使用的符卡名单。

```lua
local list = player:user_info '免费英雄'
```

#### user_level
用户等级

* 返回
    * level (integer) - 平台等级

```lua
local level = player:user_level()
```

[玩家属性]: /ac/player/attribute
[create_illusion]: /ac/api/unit?id=create_illusion
[单位-初始化]: 404
[config]: 404
[闪电]: /ac/api/lightning
[event]: /ac/api/event
[玩家的英雄]: /ac/player/玩家的英雄
[lock_camera]: /ac/api/player?id=lock_camera
[unlock_camera]: /ac/api/player?id=unlock_camera
[move_camera]: /ac/api/player?id=move_camera
[消息格式化]: /ac/player/消息格式化

