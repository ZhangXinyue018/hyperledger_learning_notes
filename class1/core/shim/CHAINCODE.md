# chaincode

## 接口实现
- `ChaincodeStub`实现了`ChaincodeStubInterface`接口
- `HistoryQueryIterator`实现了`HistoryQueryIteratorInterface`接口
- `StateQueryIterator`实现了`StateQueryIteratorInterface`接口

## 定义结构
- `ChaincodeLogger`
```golang
func (c *ChaincodeLogger) SetLevel(level LoggingLevel) {
    ...
}
func (c *ChaincodeLogger) IsEnabledFor(level LoggingLevel) bool {
    ...
}
func (c *ChaincodeLogger) Debug(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Info(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Notice(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Warning(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Error(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Critical(args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Debugf(format string, args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Infof(format string, args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Noticef(format string, args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Warningf(format string, args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Errorf(format string, args ...interface{}) {
    ...
}
func (c *ChaincodeLogger) Criticalf(format string, args ...interface{}) {
    ...
}
```

## 定义一些方法
### Start
start方法是chaincode的启动项，启动后的操作包括
- 初始化logger：`SetupChaincodeLogging`
- 初始化factory（TBD）
- 初始化streamGetter，该stream用于chaincode和peer之间的交流，将会被传入chatWithPeer方法中
- chatWithPeer

### chatWithPeer
此方法是chaincode和peer之间通信的启动项，做了如下几件事情
- 初始化了一个chaincodehandler，用以实现事件处理
- 创建了一个msgAvail的通道，用以接收peer消息
- 创建了一个errc的通道，用以接收err消息
- for循环监听通道消息，根据通道消息做对应的handle处理