((ip.src == 192.168.0.1) and (ip.dst == 192.168.0.2)) or ((ip.src == 192.168.0.2) and (ip.dst == 192.168.0.1))

显示过滤器


(ip.dst eq 210.47.0.14) && (http)

抓python urllib的包是不同的，需要reassembled后才能恢复

抓https需要wireshark版本2，

通常在目的机器写来源ip.src靠谱一些，写ip.dst有可能经过转换了