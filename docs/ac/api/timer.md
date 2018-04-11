# 计时器

计时器的精度为1毫秒，每当计时器到期便会执行回调函数。

### 创建
#### timer
启动计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * count (integer) - 循环次数
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

`count`设置为0表示永久循环。

```lua
local timer = ac.timer(1000, 10, function (timer)
    -- 每1000毫秒执行一次，执行10次
end)
```

#### loop
启动循环计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

等价于`ac.timer(timeout, 0, on_timer)`。

```lua
local timer = ac.loop(1000, function (timer)
    -- 每1000毫秒执行一次，直到手动移除触发器
end)
```

#### wait
启动单次计时器

* 参数
    * timeout (integer) - 周期（毫秒）
    * on_timer (function) - 回调函数
* 返回
    * timer (timer) - 计时器
* 回调参数
    * timer (timer) - 计时器

等价于`ac.timer(timeout, 1, on_timer)`。

```lua
local timer = ac.wait(1000, function (timer)
    -- 在1000毫秒后执行一次
end)
```

### 方法
#### clock
获取游戏时间

* 返回
    * gameclock (integer) - 游戏时间（毫秒）

```lua
local clock = ac.clock()
```

#### pause
暂停

```lua
timer:pause()
```

#### remove
移除

```lua
timer:remove()
```

#### resume
恢复

将计时器从暂停中恢复，它会恢复之前的剩余时间与执行次数继续执行。

```lua
timer:resume()
```

#### restart
重启

将计时器的剩余时间重置回周期时间，但不影响执行次数。

```lua
timer:restart()
```
