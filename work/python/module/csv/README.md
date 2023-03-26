```
这种格式的数据就是csv的序列化数据，每个双引号会变成两个双引号
"{""proto_type"":""trpc"",""cmd"":""/tencent.news.exposure.ExposureFilter/Check"",""request_header"":""{\""trans_info\"":{\""QQNewsChannelName\"":\""bmV3c19uZXdzX3dhcA==\""},\""request_id\"":4109619,\""timeout\"":50,\""caller\"":\""cmVyYW5r\"",\""func\"":\""L3RlbmNlbnQubmV3cy5leHBvc3VyZS5FeHBvc3VyZUZpbHRlci9DaGVjaw==\"",\""callee\"":\""dHJwYy5xcW5ld3NfZXhwb3N1cmUuZXhwb3N1cmVfc2VydmVyLkV4cG9zdXJldHJwYw==\""}"",""request_body"":""{\""userInfo\"":{\""qimei36\"":\""1_3329338094\"",\""deviceId\"":\""1_3329338094\""},\""header\"":{\""requestId\"":\""1687920869217405468\""},\""retFiltered\"":true,\""cxtInfo\"":{\""chlFrom\"":\""news_news_wap\""}}"",""request_bytes"":"""",""request_time"":1687920869817732864,""meta_data"":""""}"
```
import csv
csv.field_size_limit(10 * 1024 * 1024)

with open('file.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        json_str = row[0]
        print(json_str)