- &config=https%3A%2F%2Fraw.githubusercontent.com%2Frangluwme%2Frule%2Fmain%2Fclash.ini.ini

- https://github.com/Fndroid/clash_for_windows_pkg/releases/latest
- https://github.com/Mythologyli/ZJU-Rule/tree/master/Clash
```
parsers:
  - reg: ^https://.+$
    yaml:
      prepend-rules:
        - DOMAIN-KEYWORD,logitechg,REJECT
        - DOMAIN-KEYWORD,weibo,REJECT # rules最前面增加一个规则
        - DOMAIN-KEYWORD,v2ex,REJECT
        - DOMAIN-KEYWORD,cc98,REJECT
        - DOMAIN-KEYWORD,rangluw,REJECT
        - DOMAIN-KEYWORD,analytics,REJECT
        - DOMAIN-KEYWORD,adservice,REJECT
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
      append-proxies:
        - name: ZJU # proxies最后面增加一个服务
          type: socks5
          server: 127.0.0.1
          port: 1080
      prepend-proxy-groups:
        - name: 🏫 ZJUWLAN
          type: select
          proxies:
            - DIRECT
            - ZJU
      #   - name: myGroup # 建立新策略组
      #     type: fallback
      #     url: "http://www.gstatic.com/generate_204"
      #     interval: 300
      #     proxies:
      #       - DIRECT
      # commands:
      #   - proxy-groups.myGroup.proxies=[]proxyNames|香港
```
