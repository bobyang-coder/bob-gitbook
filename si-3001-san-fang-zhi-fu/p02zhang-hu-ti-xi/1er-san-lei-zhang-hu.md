二类账户目前分为两块，一块是银行的二三类账户，一块是非金融机构的二三类账户，通常我们叫电子账户。

## 1.II、III类账户的生命周期

![](/assets/二三类账户生命周期.png)

## 2.账户开立与绑卡

同一个人在同一银行只能开立一个Ⅰ类户

电子渠道开立的Ⅱ类户和Ⅲ类户需绑定银行账户进行验证，不得绑定支付机构账户验证

Ⅱ类户需向绑定账户验证5要素（4要素+账户等级）， Ⅲ类户需验证4要素。

银联支持支持Ⅱ、Ⅲ类账户跨行验证绑定账户信息

## 3.资金互转

对非面对面开立的Ⅱ、Ⅲ类户，限制非绑定账户转入

允许Ⅱ、Ⅲ类户向绑定账户直接借记资金

银联支持Ⅱ、Ⅲ类账户与绑定账户间通过借记和贷记业务实现灵活便捷的资金划付

## 4.账户使用

除面对面核身的Ⅱ类户以外不得发行实体银行卡，鼓励Ⅱ、Ⅲ类户开展云闪付二维码业务

银联支持Ⅱ、Ⅲ类账户便捷生成基于安全芯片、主机卡模拟或二维码等技术的支付工具，实现线上线下广泛受理

---

**下面介绍下银联CUPS 对Ⅱ、Ⅲ类账户业务的支持情况：**

从业务流程、联机交易处理看

![](/assets/account_biz.png)

每个环节都有匹配的交易类型服务

### 4.1**典型的市场业务应用**

**下面介绍下两个典型的市场业务应用：**

1. JD 白条云闪付 
2. 美团线下闪付

两个的模式都是互联网消费金融正式线上走向线下场景，准定向信贷起源的消费金融模式正式迈向泛场景消费的类信用卡模式

##### JD 白条云闪付

###### 从业务流程来看

“白条闪付”是由京东白条授信银行借记Ⅱ类账户为载体，开展线下手机支付应用的衍生功能。

流程主要有以下三步：

* 1） 申请“京东白条”，互联网准定向消费信贷额度申请

* 2） 申请“白条闪付”，互联网授信模式下的银行借记Ⅱ类账户申请

* 3） 申请“ApplePay等手机支付，白条额度的银行借记Ⅱ类账户申请Apple Pay等手机支付，突破线下场景

大家可以通过这个链接\([七步教你如何用京东白条闪付刷POS机](https://mp.weixin.qq.com/s?__biz=MzA3MjExMTA4Ng==&mid=2653358474&idx=1&sn=003d0db70be4d75b0d51fa3b63512d63&chksm=84f0ef0bb387661d0d866664eedc1ed6ca18d2c2e4ecbfc8aaab795f5f95476d829a84e9dae1&mpshare=1&scene=1&srcid=0609m9WvqxnJvrlrd8NNsLZm&key=ae110083100918b9bdb8299b16aec813cc15e4442a65f9122753db749ae4a091c7fb0304663fb2d7a2a55fabbb6bb0c68e82b035f4420fa7541c10b5d5c456ea8b2594055048538798e24a43cef9551a&ascene=0&uin=NDU2OTQwNTE1&devicetype=iMac+MacBookPro11%2C4+OSX+OSX+10.12.5+build%2816F73%29&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=mojlvd0PTAD0dAFtbC9ZWBd5aely%2Bh1KGQoNmwSEsVcUTbgl26xEzqvWci%2B8A4Gm)\)看下jd白条云闪付如何将线上搬到线下，可以受理所有线下商户。

###### 开户环节

下面介绍下jd白条云闪付的开卡

![](/assets/jd_bt_os_pay_openacc.png)

目前因为借记账户的等级不能很好区分，目前我了解的情况应该是仅支持绑定信用卡来开户,申卡完成后，用户拿到卡号，通过手机pay 通过银联TSM平台空中发卡完成绑定,

绑卡后，可以完成线下线上消费

###### 消费流程如下

![](/assets/jd_bt_consume.jpeg)

##### 美团线下闪付

美团线下闪付 与JD 有些差别,美团账户背后对应的是一张贷记卡 JD 对应的是消费信贷账户。

[美团“借卡下线”玩闪付，II类户成地方银行获客新渠道？](http://m.mpaypass.com.cn/news/201706/06085341.html?from=timeline)

---

### 补充了解

###### 1.[链接1:【高速路】中国银联打造“账户高速公路”的启示](https://mp.weixin.qq.com/s?__biz=MzA5MTAwODQwNw==&mid=2652850480&idx=2&sn=9dee54f23f9e9c2e672372dd827140e1&chksm=8be915c8bc9e9cde278af89f3335bf8f37c2bdc21f7aa74d22d0116460b1f50ab9c988496b48&mpshare=1&scene=2&srcid=0622qtJhjEqkYlbdxGz3BM6q&from=timeline&key=ae110083100918b9b74b4fbcdd4376414b69876c494f01ca06fed8e2db5a7450fafc6d94153e66dac7061932154a89266ad7dfb10c0bdc3bd705732fc271f9eeae4f102054d9974c347b254a1cfc4492&ascene=0&uin=NDU2OTQwNTE1&devicetype=iMac+MacBookPro11%2C4+OSX+OSX+10.12.5+build%2816F73%29&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=mojlvd0PTAD0dAFtbC9ZWBd5aely%2Bh1KGQoNmwSEsVcUTbgl26xEzqvWci%2B8A4Gm)

###### 2.从银联到发卡行传递的卡信息是PAN呢还是token？

![](/assets/WechatIMG1728.png)

![](/assets/WechatIMG1735.jpeg)

3.二三类账户区别

