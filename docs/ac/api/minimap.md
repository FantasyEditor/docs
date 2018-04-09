# 小地图

### icon
小地图图标

这个功能可以根据单位名，在小地图上创建这个单位的小地图图标。小地图图标拥有相对复杂的同步规则，用于实现一些比较特别的效果。

> 创建一个图标，所有玩家都能看到

```lua
local icon = ac.minimap.icon(player, name, point)
icon:set_sync 'all'
icon:show()
```

> 隐藏上面创建的图标，且队伍1立即知道这件事（无论是否可见），而其他队伍需要看到这个位置才知道。

```lua
icon:set_sync 'sight'
icon:hide(1)
```

> 如果能看到某个位置，则永久出现一个图标；否则永远看不到这个图标。

```lua
local icon = ac.minimap.icon(player, name, point)
icon:set_sync 'sight'
icon:show()
icon:set_sync 'none'
```

#### 创建
* 参数
    * player (player) - 玩家
    * name (string) - 单位名
    * point (point) - 创建位置
* 返回
    * icon (icon) - 小地图图标

小地图图标创建后是不可见的，需要使用[show]来显示。

```lua
local icon = ac.minimap.icon(player, name, point)
```

#### hide
隐藏

* 参数
    * *team* (integer) - 立即通知的队伍ID

若指定队伍，则该队伍的玩家无视[同步方式]立即隐藏。

```lua
icon:hide(team)
```

#### set_sync
设置同步方式

* 参数
    * sync (string) - 同步方式

同步方式见[这里][同步方式]，默认为`sight`。

```lua
icon:set_sync 'all'
```

#### set_time
设置时间

* 参数
    * time (integer) - 时间（毫秒）

客户端可以使用这个时间在图标上显示倒计时。

```lua
icon:set_time(time)
```

#### show
显示

```lua
icon:show()
```

### signal
小地图信号

用于在小地图上显示信号，信号名称在[Constant]中设置。

#### signal
发送信号

* 参数
    * player (player) - 看到信号的玩家
    * name (string) - 信号名称
    * point (point) - 信号位置

```lua
ac.minimap.signal(player, name, point)
```

[show]: /ac/api/minimap
[同步方式]: /ac/game/同步方式
[Constant]: 404
