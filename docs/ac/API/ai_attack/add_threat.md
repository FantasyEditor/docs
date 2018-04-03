> add_threat(unit, threat, time)

增加单位仇恨

* 参数
    * unit (unit) - 指定单位
    * threat (integer) - 仇恨
        + threat > 0: 单位获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位的负仇恨。
        + threat == 0: 清空单位的仇恨。
        + threat < 0: 搜敌器将不会搜索到该单位。清空单位的正仇恨。
    * *time* (integer) - 持续时间(毫秒)。若不填时间则表示无限。

为指定单位增加仇恨。若有[单位仇恨]，则单位的[队伍仇恨]与[类型仇恨]均无效。仇恨相关见[搜敌规则]。

[单位仇恨]: /ac/API/ai_attack/add_threat
[队伍仇恨]: /ac/API/ai_attack/add_team_threat
[类型仇恨]: /ac/API/ai_attack/add_type_threat
[搜敌规则]: /ac/API/ai_attack/main?id=搜敌规则
