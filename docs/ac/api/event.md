# 事件

事件其实是包含“事件”与“触发器”2个部分。你可以在任意对象上以某个事件名注册触发器，当该对象以该事件发起出事件时，便会被触发器接收到。发起事件时可以传入N个自定义参数，这些参数会被触发器接收到。你可以给同一个对象的某个事件名下注册多个触发器，当发起事件时，这些触发器会按照注册顺序反序执行。

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

全局事件。
```lua
-- 发起
ac.game:event_notify(obj, name, ...)
ac.game:event_dispatch(obj, name, ...)

-- 接收
ac.game:event(obj, name, function (trigger, ...)
end)
```

玩家事件，玩家事件发起完毕后会使用同样的参数再发起一次全局事件。
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

单位事件，玩家事件发起完毕后会使用同样的参数再发起一次玩家事件和一次全局事件，发起玩家事件时玩家为单位的[控制者]。

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

[控制着]: /ac/api/player?id=get_owner
