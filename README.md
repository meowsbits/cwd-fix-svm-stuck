### summary

I've got 3 cases where between us we've caught SVM behaving "stuckly":
1. http://gastracker.io/block/6863285 (https://github.com/ethereumproject/go-ethereum/issues/689#issue-379017427)
2. http://gastracker.io/block/6921899 (https://github.com/ethereumproject/go-ethereum/issues/689#issuecomment-438274063)
3. http://gastracker.io/block/6923501 (https://github.com/ethereumproject/go-ethereum/issues/689#issuecomment-438317511)

__note__ that the blocks linked above are the _subsequent_ blocks to reported numbers, since I expect that if an import succeeds the block is not problematic, so we should be looking at +1 above the latest-viable block when the client get stuck. This means that gastracker blocks linked above are the blocks that geth refused to import.

1. http://gastracker.io/tx/0x92b9fec752b25b20d4b03cb22b4732e99e21f7a98ed21880f2b834851209f155
2. http://gastracker.io/tx/0x49783be170167e1443f8e27f68a1dee920ff3dc88dabb77eb841b18e38afca05
3. http://gastracker.io/tx/0x9ff1d07e93f589c9731f1eda16bdb2a3e447e65df9afd9d7ea2863900b0c3cd8

```
$ echo -n "0x1801fbe59dcf188896879af2cdd88d9692e37c0000000000cd1f836eba8aa0d327155d7c000000000a829edf026d49764d7005d45fca6e6e66ede31bf4cd87a5f93dc1a6" | evmasm -d
00000000: XOR
00000001: ADD
00000002: INVALID
00000003: INVALID
00000004: SWAP14
00000005: INVALID
00000006: XOR
00000007: DUP9
00000008: SWAP7
00000009: DUP8
0000000a: SWAP11
0000000b: CALLCODE
0000000c: INVALID
0000000d: INVALID
0000000e: DUP14
0000000f: SWAP7
00000010: SWAP3
00000011: INVALID
00000012: PUSH29 0xcd1f836eba8aa0d327155d7c000000000a829edf026d4976
00000030: INVALID
00000031: PUSH17 0x5d45fca6e6e66ede31bf4cd87a5f93dc1
00000043: INVALID
```

Exported RLP/binary-encoded block for -1..+1 of blocks in question are in [./exported-blocks](./exported-blocks).

Example number `2` failed import happened for me around 20181113-09:47 (for reference looking thru my logs).

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

[...]

00007fff80bfbfd0:  ffffffffffffffff  ffffffffffffffff
00007fff80bfbfe0:  ffffffffffffffff  ffffffffffffffff
00007fff80bfbff0:  ffffffffffffffff  ffffffffffffffff
00007fff80bfc000:  ffffffffffffffff  ffffffffffffffff
00007fff80bfc010:  ffffffffffffffff  ffffffffffffffff
runtime: unknown pc 0x7f2da65e9e97
stack: frame={sp:0x7fff80bfbf20, fp:0x0} stack=[0x7fff803fe078,0x7fff80bfd0a0)
00007fff80bfbe20:  0000000000bb3008  00000000008d7637
00007fff80bfbe30:  0000000000000000  0011000000bc52cc
00007fff80bfbe40:  0000000000000000  0000000000000020
00007fff80bfbe50:  00007fff80bfbe3c  00007fff80bfbe3b
00007fff80bfbe60:  0000000001115c48  0000000000000001
00007fff80bfbe70:  0000000000bb3008  0000000000000001
00007fff80bfbe80:  00007fff80bfc1e8  0000000000000001

```

----
