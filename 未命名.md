pickable.lua控制采集
```lua
function Pickable:SetUp(product, regen, number)
    self.canbepicked = true
    self.product = product
    self.baseregentime = regen
    self.regentime = regen
    self.numtoharvest = number or 1
    //采集数量修改
end
```

模组包含的文件

modicon.xml


## modinfo.lua

mod信息文件

必须有name关键字，表示MOD名称
description= "模组介绍"
version = "模组版本"
author = "作者介绍"

forumthread = ""
官方论坛的模组编号

api_version 
api版本号，
## modmain.lua
主文件入口
