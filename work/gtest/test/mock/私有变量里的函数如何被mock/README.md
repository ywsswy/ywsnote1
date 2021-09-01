class HttpClient {
 public:
  std::string Get(const std::string& url);
  bool Post(const std::string& url, const std::string& body_data, std::string* result);
  xxxxxx
};


class WeatherForecast {
 private:
  HttpClient http_client_;
};



HttpClient的Get和Post需要被mock掉

如果是测HttpClient类，那可以class MockHttpClient : public HttpClient，然后创建对象MockHttpClient mock；玩mock即可

但是现在要是测WeatherForecast类，类内已经有了HttpClient的对象http_client_，创建一个HttpClient的子类对象也没法赋值给http_client_，除非把http_client_改成指针（但是这样是为了单测修改原代码，不符合单测原则）（当然这里可以引出写代码的好习惯，函数考虑写成virtual，成员考虑是指针）
所以一种写法就是在执行单测时把HttpClient宏替换掉，

#define HttpClient MOCK_H_ttpClient
class MOCK_H_ttpClient {
 public:
  MOCK_H_ttpClient() {}
  virtual ~MOCK_H_ttpClient() {}

  MOCK_METHOD(std::string, Get, (const std::string&), ());
  MOCK_METHOD(bool, Post, (const std::string&, const std::string&, std::string*), ());
};

正常代码里嵌入如下内容，来保证单测时都是用mock的类展开，不会链接到真实的类上
#ifdef DO_UNIT_TEST
#include "test/http_client_mock.h"
#endif  // DO_UNIT_TEST


cc_library(
    name = 'weather_forecast',
    srcs = [
        'weather_forecast.cc',
    ],
    deps = [
        '//src/lib_http_client:http_client'
    ],
    extra_cppflags = [
    ],
    extra_linkflags = [
        '-Wl,-E',
    ],
)

cc_library(
    name = 'weather_forecast_test',
    srcs = [
        'weather_forecast.cc',
    ],
    deps = [
    ],
    extra_cppflags = [
        '-DDO_UNIT_TEST',
    ],
    extra_linkflags = [
        '-Wl,-E',
    ],
)
