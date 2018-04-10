# 远程服务
这些功能可以向远程服务发送请求，用于[计费]、[积分]等功能。rpc功能只能在rpc环境中使用，使用[创建]来创建rpc环境。rpc环境可以被休眠，当处于休眠状态时代码会暂停执行，休眠结束后才会继续执行。

```lua
-- 创建rpc环境
ac.rpc(function (rpc)
    -- 记录当前时间
    local clock = ac.clock() 
    for i = 1, 5 do
        -- 主动休眠500毫秒
        rpc.sleep(500)
        -- 显示局部变量i，以及当前经过的时间
        print(i, ac.clock() - clock)
    end
end)
```

以上代码执行后将会依次显示：

```lua
1   500
2   1000
3   1500
4   2000
5   2500
```

### 创建
```lua
ac.rpc(function (rpc)
    -- 这里是rpc环境
end)
```

#### sleep
休眠

* 参数
    * clock (integer) - 休眠时间，单位为毫秒

使用这个方法可以让rpc环境休眠，经过指定休眠时间后再继续执行之后的代码。

```lua
ac.rpc(function (rpc)
    print(1) -- 立即显示 1
    rpc.sleep(1000)
    print(2) -- 1000毫秒后显示 2
end)
```

### bank
计费服务

#### query
查询余额

* 参数
    * player (player) - 查询的用户
* 返回
    * ok (boolean) - 是否成功
    * result (integer/string) - 余额/失败原因
    * *code* (integer) - 错误码

这是一个rpc功能，只能在rpc环境中使用，查询结束前会让rpc环境休眠。返回值有3种情况：
1. 查询成功，返回`true, result`，其中result为余额。
2. 查询失败，返回`false, 'error', code`。
3. 查询超时，返回`false, 'timeout'`。

```lua
ac.rpc(function (rpc)
    -- 查询player的余额
    local ok, res, code = rpc.bank.query(player)
    if ok then
        -- 请求成功，player的余额为res
    else
        -- 请求失败
        if res == 'error' then
            -- 请求失败，错误码为code
        elseif res == 'timeout' then
            -- 请求超时
        end
    end
end)
```

#### pay
扣费

* 参数
    * player (player) - 扣费的用户
    * money (integer) - 扣费
    * *reason* (string) - 扣费原因
* 返回
    * ok (boolean) - 是否成功
    * result (string) - 余额/失败原因
    * *code* (integer) - 错误码

这是一个rpc功能，只能在rpc环境中使用，扣费结束前会让rpc环境休眠。返回值有3种情况：
1. 扣费成功，返回`true, result`，其中result为扣费后的余额。
2. 扣费失败，返回`false, 'error', code`。
3. 扣费超时，返回`false, 'timeout'`。

```lua
ac.rpc(function (rpc)
    -- 将player的余额扣除money
    local ok, res, code = rpc.bank.pay(player, money, '原因')
    if ok then
        -- 请求成功，扣费后player的余额为res
    else
        -- 请求失败
        if res == 'error' then
            -- 请求失败，错误码为code
        elseif res == 'timeout' then
            -- 请求超时
        end
    end
end)
```

### score
积分服务

积分分为全局积分与本局积分，其中全局积分在提交后，会在下局游戏中获取到，可用于统计胜利次数等功能；本局积分只在本局有效，不影响其他游戏局，可用于本局游戏的战斗报告等功能。

积分使用字符串作为索引，使用数字或字符串作为值。一旦确定一个积分的类型后，你将不能再修改他的类型。

积分的操作分为累加和设置，只有数字型的积分才能进行累加操作。将积分设置为`nil`可以删除该积分，当你决定不再使用某项积分后，可以删除它。

积分初始化后会在本地创建一个积分副本，其中全局积分会被复制到这个副本上，之后获取积分时会从这个副本中获取值。当对积分进行操作时，并不会直接修改副本，而是将操作记录下来。当使用提交积分的功能提交操作记录后，才会根据操作记录更新副本。

!> 玩家离开游戏后，[设置全局积分]会失败，这是为了避免与该玩家新开游戏局的全局积分冲突。你应该在玩家离开游戏时立即将你想设置的全局积分设置完毕并提交，之后不要再设置该玩家的全局积分。

!> 在同一局中，无法删除一个已被操作过的积分，也无法操作一个已被删除的积分。

#### 初始化
使用代码`require 'ac.score'`来初始化积分。积分初始化需要一段时间，请不要立即使用它。

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

#### async_commit
异步提交积分

* 参数
    * player (player) - 玩家

提交玩家的积分，提交积分需要一段时间，在提交完成前对该玩家的积分操作均会失败。如果提交时玩家已经离开游戏，而待提交的积分中包含了[设置全局积分][set]，那么提交将会失败，请务必避免这种情况。

```lua
ac.score.async_commit(player)
```

#### commit
提交积分

* 参数
    * player (player) - 玩家
    * rpc (rpc) - rpc环境
* 返回
    * ok (boolean) - 是否成功
    * *result* (string) - 失败原因
    * *code* (integer) - 错误码

提交玩家的积分，提交积分需要一段时间，在提交完成前对该玩家的积分操作均会失败。如果提交时玩家已经离开游戏，而待提交的积分中包含了[设置全局积分][set]，那么提交将会失败，请务必避免这种情况。这是一个rpc功能，只能在rpc环境中使用，提交结束前会让rpc环境休眠。返回值有3种情况：
1. 扣费成功，返回`true`。
2. 扣费失败，返回`false, 'error', code`。
3. 扣费超时，返回`false, 'timeout'`。

```lua
ac.rpc(function (rpc)
    local ok, err, code = ac.score.commit(player, rpc)
    if ok then
        -- 提交成功
    else
        if res == 'error' then
            -- 提交失败，错误码为code
        elseif res == 'timeout' then
            -- 提交超时
        end
    end
end)
```

[计费]: /ac/api/rpc?id=计费
[积分]: /ac/api/rpc?id=积分
[创建]: /ac/api/rpc?id=创建
[set]: /ac/api/rpc?id=set
