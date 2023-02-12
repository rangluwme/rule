## 阿里云DNS：

- IPV4：223.5.5.5 223.6.6.6
- IPV6：2400:3200::1 2400:3200:baba::1
- DoH：
https://dns.alidns.com/dns-query
- DoT: 
dns.alidns.com

## 腾讯DNS：

- IPV4：119.29.29.29
- IPV6：2402:4e00::
- IP：
1.12.12.12/120.53.53.53
- DoH：
https://doh.pub/dns-query
- DoH（国密）：
https://sm2.doh.pub/dns-query
- DoT：
dot.pub


## 检测网络联通性&generate_204

- http://www.msftconnecttest.com/connecttest.txt
- http://captive.apple.com/
- http://www.apple.com/library/test/success.html

---

- https://www.google.com/search?q=%s&lr=lang_zh-CN
- https://www.google.com/search?q=%s&hl=zh_CN

- https://www.google.com/?hl=zh_CN
- https://www.google.com/?lr=lang_zh-CN

> (A).*(B)        节点名既有 A又有 B  
>
> (A)|(B)         节点名有 A 或者 B  
>
> ^((?!A).)*$     节点名不含有 A  
>
> (?!.*(A)).*(B)  节点名不含有 A，同时含有 B
