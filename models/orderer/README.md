# orderer

## 简介
orderer是hyperledger fabric网络中负责创建区块的节点，因为区块是针对channel而言的，那么orderer服务也是对channel进行的。orderer作为服务可以有很多种模式
- `solo`：单节点模式，整个channel的区块创建由单一节点完成
- `kafka`：kafka模式，orderer节点可以加入到kafka集群当中，以kafka的协同方式决定orderer的协同工作方式
- `raft`：（TBD）

## 从作者角度orderer需要支持哪些功能
- 【身份验证】对即将广播的区块进行签名
- 【共识支持】支持不同共识模式的区块创建
- 【区块创建】创建区块，包括创世区块
- 【身份管理】验证client提交的交易签名和背书签名信息？
- 【连接管理】在网络中更新peer的实时状态？
- 【网络】向peer广播创建的区块
- 【持久化】持久化区块链状态，以便进行下一次区块生成