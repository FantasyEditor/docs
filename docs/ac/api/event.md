# 事件

事件其实是包含“事件”与“触发器”2个部分。你可以在任意对象上以某个事件名注册触发器，当该对象以该事件发起出事件时，便会被触发器接收到。发起事件时可以传入N个自定义参数，这些参数会被触发器接收到。你可以给同一个对象的某个事件名下注册多个触发器，当发起事件时，这些触发器会按照注册顺序反序执行。在这些触发器依次执行的过程中，删除一个还在排队等待执行的触发器，那么它将不会执行；新建该事件名的触发器，那么它本次也不会执行。

```lua
-- 发起一个事件，自定义参数为...
ac.event_notify(obj, name, ...)

-- 也可以用这个方法来发起事件，区别之后再讲
ac.event_dispatch(obj, name, ...)

-- 这个事件会被该触发器接收到
ac.event_register(obj, name, function (trigger, ...)
    -- 接收到的自定义参数为...
end)
```

为了方便起见，我们将一些常用对象做了封装

> 全局事件。

```lua
-- 发起
ac.game:event_notify(obj, name, ...)
ac.game:event_dispatch(obj, name, ...)

-- 接收
ac.game:event(obj, name, function (trigger, ...)
end)
```

> 玩家事件，玩家事件发起完毕后会使用同样的参数再发起一次全局事件。

```lua
-- 发起
player:event_notify(obj, name, ...)
player:event_dispatch(obj, name, ...)

-- 接收
player:event(obj, name, function (trigger, ...)
end)
ac.game:event(obj, name, function (trigger, ...)
end)
```

> 单位事件，玩家事件发起完毕后会使用同样的参数再发起一次玩家事件和一次全局事件，发起玩家事件时玩家为单位的[控制者]。

```lua
-- 发起
unit:event_notify(obj, name, ...)
unit:event_dispatch(obj, name, ...)

-- 接收
unit:event(obj, name, function (trigger, ...)
end)
unit:get_owner():event(obj, name, function (trigger, ...)
end)
ac.game:event(obj, name, function (trigger, ...)
end)
```

[event_notify]与[event_dispatch]的区别在于，`event_dispatch`可以获取到触发器的返回值：当接收事件的触发器返回了一个非`nil`的值后，当前事件将被终止，并将返回值返回给`event_dispatch`的调用方。而`event_notify`的事件则无法被终止，也不关心返回值。

```lua
ac.game:event('加法', function (trigger, a, b)
    return a + b
end)

local result = ac.game:event_dispatch('加法', 1, 2)
print(result) --> 3
```

### 方法

#### event_register
注册事件

* 参数
    * object (table/userdata) - 对象
    * name (string) - 事件名
    * callback (function) - 回调函数
* 返回
    * trigger (trigger) - 触发器对象
* 回调参数
    * trigger (trigger) - 触发器对象
    * *...* (...) - 自定义数据
* 回调返回
    * *...* (...) - 自定义返回值（只有使用[event_dispatch]时有意义）

#### event_dispatch
发起事件（关心返回值）

* 参数
    * name (string) - 事件名
    * obj (table/userdata) - 对象
    * *...* (...) - 自定义数据

#### event_notify
发起事件（不关心返回值）

* 参数
    * name (string) - 事件名
    * obj (table/userdata) - 对象
    * *...* (...) - 自定义数据

### 内置事件
幻想编辑器内置了许多事件，有些事件是通过[event_dispatch]发起的，意味着你可以给他设置返回值，这些事件会特别说明。

待补充

[控制者]: /ac/api/player?id=get_owner
[event_notify]: /ac/api/event?id=event_notify
[event_dispatch]: /ac/api/event?id=event_dispatch
