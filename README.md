https://sub.zjurule.xyz/sub?target=clash&url=&config=https://raw.githubusercontent.com/rangluwme/rule/main/clash.ini

## Clash中的DNS

DNS在Clash有如下应用：

-   解析域名，判断是否命中IP类规则
-   进行直连（DIRECT）时，解析域名与远端连接
-   作为一个DNS调度服务器

## Clash for Windows

默认情况下，使用Clash for Windows（下简称CFW）并不需要配置DNS，因为默认情况下，Clash核心会使用系统DNS作为规则判别解析。

几乎所有的情况下，在Windows上使用CFW并不会因DNS污染而无法联网。因为污染要达到目的，需要符合如下情况：

1.  域名类规则没有命中
2.  命中IP类或MATCH规则且直连

### 例子

假设我们的规则文件如下：

```yaml
Rule:
  - "GEOIP,US,Proxy" # IP为美国时走代理
  - "MATCH,DIRECT" # 其余请求直连
```

此时我们网络环境中存在污染，具体的情况是，DNS会将`www.youtube.com`污染成保留地址`243.185.187.39`，如下：

```shell
$ dig www.youtube.com @114.114.114.114

; <<>> DiG 9.11.3-1ubuntu1.1-Ubuntu <<>> www.youtube.com @114.114.114.114
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16683
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.youtube.com.               IN      A

;; ANSWER SECTION:
www.youtube.com.        600     IN      A       243.185.187.39

;; Query time: 799 msec
;; SERVER: 114.114.114.114#53(114.114.114.114)
;; WHEN: Tue Mar 26 14:45:47 DST 2019
;; MSG SIZE  rcvd: 49
```

此时，由于保留地址不属于GEOIP,US范围，则请求被MATCH规则命中，最后直连。

### 说明

以上的例子中，条件非常苛刻。首先，我们的规则文件一般情况下，不会像上面这样设置，即使使用最简单的规则：

```yaml
Rule:
  - "GEOIP,CN,DIRECT"
  - "MATCH,Proxy"
```

都已经可以避免大部分的污染情况，除非污染获得的IP地址是属于CN，即国内的。然而多数情况下，为了保障国内地址的可访问性，这种情况并不多见。

假设地址被污染成国内的，也可以添加对应的域名规则在前面，抢先GEOIP命中然后分配给代理连接：

```yaml
Rule:
  - "DOMAIN-SUFFIX,youtube.com,Proxy"
  - "GEOIP,CN,DIRECT"
  - "MATCH,Proxy"
```

然而，大多数的规则维护者都注意到这个问题，所以规则模板都不会像上面这样简单，多数被污染的域名都可以正常的通过规则进行代理。

### 总结

使用CFW时，并不需要担心DNS污染问题，甚至无需配置Clash的内置DNS即可正常上网。在存在污染的网络环境下，只需要给被污染的域名添加DOMAIN类规则即可。

## Clash进行透明代理

在路由器或虚拟机中使用Clash做透明代理时，**需要使用Clash内置的DNS服务器进行解析，DNS服务器会生成IP和域名的关系，从而在Clash端反推对应的域名**（因为redir只能传递IP）。

此时必须要配置DNS服务器，并且其他设备要使用此DNS服务器，配置如下：

```yaml
dns:
  enable: true
  listen: :53 # 必须要开启Server监听
  enhanced-mode: redir-host
  nameserver:
    - 114.114.114.114
    - 223.5.5.5
  fallback:
    - 8.8.8.8
```

### 工作原理

当配置了DNS服务器并且其他设备使用此DNS服务器进行解析时，Clash工作流程如下：

1.  从`nameserver`和`fallback`里的DNS进行并发请求，并且选取`nameserver`中最先响应的结果作为基准
2.  使用GEOIP判断此IP的所属区域，如果属于国内（CN）或**保留地址**则直接响应给客户端
3.  其他情况则把`fallback`中的结果响应给客户端

### DNS污染会怎样

在客户端设备到达路由器的DNS请求是不会被篡改的，而Clash请求的DNS则有可能被篡改污染，途径如下：

-   DNS服务器主动污染
-   运营商劫持

