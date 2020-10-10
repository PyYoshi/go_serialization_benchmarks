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

2020-10-10 Results with Go 1.15 darwin/amd64 on a 2.3 GHz Intel Core i9 32GB (MacBookPro16,1)

benchmark                                    | iter  | time/iter | bytes/op  |  allocs/op |tt.sec  | tt.kb        | ns/alloc
---------------------------------------------|-------|-----------|-----------|------------|--------|--------------|-----------
BenchmarkGotinyMarshal-16                |   15196443 |     84 ns/op |    48 |   0 |   1.29 |   72942 |    0.00
BenchmarkGotinyUnmarshal-16              |    6963772 |    170 ns/op |    48 | 112 |   1.18 |   33426 |    1.52
BenchmarkGotinyNoTimeMarshal-16          |   14422630 |     77 ns/op |    48 |   0 |   1.12 |   69228 |    0.00
BenchmarkGotinyNoTimeUnmarshal-16        |    7660746 |    160 ns/op |    48 |  96 |   1.23 |   36771 |    1.67
BenchmarkMsgpMarshal-16                  |    8718069 |    126 ns/op |    97 | 128 |   1.10 |   84565 |    0.98
BenchmarkMsgpUnmarshal-16                |    5635405 |    217 ns/op |    97 | 112 |   1.22 |   54663 |    1.94
BenchmarkVmihailencoMsgpackMarshal-16    |    1809714 |    651 ns/op |   100 | 288 |   1.18 |   18097 |    2.26
BenchmarkVmihailencoMsgpackUnmarshal-16  |    1414405 |    845 ns/op |   100 | 160 |   1.20 |   14144 |    5.28
BenchmarkJsonMarshal-16                  |     608379 |   2022 ns/op |   149 | 945 |   1.23 |    9064 |    2.14
BenchmarkJsonUnmarshal-16                |     562179 |   2129 ns/op |   149 | 343 |   1.20 |    8376 |    6.21
BenchmarkJsonIterMarshal-16              |    1000000 |   1152 ns/op |   149 | 952 |   1.15 |   14900 |    1.21
BenchmarkJsonIterUnmarshal-16            |     639820 |   1881 ns/op |   149 | 447 |   1.20 |    9533 |    4.21
BenchmarkEasyJsonMarshal-16              |    1271491 |    942 ns/op |   149 | 783 |   1.20 |   18945 |    1.20
BenchmarkEasyJsonUnmarshal-16            |    1272420 |    946 ns/op |   149 | 159 |   1.20 |   18959 |    5.95
BenchmarkBsonMarshal-16                  |    1547264 |    781 ns/op |   110 | 392 |   1.21 |   17019 |    1.99
BenchmarkBsonUnmarshal-16                |    1000000 |   1155 ns/op |   110 | 232 |   1.16 |   11000 |    4.98
BenchmarkGobMarshal-16                   |    2324043 |    519 ns/op |    63 |  48 |   1.21 |   14757 |   10.81
BenchmarkGobUnmarshal-16                 |    2311201 |    482 ns/op |    63 | 112 |   1.11 |   14699 |    4.30
BenchmarkXDRMarshal-16                   |    1000000 |   1100 ns/op |    88 | 392 |   1.10 |    8800 |    2.81
BenchmarkXDRUnmarshal-16                 |    1000000 |   1009 ns/op |    88 | 224 |   1.01 |    8800 |    4.50
BenchmarkUgorjiCodecMsgpackMarshal-16    |    1503207 |    818 ns/op |    91 | 1312 |   1.23 |   13679 |    0.62
BenchmarkUgorjiCodecMsgpackUnmarshal-16  |    1410549 |    818 ns/op |    91 | 496 |   1.15 |   12835 |    1.65
BenchmarkUgorjiCodecBincMarshal-16       |    1372832 |    901 ns/op |    95 | 1328 |   1.24 |   13041 |    0.68
BenchmarkUgorjiCodecBincUnmarshal-16     |    1000000 |   1009 ns/op |    95 | 656 |   1.01 |    9500 |    1.54
BenchmarkUgorjiCodecCborMarshal-16       |    1421166 |    874 ns/op |    92 | 1312 |   1.24 |   13074 |    0.67
BenchmarkUgorjiCodecCborUnmarshal-16     |    1327608 |    867 ns/op |    92 | 496 |   1.15 |   12213 |    1.75
BenchmarkSerealMarshal-16                |     688510 |   1839 ns/op |   132 | 904 |   1.27 |    9088 |    2.03
BenchmarkSerealUnmarshal-16              |     589640 |   2067 ns/op |   132 | 1008 |   1.22 |    7783 |    2.05
BenchmarkBinaryMarshal-16                |    1286991 |    914 ns/op |    61 | 320 |   1.18 |    7850 |    2.86
BenchmarkBinaryUnmarshal-16              |    1275280 |    951 ns/op |    61 | 320 |   1.21 |    7779 |    2.97
BenchmarkFlatBuffersMarshal-16           |    5876098 |    208 ns/op |    95 |   0 |   1.22 |   55999 |    0.00
BenchmarkFlatBuffersUnmarshal-16         |    6420705 |    177 ns/op |    95 | 112 |   1.14 |   61060 |    1.58
BenchmarkCapNProtoMarshal-16             |    4012570 |    285 ns/op |    96 |  56 |   1.14 |   38520 |    5.09
BenchmarkCapNProtoUnmarshal-16           |    3992001 |    306 ns/op |    96 | 200 |   1.22 |   38323 |    1.53
BenchmarkCapNProto2Marshal-16            |    2710383 |    434 ns/op |    96 | 244 |   1.18 |   26019 |    1.78
BenchmarkCapNProto2Unmarshal-16          |    2184651 |    551 ns/op |    96 | 320 |   1.20 |   20972 |    1.72
BenchmarkHproseMarshal-16                |    1837893 |    640 ns/op |    82 | 347 |   1.18 |   15125 |    1.84
BenchmarkHproseUnmarshal-16              |    1406054 |    792 ns/op |    82 | 320 |   1.11 |   11557 |    2.48
BenchmarkHprose2Marshal-16               |    3145710 |    382 ns/op |    82 |   0 |   1.20 |   25889 |    0.00
BenchmarkHprose2Unmarshal-16             |    2887740 |    410 ns/op |    82 | 144 |   1.18 |   23737 |    2.85
BenchmarkProtobufMarshal-16              |    2148622 |    525 ns/op |    52 | 152 |   1.13 |   11172 |    3.45
BenchmarkProtobufUnmarshal-16            |    1542219 |    779 ns/op |    52 | 280 |   1.20 |    8019 |    2.78
BenchmarkGoprotobufMarshal-16            |    2157760 |    551 ns/op |    53 |  96 |   1.19 |   11436 |    5.74
BenchmarkGoprotobufUnmarshal-16          |    2480798 |    493 ns/op |    53 | 184 |   1.22 |   13148 |    2.68
BenchmarkGogoprotobufMarshal-16          |   11662501 |    102 ns/op |    53 |  64 |   1.19 |   61811 |    1.59
BenchmarkGogoprotobufUnmarshal-16        |    8090194 |    149 ns/op |    53 |  96 |   1.21 |   42878 |    1.55
BenchmarkColferMarshal-16                |   14312360 |     85 ns/op |    51 |  64 |   1.22 |   73136 |    1.33
BenchmarkColferUnmarshal-16              |    8991006 |    130 ns/op |    50 | 112 |   1.17 |   44955 |    1.16
BenchmarkGencodeMarshal-16               |   10798407 |    110 ns/op |    53 |  80 |   1.19 |   57231 |    1.38
BenchmarkGencodeUnmarshal-16             |    9253441 |    132 ns/op |    53 | 112 |   1.22 |   49043 |    1.18
BenchmarkGencodeUnsafeMarshal-16         |   18050608 |     68 ns/op |    46 |  48 |   1.23 |   83032 |    1.42
BenchmarkGencodeUnsafeUnmarshal-16       |   11413479 |    104 ns/op |    46 |  96 |   1.19 |   52502 |    1.08
BenchmarkXDR2Marshal-16                  |   10443332 |    115 ns/op |    60 |  64 |   1.20 |   62659 |    1.80
BenchmarkXDR2Unmarshal-16                |   13763044 |     87 ns/op |    60 |  32 |   1.20 |   82578 |    2.73
BenchmarkGoAvroMarshal-16                |     645024 |   1917 ns/op |    47 | 1008 |   1.24 |    3031 |    1.90
BenchmarkGoAvroUnmarshal-16              |     265792 |   4584 ns/op |    47 | 3328 |   1.22 |    1249 |    1.38
BenchmarkGoAvro2TextMarshal-16           |     646215 |   1901 ns/op |   134 | 1320 |   1.23 |    8659 |    1.44
BenchmarkGoAvro2TextUnmarshal-16         |     647492 |   1945 ns/op |   134 | 799 |   1.26 |    8676 |    2.43
BenchmarkGoAvro2BinaryMarshal-16         |    1800801 |    689 ns/op |    47 | 488 |   1.24 |    8463 |    1.41
BenchmarkGoAvro2BinaryUnmarshal-16       |    1634749 |    693 ns/op |    47 | 560 |   1.13 |    7683 |    1.24
BenchmarkIkeaMarshal-16                  |    2741359 |    442 ns/op |    55 |  72 |   1.21 |   15077 |    6.14
BenchmarkIkeaUnmarshal-16                |    2169913 |    526 ns/op |    55 | 160 |   1.14 |   11934 |    3.29
BenchmarkShamatonMapMsgpackMarshal-16    |    2142940 |    556 ns/op |    92 | 208 |   1.19 |   19715 |    2.67
BenchmarkShamatonMapMsgpackUnmarshal-16  |    2517482 |    481 ns/op |    92 | 144 |   1.21 |   23160 |    3.34
BenchmarkShamatonArrayMsgpackMarshal-16  |    2415927 |    492 ns/op |    50 | 176 |   1.19 |   12079 |    2.80
BenchmarkShamatonArrayMsgpackUnmarshal-16 |    3723447 |    327 ns/op |    50 | 144 |   1.22 |   18617 |    2.27
BenchmarkSSZNoTimeNoStringNoFloatAMarshal-16 |     399007 |   2986 ns/op |    55 | 440 |   1.19 |    2194 |    6.79
BenchmarkSSZNoTimeNoStringNoFloatAUnmarshal-16 |     221811 |   5625 ns/op |    55 | 1504 |   1.25 |    1219 |    3.74
BenchmarkFxamackerCborV2Marshal-16       |    2743736 |    425 ns/op |    87 | 136 |   1.17 |   23870 |    3.12
BenchmarkFxamackerCborV2Unmarshal-16     |    1767961 |    671 ns/op |    87 | 144 |   1.19 |   15381 |    4.66


