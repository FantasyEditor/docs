# 闪电
闪电是一种拥有2个端点的特效，端点既可以是点，也可以是单位。只有当闪电的2个端点均可见时，闪电才可见。若闪电的2个端点均为单位，那么只要其中一个单位可见，另一个单位就可见。

### 创建
使用[unit:lightning]或[player:lightning]来创建特效。

```lua
local lightning = unit:lightning
{
    model = '闪电名称',
    source = {unit, 'socket_hit'},
    target = {point, 100},
}
```

### 属性
所有特效的属性要作为[创建]特效时的参数传入。

#### model
闪电名称（string)

#### source
闪电起点

支持以下3种格式：
1. `point` - 表示端点在`point`处。
2. `{point, height}` - 表示端点在投影为`point`，高度为`height`处。
3. `{unit, socket}` - 表示端点在`unit`的`socket`节点处，`socket`是由模型决定的一个字符串。

#### target
闪电终点

同起点。

### 方法

#### remove
移除闪电

```lua
lightning:remove()
```

[创建]: /ac/api/lightning?id=创建
[unit:lightning]: /ac/api/unit?id=lightning
[player:lightning]: /ac/api/player?id=lightning
