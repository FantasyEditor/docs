# 特效

### 创建
使用[unit:effect]或[player:effect]来创建特效。

```lua
local effect = unit:effect
{
    model = '特效名称',
    target = {point, 100},
    angle = 90,
}
```

### 属性
所有特效的属性要作为[创建]特效时的参数传入。

#### model
特效名称（string)

#### target
特效位置

支持以下2种格式：
1. `point` - 表示特效创建在`point`处。
2. `{point, height}` - 表示特效创建在投影为`point`，高度为`height`处。

#### *sight*
可见形状（string)

只要形状有一部分可见，整个特效就可见。关于可见形状的描述见[这里][可见形状]。

#### *angle*
特效朝向

支持以下2种格式：
1. `number` - 表示一个二维角度。
2. `{x, y, z}` - 表示一个三维矢量。

#### *size*
整体缩放（number）

原始缩放为1。

#### *x_scale*
x轴缩放（number）

原始缩放为1。

#### *y_scale*
y轴缩放（number）

原始缩放为1。

#### *z_scale*
z轴缩放（number）

原始缩放为1。

#### *loop*
循环播放（boolean）

默认不循环。

#### *speed*
播放速度（number)

默认速度为1。

#### *time*
持续时间（number）

在指定时间后移除特效。单位为秒。

#### *sync*
同步方式

见[同步方式]。默认为`sight`。

### 方法

#### remove
移除特效

```lua
effect:remove()
```

[创建]: /ac/api/effect?id=创建
[可见形状]: /ac/game/可见形状
[同步方式]: /ac/game/同步方式
[unit:effect]: /ac/api/unit?id=effect
[player:effect]: /ac/api/player?id=effect
