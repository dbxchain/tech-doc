# 快速开始

## 环境要求
* 系统: Ubuntu 16.04 LTS 64-bit, 4.4.0-63-generic 内核或更高
* 内存: 8GB+
* 硬盘: 100GB+

## 节点安装
以下的步骤演示的是主网节点的启动

如果你是开发者，希望快速体验，可前往[测试网络]() 

如果你想基于DBXChain搭建私有链，可前往[私有链搭建](dbxchain/private-chain.md)

### 1. 下载Release包
```
```

### 2. 启动节点

```
```

就是这样了, 根据上面的步骤:

启动了一个节点监听在 127.0.0.1:28090
指定了区块信息保存在 ./trusted_node 目录下
友情提示

同步区块大约需要 6个小时, 当然这和你的网络情况有一定关系
在区块同步完成之前，你只需耐心等待，在此期间可以阅读一下文档


### 3. 查看日志

```
tail -f trusted_node/logs/witness.log
```

节点同步完成后，日志看起来是这样的:

```
2018-06-28T03:43:03 th_a:invoke handle_block         handle_block ] Got block: #10731531 time: 2018-06-28T03:43:03 latency: 60 ms from: miner11  irreversible: 10731513 (-18)			application.cpp:489
2018-06-28T03:43:06 th_a:invoke handle_block         handle_block ] Got block: #10731532 time: 2018-06-28T03:43:06 latency: 16 ms from: taffy  irreversible: 10731515 (-17)			application.cpp:489
2018-06-28T03:43:09 th_a:invoke handle_block         handle_block ] Got block: #10731533 time: 2018-06-28T03:43:09 latency: 49 ms from: david12  irreversible: 10731515 (-18)			application.cpp:489
2018-06-28T03:43:12 th_a:invoke handle_block         handle_block ] Got block: #10731534 time: 2018-06-28T03:43:12 latency: 42 ms from: miner6  irreversible: 10731516 (-18)			application.cpp:489
2018-06-28T03:43:15 th_a:invoke handle_block         handle_block ] Got block: #10731535 time: 2018-06-28T03:43:15 latency: 10 ms from: sakura  irreversible: 10731516 (-19)			application.cpp:489
2018-06-28T03:43:18 th_a:invoke handle_block         handle_block ] Got block: #10731536 time: 2018-06-28T03:43:18 latency: 57 ms from: miner9  irreversible: 10731517 (-19)			application.cpp:489
2018-06-28T03:43:21 th_a:invoke handle_block         handle_block ] Got block: #10731537 time: 2018-06-28T03:43:21 latency: 56 ms from: robin-green  irreversible: 10731517 (-20)			application.cpp:489
2018-06-28T03:43:24 th_a:invoke handle_block         handle_block ] Got block: #10731538 time: 2018-06-28T03:43:24 latency: 17 ms from: kairos  irreversible: 10731522 (-16)			application.cpp:489
2018-06-28T03:43:27 th_a:invoke handle_block         handle_block ] Got block: #10731539 time: 2018-06-28T03:43:27 latency: 21 ms from: dennis1  irreversible: 10731524 (-15)			application.cpp:489
2018-06-28T03:43:30 th_a:invoke handle_block         handle_block ] Got block: #10731540 time: 2018-06-28T03:43:30 latency: 17 ms from: aaron  irreversible: 10731524 (-16)			application.cpp:489
2018-06-28T03:43:33 th_a:invoke handle_block         handle_block ] Got block: #10731541 time: 2018-06-28T03:43:33 latency: 23 ms from: caitlin  irreversible: 10731526 (-15)			application.cpp:489
```

## 账户注册
DBXChain采用账户模型，并且引入了推荐注册机制，因此在DBXChain上注册一个账号，需要以下三个要素:

推荐人，推荐人是链上已存在的账户，会使用你的账户名和公钥帮你注册一个账号
账户名，账户名在链上是唯一的，所以请记住在DBXChain上，账户名即地址 (如DBXChain-genius)
ECC公钥，以GXC开头，Base64 编码的ECC公钥 (如何生成公钥？别担心，请往后看)
有两种方式可以完成账户的注册:

### 1. 在线钱包
使用在线钱包https://wallet.gxb.io在界面上完成上述步骤

### 2. 手动注册
推荐对私钥安全要求较高的开发者使用这种方式完成注册，保证私钥是离线的

#### 步骤1: 通过cli_wallet来生成一对公私钥
./programs/cli_wallet/cli_wallet --suggest-brain-key
{
  "brain_priv_key": "SHAP CASCADE AIRLIKE WRINKLE CUNETTE FROWNY MISREAD MOIST HANDSET COLOVE EMOTION UNSPAN SEAWARD HAGGIS TEENTY NARRAS",
  "wif_priv_key": "5J2FpCq3UmvcodkCCofXSNvHYTodufbPajwpoEFAh2TJf27EuL3",
  "pub_key": "GXC75UwALPEFECfHLjHyNSxCk1j7XzSvApQiXKEbanWgr7yvXXbdG"
}
字段解释

brain_priv_key: 助记词，是私钥的原始文本，通过助记词可以还原出私钥
wif_priv_key: 私钥，在程序中使用
pub_key: 公钥，用于链上账户注册
#### 步骤2: 通过水龙头来完成账户注册
想一个专属的账户名(account_name),如DBXChain-genius
替换下面curl命令中的 <account_name> and <public_key> 并在终端执行:
curl 'https://opengateway.gxb.io/account/register' -H 'Content-type: application/json' -H 'Accept: application/json’ -d ‘{“account”:{“name”:”<account_name>”,”owner_key”:”<public_key>”,”active_key”:”<public_key>”,”memo_key”:”<public_key>”,”refcode”:null,”referrer”:null}}’
在 GitHub 上编辑此页 上次更新: 2018/9/30 下午5:33:50