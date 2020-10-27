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
go test -bench='.*' -benchtime 10s -timeout=30m ./
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

| benchmark                                     | iter      | time/iter  | bytes/op | allocs/op | tt.sec | tt.kb  | ns/alloc |
|-----------------------------------------------|-----------|------------|----------|-----------|--------|--------|----------|
| BenchmarkGotinyMarshal-8                      | 162724083 | 72 ns/op   | 48       | 0         | 11.72  | 781075 | 0.00     |
| BenchmarkGotinyUnmarshal-8                    | 64303480  | 246 ns/op  | 48       | 112       | 15.82  | 308656 | 2.20     |
| BenchmarkGotinyNoTimeMarshal-8                | 162918541 | 72 ns/op   | 48       | 0         | 11.75  | 782008 | 0.00     |
| BenchmarkGotinyNoTimeUnmarshal-8              | 77745463  | 189 ns/op  | 48       | 96        | 14.69  | 373178 | 1.97     |
| BenchmarkMsgpMarshal-8                        | 99676527  | 147 ns/op  | 97       | 128       | 14.65  | 966862 | 1.15     |
| BenchmarkMsgpUnmarshal-8                      | 59501658  | 236 ns/op  | 97       | 112       | 14.04  | 577166 | 2.11     |
| BenchmarkVmihailencoMsgpackMarshal-8          | 18947983  | 675 ns/op  | 100      | 288       | 12.79  | 189479 | 2.34     |
| BenchmarkVmihailencoMsgpackUnmarshal-8        | 15257431  | 778 ns/op  | 100      | 160       | 11.87  | 152574 | 4.86     |
| BenchmarkJsonMarshal-8                        | 6239857   | 2282 ns/op | 152      | 944       | 14.24  | 94845  | 2.42     |
| BenchmarkJsonUnmarshal-8                      | 5952147   | 2003 ns/op | 152      | 344       | 11.92  | 90472  | 5.82     |
| BenchmarkJsonIterMarshal-8                    | 11390202  | 1217 ns/op | 152      | 952       | 13.86  | 173131 | 1.28     |
| BenchmarkJsonIterUnmarshal-8                  | 6453584   | 1783 ns/op | 152      | 447       | 11.51  | 98094  | 3.99     |
| BenchmarkEasyJsonMarshal-8                    | 12975735  | 925 ns/op  | 152      | 784       | 12.00  | 197231 | 1.18     |
| BenchmarkEasyJsonUnmarshal-8                  | 12719482  | 898 ns/op  | 152      | 160       | 11.42  | 193336 | 5.61     |
| BenchmarkBsonMarshal-8                        | 14800609  | 796 ns/op  | 110      | 392       | 11.78  | 162806 | 2.03     |
| BenchmarkBsonUnmarshal-8                      | 11019955  | 1080 ns/op | 110      | 232       | 11.90  | 121219 | 4.66     |
| BenchmarkGobMarshal-8                         | 27284218  | 440 ns/op  | 63       | 48        | 12.01  | 173254 | 9.17     |
| BenchmarkGobUnmarshal-8                       | 26840270  | 454 ns/op  | 63       | 112       | 12.19  | 170704 | 4.05     |
| BenchmarkXDRMarshal-8                         | 10440928  | 1129 ns/op | 92       | 455       | 11.79  | 96056  | 2.48     |
| BenchmarkXDRUnmarshal-8                       | 12506253  | 905 ns/op  | 92       | 239       | 11.32  | 115057 | 3.79     |
| BenchmarkUgorjiCodecMsgpackMarshal-8          | 16552945  | 765 ns/op  | 91       | 1280      | 12.66  | 150631 | 0.60     |
| BenchmarkUgorjiCodecMsgpackUnmarshal-8        | 14956684  | 774 ns/op  | 91       | 480       | 11.58  | 136105 | 1.61     |
| BenchmarkUgorjiCodecBincMarshal-8             | 15601579  | 785 ns/op  | 95       | 1312      | 12.25  | 148215 | 0.60     |
| BenchmarkUgorjiCodecBincUnmarshal-8           | 13226503  | 909 ns/op  | 95       | 640       | 12.02  | 125651 | 1.42     |
| BenchmarkUgorjiCodecCborMarshal-8             | 15388166  | 764 ns/op  | 92       | 1280      | 11.76  | 141571 | 0.60     |
| BenchmarkUgorjiCodecCborUnmarshal-8           | 15418566  | 808 ns/op  | 92       | 480       | 12.46  | 141850 | 1.68     |
| BenchmarkSerealMarshal-8                      | 7263681   | 1662 ns/op | 132      | 904       | 12.07  | 95880  | 1.84     |
| BenchmarkSerealUnmarshal-8                    | 6092763   | 2058 ns/op | 132      | 1008      | 12.54  | 80424  | 2.04     |
| BenchmarkBinaryMarshal-8                      | 13385620  | 867 ns/op  | 61       | 320       | 11.61  | 81652  | 2.71     |
| BenchmarkBinaryUnmarshal-8                    | 13552498  | 909 ns/op  | 61       | 320       | 12.32  | 82670  | 2.84     |
| BenchmarkFlatBuffersMarshal-8                 | 62664284  | 189 ns/op  | 95       | 0         | 11.84  | 596563 | 0.00     |
| BenchmarkFlatBuffersUnmarshal-8               | 75126547  | 178 ns/op  | 95       | 112       | 13.37  | 715204 | 1.59     |
| BenchmarkCapNProtoMarshal-8                   | 46792773  | 256 ns/op  | 96       | 56        | 11.98  | 449210 | 4.57     |
| BenchmarkCapNProtoUnmarshal-8                 | 39838784  | 339 ns/op  | 96       | 200       | 13.51  | 382452 | 1.70     |
| BenchmarkCapNProto2Marshal-8                  | 23700416  | 463 ns/op  | 96       | 244       | 10.97  | 227523 | 1.90     |
| BenchmarkCapNProto2Unmarshal-8                | 23588811  | 612 ns/op  | 96       | 320       | 14.44  | 226452 | 1.91     |
| BenchmarkHproseMarshal-8                      | 21301963  | 537 ns/op  | 85       | 399       | 11.44  | 181705 | 1.35     |
| BenchmarkHproseUnmarshal-8                    | 16072033  | 765 ns/op  | 85       | 320       | 12.30  | 137094 | 2.39     |
| BenchmarkHprose2Marshal-8                     | 34698306  | 341 ns/op  | 85       | 0         | 11.83  | 295976 | 0.00     |
| BenchmarkHprose2Unmarshal-8                   | 32138416  | 395 ns/op  | 85       | 144       | 12.69  | 274140 | 2.74     |
| BenchmarkProtobufMarshal-8                    | 25571618  | 481 ns/op  | 52       | 152       | 12.30  | 132972 | 3.16     |
| BenchmarkProtobufUnmarshal-8                  | 16019134  | 779 ns/op  | 52       | 280       | 12.48  | 83299  | 2.78     |
| BenchmarkGoprotobufMarshal-8                  | 50307430  | 224 ns/op  | 51       | 64        | 11.27  | 259586 | 3.50     |
| BenchmarkGoprotobufUnmarshal-8                | 48275404  | 254 ns/op  | 51       | 128       | 12.26  | 249101 | 1.98     |
| BenchmarkGoprotobufV2Marshal-8                | 59812918  | 204 ns/op  | 51       | 64        | 12.20  | 308634 | 3.19     |
| BenchmarkGoprotobufV2Unmarshal-8              | 43479012  | 281 ns/op  | 51       | 128       | 12.22  | 224351 | 2.20     |
| BenchmarkGogoprotobufMarshal-8                | 96669610  | 110 ns/op  | 51       | 64        | 10.63  | 498815 | 1.72     |
| BenchmarkGogoprotobufUnmarshal-8              | 81296978  | 153 ns/op  | 51       | 96        | 12.44  | 420305 | 1.59     |
| BenchmarkColferMarshal-8                      | 139024765 | 85 ns/op   | 51       | 64        | 11.93  | 710416 | 1.34     |
| BenchmarkColferUnmarshal-8                    | 95451918  | 126 ns/op  | 50       | 112       | 12.03  | 477259 | 1.12     |
| BenchmarkGencodeMarshal-8                     | 100000000 | 107 ns/op  | 53       | 80        | 10.70  | 530000 | 1.34     |
| BenchmarkGencodeUnmarshal-8                   | 83866508  | 126 ns/op  | 53       | 112       | 10.57  | 444492 | 1.12     |
| BenchmarkGencodeUnsafeMarshal-8               | 173408394 | 68 ns/op   | 46       | 48        | 11.81  | 797678 | 1.42     |
| BenchmarkGencodeUnsafeUnmarshal-8             | 120620438 | 102 ns/op  | 46       | 96        | 12.30  | 554854 | 1.06     |
| BenchmarkXDR2Marshal-8                        | 100000000 | 113 ns/op  | 60       | 64        | 11.30  | 600000 | 1.77     |
| BenchmarkXDR2Unmarshal-8                      | 129857448 | 87 ns/op   | 60       | 32        | 11.39  | 779144 | 2.74     |
| BenchmarkGoAvroMarshal-8                      | 6006432   | 1966 ns/op | 47       | 1008      | 11.81  | 28230  | 1.95     |
| BenchmarkGoAvroUnmarshal-8                    | 2672388   | 4593 ns/op | 47       | 3328      | 12.27  | 12560  | 1.38     |
| BenchmarkGoAvro2TextMarshal-8                 | 6745646   | 1777 ns/op | 134      | 1320      | 11.99  | 90391  | 1.35     |
| BenchmarkGoAvro2TextUnmarshal-8               | 6277456   | 1842 ns/op | 134      | 799       | 11.56  | 84117  | 2.31     |
| BenchmarkGoAvro2BinaryMarshal-8               | 21281403  | 596 ns/op  | 47       | 488       | 12.68  | 100022 | 1.22     |
| BenchmarkGoAvro2BinaryUnmarshal-8             | 18464617  | 644 ns/op  | 47       | 560       | 11.89  | 86783  | 1.15     |
| BenchmarkIkeaMarshal-8                        | 30010225  | 398 ns/op  | 55       | 72        | 11.94  | 165056 | 5.53     |
| BenchmarkIkeaUnmarshal-8                      | 24478546  | 507 ns/op  | 55       | 160       | 12.41  | 134632 | 3.17     |
| BenchmarkShamatonMapMsgpackMarshal-8          | 22963233  | 539 ns/op  | 92       | 208       | 12.38  | 211261 | 2.59     |
| BenchmarkShamatonMapMsgpackUnmarshal-8        | 27437608  | 449 ns/op  | 92       | 144       | 12.32  | 252425 | 3.12     |
| BenchmarkShamatonArrayMsgpackMarshal-8        | 26737370  | 459 ns/op  | 50       | 176       | 12.27  | 133686 | 2.61     |
| BenchmarkShamatonArrayMsgpackUnmarshal-8      | 37370890  | 307 ns/op  | 50       | 144       | 11.47  | 186854 | 2.13     |
| BenchmarkSSZNoTimeNoStringNoFloatAMarshal-8   | 4288058   | 2786 ns/op | 55       | 440       | 11.95  | 23584  | 6.33     |
| BenchmarkSSZNoTimeNoStringNoFloatAUnmarshal-8 | 2229534   | 5286 ns/op | 55       | 1504      | 11.79  | 12262  | 3.51     |
| BenchmarkFxamackerCborV2Marshal-8             | 30920371  | 412 ns/op  | 87       | 136       | 12.74  | 269007 | 3.03     |
| BenchmarkFxamackerCborV2Unmarshal-8           | 19079839  | 638 ns/op  | 87       | 144       | 12.17  | 165994 | 4.43     |


