# 测试

!> 不能在正式环境中使用。

> 通过判断`ac.test`是否存在来区分当前是测试环境还是正式环境。

#### command
获取命令行

* 返回
    * data (table) - 命令行

```lua
local commands = ac.test.command()
for key, value in pairs(commands) do
    -- 读取每个参数
end
```

#### draw_circle
画圆

* 参数
    * where (point) - 位置
    * range (number) - 半径

令客户端在指定位置显示一个圆形。

```lua
ac.test.draw_circle(where, range)
```

#### draw_rect
画矩形

* 参数
    * start (point) - 起点
    * width (number) - 宽度
    * len (number) - 长度
    * angle (number) - 角度

令客户端在指定位置显示一个矩形。

```lua
ac.test.draw_rect(where, width, len, angle)
```

#### draw_clean
清除图案

清除[draw_circle]与[draw_rect]画的图案。

```lua
ac.test.draw_clean()
```

#### message
显示消息

* 参数
    * text (string) - 消息内容

令客户端显示文本消息。

```lua
ac.test.message(text)
```

#### pause
时间停止

The world!

```lua
ac.test.pause()
```

#### resume
时间恢复

It's my turn!

```lua
ac.test.resume()
```

#### update
时间跳跃

Time leaping!

```lua
ac.test.update()
```

#### speed
时间变速

* 参数
    * factor (number) - 时间倍率

只有服务器会变速。

```lua
ac.test.speed(2.0)
```

#### topointer
获取lua对象地址

* 参数
    * obj - 对象

```lua
local pointer = ac.test.topoint(obj)
```

#### unit
获取所有单位

* 返回
    * unittable (table) - 单位表

单位表是一张[弱表]。

```lua
local unittable = ac.test.unit()
for id, unit in pairs(unittable) do
    -- 你的代码
end
```

#### unit_coreref
单位是否被引擎引用

* 参数
    * unit (unit) - 单位
* 返回
    * *ref* (boolean) - 是否引用

如果引擎不引用这个单位，意味着这是一个 **已被删除** 的单位，而脚本还在引用这个单位。

```lua
local ref = ac.test.unit_coreref(unit)
```

#### snapshot
获取Lua的内存快照

* 返回
    * dump (table) - 内存快照

```lua
local dump = ac.test.snapshot
for name, desc in pairs(dump) do
    -- 你的代码
end
```

[弱表]: http://cloudwu.github.io/lua53doc/manual.html#2.5.2
[draw_circle]: /ac/api/test?id=draw_circle
[draw_rect]: /ac/api/test?id=draw_rect
