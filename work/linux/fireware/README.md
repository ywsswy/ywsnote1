firewall-cmd --zone=public --add-port=80/tcp --permanent  #添加端口
firewall-cmd --reload #重载后才能生效
firewall-cmd --zone=public --remove-port=80/tcp --permanent

firewall-cmd --state


firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: dhcpv6-client ssh
  ports: 65534/tcp 65535/tcp 65438/tcp 65433/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
