tcpdump -i <网卡名称ifconfig可看> tcp and port <端口号> -n -nn -v -vv -w data:输出文件前缀 -W 100:100个文件 -C 30:文件大小30M -s 100：只抓包前面的100字节 -Z <用户名称如root>

示例:
tcpdump -i eth0 tcp and port 6379 -n -nn -v -vv  # 不写入文件只是实时显示
tcpdump -i eth0 tcp and [dst/src] host 10.22.33.44 -n -nn -v -vv  # 不写入文件只是实时显示
tcpdump -i eth1 'tcp and ((dst host 10.22.33.44 and port 8066) or dst host 10.22.33.55)' -n -nn -v -vv  # 因为括号是特殊字符所以要加单引号
tcpdump -i eth0 tcp and port 6379 -n -nn -v -vv -w data -W 50 -C 100 -Z root  # 写入文件

-n -nn 是不解析hostname，直接显示ip
-w是自动写文件
生成的文件可以用wireshark打开文件，也可以用
tcpdump -n -nn -r <file> -x 来读取（-x可以显示网际层的每个字节的数据），展示格式是： 
```
20:29:45.284827 IP 9-250-205-209.12691 > 183.158.84.54.9273: Flags [P.], seq 1:396, ack 2542, win 35, length 395
        0x0000:  4500 0252 e92e 4000 4006 1865 0a00 080d
        0x0010:  0e11 16f5 fbe2 33a0 b693 1dc3 6ebd 48f4
20:29:45.290063 IP 9-250-205-209.60510 > 15.36.52.172.9273: Flags [P.], seq 504:560, ack 33731, win 173, options [nop,nop,TS val 4168217983 ecr 2756072661], length 56
        0x0000:  4500 0028 6ba8 4000 7106 6715 0e11 16f5
        0x0010:  0a00 080d 33a0 fbe2 6ebd 48f4 b693 1fee
        ……
```
<时间：20:29:45.282975> <协议：IP> <源地址ip.port> > <目的地址ip.port>: <其他>

- 有时候需要关注的有win（源告诉目的可以接收多大的回包），如果win太小，那么回完全部的包可能就非常慢。
- 加参数-s和不加的两种情况下包的数量还不一样？可能经过了网络代理，最好裸连容易理解，不要nginx，客户端不要代理，然后不加-s参数即可
- 如果纯肉眼看字节的话（hexdump）抓的包前24个字节是文件头，然后是[16字节信息（4字节秒时间戳+4字节微妙+4字节原始长度+4字节捕获长度）+实际包字节]，然后循环……，所以加工代码：

```
#include <iostream>
#include <fstream>

int main() {
  std::ifstream if1("data00", std::ios::binary);
  if (!if1.is_open()) {
    std::cout << "open fail" << std::endl;
    return 0;
  }
  std::string s(std::istreambuf_iterator<char>{if1}, std::istreambuf_iterator<char>{});
  int idx = 24;
  for (int i = 0; idx < s.size(); ++i) {
    idx += 8;
    int a = *(reinterpret_cast<int*>(&s[idx]));
    idx += 4;
    int b = *(reinterpret_cast<int*>(&s[idx]));
    if (a != b) {
      std::cout << "DIFF" << i << " " << a << " " << b << std::endl;
    }
    idx += 4;
    // 加工包
    // 跳过数据链路层
    int tmp_idx = idx + 14;
    // 获取网络层大小
    int ip_head_len = (s[tmp_idx] & 0xf) * 4;
    tmp_idx += (ip_head_len + 2);
    int redis_port = 6379;
    s[tmp_idx] = (redis_port & 0xff00) >> 8;
    s[tmp_idx+1] = redis_port & 0xff;
    idx += a;
  }
  std::ofstream of1("of1", std::ios::binary);
  if(!of1.is_open()){
    std::cout << "open fail" << std::endl;
    return 0;
  }
  of1.write(s.c_str(), s.size());
}
```

Q: 
- 加了代理为什么包更多了
没代理的时候10个包，3个握手，2个http-get+ack，2个http-rsp（第二个rsp含有了FIN，即四次挥手的第一次），3个挥手
rsp能不能做成一个？


有代理的时候
有时候13个包、有时候12个；
分析13个的时候
服务端是先主动关闭的一方，12个的时候不是？少了一个ack



返回http响应数据时，为什么拆成两个包返回？
