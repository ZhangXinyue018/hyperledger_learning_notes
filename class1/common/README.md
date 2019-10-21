# /common/chaincode
common里的chaincode代码很简单，主要定义了`Metadata`，`MetadataSet`，`MetadataMapping`和`InstalledChaincode`四个基本结构体。其中`MetadataMapping`囊括了对集合中的chaincode的metadata的一些一本操作，例如更新，查询，添加到集合