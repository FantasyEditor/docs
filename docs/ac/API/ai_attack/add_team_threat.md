> add_team_threat(team, threat)

增加队伍仇恨

* 参数
    * team (integer) - 队伍id
    * threat (integer) - 仇恨
        + threat > 0: 队伍获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空队伍的负仇恨。
        + threat == 0: 清空队伍的仇恨。
        + threat < 0: 搜敌器将不会搜索到该队伍。清空队伍的正仇恨。

为指定队伍增加仇恨。若有[队伍仇恨]，则[类型仇恨]无效。仇恨相关见[搜敌规则]。

[队伍仇恨]: /ac/API/ai_attack/add_team_threat
[类型仇恨]: /ac/API/ai_attack/add_type_threat
[搜敌规则]: /ac/API/ai_attack/main?id=搜敌规则
