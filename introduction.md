# DBXChain技术文档

## DBXChain简介

DBXChain是一条公有链，是DBXChain数据交易所的底层区块链，不仅支撑着DBXChain的高频数据交易，还支持第三方开发应用，在DBXChain上开发应用不仅可以得到链上支持，还可以获得DBXChain多维度数据的对接，可以做出非常落地于民生的有价值应用。

## **DBXChain节点介绍**

DBXChain节点主要包含witness\_node和cli\_wallet两部分。

witness\_node 通过 P2P 方式连接到DBXChain网络，从网络接收最新区块，向网络广播本地签署的交易包。

cli\_wallet 通过 websocket 方式连接到 witness\_node， 管理钱包文件； 提供交易签名功能，签名后通过 witness\_node 向外广播； 通过 http rpc 的方式提供 API 供其他程序调用。

## **DBXChain程序下载**

DBXChain程序目前只提供Ubuntu 16.04 LTS 64位系统的安装包，[点击这里](https://github.com/dbxhain/dbx-core/releases/latest)下载最新程序。

## 节点端口说明

启动DBXChain见证节点witness\_node

```bash
# 可以使用2个参数，节省内存： --track-account 和 --partial-operations=true
nohup ./programs/witness_node/witness_node --data-dir=trusted_node --rpc-endpoint=127.0.0.1:28090 \
--p2p-endpoint=0.0.0.0:6789 --log-file  \
--partial-operations=true >>witness.out 2>&1 &
```

端口种类及调用说明

| **端口类型** | **端口信息** |
| :---: | :---: |
| 28090 | witness\_node提供的rpc服务端口 |
| 6789 | P2P网络的通信接口，用于广播交易消息体和区块 |

命令行钱包cli\_wallet连接witness\_node:

```
./programs/cli_wallet/cli_wallet -s ws://127.0.0.1:28090 \
--enable-rpc-log -r 127.0.0.1:8091 --data-dir=trusted_node
```

端口种类及调用说明

| **端口类型** | **端口信息** |
| :---: | :---: |
| 28090 | 连接witness\_node提供的rpc服务端口 |
| 8091 | cli\_wallet提供的rpc服务端口 |



