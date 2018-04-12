# 杂项

#### hash
哈希

* 参数
    * text (string) - 文本
* 返回
    * hash (integer) - 哈希值

```lua
local hash = ac.hash(text)
```

#### math_angle
获取夹角

* 参数
    * r1 (number) - 角度1
    * r2 (number) - 角度2
* 返回
    * angle (number) - 夹角
    * direction (integer) - 方向

`angle`的范围为[0, 180]，`direction`为-1或1，且满足`r1 + angle * direction == r2`。

```lua
local angle, direction = ac.math_angle(r1, r2)
```

#### sight_line
直线可见形状

* 参数
    * start (point) - 起点
    * angle (number) - 方向
    * len (number) - 长度
* 返回
    * sight (table) - 点的列表

可见形状见[这里][可见形状]。

```lua
local sight = ac.sight_line(start, angle, len)
```

#### sight_range
圆形可见形状

* 参数
    * poi (point) - 圆心
    * radius (number) - 半径
* 返回
    * sight (table) - 点的列表

可见形状见[这里][可见形状]。

```lua
local sight = ac.sight_range(poi, radius)
```

[可见形状]: /ac/game/可见形状

#### split
分割字符串

* 参数
    * text (string) - 要分割的字符串
    * sep (string) - 分隔符
* 返回
    * result (table) - 字符列表

分隔符只能是1个字符。

```lua
local result = ac.split('abc_123_ddd', '_') --> {'abc', '123', 'ddd'}
```
