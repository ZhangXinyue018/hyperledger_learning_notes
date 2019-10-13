# orderer核心代码

## 核心代码分析
orderer启动入口为`/orderer/common/server/main`，Start函数运行orderer
- 首先启动一个定义的操作环境，环境的启动在`core/operations/system.go`下
```golang
    opsSystem := newOperationsSystem(conf.Operations, conf.Metrics)
    err := opsSystem.Start()
```
- 启动一个账本工厂
```golang
    lf, _ := createLedgerFactory(conf, metricsProvider)
	sysChanLastConfigBlock := extractSysChanLastConfig(lf, bootstrapBlock)
    clusterBootBlock := selectClusterBootBlock(bootstrapBlock, sysChanLastConfigBlock)
```
- 启动一个签名器
```golang
    signer := localmsp.NewSigner()
```
- （TBD）
```golang
    clusterClientConfig := initializeClusterClientConfig(conf, clusterType, bootstrapBlock)
	clusterDialer := &cluster.PredicateDialer{
		ClientConfig: clusterClientConfig,
	}

	r := createReplicator(lf, bootstrapBlock, conf, clusterClientConfig.SecOpts, signer)
	// Only clusters that are equipped with a recent config block can replicate.
	if clusterType && conf.General.GenesisMethod == "file" {
		r.replicateIfNeeded(bootstrapBlock)
	}

	logObserver := floggingmetrics.NewObserver(metricsProvider)
    flogging.Global.SetObserver(logObserver)
```
- 启动一个配置文件管理器
```golang
    serverConfig := initializeServerConfig(conf, metricsProvider)
```
- 启动grpc的连接
```golang
    grpcServer := initializeGrpcServer(conf, serverConfig)
```
- 创建身份验证工具
```golang
    caSupport := &comm.CredentialSupport{
		AppRootCAsByChain:           make(map[string]comm.CertificateBundle),
		OrdererRootCAsByChainAndOrg: make(comm.OrgRootCAs),
		ClientRootCAs:               serverConfig.SecOpts.ClientRootCAs,
    }
```
- 创建channel的注册管理器
```golang
    manager := initializeMultichannelRegistrar(clusterBootBlock, r, clusterDialer, clusterServerConfig, clusterGRPCServer, conf, signer, metricsProvider, opsSystem, lf, tlsCallback)
```
- TBD

## 声明
有一些方法作者并不是十分了解或者认为并不是十分重要，但也有可能是被作者忽略的重要方法，欢迎指出