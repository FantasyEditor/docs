# 特效

#### 创建
使用[unit:effect]或[player:effect]来创建特效。

#### 属性
所有特效的属性要作为[创建]特效时的参数传入。

特效点支持以下2种格式：
1. `point` - 表示特效创建在`point`处。
2. `{point, height}` - 表示特效创建在投影为`point`，高度为`height`处。

三位矢量支持以下2种格式：
1. `number` - 表示一个二维角度。
2. `{x, y, z}` - 表示一个三维矢量。

```lua
    unit:effect
    {
        model = '特效名称',
        target = {point, 100},
        angle = 90,
    }
```

[创建]: /ac/api/effect?id=创建
[unit:effect]: /ac/api/unit?id=effect
[player:effect]: /ac/api/player?id=effect
