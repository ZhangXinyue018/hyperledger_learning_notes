# 通用

## 目的
本篇目的是为了方便快速了解hyperledger fabric的大体结构，便于快速定位所需

## 介绍
区块链都是有节点（node）组成，在共有链（permissionless blockchain）下，全部节点都有相同的记录区块，生产区块（比如挖矿）的权利。而在联盟链（permissioned blockchain）下，不同的节点可能会有不同的角色。<br>
hyperledger fabric中定义了四种节点角色，其中两种为fabric网络中的系统角色，
- `peer`：记录账本（ledger），运行智能合约（chaincode/smartcontract），为智能合约运行结果背书（endorse），发现和广播网络中其他的peer节点等
- `orderer`：生成账本
另外两种为：
- `client`：通过连接peer节点请求智能合约的运行
- `ca`：颁发任何角色的合法证书

## fabric网络
上述介绍中介绍了四种节点的角色，一个fabric网络中不只要有节点的角色，还要有一些虚拟层面的角色或基于虚拟层面的概念
1. `organization`：fabric网络中可以加入很多个organization作为网络中的一员，peer，orderer和client都需要属于网络中的organization（通过msp身份认证的方式）才能够算是加入了fabric网络。
2. `channel`：并不是网络中全部的organization共享全部的数据，不同的organization可以通过在网络上创建一个channel去形成一个属于这些organization的账本（ledger）
3. `ledger`：作用在channel上（一个ledger可以对应到共有链上的一个链）在fabric中，ledger分为两部分，一部分记录交易流水，一部分记录数据状态（world state）

## 一个完整的交易流程
hyperledger一个完整且成功的交易流程如下：
1. `client`连接网络，针对指定channel上的指定合约创建一个请求
2. `client`将请求发去不同的`peer`进行运行和背书（endorse）
3. `client`收集到运行的结果（一个对world state的读写集）和背书的结果并打包整理发去给`orderer`
4. `orderer`创建新的区块并广播给网络中此channel的`peer`
5. `peer`收到广播核实验证结果之后写入自己管理的账本中（包括更新读写集结果和添加交易流水）
6. `client`通过和`peer`之间的grpc收到账本的更新