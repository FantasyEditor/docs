> on_idle()

空闲回调

这里的`self`表示单位。这个事件会在以下3种情况触发：
1. 进入空闲状态。例如技能施放结束；行走结束；攻击结束等。
2. 攻击冷却完成。
3. 根据AI的[pulse]属性周期性调用。

在以下情况下不会触发此事件：
1. 处于死亡状态。
2. 正在攻击或施法。
3. 拥有[隐藏]。
4. 使用[ac.game:disable_ai]关闭了AI。


[pulse]: /ac/API/ai/属性
[隐藏]: /ac/unit/restriction?id=隐藏
[ac.game:disable_ai]: /ac/API/game/disable_ai