Totals:


| benchmark                            | iter      | time/iter  | bytes/op | allocs/op | tt.sec | tt.kb   | ns/alloc |
|--------------------------------------|-----------|------------|----------|-----------|--------|---------|----------|
| BenchmarkGencodeUnsafe-8             | 294028832 | 170 ns/op  | 92       | 144       | 50.01  | 2705065 | 1.18     |
| BenchmarkXDR2-8                      | 229857448 | 200 ns/op  | 120      | 96        | 46.13  | 2758289 | 2.09     |
| BenchmarkColfer-8                    | 234476683 | 211 ns/op  | 101      | 176       | 49.66  | 2370559 | 1.20     |
| BenchmarkGencode-8                   | 183866508 | 233 ns/op  | 106      | 192       | 42.84  | 1948984 | 1.21     |
| BenchmarkGotinyNoTime-8              | 240664004 | 261 ns/op  | 96       | 96        | 62.84  | 2310374 | 2.72     |
| BenchmarkGogoprotobuf-8              | 177966588 | 263 ns/op  | 103      | 160       | 46.81  | 1838394 | 1.64     |
| BenchmarkGotiny-8                    | 227027563 | 318 ns/op  | 96       | 112       | 72.19  | 2179464 | 2.84     |
| BenchmarkFlatBuffers-8               | 137790831 | 367 ns/op  | 190      | 112       | 50.57  | 2623537 | 3.28     |
| BenchmarkMsgp-8                      | 159178185 | 383 ns/op  | 194      | 240       | 60.97  | 3088056 | 1.60     |
| BenchmarkGoprotobuf-8                | 98582834  | 478 ns/op  | 103      | 192       | 47.12  | 1017374 | 2.49     |
| BenchmarkGoprotobufV2-8              | 103291930 | 485 ns/op  | 103      | 192       | 50.10  | 1065972 | 2.53     |
| BenchmarkCapNProto-8                 | 86631557  | 595 ns/op  | 192      | 256       | 51.55  | 1663325 | 2.32     |
| BenchmarkHprose2-8                   | 66836722  | 736 ns/op  | 170      | 144       | 49.19  | 1140234 | 5.11     |
| BenchmarkShamatonArrayMsgpack-8      | 64108260  | 766 ns/op  | 100      | 320       | 49.11  | 641082  | 2.39     |
| BenchmarkGob-8                       | 54124488  | 894 ns/op  | 127      | 160       | 48.39  | 687922  | 5.59     |
| BenchmarkIkea-8                      | 54488771  | 905 ns/op  | 110      | 232       | 49.31  | 599376  | 3.90     |
| BenchmarkShamatonMapMsgpack-8        | 50400841  | 988 ns/op  | 184      | 352       | 49.80  | 927375  | 2.81     |
| BenchmarkFxamackerCborV2-8           | 50000210  | 1050 ns/op | 174      | 280       | 52.50  | 870003  | 3.75     |
| BenchmarkCapNProto2-8                | 47289227  | 1075 ns/op | 192      | 564       | 50.84  | 907953  | 1.91     |
| BenchmarkGoAvro2Binary-8             | 39746020  | 1240 ns/op | 94       | 1048      | 49.29  | 373612  | 1.18     |
| BenchmarkProtobuf-8                  | 41590752  | 1260 ns/op | 104      | 432       | 52.40  | 432543  | 2.92     |
| BenchmarkHprose-8                    | 37373996  | 1302 ns/op | 170      | 719       | 48.66  | 637600  | 1.81     |
| BenchmarkVmihailencoMsgpack-8        | 34205414  | 1453 ns/op | 200      | 448       | 49.70  | 684108  | 3.24     |
| BenchmarkUgorjiCodecMsgpack-8        | 31509629  | 1539 ns/op | 182      | 1760      | 48.49  | 573475  | 0.87     |
| BenchmarkUgorjiCodecCbor-8           | 30806732  | 1572 ns/op | 184      | 1760      | 48.43  | 566843  | 0.89     |
| BenchmarkUgorjiCodecBinc-8           | 28828082  | 1694 ns/op | 190      | 1952      | 48.83  | 547733  | 0.87     |
| BenchmarkBinary-8                    | 26938118  | 1776 ns/op | 122      | 640       | 47.84  | 328645  | 2.77     |
| BenchmarkEasyJson-8                  | 25695217  | 1823 ns/op | 304      | 944       | 46.84  | 781134  | 1.93     |
| BenchmarkBson-8                      | 25820564  | 1876 ns/op | 220      | 624       | 48.44  | 568052  | 3.01     |
| BenchmarkXDR-8                       | 22947181  | 2034 ns/op | 184      | 694       | 46.67  | 422228  | 2.93     |
| BenchmarkJsonIter-8                  | 17843786  | 3000 ns/op | 304      | 1399      | 53.53  | 542451  | 2.14     |
| BenchmarkGoAvro2Text-8               | 13023102  | 3619 ns/op | 268      | 2119      | 47.13  | 349019  | 1.71     |
| BenchmarkSereal-8                    | 13356444  | 3720 ns/op | 264      | 1912      | 49.69  | 352610  | 1.95     |
| BenchmarkJson-8                      | 12192004  | 4285 ns/op | 304      | 1288      | 52.24  | 370636  | 3.33     |
| BenchmarkGoAvro-8                    | 8678820   | 6559 ns/op | 94       | 4336      | 56.92  | 81580   | 1.51     |
| BenchmarkSSZNoTimeNoStringNoFloatA-8 | 6517592   | 8072 ns/op | 110      | 1944      | 52.61  | 71693   | 4.15     |




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
