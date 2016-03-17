# 比特币测试网络盒子

创建你自己的私有比特币测试网络。

如果你是在本机运行两个比特币节点，你必须事先安装好了`bitcoind`和`bitcoin-cli`，并且这两个文件在系统路径里面。
如果你是在docker容器里面运行比特币节点，则无需在本机安装上面的两个程序，当然docker容器里面需要安装比特币的程序。

## 启动测试网络盒子

节点1会监听端口19000, 节点2通过该端口连接节点1，因为两个节点都位于同一个机器，所以节点2不能监听同一个端口。

节点1的JSON-RPC服务器端口是19001，节点2的JSON-RPC服务器端口是19011。


```
$ make start
```

## 检查节点状态

```
$ make getinfo
bitcoin-cli -datadir=1  getinfo
{
    "version" : 90300,
    "protocolversion" : 70002,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "blocks" : 0,
    "timeoffset" : 0,
    "connections" : 1,
    "proxy" : "",
    "difficulty" : 0.00000000,
    "testnet" : false,
    "keypoololdest" : 1413617762,
    "keypoolsize" : 101,
    "paytxfee" : 0.00000000,
    "relayfee" : 0.00001000,
    "errors" : ""
}
bitcoin-cli -datadir=2  getinfo
{
    "version" : 90300,
    "protocolversion" : 70002,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "blocks" : 0,
    "timeoffset" : 0,
    "connections" : 1,
    "proxy" : "",
    "difficulty" : 0.00000000,
    "testnet" : false,
    "keypoololdest" : 1413617762,
    "keypoolsize" : 101,
    "paytxfee" : 0.00000000,
    "relayfee" : 0.00001000,
    "errors" : ""
}
```

## 生成节点

生成一个块:

```
$ make generate
```

生成10个块:

```
$ make generate BLOCKS=10
```

## 发送比特币/转账
To send bitcoins that you've generated:

```
$ make send ADDRESS=mxwPtt399zVrR62ebkTWL4zbnV1ASdZBQr AMOUNT=10
```

## 把钱转回节点1
在节点1产生块获得比特币，上面把币转给节点2后，如果需要再转回给节点1，
需要为节点1生成一个新的收款地址，你可以指定与该地址绑定的账号。

```
$ make address ACCOUNT=testwithdrawals
```

## 停止运行

```
$ make stop
```

清除运行时产生的文件，恢复到最初状态:

```
$ make clean
```

## 使用Docker容器运行比特币节点
测试网络盒子可以运行在[Docker](https://www.docker.com/)中。

### 构建容器镜像

拉取镜像
  * `docker pull blockchain-university/bitcoin-testnet-box`

或者在当前目录构建镜像
  * `docker build -t bitcoin-testnet-box .`

### 运行容器
在容器里面会在后台运行两个比特币节点，你可以从容器外面通过JSON-RPC控制这两个节点。

* `$ docker run -t -i blockchain-university/bitcoin-testnet-box`
