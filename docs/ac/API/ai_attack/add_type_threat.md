> add_type_threat(type, threat)

增加类型仇恨

* 参数
    * type (string) - 单位类型，使用[UnitType]
    * threat (integer) - 仇恨
        + threat > 0: 单位类型获得该等级的仇恨，不同等级的仇恨可以共存，最高的仇恨生效。清空单位类型的负仇恨。
        + threat == 0: 清空单位类型的仇恨。
        + threat < 0: 搜敌器将不会搜索到该单位类型。清空单位类型的正仇恨。

为指定单位类型增加仇恨。仇恨相关见[搜敌规则]。

[搜敌规则]: /ac/API/ai_attack/main?id=搜敌规则
[UnitType]: 404
