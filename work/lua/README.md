
type(<var>)

local ava_event = {} 
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