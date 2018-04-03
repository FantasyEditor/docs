# 搜敌器
搜敌器可以用来快速搜索可攻击的单位。搜敌器可以添加若干规则，一般某一类单位会共用一个搜敌器。

#### 搜敌规则
1. 搜索范围为 [攻击范围]+[搜敌范围] 的可以攻击的敌人。
2. 将搜索到的敌人根据仇恨排序。
    + 优先使用[单位仇恨]，其次使用[队伍仇恨]，最后使用[类型仇恨]。
    + 若仇恨为负数，则将单位排除。
3. 搜索到仇恨最高的敌人。

#### 例子
这是一个小兵使用的搜敌器的例子：

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

假定我们现在拥有一个小兵`u`，让他进行一次搜敌并攻击搜索到的目标：

```lua
    -- 这里的ai_attack是上面例子中创建出来的
    local target = ai_attack(u)
    -- 判断是不是搜索到了敌人
    if target then
        -- 命令小兵攻击敌人
        u:attack(target)
    end
```

[攻击范围]: /ac/unit/attribute?id=攻击范围
[搜敌范围]: /ac/unit/attribute?id=搜敌范围
[单位仇恨]: /ac/API/ai_attack/add_threat
[队伍仇恨]: /ac/API/ai_attack/add_team_threat
[类型仇恨]: /ac/API/ai_attack/add_type_threat
