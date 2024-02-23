type(<var>)

local ava_event = {}  -- table类型
ava_event["ON_EVENT_GROUP_REVOKE"] = true  

            
function YLog(line, level, ...)
--{
    io.stderr:write(string.format("[%s] [%s] [%s:%d]",
                        os.date("%Y-%m-%d %H:%M:%S"),
                        level,
                        arg0,
                        line
                        ))
    local args = { ... }
    for k, v in pairs(args) do
        io.stderr:write(string.format("%s", v))
    end
    io.stderr:write("\n")
end --}


-- 这是单行注释
--[[多行注释的方法
# API
## 发送群消息   
        luaRes =
            Api.Api_SendMsg(
            CurrentQQ,
            {
                toUser = data.FromGroupId,
                sendToType = 2,
                sendMsgType = "TextMsg",
                groupid = 0,
                content = keyWord,
                atUser = 0
            }
        )
        YLog(debug.getinfo(1).currentline, "INFO", "SendMsg Ret-->", luaRes.Ret)
]]


-- table类型的遍历
```
-- 只有这种能遍历全，但是不是按照key的大小顺序来的
for k, v in pairs(tbtest) do  
    print(k, v)
end 
-- 如果能保证k都是从1开始，连续递增的话，可以用下面这种来遍历
for k, v in ipairs(tbtest) do  
    print(k, v)
end 
-- 获取table的大小，仅当k都是从1开始连续递增的情况才能获取正确
#(tbtest)
```