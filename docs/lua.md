# lua

幻想编辑器的Lua版本是5.3.4。为了安全性和其他一些因素考虑，我们对部分Lua标准api进行了定制。

> 文件系统

我们为你提供了受限的读取文件的能力，你可以使用`io.open`或`io.lines`读取地图的文件，例如

``` lua
  local f = assert(io.open 'script/main.lua')
  print(f:read 'a')
  f:close()

  for line in io.lines 'script/main.lua' do
    print(line)
  end
```

但你无法通过`io.open`来修改一个文件。

!> 文件路径编码都是utf8

> 加载器

你无法加载一个c模块，所以`package.loadlib`和`package.cpath`都是无效的。加载lua模块使用的是地图内的路径(和`io.open`一样)，默认的`package.path`是

``` lua
"script/?.lua;script/?/init.lua"
```

> 不支持直接访问操作系统资源的API

包括

* os.execute
* os.exit
* os.getenv
* os.remove
* os.rename
* os.setlocale
* os.tmpname
* io.input
* io.output
* io.tmpfile
* io.popen

> 随机数

我们定制了随机数发生器，以保证随机数不会受到外部因素的影响，并且我们关闭了`math.randomseed`。

> 三角函数使用角度制

包括

* math.sin
* math.cos
* math.tan
* math.asin
* math.acos
* math.atan

> 不支持从文件加载代码

包括

* dofile
* loadfile

> 部分支持`coroutine`

不要直接使用，而是用`ac.rpc`。

> 在正式环境不支持`debug`库
