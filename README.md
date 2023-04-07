# rule
自用

https://github.com/Mythologyli/ZJU-Rule/tree/master/Clash/Providers

https://ghproxy.com/https://github.com/rangluwme/rule/releases/download/1.0/Clash.7z

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
        - DOMAIN-KEYWORD,logitechg,REJECT
        - DOMAIN-KEYWORD,analytics,REJECT 
        - DOMAIN-KEYWORD,adservice,REJECT
        - DOMAIN-SUFFIX,log-global.aliyuncs.com,REJECT
        - RULE-SET,BanAD,REJECT
        - RULE-SET,BanProgramAD,REJECT
        - RULE-SET,BanEasyListChina,REJECT
        - RULE-SET,BanEasyList,REJECT
        - RULE-SET,BanEasyPrivacy,REJECT
        - RULE-SET,reject,REJECT

        - RULE-SET,proxylist,💥 Proxy Mode
        - RULE-SET,Microsoft,DIRECT
        - RULE-SET,direct,DIRECT
        - RULE-SET,cncidr,DIRECT
        - RULE-SET,directlist,DIRECT

        - RULE-SET,ProxyLite,💥 Proxy Mode
        - RULE-SET,ProxyGFWlist,💥 Proxy Mode
        - GEOIP,CN,DIRECT
        - MATCH,💥 Proxy Mode
      prepend-proxy-groups:
        - name: 💥 Proxy Mode
          type: select
          # type: url-test
          # url: http://www.apple.com/library/test/success.html
          # interval: 10
          proxies:
          - 🇭🇰 香港
          - 🇨🇳 台湾
          - 🇸🇬 新加坡
          - 🌍 Proxy Network 
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
        - name: 🌍 Proxy Network
          type: url-test
          url: http://www.apple.com/library/test/success.html
          interval: 10
      commands:
        - proxy-groups.🇭🇰 香港.proxies=[]proxyNames|港|HK
        - proxy-groups.🇨🇳 台湾.proxies=[]proxyNames|台|TW
        - proxy-groups.🇸🇬 新加坡.proxies=[]proxyNames|新|狮城|SG
        - proxy-groups.🌍 Proxy Network.proxies=[]proxyNames|日|韩|美|US|KR|JP
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
