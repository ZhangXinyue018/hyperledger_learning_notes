# scc
scc文件定义了系统chaincode的结构体
```golang
type SCC struct {
	Protobuf  Protobuf
	Functions SCCFunctions
}
```
其中的`SCCFunctions`定义了一个接口
```golang
type SCCFunctions interface {
	// InstallChaincode persists a chaincode definition to disk
	InstallChaincode(name, version string, chaincodePackage []byte) (hash []byte, err error)

	// QueryInstalledChaincode returns the hash for a given name and version of an installed chaincode
	QueryInstalledChaincode(name, version string) (hash []byte, err error)
}
```