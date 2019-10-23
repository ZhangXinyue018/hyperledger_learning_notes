# lifecycle
lifecycle结构实现了SCCFunctions，并指定了两个成员变量

```golang
type Lifecycle struct {
	ChaincodeStore ChaincodeStore
	PackageParser  PackageParser
}
```

其中ChaincodeStore和PackageParser都是接口。<br>

下面展示了SCCFunctions的InstallChaincode的实现
```golang
func (l *Lifecycle) InstallChaincode(name, version string, chaincodeInstallPackage []byte) ([]byte, error) {
	_, err := l.PackageParser.Parse(chaincodeInstallPackage)
	if err != nil {
		...
	}

	hash, err := l.ChaincodeStore.Save(name, version, chaincodeInstallPackage)
	if err != nil {
		...
	}

	...
}
```
从上述代码可以看出，这里面的重点就是PackageParser.Parse和ChaincodeStore.Save，那么这两个的实现是什么，就取决于lifecycle在调用方是如何构造的了