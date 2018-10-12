# 私有链搭建


## 1、创建和编辑初始文件

初始文件是用来定义区块链网络初始状态，如下：
* 修改初始文件中账户， 以及账户名和公钥
* 区块链资产和初次分发（包含核心资产）
* 私链参数的最初基准（包括费用）
* 初始见证人的账户签名秘钥


创建一个名为`my-genesis.json`的初始文件：

```
$ witness_node --create-genesis-json my-genesis.json
```

石墨烯系统中默认的初始化块中包含唯一一个账户`nathan`，创世区块中的所有见证人、理事会成员和基金会都是改账户。 

eg.1 修改`nathan`私钥

`nathan`的默认私钥为:
> 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3

通过手动修改初始文件对私钥进行修改。

eg.2 修改活跃见证人更新时间
```
"maintenance_interval": 600
```


## 4、初始化证人节点，获取链ID

链ID是初始状态的哈希值。它用来区分不同的链。

```
witness_node --genesis-json my-genesis.json
```

如修改了初始文件，链ID将会变化，因此会创建出一条不同的链。

当这条信息出现时:

```
3501235ms th_a main.cpp:165 main] Started witness node on a chain with 0 blocks.
3501235ms th_a main.cpp:166 main] Chain ID is 6e340b9cffb37a989ca544e6bb780a2c78901d3fb33738768511a30617afa01d
```

此时你的初始化已经完成，按`ctrl-c` 关闭见证人节点。

因此, 你应该生成了两个文件:

* 在词典库`data`中创建了一个新文件`config.ini`
* Your blockchain id is now known - it’s displayed in the message above.

**注意**

**请注意你的区块链ID会和上述例子中的ID不同。请记录下你的ID，在之后你将会用到它。**

## 5、配置见证人

用文本编辑器打开刚生成的`data/config.ini`, 做如下设置, 必要时请不要注释这些代码:

```
rpc-endpoint = 127.0.0.1:11011
genesis-json = my-genesis.json
enable-stale-production = true
```

在`config.ini`中定位以下语句:

```
# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
```

并添加如下词条:

```
witness-id = "1.6.1"
witness-id = "1.6.2"
witness-id = "1.6.3"
witness-id = "1.6.4"
witness-id = "1.6.5"
witness-id = "1.6.6"
witness-id = "1.6.7"
witness-id = "1.6.8"
witness-id = "1.6.9"
witness-id = "1.6.10"
witness-id = "1.6.11"
```

上述列表授权了见证人节点用见证人ID来生成区块. 正常情况下，每个见证人的节点不同，但在私有链中，我们会先设定成全体见证人在同一个节点生产区块。这些见证人ID的私钥（用来签署区块）已经在`config.ini`中提供：

```
# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["DBX6MRyA...T5GDW5CV","5KQwrPb...tP79zkvFD3"]
```

## 6、开始生产区块

通过以下步骤，你可以生产基于你私链的第一个区块了，在见证人节点中运行以下命令:

```
witness_node --data-dir data
```

之后私链的区块将开始生成，你会看到如下指示:

```
********************************
*                              *
*   ------- NEW CHAIN ------   *
*   - Welcome to Graphene! -   *
*   ------------------------   *
*                              *
********************************
```

之后data/log/witness.log文件会有更多成功生成区块的日志生成:

```
2322793ms th_a  main.cpp:176     main    ] Started witness node on a chain with 0 blocks.
2322794ms th_a  main.cpp:177     main    ] Chain ID is 6e340b9cffb37a989ca544e6bb780a2c78901d3fb33738768511a30617afa01d
2324613ms th_a  witness.cpp:185  block_production_loo ] Generated block #1 with timestamp 2016-01-21T22:38:40 at time 2016-01-21T22:38:40
2325604ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2342604ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2343609ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2344604ms th_a  witness.cpp:185  block_production_loo ] Generated block #2 with timestamp 2016-01-21T22:39:00 at time 2016-01-21T22:39:00
2345605ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2349616ms th_a  witness.cpp:185  block_production_loo ] Generated block #3 with timestamp 2016-01-21T22:39:05 at time 2016-01-21T22:39:05
2350602ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2353612ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2354605ms th_a  witness.cpp:185  block_production_loo ] Generated block #4 with timestamp 2016-01-21T22:39:10 at time 2016-01-21T22:39:10
2355609ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
2356609ms th_a  witness.cpp:194  block_production_loo ] Not producing block because slot has not yet arrived
```