如果使用的DNS服务器主动污染IP地址，那么更换一个即可。但是如果是被运营商劫持，那么就需要其他的手段来防止了。

假设，我们获得的IP是被污染了的，那么这个时候Clash其实不一定不能正常工作，在没有命中域名规则的前提下，按照IP指向的策略进行下一步分析：

-   被代理
-   被直连

**当污染IP被判定为需要代理时，此时Clash依然能正常工作，这是因为Clash实际上并不会向代理服务器发送这个IP地址，而是直接将host发送给代理服务器，在代理服务器中进行DNS解析，这样可以让请求获取到更合适的IP地址，更好的利用CDN资源。**

当污染IP被判定为直连时，此时请求会失败，但是注意前提是没有命中域名类规则，添加域名类规则即可解决。

**特殊情况下，当污染IP为保留地址（如：243.185.187.39）并进行响应给客户端时，浏览器（Chrome和Firefox）会断开连接并提示“无法找到IP地址”。要解决这个问题，只能从抗污染入手。**

注意：目前Clash的工作模式下，客户端得到的IP地址和真正请求的IP地址有可能不一样。

### 解决办法

解决污染的思路其实很多，而这里介绍一个最简单的，不需要另行搭建其他DNS服务。

假如你的网络环境会劫持DNS并响应保留地址导致无法联网，那么解决的方案就是不使用常规UDP进行DNS请求，转而使用DNS over TLS或改为TCP进行请求，目前已有部分DNS提供此类支持。

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: redir-host
  nameserver:
    - 'tls://dns.rubyfish.cn:853'
  fallback:
    - 'tls://1.1.1.1:853'
    - 'tcp://1.1.1.1:53'
    - 'tcp://208.67.222.222:443'
    - 'tls://dns.google'
```

## 特殊情况

以下说明一下DNS工作的特殊情况，用以调整Clash的DNS设置。

### 网易云音乐

由于版权问题，网易云音乐的DNS分流行为非常诡异，初步猜测：

-   使用[EDNS Client Subnet](https://en.wikipedia.org/wiki/EDNS_Client_Subnet)区分地区，对于非大陆返回103.65.41.125(6)两个错误IP
-   国内污染成正常连接的IP（运营商）
-   国外DNS解析为错误IP（tls://1.1.1.1:853解析可知）

而上面的解决办法，正是针对网易云音乐而给出的，因为`1.1.1.1`基于隐私考虑并不会按地区分发不同IP，所以当网易云音乐请求被解析时，会得到两个错误的IP，如果此时进行直连，则连接失败，APP中提示网络错误。而rubyfish面向国内用户，会直接返回正确IP。

使用`fallback`区分的原因就是避免fallback中的DNS先于nameserver中DNS响应！

如果你想要混合使用国内外的DNS，可以尝试下面的方法。

#### 代理“直连”

假设你使用的服务提供商提供了质量较好的**国内节点**时，可以考虑将`DIRECT`改为国内节点，这样可以使得国内请求也被远程代理。

即使我们获得了错误IP，请求也会在远程服务器中重新获取到正确的本地IP。同时，我们可以不使用fallback进行排错而直接利用所有DNS中的最优结果（虽然这个结果并不重要）而节省时间。

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: redir-host
  nameserver:
    - 'tls://dns.rubyfish.cn:853'
    - 'tls://1.1.1.1:853'
    - 'tcp://1.1.1.1'
    - 'tcp://208.67.222.222:443'
    - 'tls://dns.google'
```

### 使用fake-ip代替redir-host

Clash已经在dev分支中提交fake-ip的支持。

在redir-host模式下，Clash先将域名进行解析，再将具体的IP地址响应给客户端，并且记录其对应关系。

而在fake-ip模式下则不进行DNS解析，而是直接生成一个“假IP”并响应给客户端，再记录对应关系。

这可以有如下两个好处：

-   如果最后判定为代理，则有可能节省一次本地的DNS请求
-   不需要担心DNS污染（因为配置文件中的DNS仅用作规则判定）

#### 如何调整配置

只需要将配置中的`enhanced-mode`改为`fake-ip`即可，如下：

```yaml
dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: fake-ip
  nameserver:
    - 114.114.114.114 # 随便填写你喜欢的DNS
```
