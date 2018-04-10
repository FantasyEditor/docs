# 点
点表示XY平面上的位置。引擎内部不会修改一个已有点的位置（建议也不要自己去修改），所有会返回点的API都会返回一个创建出来的点。例如将点向某个方向移动，实际上是创建了一个新的点，而不是修改了传入的点。

#### 创建
* 参数
    * x (number) - X坐标
    * y (number) - Y坐标
* 返回
    * point (point) - 点

```lua
local point = ac.point(x, y)
```

#### angle
求角度

* 参数
    * target (point) - 目标点
* 返回
    * angle (number) - `point`到`target`的方向

返回值范围为`(-180, 180]`。作为一个常用操作，我们提供了一个语法糖`point / target`。

```lua
local angle = point1:angle(point2)
local angle = point1 / point2
```

#### copy
复制

* 返回
    * new_point (point) - 复制出来的点

```lua
local new_point = point:copy()
```

#### distance
求距离

* 参数
    * target (point) - 目标点
* 返回
    * distance (number) - `point`到`target`的距离

作为一个常用操作，我们提供了一个语法糖`point * target`。

```lua
local distance = point1:distance(point2)
local distance = point1 * point2
```

#### get_point
获取点

* 返回
    * point (point) - 自己

这个方法不会创建一个新的点，返回的点就是对象自己。

```lua
local point = point:get_point()
```

#### get_xy
获取坐标

* 返回
    * x (number) - X坐标
    * y (number) - Y坐标

```lua
local x, y = point:get_xy()
```

#### is_block
是否是地形阻挡

* 返回
    * result (boolean) - 是否是地形阻挡

```lua
local result = point:is_block()
```

#### play_sound
播放音效

* 参数
    * path (string) - 音效路径
    * distance (number) - 截断距离

当[玩家的英雄]与点的距离超过截断距离时，将听不到音效。

```lua
point:play_sound(path, distance)
```

#### 移动
* 参数
    * angle (number) - 方向
    * distance (number) - 距离
* 返回
    * new_point (point) - 新的点

```lua
local new_point = point - {anlge, distance}
```
