# 区域
区域是由多个顶点构成的闭合形状，当单位进入该区域时会触发[on_enter]事件，当单位离开该区域时会触发[on_leave]事件。此外创建区域时，已经在区域内的单位也会触发[on_enter]事件，移除区域时还待在区域内的单位也会触发[on_leave]事件。

### 创建
* 参数
    * list (table) - 顶点列表
* 返回
    * region (region) - 区域

顶点列表是一个存放了多个[点]的序列，这些点依次连接起来（最后一个点连接第一个点）形成一个区域。注意，如果这个列表无法形成一个闭合形状（比如连线有交叉），那么相关事件的行为是未定义的。

```lua
local region = ac.region.polygon
{
    point1,
    point2,
    point3,
    point4,
}
```

### 事件
#### on_enter
进入区域

* 事件参数
    * unit (unit) - 进入区域的单位

```lua
function region:on_enter(unit)
    -- 你的代码
end
```

#### on_leave
离开区域

* 事件参数
    * unit (unit) - 离开区域的单位

```lua
function region:on_leave(unit)
    -- 你的代码
end
```

### 方法
#### remove
移除区域

```lua
region:remove()
```

[on_enter]: /ac/api/region?id=on_enter
[on_leave]: /ac/api/region?id=on_leave
[点]: /ac/api/point
