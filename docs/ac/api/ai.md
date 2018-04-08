# AI
这里提供了一个行为树框架，你可以利用它来定制AI。

> 制作一个AI，令单位在空闲时利用搜敌器`ai_attack`搜索附近的敌人并攻击：

```lua
    -- 给AI设定一个合适的名字，以便在其他地方添加给单位
    local mt = ac.ai['空闲时搜敌']
    -- 每500毫秒执行一次 on_idle
    mt.pulse = 500
    -- 当单位空闲时执行搜敌
    function mt:on_idle()
        -- 这里的self为执行AI的单位
        local target = ai_attack(self)
        if target then
            self:attack(target)
        end
    end
```

> 你可以在合适的时候给单位添加AI，例如我们有个单位`u`：

```lua
u:add_ai '空闲时搜敌' {}
```

### 创建
创建/获取AI
* 参数
    * name (string) - AI名
* 返回
    * ai (ai) - AI对象

使用[unit:add_ai]来添加AI。

```lua
-- 将创建的AI保存下来，你之后可能需要修改它的属性，或是为它注册事件
local mt = ac.ai[name]
```

### 属性
属性只能在[创建]时设置。

#### pulse
心跳

执行[ai:on_idle]的周期，类型为`integer`，单位为`毫秒`。

### 事件
通过注册函数来接收事件。和其他对象的事件不同，AI事件中`self`表示执行AI的单位，而不是AI对象。

#### on_add
获得AI事件

* 回调参数
    * data (table) - AI数据，使用[unit:add_ai]添加AI时作为 **参数2** 传入。

单位添加AI时触发此事件。

```lua
function mt:on_add(data)
    -- 你的代码
end
```

#### on_idle
空闲事件

这个事件会在以下3种情况触发：
1. 进入空闲状态。例如技能施放结束；行走结束；攻击结束等。
2. 攻击冷却完成。
3. 根据AI的[pulse]属性周期性调用。

在以下情况下不会触发此事件：
1. 处于死亡状态。
2. 正在攻击或施法。
3. 拥有[隐藏]。
4. 使用[ac.game:disable_ai]关闭了AI。

```lua
function mt:on_idle()
    -- 你的代码
end
```

#### on_remove
移除AI事件

单位移除AI时触发此事件。

```lua
function mt:on_remove()
    -- 你的代码
end
```

[pulse]: /ac/api/ai?id=pulse
[隐藏]: /ac/unit/restriction?id=隐藏
[ac.game:disable_ai]: /ac/api/game?id=disable_ai
[unit:add_ai]: /ac/api/unit?id=add_ai
[ai:on_idle]: /ac/api/ai?id=on_idle
[创建]: /ac/api/ai?id=pulse
