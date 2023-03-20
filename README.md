
- http://www.msftconnecttest.com/connecttest.txt
- http://captive.apple.com/
- http://www.apple.com/library/test/success.html

- https://github.com/Mythologyli/ZJU-Rule/tree/master/Clash/Providers

---
- https://invite.efshop.cc/#/register?code=vHGeVVP1
- https://www.google.com/search?q=%s&lr=lang_zh-CN
- https://www.google.com/search?q=%s&hl=zh_CN

- https://www.google.com/?hl=zh_CN
- https://www.google.com/?lr=lang_zh-CN
---

> (A).*(B)        节点名既有 A又有 B  
>
> (A)|(B)         节点名有 A 或者 B  
>
> ^((?!A).)*$     节点名不含有 A  
>
> (?!.*(A)).*(B)  节点名不含有 A，同时含有 B

```
parsers: # array
  - reg: ^.*$  
    code: |
      module.exports.parse = (raw, { yaml }) => {
        const rawObj = yaml.parse(raw)
        const groups = []
        const rules = []
        return yaml.stringify({ ...rawObj, 'proxy-groups': groups, rules })
      } 
    yaml:
      prepend-rules:
        # - DOMAIN-KEYWORD,logitechg,REJECT
        - DOMAIN-KEYWORD,analytics,REJECT 
        - DOMAIN-KEYWORD,adservice,REJECT

        - DOMAIN-KEYWORD,weibo,REJECT
        - DOMAIN-KEYWORD,sinaimg,REJECT
        - DOMAIN-KEYWORD,v2ex,REJECT
        - DOMAIN-KEYWORD,cc98,REJECT
        - DOMAIN-KEYWORD,rangluw,REJECT
        - DOMAIN-KEYWORD,youtube,REJECT
        - DOMAIN-KEYWORD,googlevideo,REJECT
        - DOMAIN-SUFFIX,ytimg.com,REJECT
        - DOMAIN-SUFFIX,t.me,REJECT
        - DOMAIN-SUFFIX,tdesktop.com,REJECT
        - DOMAIN-SUFFIX,telegra.ph,REJECT
        - DOMAIN-SUFFIX,telegram.me,REJECT
        - DOMAIN-SUFFIX,telegram.org,REJECT
        - DOMAIN-SUFFIX,telesco.pe,REJECT
        - IP-CIDR,91.108.0.0/16,REJECT
        - IP-CIDR,109.239.140.0/24,REJECT
        - IP-CIDR,149.154.160.0/20,REJECT
        - IP-CIDR6,2001:67c:4e8::/48,REJECT
        - IP-CIDR6,2001:b28:f23d::/48,REJECT
        - IP-CIDR6,2001:b28:f23f::/48,REJECT

        - RULE-SET,BanAD,REJECT
        - RULE-SET,BanProgramAD,REJECT
        - RULE-SET,reject,REJECT

        - RULE-SET,zju,DIRECT
        - RULE-SET,proxylist,💥 Proxy Network
        
        - RULE-SET,Microsoft,DIRECT
        - RULE-SET,direct,DIRECT
        - RULE-SET,cncidr,DIRECT
        - RULE-SET,directlist,DIRECT

        - RULE-SET,ProxyLite,💥 Proxy Network
        - RULE-SET,ProxyGFWlist,💥 Proxy Network
        - GEOIP,CN,DIRECT
        - MATCH,💥 Proxy Network
      prepend-proxy-groups:
        - name: 💥 Proxy Network
          type: select
          proxies:
          - 🇭🇰 香港
          - 🇨🇳 台湾
          - 🇸🇬 新加坡       
        - name: 🇭🇰 香港 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 100
        - name: 🇨🇳 台湾 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 100
        - name: 🇸🇬 新加坡 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 100
      commands:
        - proxy-groups.🇭🇰 香港.proxies=[]proxyNames|香港 
        - proxy-groups.🇨🇳 台湾.proxies=[]proxyNames|台
        - proxy-groups.🇸🇬 新加坡.proxies=[]proxyNames|新加坡
      mix-rule-providers: 
        directlist: 
          type: http
          behavior: classical
          url: "https://cdn.jsdelivr.net/gh/rangluwme/rule/directlist.yaml"
          path: ./ruleset/directlist.yaml
          interval: 86400
        zju: 
          type: http
          behavior: classical
          url: "https://cdn.jsdelivr.net/gh/rangluwme/rule/zju.yaml"
          path: ./ruleset/zju.yaml
          interval: 86400
        proxylist: 
          type: http
          behavior: classical
          url: "https://cdn.jsdelivr.net/gh/rangluwme/rule/proxylist.yaml"
          path: ./ruleset/proxylist.yaml
          interval: 86400
        reject: # 广告域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
          path: ./ruleset/reject.yaml
          interval: 86400
        direct: # 直连域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt"
          path: ./ruleset/direct.yaml
          interval: 86400
        cncidr: # 中国大陆 IP 地址列表
          type: http
          behavior: ipcidr
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt"
          path: ./ruleset/cncidr.yaml
          interval: 86400
        BanAD:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanAD.yaml"
          interval: 86400
          path: ./ACL4SSR/BanAD.yaml
        BanProgramAD:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanProgramAD.yaml"
          interval: 86400
          path: ./ACL4SSR/BanProgramAD.yaml
        Microsoft:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/Ruleset/Microsoft.yaml"
          interval: 86400
          path: ./ACL4SSR/Microsoft.yaml
        ProxyLite:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/ProxyLite.yaml"
          interval: 86400
          path: ./ACL4SSR/ProxyLite.yaml
        ProxyGFWlist:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/ProxyGFWlist.yaml"
          interval: 86400
          path: ./ACL4SSR/ProxyGFWlist.yaml
```
---

```
mixin:
  tun:
    enable: true
    stack: gvisor
    auto-route: true
    auto-detect-interface: true
    dns-hijack:
      - any:53
  dns:
    enable: true
    listen: 0.0.0.0:53
    ipv6: true
    enhanced-mode: fake-ip
    fake-ip-range: 198.18.0.1/16
    use-hosts: true # 查询 hosts 并返回 IP 记录
    nameserver-policy:
      '+.zju.edu.cn': '10.105.1.124'
      '+.cc98.org': '10.105.1.124'
      '+.cc98.top': '10.105.1.124'
    # proxy-server-nameserver:
    default-nameserver:
      - 223.5.5.5
      - 1.1.1.1
    nameserver:
      - https://doh.pub/dns-query
      - https://dns.alidns.com/dns-query
    fallback:
      - https://east.lele233.pro/dns-query
      - https://east.lele233.pro/cdn
      - https://south.lele233.pro/dns-query
      - https://south.lele233.pro/cdn
      - https://hk.lele233.cn:2333/dns-query
      - https://cn-east.iqiqzz.com/dns-query
      - https://cn-east.iqiqzz.com/cdn
      - https://cn-south.iqiqzz.com/dns-query
      - https://cn-south.iqiqzz.com/cdn
    fallback-filter:
      geoip: true
      geoip-code: CN
      ipcidr:
         - 240.0.0.0/4
         - 0.0.0.0/32
         - 127.0.0.1/32
    fake-ip-filter:
      - www.msftconnecttest.com 
```
