

# libDatabox
`import "./"`

* [Overview](#pkg-overview)
* [Index](#pkg-index)
* [Subdirectories](#pkg-subdirectories)

## <a name="pkg-overview">Overview</a>
A golang library for interfacing with Databox APIs.

Install using go get github.com/tsh2/lib-go-databox

Examples can be found in the samples directory




## <a name="pkg-index">Index</a>
* [Constants](#pkg-constants)
* [func ExportLongpoll(destination string, payload string) (string, error)](#ExportLongpoll)
* [func GetHttpsCredentials() string](#GetHttpsCredentials)
* [type AggregationType](#AggregationType)
* [type BinaryKeyValue_0_3_0](#BinaryKeyValue_0_3_0)
  * [func NewBinaryKeyValueClient(reqEndpoint string, enableLogging bool) (BinaryKeyValue_0_3_0, error)](#NewBinaryKeyValueClient)
* [type BinaryObserveResponse](#BinaryObserveResponse)
* [type DataSourceMetadata](#DataSourceMetadata)
  * [func HypercatToDataSourceMetadata(hypercatDataSourceDescription string) (DataSourceMetadata, string, error)](#HypercatToDataSourceMetadata)
* [type Filter](#Filter)
* [type FilterType](#FilterType)
* [type JSONKeyValue_0_3_0](#JSONKeyValue_0_3_0)
  * [func NewJSONKeyValueClient(reqEndpoint string, enableLogging bool) (JSONKeyValue_0_3_0, error)](#NewJSONKeyValueClient)
* [type JSONTimeSeriesBlob_0_3_0](#JSONTimeSeriesBlob_0_3_0)
  * [func NewJSONTimeSeriesBlobClient(reqEndpoint string, enableLogging bool) (JSONTimeSeriesBlob_0_3_0, error)](#NewJSONTimeSeriesBlobClient)
* [type JSONTimeSeriesQueryOptions](#JSONTimeSeriesQueryOptions)
* [type JSONTimeSeries_0_3_0](#JSONTimeSeries_0_3_0)
  * [func NewJSONTimeSeriesClient(reqEndpoint string, enableLogging bool) (JSONTimeSeries_0_3_0, error)](#NewJSONTimeSeriesClient)
* [type JsonObserveResponse](#JsonObserveResponse)
* [type TextKeyValue_0_3_0](#TextKeyValue_0_3_0)
  * [func NewTextKeyValueClient(reqEndpoint string, enableLogging bool) (TextKeyValue_0_3_0, error)](#NewTextKeyValueClient)
* [type TextObserveResponse](#TextObserveResponse)


#### <a name="pkg-files">Package files</a>
[core-store-kv-bin.go](/src/target/core-store-kv-bin.go) [core-store-kv-json.go](/src/target/core-store-kv-json.go) [core-store-kv-text.go](/src/target/core-store-kv-text.go) [core-store-ts-json-blob.go](/src/target/core-store-ts-json-blob.go) [core-store-ts-json.go](/src/target/core-store-ts-json.go) [export.go](/src/target/export.go) [types.go](/src/target/types.go) [utils.go](/src/target/utils.go) 


## <a name="pkg-constants">Constants</a>
``` go
const (
    Equals            FilterType      = "equals"
    Contains          FilterType      = "contains"
    Sum               AggregationType = "sum"
    Count             AggregationType = "count"
    Min               AggregationType = "min"
    Max               AggregationType = "max"
    Mean              AggregationType = "mean"
    Median            AggregationType = "median"
    StandardDeviation AggregationType = "sd"
)
```
Allowed values for FilterType and AggregationFunction




## <a name="ExportLongpoll">func</a> [ExportLongpoll](/src/target/export.go?s=339:410#L5)
``` go
func ExportLongpoll(destination string, payload string) (string, error)
```
ExportLongpoll exports data to external service (payload must be an escaped json string)
permissions must be requested in the app manifest (drivers dont need to use the export service)



## <a name="GetHttpsCredentials">func</a> [GetHttpsCredentials](/src/target/utils.go?s=2426:2459#L93)
``` go
func GetHttpsCredentials() string
```
GetHttpsCredentials Returns a string containing the HTTPS credentials to pass to https server when offering an https server.
These are read form /run/secrets/DATABOX.pem and are generated by the container-manger at run time.




## <a name="AggregationType">type</a> [AggregationType](/src/target/core-store-ts-json.go?s=124:151#L2)
``` go
type AggregationType string
```









## <a name="BinaryKeyValue_0_3_0">type</a> [BinaryKeyValue_0_3_0](/src/target/core-store-kv-bin.go?s=113:1007#L1)
``` go
type BinaryKeyValue_0_3_0 interface {
    // Write text value to key
    Write(dataSourceID string, key string, payload []byte) error
    // Read text values from key.
    Read(dataSourceID string, key string) ([]byte, error)
    //ListKeys returns an array of key registed under the dataSourceID
    ListKeys(dataSourceID string) ([]string, error)
    // Get notifications of updated values for a key. Returns a channel that receives BinaryObserveResponse containing a JSON string when a new value is added.
    ObserveKey(dataSourceID string, key string) (<-chan BinaryObserveResponse, error)
    // Get notifications of updated values for any key. Returns a channel that receives BinaryObserveResponse containing a JSON string when a new value is added.
    Observe(dataSourceID string) (<-chan BinaryObserveResponse, error)
    // Get notifications of updated values
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewBinaryKeyValueClient">func</a> [NewBinaryKeyValueClient](/src/target/core-store-kv-bin.go?s=1374:1472#L24)
``` go
func NewBinaryKeyValueClient(reqEndpoint string, enableLogging bool) (BinaryKeyValue_0_3_0, error)
```
NewBinaryKeyValueClient returns a new NewBinaryKeyValueClient to enable reading and writing of binary data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="BinaryObserveResponse">type</a> [BinaryObserveResponse](/src/target/types.go?s=293:413#L13)
``` go
type BinaryObserveResponse struct {
    TimestampMS  int64
    DataSourceID string
    Key          string
    Data         []byte
}
```









## <a name="DataSourceMetadata">type</a> [DataSourceMetadata](/src/target/types.go?s=446:685#L26)
``` go
type DataSourceMetadata struct {
    Description    string
    ContentType    string
    Vendor         string
    DataSourceType string
    DataSourceID   string
    StoreType      string
    IsActuator     bool
    Unit           string
    Location       string
}
```






### <a name="HypercatToDataSourceMetadata">func</a> [HypercatToDataSourceMetadata](/src/target/utils.go?s=6485:6592#L227)
``` go
func HypercatToDataSourceMetadata(hypercatDataSourceDescription string) (DataSourceMetadata, string, error)
```
HypercatToDataSourceMetadata is a helper function to convert the hypercat description of a datasource to a DataSourceMetadata instance
Also returns the store url for this data source.





## <a name="Filter">type</a> [Filter](/src/target/core-store-ts-json.go?s=746:829#L20)
``` go
type Filter struct {
    TagName    string
    FilterType FilterType
    Value      string
}
```
Filter types to hold the required data to apply the filtering functions of the structured json API










## <a name="FilterType">type</a> [FilterType](/src/target/core-store-ts-json.go?s=153:175#L4)
``` go
type FilterType string
```









## <a name="JSONKeyValue_0_3_0">type</a> [JSONKeyValue_0_3_0](/src/target/core-store-kv-json.go?s=133:1162#L3)
``` go
type JSONKeyValue_0_3_0 interface {
    // Write JSON value
    Write(dataSourceID string, key string, payload []byte) error
    // Read JSON values. Returns a []bytes containing a JSON string.
    Read(dataSourceID string, key string) ([]byte, error)
    //ListKeys returns an array of key registed under the dataSourceID
    ListKeys(dataSourceID string) ([]string, error)
    // Get notifications of updated values for a key. Returns a channel that receives JsonObserveResponse containing a JSON string when a new value is added.
    ObserveKey(dataSourceID string, key string) (<-chan JsonObserveResponse, error)
    // Get notifications of updated values for any key. Returns a channel that receives JsonObserveResponse containing a JSON string when a new value is added.
    Observe(dataSourceID string) (<-chan JsonObserveResponse, error)
    // RegisterDatasource make a new data source for available to the rest of datbox. This can only be used on stores that you have requested in your manifest.
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewJSONKeyValueClient">func</a> [NewJSONKeyValueClient](/src/target/core-store-kv-json.go?s=1521:1615#L26)
``` go
func NewJSONKeyValueClient(reqEndpoint string, enableLogging bool) (JSONKeyValue_0_3_0, error)
```
NewJSONKeyValueClient returns a new NewJSONKeyValueClient to enable reading and writing of JSON data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="JSONTimeSeriesBlob_0_3_0">type</a> [JSONTimeSeriesBlob_0_3_0](/src/target/core-store-ts-json-blob.go?s=124:2391#L2)
``` go
type JSONTimeSeriesBlob_0_3_0 interface {
    // Write  will be timestamped with write time in ms since the unix epoch by the store
    Write(dataSourceID string, payload []byte) error
    // WriteAt will be timestamped with timestamp provided in ms since the unix epoch
    WriteAt(dataSourceID string, timestamp int64, payload []byte) error
    // Read the latest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Latest(dataSourceID string) ([]byte, error)
    // Read the earliest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Earliest(dataSourceID string) ([]byte, error)
    // Read the last N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    LastN(dataSourceID string, n int) ([]byte, error)
    // Read the first N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    FirstN(dataSourceID string, n int) ([]byte, error)
    // Read values written after the provided timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Since(dataSourceID string, sinceTimeStamp int64) ([]byte, error)
    // Read values written between the start timestamp and end timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Range(dataSourceID string, formTimeStamp int64, toTimeStamp int64) ([]byte, error)
    //Length retruns the number of records stored for that dataSourceID
    Length(dataSourceID string) (int, error)
    // Get notifications when a new value is written
    // the returned chan receives JsonObserveResponse of the form {"TimestampMS":213123123,"Json":byte[]}
    Observe(dataSourceID string) (<-chan JsonObserveResponse, error)
    // registerDatasource is used by apps and drivers to register data sources in stores they own.
    RegisterDatasource(metadata DataSourceMetadata) error
    // GetDatasourceCatalogue is used by drivers to get a list of registered data sources in stores they own.
    GetDatasourceCatalogue() ([]byte, error)
}
```






### <a name="NewJSONTimeSeriesBlobClient">func</a> [NewJSONTimeSeriesBlobClient](/src/target/core-store-ts-json-blob.go?s=2752:2858#L44)
``` go
func NewJSONTimeSeriesBlobClient(reqEndpoint string, enableLogging bool) (JSONTimeSeriesBlob_0_3_0, error)
```
NewJSONTimeSeriesBlobClient returns a new jSONTimeSeriesBlobClient to enable interaction with a time series data store in unstructured JSON format
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="JSONTimeSeriesQueryOptions">type</a> [JSONTimeSeriesQueryOptions](/src/target/core-store-ts-json.go?s=911:1019#L27)
``` go
type JSONTimeSeriesQueryOptions struct {
    AggregationFunction AggregationType
    Filter              *Filter
}
```
JSONTimeSeriesQueryOptions described the options for the structured json API










## <a name="JSONTimeSeries_0_3_0">type</a> [JSONTimeSeries_0_3_0](/src/target/core-store-ts-json.go?s=1094:3485#L33)
``` go
type JSONTimeSeries_0_3_0 interface {
    // Write  will be timestamped with write time in ms since the unix epoch by the store
    Write(dataSourceID string, payload []byte) error
    // WriteAt will be timestamped with timestamp provided in ms since the unix epoch
    WriteAt(dataSourceID string, timestamp int64, payload []byte) error
    // Read the latest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Latest(dataSourceID string) ([]byte, error)
    // Read the earliest value.
    // return data is a JSON object of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Earliest(dataSourceID string) ([]byte, error)
    // Read the last N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    LastN(dataSourceID string, n int, opt JSONTimeSeriesQueryOptions) ([]byte, error)
    // Read the first N values.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    FirstN(dataSourceID string, n int, opt JSONTimeSeriesQueryOptions) ([]byte, error)
    // Read values written after the provided timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Since(dataSourceID string, sinceTimeStamp int64, opt JSONTimeSeriesQueryOptions) ([]byte, error)
    // Read values written between the start timestamp and end timestamp in in ms since the unix epoch.
    // return data is an array of JSON objects of the format {"timestamp":213123123,"data":[data-written-by-driver]}
    Range(dataSourceID string, formTimeStamp int64, toTimeStamp int64, opt JSONTimeSeriesQueryOptions) ([]byte, error)
    //Length retruns the number of records stored for that dataSourceID
    Length(dataSourceID string) (int, error)
    // Get notifications when a new value is written
    // the returned chan receives JsonObserveResponse of the form {"TimestampMS":213123123,"Json":byte[]}
    Observe(dataSourceID string) (<-chan JsonObserveResponse, error)
    // registerDatasource is used by apps and drivers to register data sources in stores they own.
    RegisterDatasource(metadata DataSourceMetadata) error
    // GetDatasourceCatalogue is used by drivers to get a list of registered data sources in stores they own.
    GetDatasourceCatalogue() ([]byte, error)
}
```
JSONTimeSeries_0_3_0 described the the structured json timeseries API







### <a name="NewJSONTimeSeriesClient">func</a> [NewJSONTimeSeriesClient](/src/target/core-store-ts-json.go?s=4041:4139#L76)
``` go
func NewJSONTimeSeriesClient(reqEndpoint string, enableLogging bool) (JSONTimeSeries_0_3_0, error)
```
NewJSONTimeSeriesClient returns a new jSONTimeSeriesClient to enable interaction with a structured timeseries data store in JSON format.
The data written must contain at least {"value":[any numeric value]}. This is used in the aggregation functions. Other data can be store and used at KV pairs to filter the data but it can not be processed.
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="JsonObserveResponse">type</a> [JsonObserveResponse](/src/target/types.go?s=53:171#L1)
``` go
type JsonObserveResponse struct {
    TimestampMS  int64
    DataSourceID string
    Key          string
    Json         []byte
}
```









## <a name="TextKeyValue_0_3_0">type</a> [TextKeyValue_0_3_0](/src/target/core-store-kv-text.go?s=113:1155#L1)
``` go
type TextKeyValue_0_3_0 interface {
    // Write text value
    Write(dataSourceID string, key string, payload string) error
    // Read text values. Returns a string containing the text written to the key.
    Read(dataSourceID string, key string) (string, error)
    //ListKeys returns an array of key registed under the dataSourceID
    ListKeys(dataSourceID string) ([]string, error)
    // Get notifications of updated values for a key. Returns a channel that receives TextObserveResponse containing a JSON string when a new value is added.
    ObserveKey(dataSourceID string, key string) (<-chan TextObserveResponse, error)
    // Get notifications of updated values for any key. Returns a channel that receives TextObserveResponse containing a JSON string when a new value is added.
    Observe(dataSourceID string) (<-chan TextObserveResponse, error)
    // RegisterDatasource make a new data source for available to the rest of datbox. This can only be used on stores that you have requested in your manifest.
    RegisterDatasource(metadata DataSourceMetadata) error
}
```






### <a name="NewTextKeyValueClient">func</a> [NewTextKeyValueClient](/src/target/core-store-kv-text.go?s=1513:1607#L24)
``` go
func NewTextKeyValueClient(reqEndpoint string, enableLogging bool) (TextKeyValue_0_3_0, error)
```
NewTextKeyValueClient returns a new TextKeyValue_0_3_0 to enable reading and writing of string data key value to the store
reqEndpoint is provided in the DATABOX_ZMQ_ENDPOINT environment varable to databox apps and drivers.





## <a name="TextObserveResponse">type</a> [TextObserveResponse](/src/target/types.go?s=173:291#L6)
``` go
type TextObserveResponse struct {
    TimestampMS  int64
    DataSourceID string
    Key          string
    Text         string
}
```













## Development of databox was supported by the following funding
```
EP/N028260/1, Databox: Privacy-Aware Infrastructure for Managing Personal Data
EP/N028260/2, Databox: Privacy-Aware Infrastructure for Managing Personal Data
EP/N014243/1, Future Everyday Interaction with the Autonomous Internet of Things
EP/M001636/1, Privacy-by-Design: Building Accountability into the Internet of Things EP/M02315X/1, From Human Data to Personal Experience
```
