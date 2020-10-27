# Benchmarks of Go serialization methods

[![Gitter chat](https://badges.gitter.im/alecthomas.png)](https://gitter.im/alecthomas/Lobby)

This is a test suite for benchmarking various Go serialization methods.

## Tested serialization methods

- [encoding/gob](http://golang.org/pkg/encoding/gob/)
- [encoding/json](http://golang.org/pkg/encoding/json/)
- [github.com/json-iterator/go](https://github.com/json-iterator/go)
- [github.com/alecthomas/binary](https://github.com/alecthomas/binary)
- [github.com/davecgh/go-xdr/xdr](https://github.com/davecgh/go-xdr)
- [github.com/Sereal/Sereal/Go/sereal](https://github.com/Sereal/Sereal)
- [github.com/ugorji/go/codec](https://github.com/ugorji/go/tree/master/codec)
- [github.com/vmihailenco/msgpack/v4](https://github.com/vmihailenco/msgpack)
- [labix.org/v2/mgo/bson](https://labix.org/v2/mgo/bson)
- [github.com/tinylib/msgp](https://github.com/tinylib/msgp) *(code generator for msgpack)*
- [github.com/golang/protobuf](https://github.com/golang/protobuf) (generated code)
- [github.com/gogo/protobuf](https://github.com/gogo/protobuf) (generated code, optimized version of `goprotobuf`)
- [go.dedis.ch/protobuf](https://go.dedis.ch/protobuf) (reflection based)
- [github.com/google/flatbuffers](https://github.com/google/flatbuffers)
- [github.com/hprose/hprose-go/io](https://github.com/hprose/hprose-go)
- [github.com/glycerine/go-capnproto](https://github.com/glycerine/go-capnproto)
- [zombiezen.com/go/capnproto2](https://godoc.org/zombiezen.com/go/capnproto2)
- [github.com/andyleap/gencode](https://github.com/andyleap/gencode)
- [github.com/pascaldekloe/colfer](https://github.com/pascaldekloe/colfer)
- [github.com/linkedin/goavro](https://github.com/linkedin/goavro)
- [github.com/ikkerens/ikeapack](https://github.com/ikkerens/ikeapack)
- [github.com/niubaoshu/gotiny](https://github.com/niubaoshu/gotiny)
- [github.com/prysmaticlabs/go-ssz](https://github.com/prysmaticlabs/go-ssz)
- [github.com/fxamacker/cbor/v2](https://github.com/fxamacker/cbor)

## Running the benchmarks

```bash
go get -u -t
go test -bench='.*' ./
```

To update the table in the README:

```bash
./stats.sh
```

## Recommendation

If performance, correctness and interoperability are the most
important factors, [gogoprotobuf](https://gogo.github.io/) is
currently the best choice. It does require a pre-processing step (eg.
via Go 1.4's "go generate" command).

But as always, make your own choice based on your requirements.

## Data

The data being serialized is the following structure with randomly generated values:

```go
type A struct {
    Name     string
    BirthDay time.Time
    Phone    string
    Siblings int
    Spouse   bool
    Money    float64
}
```


## Results

|            | Info                                     |
|------------|------------------------------------------|
| Date       | 2020-10-27                               |
| Go version | 1.15                                     |
| CPU        | Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz |
| RAM        | DDR4-3200 64GB                           |
| OS         | Ubuntu 20.04.1 LTS                       |

---

| benchmark                                     | iter     | time/iter  | bytes/op | allocs/op | tt.sec | tt.kb  | ns/alloc |
|-----------------------------------------------|----------|------------|----------|-----------|--------|--------|----------|
| BenchmarkGotinyMarshal-8                      | 16398382 | 72 ns/op   | 48       | 0         | 1.19   | 78712  | 0.00     |
| BenchmarkGotinyUnmarshal-8                    | 7148515  | 165 ns/op  | 48       | 112       | 1.18   | 34312  | 1.47     |
| BenchmarkGotinyNoTimeMarshal-8                | 16351544 | 74 ns/op   | 48       | 0         | 1.22   | 78487  | 0.00     |
| BenchmarkGotinyNoTimeUnmarshal-8              | 7867641  | 153 ns/op  | 48       | 96        | 1.20   | 37764  | 1.59     |
| BenchmarkMsgpMarshal-8                        | 10502289 | 111 ns/op  | 97       | 128       | 1.17   | 101872 | 0.87     |
| BenchmarkMsgpUnmarshal-8                      | 6063904  | 238 ns/op  | 97       | 112       | 1.44   | 58819  | 2.12     |
| BenchmarkVmihailencoMsgpackMarshal-8          | 1655041  | 692 ns/op  | 100      | 288       | 1.15   | 16550  | 2.40     |
| BenchmarkVmihailencoMsgpackUnmarshal-8        | 1505500  | 793 ns/op  | 100      | 160       | 1.19   | 15055  | 4.96     |
| BenchmarkJsonMarshal-8                        | 588980   | 1920 ns/op | 152      | 944       | 1.13   | 8952   | 2.03     |
| BenchmarkJsonUnmarshal-8                      | 598700   | 1986 ns/op | 152      | 344       | 1.19   | 9100   | 5.77     |
| BenchmarkJsonIterMarshal-8                    | 1000000  | 1028 ns/op | 152      | 952       | 1.03   | 15200  | 1.08     |
| BenchmarkJsonIterUnmarshal-8                  | 619207   | 1739 ns/op | 152      | 447       | 1.08   | 9411   | 3.89     |
| BenchmarkEasyJsonMarshal-8                    | 1288803  | 931 ns/op  | 152      | 784       | 1.20   | 19589  | 1.19     |
| BenchmarkEasyJsonUnmarshal-8                  | 1344949  | 891 ns/op  | 152      | 160       | 1.20   | 20443  | 5.57     |
| BenchmarkBsonMarshal-8                        | 1593228  | 886 ns/op  | 110      | 392       | 1.41   | 17525  | 2.26     |
| BenchmarkBsonUnmarshal-8                      | 1116942  | 1066 ns/op | 110      | 232       | 1.19   | 12286  | 4.59     |
| BenchmarkGobMarshal-8                         | 2735154  | 442 ns/op  | 63       | 48        | 1.21   | 17395  | 9.21     |
| BenchmarkGobUnmarshal-8                       | 2687490  | 448 ns/op  | 63       | 112       | 1.20   | 17092  | 4.00     |
| BenchmarkXDRMarshal-8                         | 1000000  | 1172 ns/op | 92       | 455       | 1.17   | 9200   | 2.58     |
| BenchmarkXDRUnmarshal-8                       | 1269794  | 899 ns/op  | 92       | 240       | 1.14   | 11682  | 3.75     |
| BenchmarkUgorjiCodecMsgpackMarshal-8          | 1604208  | 764 ns/op  | 91       | 1280      | 1.23   | 14598  | 0.60     |
| BenchmarkUgorjiCodecMsgpackUnmarshal-8        | 1501878  | 767 ns/op  | 91       | 480       | 1.15   | 13667  | 1.60     |
| BenchmarkUgorjiCodecBincMarshal-8             | 1570681  | 811 ns/op  | 95       | 1312      | 1.27   | 14921  | 0.62     |
| BenchmarkUgorjiCodecBincUnmarshal-8           | 1433302  | 835 ns/op  | 95       | 640       | 1.20   | 13616  | 1.30     |
| BenchmarkUgorjiCodecCborMarshal-8             | 1472666  | 738 ns/op  | 92       | 1280      | 1.09   | 13548  | 0.58     |
| BenchmarkUgorjiCodecCborUnmarshal-8           | 1517166  | 824 ns/op  | 92       | 480       | 1.25   | 13957  | 1.72     |
| BenchmarkSerealMarshal-8                      | 722277   | 1617 ns/op | 132      | 904       | 1.17   | 9534   | 1.79     |
| BenchmarkSerealUnmarshal-8                    | 599400   | 1945 ns/op | 132      | 1008      | 1.17   | 7912   | 1.93     |
| BenchmarkBinaryMarshal-8                      | 1442703  | 841 ns/op  | 61       | 320       | 1.21   | 8800   | 2.63     |
| BenchmarkBinaryUnmarshal-8                    | 1372986  | 883 ns/op  | 61       | 320       | 1.21   | 8375   | 2.76     |
| BenchmarkFlatBuffersMarshal-8                 | 6209752  | 189 ns/op  | 95       | 0         | 1.17   | 59054  | 0.00     |
| BenchmarkFlatBuffersUnmarshal-8               | 7514677  | 157 ns/op  | 95       | 112       | 1.18   | 71614  | 1.40     |
| BenchmarkCapNProtoMarshal-8                   | 4738880  | 252 ns/op  | 96       | 56        | 1.19   | 45493  | 4.50     |
| BenchmarkCapNProtoUnmarshal-8                 | 4474824  | 281 ns/op  | 96       | 200       | 1.26   | 42958  | 1.41     |
| BenchmarkCapNProto2Marshal-8                  | 2821168  | 403 ns/op  | 96       | 244       | 1.14   | 27083  | 1.65     |
| BenchmarkCapNProto2Unmarshal-8                | 2237100  | 578 ns/op  | 96       | 320       | 1.29   | 21476  | 1.81     |
| BenchmarkHproseMarshal-8                      | 2157840  | 537 ns/op  | 85       | 456       | 1.16   | 18406  | 1.18     |
| BenchmarkHproseUnmarshal-8                    | 1709163  | 701 ns/op  | 85       | 319       | 1.20   | 14562  | 2.20     |
| BenchmarkHprose2Marshal-8                     | 3522960  | 342 ns/op  | 85       | 0         | 1.20   | 30050  | 0.00     |
| BenchmarkHprose2Unmarshal-8                   | 3188289  | 401 ns/op  | 85       | 144       | 1.28   | 27196  | 2.78     |
| BenchmarkProtobufMarshal-8                    | 2397105  | 470 ns/op  | 52       | 152       | 1.13   | 12464  | 3.09     |
| BenchmarkProtobufUnmarshal-8                  | 1554627  | 820 ns/op  | 52       | 280       | 1.27   | 8084   | 2.93     |
| BenchmarkGoprotobufMarshal-8                  | 5486998  | 210 ns/op  | 51       | 64        | 1.15   | 28312  | 3.28     |
| BenchmarkGoprotobufUnmarshal-8                | 5195605  | 244 ns/op  | 51       | 128       | 1.27   | 26757  | 1.91     |
| BenchmarkGogoprotobufMarshal-8                | 9518772  | 117 ns/op  | 51       | 64        | 1.11   | 49116  | 1.83     |
| BenchmarkGogoprotobufUnmarshal-8              | 7351138  | 162 ns/op  | 51       | 96        | 1.19   | 37931  | 1.69     |
| BenchmarkColferMarshal-8                      | 12991268 | 96 ns/op   | 51       | 64        | 1.25   | 66255  | 1.51     |
| BenchmarkColferUnmarshal-8                    | 7956777  | 136 ns/op  | 52       | 112       | 1.08   | 41375  | 1.21     |
| BenchmarkGencodeMarshal-8                     | 9259624  | 113 ns/op  | 53       | 80        | 1.05   | 49076  | 1.41     |
| BenchmarkGencodeUnmarshal-8                   | 8538992  | 149 ns/op  | 53       | 112       | 1.27   | 45256  | 1.33     |
| BenchmarkGencodeUnsafeMarshal-8               | 15096765 | 78 ns/op   | 46       | 48        | 1.19   | 69445  | 1.64     |
| BenchmarkGencodeUnsafeUnmarshal-8             | 9905109  | 118 ns/op  | 46       | 96        | 1.17   | 45563  | 1.23     |
| BenchmarkXDR2Marshal-8                        | 8939934  | 113 ns/op  | 60       | 64        | 1.01   | 53639  | 1.77     |
| BenchmarkXDR2Unmarshal-8                      | 14241745 | 84 ns/op   | 60       | 32        | 1.20   | 85450  | 2.64     |
| BenchmarkGoAvroMarshal-8                      | 636625   | 2068 ns/op | 47       | 1008      | 1.32   | 2992   | 2.05     |
| BenchmarkGoAvroUnmarshal-8                    | 219256   | 4991 ns/op | 47       | 3328      | 1.09   | 1030   | 1.50     |
| BenchmarkGoAvro2TextMarshal-8                 | 513855   | 1979 ns/op | 134      | 1320      | 1.02   | 6885   | 1.50     |
| BenchmarkGoAvro2TextUnmarshal-8               | 617684   | 1759 ns/op | 134      | 799       | 1.09   | 8276   | 2.20     |
| BenchmarkGoAvro2BinaryMarshal-8               | 2147502  | 567 ns/op  | 47       | 488       | 1.22   | 10093  | 1.16     |
| BenchmarkGoAvro2BinaryUnmarshal-8             | 1962423  | 616 ns/op  | 47       | 560       | 1.21   | 9223   | 1.10     |
| BenchmarkIkeaMarshal-8                        | 2989714  | 399 ns/op  | 55       | 72        | 1.19   | 16443  | 5.54     |
| BenchmarkIkeaUnmarshal-8                      | 2361486  | 491 ns/op  | 55       | 160       | 1.16   | 12988  | 3.07     |
| BenchmarkShamatonMapMsgpackMarshal-8          | 2326257  | 563 ns/op  | 92       | 208       | 1.31   | 21401  | 2.71     |
| BenchmarkShamatonMapMsgpackUnmarshal-8        | 2708852  | 460 ns/op  | 92       | 144       | 1.25   | 24921  | 3.19     |
| BenchmarkShamatonArrayMsgpackMarshal-8        | 2610681  | 461 ns/op  | 50       | 176       | 1.20   | 13053  | 2.62     |
| BenchmarkShamatonArrayMsgpackUnmarshal-8      | 3705327  | 334 ns/op  | 50       | 144       | 1.24   | 18526  | 2.32     |
| BenchmarkSSZNoTimeNoStringNoFloatAMarshal-8   | 370837   | 2793 ns/op | 55       | 440       | 1.04   | 2039   | 6.35     |
| BenchmarkSSZNoTimeNoStringNoFloatAUnmarshal-8 | 228956   | 5381 ns/op | 55       | 1504      | 1.23   | 1259   | 3.58     |
| BenchmarkFxamackerCborV2Marshal-8             | 3050332  | 398 ns/op  | 87       | 136       | 1.21   | 26537  | 2.93     |
| BenchmarkFxamackerCborV2Unmarshal-8           | 1879719  | 627 ns/op  | 87       | 144       | 1.18   | 16353  | 4.35     |


Totals:


| benchmark                            | iter     | time/iter  | bytes/op | allocs/op | tt.sec | tt.kb  | ns/alloc |
|--------------------------------------|----------|------------|----------|-----------|--------|--------|----------|
| BenchmarkGencodeUnsafe-8             | 25001874 | 196 ns/op  | 92       | 144       | 4.92   | 230017 | 1.37     |
| BenchmarkXDR2-8                      | 23181679 | 197 ns/op  | 120      | 96        | 4.58   | 278180 | 2.06     |
| BenchmarkGotinyNoTime-8              | 24219185 | 227 ns/op  | 96       | 96        | 5.51   | 232504 | 2.37     |
| BenchmarkColfer-8                    | 20948045 | 232 ns/op  | 103      | 176       | 4.87   | 215764 | 1.32     |
| BenchmarkGotiny-8                    | 23546897 | 237 ns/op  | 96       | 112       | 5.60   | 226050 | 2.12     |
| BenchmarkGencode-8                   | 17798616 | 262 ns/op  | 106      | 192       | 4.66   | 188665 | 1.36     |
| BenchmarkGogoprotobuf-8              | 16869910 | 279 ns/op  | 103      | 160       | 4.71   | 174097 | 1.74     |
| BenchmarkFlatBuffers-8               | 13724429 | 346 ns/op  | 190      | 112       | 4.75   | 261313 | 3.09     |
| BenchmarkMsgp-8                      | 16566193 | 349 ns/op  | 194      | 240       | 5.78   | 321384 | 1.45     |
| BenchmarkGoprotobuf-8                | 10682603 | 454 ns/op  | 103      | 192       | 4.85   | 110137 | 2.36     |
| BenchmarkCapNProto-8                 | 9213704  | 533 ns/op  | 192      | 256       | 4.91   | 176903 | 2.08     |
| BenchmarkHprose2-8                   | 6711249  | 743 ns/op  | 170      | 144       | 4.99   | 114493 | 5.16     |
| BenchmarkShamatonArrayMsgpack-8      | 6316008  | 795 ns/op  | 100      | 320       | 5.02   | 63160  | 2.48     |
| BenchmarkGob-8                       | 5422644  | 890 ns/op  | 127      | 160       | 4.83   | 68976  | 5.56     |
| BenchmarkCapNProto2-8                | 5058268  | 981 ns/op  | 192      | 564       | 4.96   | 97118  | 1.74     |
| BenchmarkShamatonMapMsgpack-8        | 5035109  | 1023 ns/op | 184      | 352       | 5.15   | 92646  | 2.91     |
| BenchmarkFxamackerCborV2-8           | 4930051  | 1025 ns/op | 174      | 280       | 5.05   | 85782  | 3.66     |
| BenchmarkGoAvro2Binary-8             | 4109925  | 1183 ns/op | 94       | 1048      | 4.86   | 38633  | 1.13     |
| BenchmarkHprose-8                    | 3867003  | 1238 ns/op | 170      | 775       | 4.79   | 65932  | 1.60     |
| BenchmarkProtobuf-8                  | 3951732  | 1290 ns/op | 104      | 432       | 5.10   | 41098  | 2.99     |
| BenchmarkVmihailencoMsgpack-8        | 3160541  | 1485 ns/op | 200      | 448       | 4.69   | 63210  | 3.31     |
| BenchmarkUgorjiCodecMsgpack-8        | 3106086  | 1531 ns/op | 182      | 1760      | 4.76   | 56530  | 0.87     |
| BenchmarkUgorjiCodecCbor-8           | 2989832  | 1562 ns/op | 184      | 1760      | 4.67   | 55012  | 0.89     |
| BenchmarkUgorjiCodecBinc-8           | 3003983  | 1646 ns/op | 190      | 1952      | 4.94   | 57075  | 0.84     |
| BenchmarkBinary-8                    | 2815689  | 1724 ns/op | 122      | 640       | 4.85   | 34351  | 2.69     |
| BenchmarkEasyJson-8                  | 2633752  | 1822 ns/op | 304      | 944       | 4.80   | 80066  | 1.93     |
| BenchmarkBson-8                      | 2710170  | 1952 ns/op | 220      | 624       | 5.29   | 59623  | 3.13     |
| BenchmarkXDR-8                       | 2269794  | 2071 ns/op | 184      | 695       | 4.70   | 41764  | 2.98     |
| BenchmarkJsonIter-8                  | 1619207  | 2767 ns/op | 304      | 1399      | 4.48   | 49223  | 1.98     |
| BenchmarkSereal-8                    | 1321677  | 3562 ns/op | 264      | 1912      | 4.71   | 34892  | 1.86     |
| BenchmarkGoAvro2Text-8               | 1131539  | 3738 ns/op | 268      | 2119      | 4.23   | 30325  | 1.76     |
| BenchmarkJson-8                      | 1187680  | 3906 ns/op | 304      | 1288      | 4.64   | 36105  | 3.03     |
| BenchmarkGoAvro-8                    | 855881   | 7059 ns/op | 94       | 4336      | 6.04   | 8045   | 1.63     |
| BenchmarkSSZNoTimeNoStringNoFloatA-8 | 599793   | 8174 ns/op | 110      | 1944      | 4.90   | 6597   | 4.20     |




## Issues


The benchmarks can also be run with validation enabled.

```bash
VALIDATE=1 go test -bench='.*' ./
```

Unfortunately, several of the serializers exhibit issues:

1. **(minor)** BSON drops sub-microsecond precision from `time.Time`.
3. **(minor)** Ugorji Binc Codec drops the timezone name (eg. "EST" -> "-0500") from `time.Time`.

```
--- FAIL: BenchmarkBsonUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20b999e3621bd773 2016-01-19 14:05:02.469416459 -0800 PST f017c8e9de 4 true 0.20887343719329818}
        &{20b999e3621bd773 2016-01-19 14:05:02.469 -0800 PST f017c8e9de 4 true 0.20887343719329818}
--- FAIL: BenchmarkUgorjiCodecBincUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 PST 71f3bf4233 0 false 0.8712180830484527}
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 -0800 71f3bf4233 0 false 0.8712180830484527}
```

All other fields are correct however.

Additionally, while not a correctness issue, FlatBuffers, ProtoBuffers, Cap'N'Proto and ikeapack do not
support time types directly. In the benchmarks an int64 value is used to hold a UnixNano timestamp.
