# AI
这里提供了一个行为树框架，你可以利用它来定制AI。


* [构造](/ac/API/ai/构造)
* [属性](/ac/API/ai/属性)
* [事件](/ac/API/ai/事件/main)

#### 例子
制作一个AI，令单位在空闲时利用搜敌器`ai_attack`搜索附近的敌人并攻击：

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

你可以在合适的时候给单位添加AI，例如我们有个单位`u`：

```lua
    u:add_ai '空闲时搜敌' {}
```