Totals:


benchmark                                    | iter  | time/iter | bytes/op  |  allocs/op |tt.sec  | tt.kb        | ns/alloc
---------------------------------------------|-------|-----------|-----------|------------|--------|--------------|-----------
BenchmarkGencodeUnsafe-16                |   29464087 |    172 ns/op |    92 | 144 |   5.07 |  271069 |    1.20
BenchmarkXDR2-16                         |   24206376 |    202 ns/op |   120 |  96 |   4.90 |  290476 |    2.11
BenchmarkColfer-16                       |   23303366 |    215 ns/op |   101 | 176 |   5.01 |  235597 |    1.22
BenchmarkGotinyNoTime-16                 |   22083376 |    237 ns/op |    96 |  96 |   5.24 |  212000 |    2.47
BenchmarkGencode-16                      |   20051848 |    242 ns/op |   106 | 192 |   4.85 |  212549 |    1.26
BenchmarkGogoprotobuf-16                 |   19752695 |    251 ns/op |   106 | 160 |   4.96 |  209378 |    1.57
BenchmarkGotiny-16                       |   22160215 |    254 ns/op |    96 | 112 |   5.65 |  212738 |    2.27
BenchmarkMsgp-16                         |   14353474 |    343 ns/op |   194 | 240 |   4.92 |  278457 |    1.43
BenchmarkFlatBuffers-16                  |   12296803 |    385 ns/op |   190 | 112 |   4.73 |  234131 |    3.44
BenchmarkCapNProto-16                    |    8004571 |    591 ns/op |   192 | 256 |   4.73 |  153687 |    2.31
BenchmarkHprose2-16                      |    6033450 |    792 ns/op |   164 | 144 |   4.78 |   99250 |    5.50
BenchmarkShamatonArrayMsgpack-16         |    6139374 |    819 ns/op |   100 | 320 |   5.03 |   61393 |    2.56
BenchmarkIkea-16                         |    4911272 |    968 ns/op |   110 | 232 |   4.75 |   54023 |    4.17
BenchmarkCapNProto2-16                   |    4895034 |    985 ns/op |   192 | 564 |   4.82 |   93984 |    1.75
BenchmarkGob-16                          |    4635244 |   1001 ns/op |   127 | 160 |   4.64 |   58913 |    6.26
BenchmarkShamatonMapMsgpack-16           |    4660422 |   1037 ns/op |   184 | 352 |   4.83 |   85751 |    2.95
BenchmarkGoprotobuf-16                   |    4638558 |   1044 ns/op |   106 | 280 |   4.84 |   49168 |    3.73
BenchmarkFxamackerCborV2-16              |    4511697 |   1096 ns/op |   174 | 280 |   4.94 |   78503 |    3.91
BenchmarkProtobuf-16                     |    3690841 |   1304 ns/op |   104 | 432 |   4.81 |   38384 |    3.02
BenchmarkGoAvro2Binary-16                |    3435550 |   1382 ns/op |    94 | 1048 |   4.75 |   32294 |    1.32
BenchmarkHprose-16                       |    3243947 |   1432 ns/op |   164 | 667 |   4.65 |   53362 |    2.15
BenchmarkVmihailencoMsgpack-16           |    3224119 |   1496 ns/op |   200 | 448 |   4.82 |   64482 |    3.34
BenchmarkUgorjiCodecMsgpack-16           |    2913756 |   1636 ns/op |   182 | 1808 |   4.77 |   53030 |    0.90
BenchmarkUgorjiCodecCbor-16              |    2748774 |   1741 ns/op |   184 | 1808 |   4.79 |   50577 |    0.96
BenchmarkBinary-16                       |    2562271 |   1865 ns/op |   122 | 640 |   4.78 |   31259 |    2.91
BenchmarkEasyJson-16                     |    2543911 |   1888 ns/op |   298 | 942 |   4.80 |   75808 |    2.00
BenchmarkUgorjiCodecBinc-16              |    2372832 |   1910 ns/op |   190 | 1984 |   4.53 |   45083 |    0.96
BenchmarkBson-16                         |    2547264 |   1936 ns/op |   220 | 624 |   4.93 |   56039 |    3.10
BenchmarkXDR-16                          |    2000000 |   2109 ns/op |   176 | 616 |   4.22 |   35200 |    3.42
BenchmarkJsonIter-16                     |    1639820 |   3033 ns/op |   298 | 1399 |   4.97 |   48866 |    2.17
BenchmarkGoAvro2Text-16                  |    1293707 |   3846 ns/op |   268 | 2119 |   4.98 |   34671 |    1.82
BenchmarkSereal-16                       |    1278150 |   3906 ns/op |   264 | 1912 |   4.99 |   33743 |    2.04
BenchmarkJson-16                         |    1170558 |   4151 ns/op |   298 | 1288 |   4.86 |   34882 |    3.22
BenchmarkGoAvro-16                       |     910816 |   6501 ns/op |    94 | 4336 |   5.92 |    8561 |    1.50
BenchmarkSSZNoTimeNoStringNoFloatA-16    |     620818 |   8611 ns/op |   110 | 1944 |   5.35 |    6828 |    4.43




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
