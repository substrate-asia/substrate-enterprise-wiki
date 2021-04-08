# 会议记录

## 2021-04-08

**Actions & Updates:**

* 现有CA是否满足测试要求 - Lester
* 测试国密
* 添加rhd共识 - Kaichao
* ink合约的押金模型，多种token支付手续费 - Lipeng
* runtime 账户的权限管理
* administration 浏览器、综合管理器

## 2021-03-25

**Updates & Actions:**

* CA的使用文档 done - Junius
* 确认信通院的测试是否需要 CA - Li Qiang
* 加密链上数据功能 - Jimmy
* 授权读取链上内容（如使用Kong gateway）, 信通院有没有明确规定 - Jimmy
* 国密第一版这个星期应该可以写好 - Maggie
* Skip empty blocks [disscussion here](https://github.com/paritytech/substrate/issues/7952) - Kaichao

## 2021-03-11

Meeting cancelled.

**Updates:**

*Lipeng:*

* 合约这边的权限管理我们已经在jupiter上进行设计了，关于update, kill, governance等，以及gas收费和rent租赁
* 这块公链和联盟链是通的
* Parity做的仅仅是方案之一，社区是可以自定义的，我们也在学习过程中

*Li Qiang:*
国密

https://my.oschina.net/u/4582573/blog/4497143

密码作为国家三大安全支撑技术，是我国重要战略资源，也是保障网络与信息安全的核心技术与基础支撑。2020 年初，由第十三届全国人民代表大会常务委员会第十四次会议表决通过的《密码法》正式实施，这对于我国国家政治安全、经济安全、国防安全和信息安全有着重要的意义。
自 10 月 24 日起，区块链作为核心技术自主创新重要突破口，加快推动区块链技术和产业创新发展。区块链作为核心技术，密码算法则是核心中的核心，区块链国密算法改造对于国家战略的重要性不言而喻。2020 年 2 月，中国人民银行正式下发《金融分布式账本技术安全规范》，其中对密码算法的各方面制定了相关行业标准，强调分布式账本系统应使用符合国家标准的密码模块进行密码算法运算和密钥存储。更进一步加强的是，《安全规范》指出在节点通信传输安全性上，应使用符合国家密码标准的技术来建立安全通信通道，避免因传输协议受到攻击而出现的保密性破坏。目前，行业中符合国密标准的区块链企业，其产品仅仅实现了账本数据层面国密改造，在网络通信层面上，并没有实现在完整 TLS 协议中使用自主研发的国密证书。而在金融行业中，经常涉及各类敏感数据，在处理、共享和使用过程中面临违规越权使用或被用于非法用途等数据泄漏的安全风险。自中国人民银行发布《金融分布式账本技术安全规范》后，为了避免出现安全问题，金融机构对区块链技术应用的安全要求进一步提高。企业在参与金融项目招标时，产品是否满足节点通信安全标准，成为重要考核指标。
至今，国内尚没有符合《GMT 0024-2014 SSL VPN 技术规范》标准的 Rust 版支持国密的TLS 代码库。因此，作为国内 Rust 技术重要推动力之一，溪塔科技 Rust 工程研发团队调研了几乎所有 Rust 版本实现的 TLS 代码库，发现 rustls 代码库对于 Rust 原生加密算法支持优秀，最终选择了 rustls 为基础代码库进行进一步改造。
在此过程中，工程研发团队花了大量时间改造优化了大量代码库内容，比如基于 libp2p 开发的 p2p 库（tentacle），以及 rustls 所使用的加密算法库 ring 等。本次改造内容主要集中在对基于国密算法产生的 x509v3 证书作了支持以及新增加国密密码套件 ECDHE-SM4-SM3。当前，溪塔科技 Rivus 软件中添加了产生国密证书的工具 create-certificate，企业只需要添加简单的配置项，就可以在自身产品中使用“纯国密”算法改造的 TLS，进行符合国家标准的安全通信。

https://github.com/wangmarkqi/rust_sm   塔西

*Yangyu:*

由于可信区块链评测标准不能公开到网上，我已私发给咱们某些需要的人，还有需要的也可以私信我。
后续我会跟进Substrate中和账户密码，网络这两个相关的模块

*Kaichao:*

我看了联盟链对共识的要求，Substrate目前的功能已经比较完善了，需要做的是
* 通过授权才能添加、删除验证人或挖矿节点，我在把 https://github.com/gautamdhameja/substrate-validator-set 迁移到babe环境里
* 验证人 DDoS 功能

comments from Aten,

这个我稍微提一点哈，就是联盟链里面为了节省资源，有一些会设计一个不出空块的概念，就是相当于如果这个块内没有交易，那么这个块就不打包。相当于并不是以时间去延伸区块的，而是有资源才去。因为Substrate基于以太坊体系，每个区块就是一次状态的commit，如果本来没有交易的话，对于联盟链场景来说并没有有需要对状态一次commit的必要性。因为实际上mpt树绝大部分存储耗费都在commit后附加的一堆存储上，所以如果能减少不必要的状态commit会对节省资源有很大效果。
这种不出空块的设计应该是这样的，类比substrate加个pow，copy一份babe/aura 并在这个基础上加上不出空块的逻辑，grandpa的逻辑不用变，然后用户集成的时候相当于可选新共识模式，然后就可以选择这种不出空块的babe/aura的模式。

comments from Guo Bin,
不出快对生产环境挺有帮助，早上我还想，一直出空块，对嵌入式区块链环境，受不了

*Guo Bin:*

节点身份验证这块的需求，我们做了初步梳理，以及参照fabric的文档，开展初步设计
https://shimo.im/sheets/WRDdHQKCvc6YJdhy/MODOC/ 《节点通信需求调研情况》

*Jimmy:*

更新了[Google sheet - task details](https://docs.google.com/spreadsheets/d/109i2MazaxlTuHeGkm9srD9JeFJcp299UyHxR0Bn27ak/edit#gid=0) 账本数据 一节。

## 2020-02-25

**会议内容：**

* 分享了几个机构的测试报告，公开测试结果如可公开会放在archive文件夹下
* 分享了Hyperledger里国密算法的现状
* 讨论了规范里的功能细节，及每个细节目前在Substrate里的支持程度，工作组成员认领了感兴趣的功能细节

**Actions:**

* Substrate里需要进行国密改密改造的feature list - Maggie
* 秘猿的国密Rust库是否满足Substrate开发联盟链的需求 - Maggie, Yuan Bo
* 现有的Substrate身份体系是否满足节点通信对身份的要求 - Zhou Jun, Li Qiang, Guobin
* 调研CA在联盟链里需求的优先级，如果需要，制定相应的开发策略 - Zhou Jun, Li Qiang, Guobin
* 调研Substrate在联盟链账本数据一节的适用性 - Jimmy
* 调研Substrate已有共识协议是否满足联盟链的需求 - Kaichao
* 移植PBFT、Overlord共识协议，及对应的性能测试 - Mike Tang
* 调研Substrate智能合约是否满足联盟链的需求 - Lipeng
* 身份管理、隐私保护、运维、治理、监管优先级较低
* 确认是否上传《可信区块链测试报告》- Yangyu
* 定期更新forked的Substrate，并创建合作主分支 - Kaichao
* 确认调研的结果是否满足自己的需求 - Guobin, Li Qiang


## 2020-02-04

* 讨论了 Substrate联盟链推进计划 启动的原因和已有的成果
* 讨论了本计划的目标，原则，资源，协作的基本形式
* 分享了各自对联盟链的一些重点的需求，如国密，嵌入式硬件
* 概览了 金融分布式账本技术安全规范 的主要内容
* 分享了行业内的一些区块链平台的测试机构和资料
