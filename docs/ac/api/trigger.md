# 触发器

触发器是包含一个回调函数的对象，它挂载在[事件]之下，在事件触发时触发器会接收到事件以及事件的自定义参数。

### 创建
```lua
local trigger = ac.event_register(obj, '事件名', function (trigger, ...)
    -- 接收到的自定义参数为...
end)
-- 全局事件
local trigger = ac.game:event('事件名', function (trigger, ...)
    -- 接收到的自定义参数为...
end)
-- 玩家事件
local trigger = player:event('事件名', function (trigger, ...)
    -- 接收到的自定义参数为...
end)
-- 单位事件
local trigger = unit:event('事件名', function (trigger, ...)
    -- 接收到的自定义参数为...
end)
```

### 方法
#### disable
禁用

禁用后的触发器不会执行。

```lua
trigger:disable()
```

#### enable
启用

```lua
trigger:enable()
```

#### is_enable
获取是否启用

* 返回
    * result (boolean) - 是否启用

```lua
local result = trigger:is_enable()
```

#### remove
移除

```lua
trigger:remove()
```

#### 执行

* 参数
    * ... - 自定义参数
* 返回
    * ... - 回调函数的返回值

```lua
local result = trigger(...)
```

[事件]: /ac/api/event
