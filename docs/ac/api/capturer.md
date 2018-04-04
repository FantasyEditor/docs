# 弹道捕获器
弹道捕获器是附着在单位身上的一个圆形范围，当有弹道进入该范围时会触发[on_enter]事件；当有弹道离开该范围时会触发[on_leave]事件。此外，当弹道捕获器创建出来时一开始就在内部的弹道也会触发[on_enter]事件；当弹道捕获器移除时依然在内部的弹道也会触发[on_leave]事件。

弹道捕获器的范围会跟随着附着单位移动，每个单位只能拥有一个捕获器。如果你需要给不特定的单位添加弹道捕获器，应该使用[马甲]令其[跟随]单位，将弹道捕获器附着在这个[马甲]身上。

弹道被捕获后，你可以使用[unit:each_mover]来找到弹道的[运动]。

### 构建
使用[unit:capturer]来创建弹道捕获器。

```lua
    -- 将创建出来的弹道捕获器保存下来，之后需要给他注册事件
    local capturer = unit:capturer
    {
        -- 弹道捕获器的范围
        radius = 500,
    }
```

### 事件
事件需要在[构造]弹道捕获器时注册。事件中的`self`表示捕获器对象。

#### on_enter
弹道进入事件

* 回调参数
    * missile (unit) - 进入范围的弹道单位

```lua
    function capturer:on_enter(missile)
        -- 你的代码
    end
```

#### on_leave
弹道离开事件

* 回调参数
    * missile (unit) - 进入范围的弹道单位

```lua
    function capturer:on_leave(missile)
        -- 你的代码
    end
```

### 方法

#### remove
移除捕获器

```lua
    capturer:remove()
```

[unit:capturer]: /ac/api/unit?id=capturer
[unit:each_mover]: /ac/api/unit?id=each_mover
[on_enter]: /ac/api/capturer?id=on_enter
[on_leave]: /ac/api/capturer?id=on_leave
[马甲]: /ac/game/马甲
[跟随]: /ac/api/unit?id=follow
[运动]: /ac/api/mover
