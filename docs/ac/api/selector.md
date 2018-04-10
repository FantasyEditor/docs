# 选取器
选取器可以通过指定一个形状和若干规则来选取某个区域内的多个单位。选取器有一些默认的规则：
+ 形状为半径99999的圆形
+ 不选取拥有[无敌]或[蝗虫]行为限制的单位
+ 只选取活着的生物

> 选取`target`附近的敌方英雄

```lua
local group = ac.selector()
    -- 圆心为target，半径为1000的圆形
    : in_range(target, 1000)
    -- 只选取hero的敌人
    : is_enemy(hero)
    -- 只选取类型为英雄的单位
    : of_type {'英雄'}
    -- 获取选取结果
    : get()
```

> 获取到的选取结果是一张存放了所有被选到单位的序列，你可以遍历这张表来执行你的代码

```lua
for _, unit in ipairs(group) do
    -- 你的代码
end
```

> 如果你想在选取后直接遍历，可以这么写

```lua
for _, unit in ac.selector()
    : in_range(target, 1000)
    : is_enemy(hero)
    : of_type {'英雄'}
    : ipairs()
do
    -- 你的代码
end
```

> 选取器可以进行排序，以便你选取到最特殊的几个单位

```lua
-- 选取target附近的单位，并将距离近的单位排在前面
local group = ac.selector()
    : in_range(target, 1000)
    : sort_nearest_unit(target)

-- 队伍第1位的单位就是离target最近的单位
local unit = group[1]
```

### 创建
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
```

#### selector_type_of
设置默认选取类型

* 参数
    * list (table) - 类型列表

设置默认选取类型后，相当于[创建]选取器后自动使用了[of_type]

```lua
ac.selector_type_of = {'英雄', '小兵', '野怪'}
```

### 形状
选取器只能拥有一个形状，后设置的形状覆盖之前的形状

#### in_line
直线

* 参数
    * start (point) - 起点
    * angle (number) - 方向
    * len (number) - 长度
    * width (number) - 宽度
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : in_line(start, angle, len, width)
```

#### in_range
圆形

* 参数
    * center (point) - 圆心
    * range (number) - 半径
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : in_range(center, range)
```

#### in_sector
扇形

* 参数
    * center (point) - 圆心
    * range (number) - 半径
    * angle (number) - 角度
    * section (number) - 扇形区间
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : in_sector(center, range, angle, section)
```

### 规则
根据形状选取到单位后，会根据规则进行一次筛选。

#### is_ally
只选取友方

* 参数
    * who (unit/player) - 参考单位或玩家
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : is_ally(hero)
```

#### is_enemy
只选取敌方

* 参数
    * who (unit/player) - 参考单位或玩家
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : is_enemy(hero)
```

#### is_not
排除指定单位

* 参数
    * unit (unit) - 排除的单位
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : is_not(unit)
```

#### of_add
添加选取类型

* 参数
    * type (string) - 单位类型
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : of_add '建筑'
```

#### of_add
移除选取类型

* 参数
    * type (string) - 单位类型
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : of_remove '英雄'
```

#### of_type
设置选取类型

* 参数
    * list (table) - 单位类型列表
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : of_type {'英雄', '小兵', '建筑'}
```

#### of_visible
只选取可见单位

* 参数
    * unit (unit) - 参考单位
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : of_visible(unit)
```

#### of_not_illusion
排除幻象单位

* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : of_not_illusion()
```

#### allow_god
允许选取无敌单位

* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : allow_god()
```

#### add_filter
自定义规则

* 参数
    * filter (function) - 规则
* 返回
    * selector (selector) - 选取器

选取器可以添加多个自定义规则，他们按照添加顺序依次执行。规则是一个函数，它会接收一个单位作为参数，应该返回`true`或者`false`，返回`false`表示该单位被排除。

```lua
local selector = ac.selector()
    : add_filter(function (unit)
        -- 排除所有生命值大于50%的单位
        if unit:get '生命' / unit:get '生命上限' > 0.5 then
            return false
        else
            return true
        end
    end)
```

### 排序
#### sort_nearest_unit
近的单位在前面

* 参数
    * where (point/unit) - 参考位置
* 返回
    * selector (selector) - 选取器

```lua
local selector = ac.selector()
    : sort_nearest_unit(where)
```

#### set_sorter
自定义排序

* 参数
    * sorter (function) - 排序器
* 返回
    * selector (selector) - 选取器

排序器必须是一个可以接收2个单位为参数的函数，当第1个单位需要排在第2个单位之前时，返回`true`。内部使用[table.sort]实现。

```lua
local selector = ac.selector()
    -- 血量少的排在前面
    : set_sorter(function (unit1, unit2)
        if unit1:get '生命' < unit2:get '生命' then
            return true
        else
            return false
        end
    end)
```

### 选取
执行选取器，将选取到的单位按需求返回

#### get
选取

* 返回
    * group (table) - 单位列表

```lua
local group = ac.selector()
    : get()
```

#### ipairs
遍历

* 遍历
    * index (integer) - 序列
    * unit (unit) - 单位

```lua
for index, unit in ac.selector()
    : ipairs()
do
    -- 你的代码
end
```

#### random
随机

* 返回
    * unit (unit) - 单位

返回选取单位列表中的随机单位，如果单位列表为空，则返回`nil`。

```lua
local unit = ac.selector()
    : random()
```

[无敌]: /ac/unit/restriction?id=无敌
[蝗虫]: /ac/unit/restriction?id=蝗虫
[创建]: /ac/api/selector?id=创建
[of_type]: /ac/api/selector?id=of_type
[table.sort]: http://cloudwu.github.io/lua53doc/manual.html#pdf-table.sort