如果witness.log无日志生成，可以将日志打印打控制台，可以修改data/config.ini文件如下，然后重新启动witness

```
[logger.default]
level=debug
appenders=stderr
```

## 7、客户端（Cli）用法

现在可以将客户端和你的私链的见证人节点相关联。先确保你的见证人节点在运行状态，在另外一个CMD中运行以下命令：

```
cli_wallet --wallet-file=my-wallet.json --chain-id 6e340b9cffb37a989ca544e6bb780a2c78901d3fb33738768511a30617afa01d --server-rpc-endpoint=ws://127.0.0.1:11011
```

**注意**

请确保用**你自己私链的区块链ID**替代上述ID`8b7bd36a...4294824`区块链ID传递给客户端时需要匹配生成的ID，且给见证人节点需要使用此区块链ID。

如果你收到`set_password`提示，意味着你的客户端已经成功匹配见证人节点。

### 创建一个新钱包

首先你需要为你的钱包创建一个新的密码。这个密码被用于加密所有钱包的私钥。在教程中我们使用如下密码：`supersecret`

但你可以使用字母和数字的组合来创建属于你的密码。通过以下命令来创建你的密码：:

```
>>> set_password supersecret
```

现在你可以解锁你新建的钱包了：

```
unlock supersecret
```

### 获得初始份额

在石墨烯中，资产账户包含在钱包账户中， 要向你的钱包中添加钱包账户, 你需要知道账户名以及账户的私钥。 在例子中，我们将通过`import_key`命令向现有钱包中添加一个名叫`nathan`的账户：

```
import_key nathan 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
```

**注意**

注意`nathan`在初始文件中会被用于定义账户名. 如果你修改过`my-genesies.json` 文件，你可以填入一个不同的名字。并且，请注意`5KQwrPbwdL...P79zkvFD3`是定义在`config.ini`内的私钥

现在我们已经将私钥导入进钱包，但没有和储蓄相关联。储蓄被保存在初始账户内。这些储蓄可以通过`import_balance`命令来申明，无需申明费用：

```
import_balance nathan ["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"] true
```

因此，我们导入了一个名为`nathan`的账户，且账户储蓄已经完全与DBX相关联，因为我们已经声称这些储蓄被保存在了初始文件内。你可以通过以下命令来检视你的账户：

```
get_account nathan
```

用以下命令获取账户余额：

```
list_account_balances nathan
```

### 创建其他账户

现在我们讲创建一个新的账户`alpha` ，这样我们可以在 `nathan`和`alpha`两个账户中来回转账了。

通常我们用一个已有账户来创建新账户，因为登记员需要缴纳注册费用。 并且，登记员的账户需要进入Also, there is the requirement  lifetime member \(LTM\)状态。因此我们必须在创建新账户前，先将账户`nathan`升级到LTM状态， 使用`upgrade_account`命令来升级账户：

```
upgrade_account nathan DBX true
```

**注意**

你需要重启客户端，否则将无法识别`nathan`已经成功升级。通过`ctrl-c`停止客户端，然后通过如下指令重启客户端：

```
cli_wallet --wallet-file=my-wallet.json --chain-id 6e340b9cffb37a989ca544e6bb780a2c78901d3fb33738768511a30617afa01d --server-rpc-endpoint=ws://127.0.0.1:11011
```

确认`nathan`已经拥有LTM权限：

```
get_account nathan
```

返回的信息中，在`membership_expiration_date`边上你会发现`1969-12-31T23:59:59`。 如果你看到`1970-01-01T00:00:00`，说明之前的操作出现了错误，`nathan`没能成功升级。

成功升级后，我们可以通过`nathan`来注册新账户，但首先我们需要拥有新账户的公钥。通过使用`suggest_brain_key`命令来完成：

```
suggest_brain_key
```

然后调用register\_account / register\_account2接口创建新帐户

```
register_account alpha DBX6vQtDEgHSickqe9itW8fbFyUrKZK5xsg4FRHzQZ7hStaWqEKhZ DBX6vQtDEgHSickqe9itW8fbFyUrKZK5xsg4FRHzQZ7hStaWqEKhZ nathan nathan 10
```

最终将有类似如下的回复：

```
list_account_balances alpha
```



