# Interface
这里定义了一系列的接口

### Chaincode
全部的chaincode，无论是系统的，还是开发的，都要实现该接口
```golang
type Chaincode interface {
	Init(stub ChaincodeStubInterface) pb.Response
	Invoke(stub ChaincodeStubInterface) pb.Response
}
```
### ChaincodeStubInterface
对ledger数据的操作接口，写chaincode的时候通过这个接口对ledger数据进行操作
```golang
type ChaincodeStubInterface interface {
	GetArgs() [][]byte
	GetStringArgs() []string
	GetFunctionAndParameters() (string, []string)
	GetArgsSlice() ([]byte, error)
	GetTxID() string
	GetChannelID() string
	InvokeChaincode(chaincodeName string, args [][]byte, channel string) pb.Response
	GetState(key string) ([]byte, error)
	PutState(key string, value []byte) error
	DelState(key string) error
	SetStateValidationParameter(key string, ep []byte) error
	GetStateValidationParameter(key string) ([]byte, error)
	GetStateByRange(startKey, endKey string) (StateQueryIteratorInterface, error)
	GetStateByRangeWithPagination(startKey, endKey string, pageSize int32,
		bookmark string) (StateQueryIteratorInterface, *pb.QueryResponseMetadata, error)
	GetStateByPartialCompositeKey(objectType string, keys []string) (StateQueryIteratorInterface, error)
	GetStateByPartialCompositeKeyWithPagination(objectType string, keys []string,
		pageSize int32, bookmark string) (StateQueryIteratorInterface, *pb.QueryResponseMetadata, error)
	CreateCompositeKey(objectType string, attributes []string) (string, error)
	SplitCompositeKey(compositeKey string) (string, []string, error)
	GetQueryResult(query string) (StateQueryIteratorInterface, error)
	GetQueryResultWithPagination(query string, pageSize int32,
		bookmark string) (StateQueryIteratorInterface, *pb.QueryResponseMetadata, error)
	GetHistoryForKey(key string) (HistoryQueryIteratorInterface, error)
	GetPrivateData(collection, key string) ([]byte, error)
	GetPrivateDataHash(collection, key string) ([]byte, error)
	PutPrivateData(collection string, key string, value []byte) error
	DelPrivateData(collection, key string) error
	SetPrivateDataValidationParameter(collection, key string, ep []byte) error
	GetPrivateDataValidationParameter(collection, key string) ([]byte, error)
	GetPrivateDataByRange(collection, startKey, endKey string) (StateQueryIteratorInterface, error)
	GetPrivateDataByPartialCompositeKey(collection, objectType string, keys []string) (StateQueryIteratorInterface, error)
	GetPrivateDataQueryResult(collection, query string) (StateQueryIteratorInterface, error)
	GetCreator() ([]byte, error)
	GetTransient() (map[string][]byte, error)
	GetBinding() ([]byte, error)
	GetDecorations() map[string][]byte
	GetSignedProposal() (*pb.SignedProposal, error)
	GetTxTimestamp() (*timestamp.Timestamp, error)
	SetEvent(name string, payload []byte) error
}
```
### HistoryQueryIteratorInterface
用以history结果的循环，主要有`HasNext`，`Close`和`Next`
```golang
type HistoryQueryIteratorInterface interface {
	// Inherit HasNext() and Close()
	CommonIteratorInterface

	// Next returns the next key and value in the history query iterator.
	Next() (*queryresult.KeyModification, error)
}
```
### StateQueryIteratorInterface
用以ChaincodeStubInterface的某些（例如GetStateByRange）结果的循环，主要有`HasNext`，`Close`和`Next`
```golang
type StateQueryIteratorInterface interface {
	// Inherit HasNext() and Close()
	CommonIteratorInterface

	// Next returns the next key and value in the range and execute query iterator.
	Next() (*queryresult.KV, error)
}
```