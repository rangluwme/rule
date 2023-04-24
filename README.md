
# Code
- https://github.com/Mythologyli/ZJU-Rule/tree/master/Clash/Providers
---
- http://www.msftconnecttest.com/connecttest.txt
- http://captive.apple.com/
- http://www.apple.com/library/test/success.html
- https://www.google.com/search?q=%s&lr=lang_zh-CN
- https://www.google.com/search?q=%s&hl=zh_CN
- https://www.google.com/?hl=zh_CN
- https://www.google.com/?lr=lang_zh-CN

https://mayi-site.com/#/register?code=qUnubXcR

https://home.xsus.me/index.php/#/register?code=RLxsKCoU

https://invite.efshop.cc/#/register?code=vHGeVVP1

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
        - RULE-SET,BanAD,REJECT
        - RULE-SET,BanProgramAD,REJECT
        - RULE-SET,BanEasyListChina,REJECT
        - RULE-SET,BanEasyList,REJECT
        - RULE-SET,BanEasyPrivacy,REJECT
        - RULE-SET,reject,REJECT
        - RULE-SET,rejectlist,REJECT

        - RULE-SET,proxylist,🌍
        - RULE-SET,Microsoft,DIRECT
        - RULE-SET,directlist,DIRECT
        - RULE-SET,direct,DIRECT
        - RULE-SET,cncidr,DIRECT
        - RULE-SET,ProxyLite,🌍
        - RULE-SET,ProxyGFWlist,🌍
        - GEOIP,CN,DIRECT
        - MATCH,🌍
      prepend-proxy-groups:
        - name: 🌍
          type: select
          # type: url-test
          # url: http://www.apple.com/library/test/success.html
          # interval: 10
          proxies:
          - 🇭🇰 香港
          - 🇨🇳 台湾
          - 🇸🇬 新加坡
          - 🇺🇲 美国
        - name: 🇭🇰 香港
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 10
        - name: 🇨🇳 台湾 
          # type: select
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 10
        - name: 🇸🇬 新加坡 
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 10
        - name: 🇺🇲 美国
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 10
      commands:
        - proxy-groups.🇭🇰 香港.proxies=[]proxyNames|港|HK
        - proxy-groups.🇨🇳 台湾.proxies=[]proxyNames|台|TW
        - proxy-groups.🇸🇬 新加坡.proxies=[]proxyNames|新|狮城|SG
        - proxy-groups.🇺🇲 美国.proxies=[]proxyNames|美|US
        # 一些可能用到的正则过滤节点示例，使分组更细致
        # []proxyNames|a                         # 包含a
        # []proxyNames|^(.*)(a|b)+(.*)$          # 包含a或b
        # []proxyNames|^(?=.*a)(?=.*b).*$        # 包含a和b
        # []proxyNames|^((?!b).)*a((?!b).)*$     # 包含a且不包含b
        # []proxyNames|^((?!b|c).)*a((?!b|c).)*$ # 包含a且不包含b或c
      mix-rule-providers: 
        directlist: 
          type: http
          behavior: classical
          url: "https://raw.githubusercontent.com/rangluwme/rule/main/directlist.yaml"
          path: ./ruleset/directlist.yaml
          interval: 86400
        proxylist: 
          type: http
          behavior: classical
          url: "https://raw.githubusercontent.com/rangluwme/rule/main/proxylist.yaml"
          path: ./ruleset/proxylist.yaml
          interval: 86400
        reject: # 广告域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
          path: ./ruleset/reject.yaml
          interval: 86400
        rejectlist: # 广告域名列表
          type: http
          behavior:  classical
          url: "https://raw.githubusercontent.com/rangluwme/rule/main/rejectlist.yaml"
          path: ./ruleset/rejectlist.yaml
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
          path: ./ruleset/BanAD.yaml
        BanProgramAD:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanProgramAD.yaml"
          interval: 86400
          path: ./ruleset/BanProgramAD.yaml
        BanEasyList:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanEasyList.yaml"
          interval: 86400
          path: ./ruleset/BanEasyList.yaml
        BanEasyListChina:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanEasyListChina.yaml"
          interval: 86400
          path: ./ruleset/BanEasyListChina.yaml
        BanEasyPrivacy:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/BanEasyPrivacy.yaml"
          interval: 86400
          path: ./ruleset/BanEasyPrivacy.yaml
        Microsoft:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/Ruleset/Microsoft.yaml"
          interval: 86400
          path: ./ruleset/Microsoft.yaml
        ProxyLite:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/ProxyLite.yaml"
          interval: 86400
          path: ./ruleset/ProxyLite.yaml
        ProxyGFWlist:
          behavior: classical 
          type: http
          url: "https://cdn.jsdelivr.net/gh/Mythologyli/ZJU-Rule/Clash/Providers/ProxyGFWlist.yaml"
          interval: 86400
          path: ./ruleset/ProxyGFWlist.yaml
```
