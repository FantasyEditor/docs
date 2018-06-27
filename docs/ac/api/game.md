# 游戏

### 属性
#### max_level
等级上限

用于[英雄升级]库，表示单位可以通过经验达到的等级上限。

```lua
local level = ac.game.max_level
```

### 方法
#### get_winner
获取胜利队伍

* 返回
    * team_id (integer) - 队伍ID

```lua
local team_id = ac.game:get_winner()
```

#### on_reborn
设置英雄复活

* 回到参数
    * dead_hero (unit) - 死亡的英雄
* 回调返回
    * reborn_time (integer) - 复活时间（毫秒）
    * *reborn_point* (point) - 复活位置

用于[英雄复活]，当有单位类型为`英雄`的单位死亡时会执行回调，并使用回调函数返回的时间与位置来决定复活事件与复活后的位置。复活位置默认为单位死亡时的位置。

```lua
ac.game:on_reborn(function (hero)
    return 10000, hero:get_point()
end)
```

#### play_music
播放音乐

* 参数
    * path (string) - 音乐路径

```lua
ac.game:play_music(path)
```

#### play_sound
播放音效

* 参数
    * path (string) - 音效路径
    * *team* (integer) - 队伍ID

```lua
-- 对所有玩家播放音效
ac.game:play_music(path)

-- 对队伍1的玩家播放音效
ac.game:play_music(path, 1)
```

#### player_attribute_sync
设置玩家属性同步方式

* 参数
    * name (string) - 属性名
    * sync (string) - 同步方式

玩家属性见[这里][玩家属性]。同步方式见[这里][同步方式]，默认同步方式为`all`。

```lua
ac.game:player_attribute_sync('金钱', 'all')
```

#### show_timer
设置客户端显示的时间

* 参数
    * time (integer) - 当前时间(毫秒)
    * type (boolean) - 是否是倒计时

```lua
ac.game:show_timer(50000, true)
```

#### set_level_exp
设置升级经验

* 参数
    * list (table) - 升级所需经验列表

用于[英雄升级]库，列表中每一项表示英雄从该等级提升到下一等级所需要的经验。设置后，[max_level]会被设置为列表大小+1。

```lua
ac.game:set_level_exp
{
    100,
    200,
    300,
    400,
}
```

#### set_winner
设置胜利队伍

* 参数
    * team_id (integer) - 胜利队伍

```lua
ac.game:set_winner(1)
```

#### status
获取游戏阶段

* 返回
    * status (integer) - 游戏阶段

```lua
local status = ac.game:status()
```

#### unit_attribute_max
设置单位属性上限

* 参数
    * state (string) - 单位属性
    * value (number) - 上限值

```lua
ac.game:unit_attribute_max('移动速度', 600)
```

只会影响之后创建出来的单位。单位属性的说明见[这里][单位属性]。

#### unit_attribute_min
设置单位属性下限

* 参数
    * state (string) - 单位属性
    * value (number) - 下限值

```lua
ac.game:unit_attribute_min('移动速度', 100)
```

只会影响之后创建出来的单位。单位属性的说明见[这里][单位属性]。

#### unit_attribute_sync
设置单位属性同步方式

* 参数
    * state (string) - 单位属性
    * sync (string) - 同步方式

只会影响之后创建出来的单位。单位属性见[这里][单位属性]。同步方式见[这里][同步方式]，默认同步方式为`none`。

```lua
ac.game:unit_attribute_sync('护盾', 'all')
```

#### wtf
无限火力

* 参数
    * *enable* (boolean) - 开/关
* 返回
    * is_enable (boolean) - 目前的开关状态

令技能无冷却无消耗。

```lua
ac.game.wtf(true)
```

[max_level]: /ac/api/game?id=max_level

[同步方式]: /ac/game/同步方式
[玩家属性]: /ac/player/attribute
[单位属性]: /ac/unit/attribute
[英雄升级]: /ac/game/英雄升级
[英雄复活]: 404
