# 初始文件说明
初始区块是区块链的起始 — 第一个区块，区块0，唯一没有指向前面区块的一个区块。协议确保其他节点不会和你的区块链一致，除非他们和你有相同的初始区块，这样可以创建多个私有的区块链。
```
{
     "config": {s
        "chainId": 1022,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x400",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```
存储文件为==genesis.json==。启动geth节点的时候引用该文件，以下为各个参数的说明。

-   mixhash：与nonce配合用于挖矿，由上一个区块的一部分生成的hash。注意他和nonce的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。
-   nonce nonce就是一个64位随机数，用于挖矿，注意他和mixhash的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。
-   difficulty 设置当前区块的难度，如果难度过大，cpu挖矿就很难，这里设置较小难度
-   alloc
用来预置账号以及账号的以太币数量，因为私有链挖矿比较容易，所以我们不需要预置有币的账号，需要的时候自己创建即可以。
-   coinbase
矿工的账号，随便填
-   timestamp
设置创世块的时间戳
-   parentHash
上一个区块的hash值，因为是创世块，所以这个值是0
-   extraData
附加信息，随便填，可以填你的个性信息
-   gasLimit
该值设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，因为我们是私有链，所以填最大。



```
--genesis /[path]/CustomGenesis.json
```
# 私有网络命令说明
以下是常用的确保私有网络的命令，这些命令都会用在geth的客户端
```
--nodiscover
```
-   1、如果具有相同的初始文件和网络ID，节点可能会被其它区块链发现并无意添加，该命令可以确保节点不会被发现，或非手动添加节点。

```
--maxpeers 0
```
-   2、设置节点的数量，如果不希望有其他节点接入，设置为0，通过调整数字来实现加入节点的数量。

```
--rpc
```
-   3、启动rpc通信，可以进行智能合约的部署和调试

```
--rpcapi "db,eth,net,web3"
```
-   4、设置允许连接的rpc的客户端，一般为db,eth,net,web3

```
--rpcprot "8080"
```
-   5、geth默认的rpc端口是8080

```
--rpccorsdomain "http://chriseth.github.io/browser-solidity/"
```
-   6、设置特定的URL能够连接到节点来执行RPC的定制任务，若输入wildcard ( * )，会使所有的URL都能连接到RPC实例中

```
--datadir "/home/TestChain1"
```
-   7、设置私有链数据所存储的数据目录，一般会选择一个与以太坊公有链文件夹分开的位置。

```
--identity "TestnetMainNode"
```
-   8、区块链的标示，随便填写，用于标示目前网络的名字。


```
--networkid 94784
```
-   9、设置当前区块链的网络ID，用于区分不同的网络，是一个数字。


# 获取go-ethereum
-   从github获取go-ethereum

```
go get github.com/ethereum/go-ethereum

```
-   编译所有的程序

```
make all
```


# 主要命令说明

-   启动加载创始块命令

```
$ geth --datadir "/home/makang/eth" init /path/to/genesis.json
```

-   通过命令方式启动，直接执行如下命令：

```
geth --identity "mkethereum" --rpc --rpcaddr "ip address" --rpccorsdomain "*" --datadir "/home/makang/eth" --port "30303" --ipcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3,solc" --rpcapi "db,eth,net,web3,personal" --networkid 95518 --nodiscover console
sudo docker run -it --name eth1 --rpc --identity "eth1" -p 30303:30303 ethereum/client-go
geth --identity "mkwinethereum" --rpc --rpcaddr "ip address"  --rpccorsdomain "*" --datadir "C:\Program Files\Mist\nodes" --port "30304" --ipcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3,solc" --rpcapi "db,eth,net,web3,personal" --networkid 95518 --nodiscover console

nohup geth --nodiscover --maxpeers 3 --identity "mkprivateethereum" --rpc --rpccorsdomain "*" --port "30303" --rpcapi "db,eth,net,web3" --networkid "1104"

```
-   通过docker启动私有链的创世块

```
sudo docker run --name eth1 -v ${PWD}:/ethereum ethereum/client-go init /ethereum/genesis.json
```

-   进入命令控制台命令

```
geth attach ipc:geth.ipc  //geth.ipc 为ipc文件地址
geth attach http://host:8545
geth attach ws://host:8546


geth.exe attach ipc:\\.\pipe\geth.ipc  //windows环境下使用该命令
```
==注意：可以通过设置rpcapi、ipcapi、wsapi设置通过http-prc、ipc-rpc、ws-prc方式访问API接口方式==

# 在一台机器创建私有链初始化步骤
-   创建三个不同的datafile目录，用于存储不同节点的区块data

```
mkdir $PWD/geth/datafile/node1
mkdir $PWD/geth/datafile/node2
mkdir $PWD/geth/datafile/node3
```
-   创建genesis.json创始块文件

```
sudo vim $PWD/geth/config/genesis.json
```

1.初始化第一个节点并创建创始块

```
geth --datadir "/home/makang/geth/datafile/node1" init  /home/makang/geth/config/genesis.json
```
2.启动第一个节点

```
nohup geth --datadir "/home/makang/geth/datafile/node1" --nodiscover --maxpeers 3 --identity "node1" --rpc --rpcport "8545" --rpcaddr "0.0.0.0" --rpccorsdomain "*" --port "30303" --rpcapi "db,eth,net,web3" --networkid "1104" &
```
3.初始化各个节点
4.启动各个节点，端口号不相同

```
nohup geth --datadir /home/makang/geth/datafile/node2 --nodiscover --maxpeers 3 --identity node2 --port 30304 --rpcapi db,eth,net,web3 --networkid 1104 &

```
5.查看节点信息

```
admin.nodeInfo
```
6.在另外一个节点中加入enode，替换@后面的IP地址

```
admin.addPeer("enode://c40be52fdbccf7c7ee4832b581516abf3fb0fbe695bcbeaad89886cd9f49f712c98c4cd67abfe81ca0429f95e2f9006f0349b88f43240336ef28504629961a2c@192.168.21.128:30304?discport=0")
```
### 使用mist打开私有链
```
mist.exe --rpc http://ip address:8545
```
