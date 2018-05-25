# 简易AI
简易AI的详细说明请见[简易AI]

#### add_team_threat
增加队伍仇恨

* 参数
    * unit (unit) - 单位
    * team (integer) - 队伍id
    * threat (integer) - 仇恨
        + threat > 0: 队伍获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空队伍的负仇恨。
        + threat == 0: 清空队伍的仇恨。
        + threat < 0: 不会搜索到该队伍。清空队伍的正仇恨。

为指定队伍增加仇恨。若有[队伍仇恨]，则[类型仇恨]无效。仇恨相关见[搜敌规则]。

```lua
ac.simple_ai.add_team_threat(unit, team, threat)
```

#### add_threat
增加单位仇恨

* 参数
    * unit (unit) - 单位
    * target (unit) - 指定单位
    * threat (integer) - 仇恨
        + threat > 0: 单位获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位的负仇恨。
        + threat == 0: 清空单位的仇恨。
        + threat < 0: 不会搜索到该单位。清空单位的正仇恨。
    * *time* (integer) - 持续时间(毫秒)。若不填时间则表示无限。

为指定单位增加仇恨。若有[单位仇恨]，则单位的[队伍仇恨]与[类型仇恨]均无效。仇恨相关见[搜敌规则]。

```lua
ac.simple_ai.add_threat(unit, target, threat, time)
```

#### add_type_threat
增加类型仇恨

* 参数
    * unit (unit) - 单位
    * type (string) - [单位类型]
    * threat (integer) - 仇恨
        + threat > 0: 单位类型获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位类型的负仇恨。
        + threat == 0: 清空单位类型的仇恨。
        + threat < 0: 不会搜索到该单位类型。清空单位类型的正仇恨。

为指定单位类型增加仇恨。仇恨相关见[搜敌规则]。

```lua
ac.simple_ai.add_type_threat(unit, type, threat)
```

#### chase_limit
设置追击距离限制

* 参数
    * unit (unit) - 单位
    * limit (float) - 距离限制

当自动攻击的追击超出此范围后会强制返回。设置为0表示不允许追击，设置为nil表示无距离限制。默认为无距离限制。

```lua
ac.simple_ai.chase_limit(unit, 500)
```

#### search
开关自动攻击

* 参数
    * unit (unit) - 单位
    * open (boolean) - 开启或关闭

默认为开启。

```lua
ac.simple_ai.search(unit, true)
```

#### walk
沿着路线移动

* 参数
    * unit (unit) - 单位
    * points (table) - 点列表

单位会沿着列表中的点移动，移动过程中可以自动攻击。

```lua
ac.simple_ai.walk(unit, {
    ac.point(1000, 1000),
    ac.point(1000, 4000),
    ac.point(4000, 4000),
    ac.point(4000, 1000),
    ac.point(1000, 1000),
})
```

[简易AI]: 404
[单位类型]: /ac/unit/单位类型
[搜敌规则]: /ac/api/ai_attack?id=搜敌规则
[单位仇恨]: /ac/api/simple_ai?id=add_threat
[队伍仇恨]: /ac/api/simple_ai?id=add_team_threat
[类型仇恨]: /ac/api/simple_ai?id=add_type_threat
