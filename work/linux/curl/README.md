# application/x-www-form-urlencoded
curl -s -d '{"key":"xxxxxxxxxxx"}' http://xxx |jq .             # -X POST 


# multipart/form-data
curl localhost:3000/api/multipart -F raw=@raw.data -F hello=world


# application/json
curl -s -H "Content-Type:application/json" -X POST -d"xx\"x'x" 'http://xxx' |jq .



# para

-s 
-X POST -d"xxx"
-X GET
--header "Host: www.baidu.com" 配置host的域名，🐂
-H "Accept: application/json;Content-Type:application/x-www-form-urlencoded"
-k 可以支持https
'http:xxx'

# example
## 请求百度。指定使用哪个网关vip
curl --keepalive-time 600 --header 'Host: baidu.com' -k 'https://111.234.15.22/s?wd=nihao'