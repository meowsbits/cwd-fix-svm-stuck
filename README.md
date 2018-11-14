### @whilei notes and latest states:

- [http://github.com/whilei/go-ethereum/tree/dev/evm-svm](http://github.com/whilei/go-ethereum/tree/dev/evm-svm)

```shell
$ cd go-ethereum
$ ./scripts/build_sputnikvm_evmcmd.sh build
$ ./bin/evm --input "0x1801fbe5449b1eadae23ffcdfc9645f51ece20000000000082897a5f7a919911ea5e8e670000000023ed8020b9500db61414bc6600987457c706f604c556bf557459abee" --sputnikvm
thread '<unnamed>' panicked at 'arithmetic operation overflow', /home/ia/.cargo/registry/src/github.com-1ecc6299db9ec823/etcommon-bigint-0.2.9/src/uint/mod.rs:1184:1
note: Run with `RUST_BACKTRACE=1` for a backtrace.
fatal runtime error: failed to initiate panic, error 5
SIGABRT: abort
PC=0x7f70ac553e97 m=0 sigcode=18446744073709551610

goroutine 0 [idle]:
runtime: unknown pc 0x7f70ac553e97
stack: frame={sp:0x7ffec14868d0, fp:0x0} stack=[0x7ffec0c88888,0x7ffec14878b0)
00007ffec14867d0:  0000000000bb28e8  00000000008d70c7
00007ffec14867e0:  0000000000000000  0011000000b2f2cc
00007ffec14867f0:  0000000000000000  0000000000000020
00007ffec1486800:  00007ffec14867ec  00007ffec14867eb
00007ffec1486810:  00007ffec1486828  00000000008c60d7
```

----
