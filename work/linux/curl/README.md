# application/x-www-form-urlencoded
curl -s -d '{"key":"xxxxxxxxxxx"}' http://xxx |jq .             # -X POST 


# multipart/form-data
curl localhost:3000/api/multipart -F raw=@raw.data -F hello=world


# application/json
curl -s -H "Content-Type:application/json" -X POST -d"xx\"x'x" 'http://xxx' |jq .



# para

-u <user>:<passwd> 如果服务端启用了basic auth，这个可以自动输入账号密码
-s quiet
-X POST -d"xxx"  #  也可以把-d的内容写到文件里 -d @<file_name>
-X GET
--header "Host: www.baidu.com" 配置host的域名，🐂
-H "Accept: application/json" 
-H "Content-Type:application/x-www-form-urlencoded" 多个header必须都单独写
-k 可以支持https(跳过安全验证来访问)
'http:xxx'

# example
## 请求百度。指定使用哪个网关vip
curl --keepalive-time 600 --header 'Host: baidu.com' -k 'https://111.234.15.22/s?wd=nihao'