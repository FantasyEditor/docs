# 攻击
+ 攻击定义在[CommonSpellData]中。
+ 将攻击配置在单位的[AttackSkill]中，单位就可以使用它进行攻击。

## active_cd
**`attack:active_cd(max_cd)` 激活冷却**

* 参数
    * max_cd (number) - 冷却上限(秒)

## get_cd
**`attack:get_cd()` 获取冷却**

* 返回
    * cd (number) - 冷却(秒)

## get_name
**`attack:get_name()` 获取攻击名**

* 返回
    * name (string) - 攻击名

## is_common_attack
**`skill:is_common_attack()` 是否是攻击技能**

+ 总是返回 true 。
+ 区别于[skill:is_common_attack]。

* 返回
    * result (boolean) - 结果

## is_skill
**`attack:is_skill()` 是否是技能对象**

+ 总是返回 false 。
+ 区别于[skill:is_skill]。

* 返回
    * result (boolean) - 结果

## set_cd
**`attack:set_cd(cd)` 设置冷却**

+ 对不在冷却状态的攻击无效
+ 冷却的有效范围是[0, 当前冷却上限]

* 参数
    * cd (number) - 冷却(秒)

## stop
**`attack:stop()` 打断攻击**

[CommonSpellData]: 404
[AttackSkill]: 404
[skill:is_common_attack]: /ac/API/skill?id=is_common_attack
[skill:is_skill]: /ac/API/skill?id=is_skill
