# 搜敌器
搜敌器可以用来快速搜索可攻击的单位。搜敌器可以添加若干规则，一般某一类单位会共用一个搜敌器。

#### 搜敌规则
1. 搜索范围为 [攻击范围]+[搜敌范围] 的可以攻击的敌人。
2. 将搜索到的敌人根据仇恨排序。
    + 优先使用[单位仇恨]，其次使用[队伍仇恨]，最后使用[类型仇恨]。
    + 若仇恨为负数，则将单位排除。
3. 搜索到仇恨最高的敌人。

> 这是一个小兵使用的搜敌器的例子：

```lua
-- 创建搜敌器
local ai_attack = ac.ai_attack {}
-- 不攻击野怪，排除队伍3(我们假定野怪是队伍3)
ai_attack:add_team_threat(3, -1)
-- 最先攻击敌方小兵
ai_attack:add_type_threat('小兵', 3)
-- 其次攻击敌方召唤物
ai_attack:add_type_threat('召唤物', 2)
-- 再次攻击敌方建筑
ai_attack:add_type_threat('建筑', 1)
```

> 假定我们现在拥有一个小兵`u`，让他进行一次搜敌并攻击搜索到的目标：

```lua
-- 这里的ai_attack是上面例子中创建出来的
local target = ai_attack(u)
-- 判断是不是搜索到了敌人
if target then
    -- 命令小兵攻击敌人
    u:attack(target)
end
```

#### 创建
创建搜敌器

* 参数
    * data (table) - 自定义数据
* 返回
    * ai_attack (ai_attack) - 搜敌器

```lua
local ai_attack = ac.ai_attack {}
```

#### add_team_threat
增加队伍仇恨

* 参数
    * team (integer) - 队伍id
    * threat (integer) - 仇恨
        + threat > 0: 队伍获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空队伍的负仇恨。
        + threat == 0: 清空队伍的仇恨。
        + threat < 0: 搜敌器将不会搜索到该队伍。清空队伍的正仇恨。

为指定队伍增加仇恨。若有[队伍仇恨]，则[类型仇恨]无效。仇恨相关见[搜敌规则]。

```lua
ai_attack:add_team_threat(team, threat)
```

#### add_threat
增加单位仇恨

* 参数
    * unit (unit) - 指定单位
    * threat (integer) - 仇恨
        + threat > 0: 单位获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位的负仇恨。
        + threat == 0: 清空单位的仇恨。
        + threat < 0: 搜敌器将不会搜索到该单位。清空单位的正仇恨。
    * *time* (integer) - 持续时间(毫秒)。若不填时间则表示无限。

为指定单位增加仇恨。若有[单位仇恨]，则单位的[队伍仇恨]与[类型仇恨]均无效。仇恨相关见[搜敌规则]。

```lua
ai_attack:add_threat(unit, threat, time)
```

#### add_type_threat
增加类型仇恨

* 参数
    * type (string) - 单位类型，使用[UnitType]
    * threat (integer) - 仇恨
        + threat > 0: 单位类型获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位类型的负仇恨。
        + threat == 0: 清空单位类型的仇恨。
        + threat < 0: 搜敌器将不会搜索到该单位类型。清空单位类型的正仇恨。

为指定单位类型增加仇恨。仇恨相关见[搜敌规则]。

```lua
ai_attack:add_threat(type, threat)
```

#### ai_attack
进行搜敌

* 返回
    * *target* (unit) - 搜索到的单位

根据[搜敌规则]搜索敌人。

```lua
local target = ai_attack(unit)
```

[搜敌规则]: /ac/api/ai_attack/main?id=搜敌规则
[UnitType]: 404
[单位仇恨]: /ac/api/ai_attack?id=add_threat
[队伍仇恨]: /ac/api/ai_attack?id=add_team_threat
[类型仇恨]: /ac/api/ai_attack?id=add_type_threat
[攻击范围]: /ac/unit/attribute?id=攻击范围
[搜敌范围]: /ac/unit/attribute?id=搜敌范围
