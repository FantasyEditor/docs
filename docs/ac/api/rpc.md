# 远程服务
这些功能可以向远程服务发送请求，用于[计费]、[积分]等功能。调用远程服务会有3种结果：成功、失败与超市。成功时会返回该请求的返回值，失败时则会返回错误码。通过注册`ok`、`error`和`timeout`事件可以获取请求结果。当然如果你不关心请求结果你可以不注册这些事件。

### bank
计费服务

#### query
查询余额

* 参数
    * player (player) - 查询的用户
* 返回
    * money (number) - 余额

```lua
ac.rpc.bank.query(player)
{
    ok = function (money)
        -- 查询成功，player的余额为money
    end,
    error = function (code)
        -- 查询失败，错误码为code
    end,
    timeout = function ()
        -- 查询超时
    end,
}
```

#### pay
扣费

* 参数
    * player (player) - 扣费的用户
    * money (integer) - 扣费
    * *reason* (string) - 扣费原因
* 返回
    * money (number) - 余额

```lua
ac.rpc.bank.pay(player, money, '重新随机') -- 将player的余额扣除money，原因为“重新随机”
{
    ok = function (money)
        -- 扣费成功，扣费后player的余额为money
    end,
    error = function (code)
        -- 扣费失败，错误码为code
    end,
    timeout = function ()
        -- 扣费超时
    end,
}
```

### score
积分服务

积分分为全局积分与本局积分，其中全局积分在提交后，会在下局游戏中获取到，可用于统计胜利次数等功能；本局积分只在本局有效，不影响其他游戏局，可用于本局游戏的战斗报告等功能。

积分使用字符串作为索引，使用数字或字符串作为值。一旦确定一个积分的类型后，你将不能再修改他的类型。

积分的操作分为累加和设置，只有数字型的积分才能进行累加操作。将积分设置为`nil`可以删除该积分，当你决定不再使用某项积分后，可以删除它。

积分初始化后会在本地创建一个积分副本，其中全局积分会被复制到这个副本上，之后获取积分时会从这个副本中获取值。当对积分进行操作时，并不会直接修改副本，而是将操作记录下来。当使用提交积分的功能提交操作记录后，才会根据操作记录更新副本。

除了提交积分外，其它积分操作都是本地操作，会立即返回。

!> 玩家离开游戏后，[设置全局积分]会失败，这是为了避免与该玩家新开游戏局的全局积分冲突。你应该在玩家离开游戏时立即将你想设置的全局积分设置完毕并提交，之后不要再设置该玩家的全局积分。

!> 在同一局中，无法删除一个已被操作过的积分，也无法操作一个已被删除的积分。

#### 初始化
使用代码`require 'ac.score'`来初始化积分。积分初始化需要一段时间，请不要立即使用它。

#### on_init
初始化事件

* 参数
    * player (player) - 玩家
    * events (table) - 响应事件

当积分初始化完成时，会根据初始化结果执行对应事件。如果使用该方法时积分已经初始化完毕，则会立即执行。

```lua
ac.score.on_init(player) -- 注册player的积分初始化事件
{
    ok = function ()
        -- 初始化完成
    end,
    error = function ()
        -- 初始化失败
    end,
    timeout = function ()
        -- 初始化超时
    end,
}
```

#### get
获取全局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
* 返回
    * score (string/number/nil) - 积分

获取该用户上次提交后的全局积分。

```lua
local score = ac.score.get(player, key)
```

#### set
设置全局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
    * score (string/number/nil) - 积分
* 返回
    * ok (boolean) - 是否成功

```lua
ac.score.set(player, key, score)
```

#### add
累加全局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
    * score (number) - 积分
* 返回
    * ok (boolean) - 是否成功

```lua
ac.score.add(player, key, score)
```

#### lget
获取本局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
* 返回
    * score (string/number/nil) - 积分

获取该用户上次提交后的本局积分。

```lua
local score = ac.score.lget(player, key)
```

#### lset
设置本局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
    * score (string/number/nil) - 积分
* 返回
    * ok (boolean) - 是否成功

```lua
ac.score.lset(player, key, score)
```

#### ladd
累加本局积分

* 参数
    * player (player) - 玩家
    * key (string) - 索引
    * score (number) - 积分
* 返回
    * ok (boolean) - 是否成功

```lua
ac.score.ladd(player, key, score)
```

#### commit
提交积分

* 参数
    * player (player) - 玩家
    * events (table) - 响应事件

提交玩家的积分，提交积分需要一段时间，在提交完成前对该玩家的积分操作均会失败。如果提交时玩家已经离开游戏，而待提交的积分中包含了[设置全局积分][set]，那么提交将会失败，请务必避免这种情况。

```lua
ac.score.commit(player) -- 提交player的积分
{
    ok = function ()
        -- 提交成功
    end,
    error = function (code)
        -- 提交失败，错误码为code
    end,
    timeout = function ()
        -- 提交超时
    end,
}
```

[计费]: /ac/api/rpc?id=计费
[积分]: /ac/api/rpc?id=积分
[创建]: /ac/api/rpc?id=创建
[set]: /ac/api/rpc?id=set
