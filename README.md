- &config=https%3A%2F%2Fraw.githubusercontent.com%2Frangluwme%2Frule%2Fmain%2Fclash.ini.ini

- https://github.com/Mythologyli/ZJU-Rule/tree/master/Clash
```
parsers: # array
  - reg: ^.*$  
  # - reg: ^.*$ 匹配所有订阅，或  - url: https://example.com/profile.yaml 指定订阅
  # 下面是删除服务商自带的策略组和规则
    code: |
      module.exports.parse = (raw, { yaml }) => {
        const rawObj = yaml.parse(raw)
        const groups = []
        const rules = []
        return yaml.stringify({ ...rawObj, 'proxy-groups': groups, rules })
      } 
  # 建立自己的配置
    yaml:
      append-proxies:
        - name: ZJU # proxies最后面增加一个服务
          type: socks5
          server: 127.0.0.1
          port: 1080
      prepend-proxy-groups: # 建立策略组
        - name: 🏫 ZJUWLAN
          type: select
          proxies:
          - DIRECT
          - ZJU
        - name: 💥 Proxy Network
          type: fallback
          url: http://www.apple.com/library/test/success.html
          interval: 300
          proxies:
          - 🇭🇰 香港
          - 🇨🇳 台湾
          - 🇸🇬 新加坡
        
        - name: 🇭🇰 香港 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 300
        
        - name: 🇨🇳 台湾 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 300
          
        - name: 🇸🇬 新加坡 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 300
          
      commands:
        - proxy-groups.🇭🇰 香港.proxies=[]proxyNames|香港 # 向指定策略组添加订阅中的节点名，可使用正则过滤
        - proxy-groups.🇨🇳 台湾.proxies=[]proxyNames|台
        - proxy-groups.🇸🇬 新加坡.proxies=[]proxyNames|新加坡

       # 为各个策略组添加一个DIRECT，避免机场节点无法匹配上面的正则筛选而导致配置出错。应该有其他办法避免，但是我不会。
        # - proxy-groups.香港.proxies.0+DIRECT
        # - proxy-groups.台湾.proxies.0+DIRECT
        # - proxy-groups.新加坡.proxies.0+DIRECT
        # - proxy-groups.节点名字.proxies.0+DIRECT # 向指定分组第一个位置添加一个 DIRECT 节点名
        # []proxyNames|a                         # 包含a
        # []proxyNames|^(.*)(a|b)+(.*)$          # 包含a或b
        # []proxyNames|^(?=.*a)(?=.*b).*$        # 包含a和b
        # []proxyNames|^((?!b).)*a((?!b).)*$     # 包含a且不包含b
        # []proxyNames|^((?!b|c).)*a((?!b|c).)*$ # 包含a且不包含b或c
        # 更多正则教程，请看这里：https://deerchao.cn/tutorials/regex/regex.htm#top
        
  # 添加规则
      prepend-rules: # 规则由上往下遍历，如上面规则已经命中，则不再往下处理
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

        - RULE-SET,BanAD,REJECT
        - RULE-SET,BanProgramAD,REJECT
        - RULE-SET,reject,REJECT

        - RULE-SET,zju,🏫 ZJUWLAN
        - RULE-SET,proxylist,💥 Proxy Network
        
        - RULE-SET,directlist,DIRECT
        - RULE-SET,Microsoft,DIRECT
        - RULE-SET,cncidr,DIRECT
        - RULE-SET,direct,DIRECT
        - GEOIP,CN,DIRECT
        - MATCH,💥 Proxy Network
  # 添加规则集
      mix-rule-providers: 
        directlist: # 需要直连的常见软件列表
          type: http
          behavior: classical
          url: "https://raw.githubusercontent.com/rangluwme/rule/main/directlist.yaml"
          path: ./ruleset/directlist.yaml
          interval: 86400
        zju: # 需要直连的常见软件列表
          type: http
          behavior: classical
          url: "https://cdn.jsdelivr.net/gh/rangluwme/rule/zju.yaml"
          path: ./ruleset/zju.yaml
          interval: 86400
        proxylist: # 需要直连的常见软件列表
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
          url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanAD.yaml"
          interval: 86400
          path: ./ACL4SSR/BanAD.yaml
        BanProgramAD:
          behavior: classical 
          type: http
          url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanProgramAD.yaml"
          interval: 86400
          path: ./ACL4SSR/BanProgramAD.yaml
        Microsoft:
          behavior: classical 
          type: http
          url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Microsoft.yaml"
          interval: 86400
          path: ./ACL4SSR/Microsoft.yaml





```
