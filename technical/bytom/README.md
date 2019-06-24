# Bytom

## 我在比原的工作

+ bytom
    * 可以看看 release notes
        - 实现 set-mining-address & set-coinbase-arbitrary 等挖矿相关功能 
    * PoW 相关完善及优化
        + 优化tensority
            * go 920ms -> go 820ms -> simd 160ms -> cuda 6ms
        + get work 定时更新变为出块即更新，更加及时
    - [x] consume newBlockCh for vault_mode
        + 被马总解决了
    - block_recommit 交易数据及时入块
        + 其实就是用 map
        + ticker 定期生成 NewBlockTemplate，因为 新添交易的话 commitment 会改变，blockheader 也会改变，不用 map 的话，submit work不对应
        + 老 map 要注意 GC
    - 实现 stratumn miner，cpu 版本和 gpu 版本
    - 节点 solo mining 集成显卡挖矿算法
    - 全局交易索引
+ Precogs
    * 负责先知节点计划，进行比原网络中的节点发现和状态统计
    * 涉及 P2P
+ 中心化钱包
    * api & database schema
    * build tx
        - utxo >21 build fail
        - retire类型交易 utxo 是否忘了处理。应该变成不可用。
    * fee estimate
    * multisign
        - [ ] delete unconfirmed utxo
        - [ ] 调研if txProposalSign.Signatures == "" {是否可以去除
    * all utxos
        - check `utxo.Asset.Asset` in `btm.go`
        - raw sql
        - address str for address
    * 关于null的索引不生效的传说是真的么?
        - 应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描
        - https://www.v2ex.com/t/60437
        - https://dev.mysql.com/doc/refman/5.7/en/is-null-optimization.html
    * TEXT
        - varchar 是可变长字符串,不预先分配存储空间,长度不要超过 5000,如果存储长度大于此值,定义字段类型为 text ,独立出来一张表,用主键来对应,避免影响其它字段索引效率。
        - 定长，累积很占空间
        - utxo use hash for cp? 
        - server-side max exec time
        - https://dev.mysql.com/doc/refman/8.0/en/full-text-adding-collation.html
    * redis
+ vapor
    * 设计跨链交易
    * 设计federation模块
        * https://github.com/nsqio/nsq
        * etcd raft
        * build
            - 使用 builder template
                + 是否需要区分 vapor 和 bytom？
                + signInst
                    * 我觉得 根本就不需要 signInsts
                    * 那么 数据库里面存什么呢
                    * signs 还是可以存， 但是 inst没必要
                    * 自己 打字，自己-自己聊天, 梳理思路
    + 充值提现手续费闭环
        * 侧链可改造成手续费使用非 btm
        * 充值收取一定 btm，提现时可以用
    * https://btcpayserver.org/
        - https://github.com/btcpayserver
    * test
        - [x] database/store_test.go
        - [ ] database/utxo_view_test.go
            + TestGetTransactionsUtxo
        - [ ] protocol/state/utxo_view_test.go
        - [ ] protocol/txpool_test.go
        - [ ] test/bench_blockchain_test.go
        - [ ] test/chain_test_util.go
        - [ ] test/utxo_view/utxo_view_test.go
        - add more test?
    * xuperunion
        - check-package: $GOPATH/src/github.com/xuperchain/xuperunion/crypto/account/address.go:72:15: builtinShadow: shadowing of predeclared identifier: error
        - /home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/test/test_write.go:10:6: main redeclared in this block
        - /home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/test/test_read.go:10:6:    other declaration of main
        - check-package: $GOPATH/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:25:11: builtinShadow: shadowing of predeclared identifier: len
        - check-package: $GOPATH/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:25:16: builtinShadow: shadowing of predeclared identifier: cap
        - check-package: $GOPATH/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:46:11: builtinShadow: shadowing of predeclared identifier: len
```
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/test/test_write.go:10:6: main redeclared in this block
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/test/test_read.go:10:6:    other declaration of main
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:9:18: undeclared name: NewTrap
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:26:20: undeclared name: Context
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:40:3: undeclared name: Throw
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:77:3: undeclared name: Throw
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:27:9: invalid operation: ctx (variable of type *invalid type) has no field or method Memory
/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/exec/codec.go:29:3: undeclared name: Throw
```


```
X [/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/account/filekey.go:120] - G101: Potential hardcoded credentials (Confidence: LOW, Severity: HIGH)
  > password := "jingbo is handsome!"


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:383] - G301: Expect directory permissions to be 0750 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.MkdirAll(bindir, 0755)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/compile/compile.go:47] - G204: Subprocess launched with variable (Confidence: HIGH, Severity: MEDIUM)
  > exec.Command(cfg.Wasm2cPath, source)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/cli.go:179] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(certPath + "/cert.crt")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/compile/compile.go:24] - G204: Subprocess launched with variable (Confidence: HIGH, Severity: MEDIUM)
  > exec.Command(cfg.Wat2wasmPath, "-o", target, source)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/comm_trans.go:541] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > os.Open(filename)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/createchains.go:67] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(js)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/multisig_send.go:97] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(file)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/pluginmgr/pluginmgr.go:65] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > os.Open(confPath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:675] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(tlsPath + "/cert.crt")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/util.go:64] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(path + "net_private.key")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/util.go:37] - G301: Expect directory permissions to be 0750 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.MkdirAll(path, 0777)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:826] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > os.Open(path)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:353] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filepath.Join(fs.path, name))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_deploy.go:76] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(codepath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:272] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(currentPath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:222] - G302: Expect file permissions to be 0600 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.OpenFile(filepath.Join(fs.path, "LOG"), os.O_WRONLY|os.O_CREATE, 0644)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:139] - G302: Expect file permissions to be 0600 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.OpenFile(filepath.Join(path, "LOG"), os.O_WRONLY|os.O_CREATE, 0644)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:116] - G301: Expect directory permissions to be 0750 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.MkdirAll(path, 0755)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/transfer.go:92] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(file)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/file_storage_unix.go:36] - G302: Expect file permissions to be 0600 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.OpenFile(path, flag|os.O_CREATE, 0644)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_access.go:109] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filename)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/wasm_deploy.go:99] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(codepath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/account/filekey.go:431] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filename + "/private.key")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/account/filekey.go:426] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filename + "/public.key")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/account/filekey.go:421] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filename + "/address")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/compile/compile.go:82-90] - G204: Subprocess launched with variable (Confidence: HIGH, Severity: MEDIUM)
  > exec.Command("cc", "-shared", "-fPIC",
        "-std=c99",
        "-O"+strconv.Itoa(cfg.OptLevel),
        "-o"+target,
        "-I.",
        "-I"+tmpdir,
        "-lm",
        csource,
    )


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/account/filekey.go:97] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(filename)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:144] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(datapath + "/xuper.json")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/file_storage_unix.go:89] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > os.Open(name)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/common/common.go:58] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(file)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/driver/native/driver.go:26] - G302: Expect file permissions to be 0600 or less (Confidence: HIGH, Severity: MEDIUM)
  > os.OpenFile("stderr.log", os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0666)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/xvm/creator.go:67] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(src)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/xvm/code_manager.go:90] - G304: Potential file inclusion via variable (Confidence: HIGH, Severity: MEDIUM)
  > ioutil.ReadFile(descpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/cli.go:98] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > viper.BindPFlags(rootFlags)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:51] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyVoteCandidate))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:107] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyRevoke))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:109] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyCandidateVote), []byte(strconv.FormatInt(voteInfo.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:110] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyVoteCandidate), []byte(strconv.FormatInt(voteInfo.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:130] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > txid, _ := tp.utxoVM.GetFromTable(nil, []byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:144] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:145] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyNominateRecord))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:146] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyCanInfo))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:156] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:157] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyNominateRecord))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:158] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyCanInfo))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:190] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(key), []byte(txNom))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:191] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyNominateRecord), []byte(txNom))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:206] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(revokeCandidateKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:207] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(revokeCandidateKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:209] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyRevoke))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:223] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:66] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyCandidateVote), []byte(strconv.FormatInt(voteInfo.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:68] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyVoteCandidate), []byte(strconv.FormatInt(voteInfo.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:132] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyCandidateVote))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:134] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyVoteCandidate))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:137] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyRevoke), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:175] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(key), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:176] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyNominateRecord), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:177] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyCanInfo), []byte(canInfoValue))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:190] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(key), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:191] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyNominateRecord), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:192] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyCanInfo), []byte(canInfoValue))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:226] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > txid, _ := tp.utxoVM.GetFromTable(nil, []byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:236] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:237] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyNominateRecord))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:238] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(revokeKey), []byte(strconv.FormatInt(blVal.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:244] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyRevoke), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:254] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:255] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyNominateRecord))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:256] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(revokeKey), []byte(strconv.FormatInt(val, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:262] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(keyRevoke), desc.Tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:302] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > proposersJSON, _ := json.Marshal(proposers)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/run.go:304] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(key), proposersJSON)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/tdpos.go:480] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/tdpos.go:483] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Put([]byte(key), []byte(strconv.FormatInt(value.ballots, 10)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/contract.go:148] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > handler.vm.SetContext(ctx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/contract.go:155] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > handler.vm.Finalize(blockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/bridge/context.go:37] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > _, wset, _ := c.Cache.GetRWSets()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/acl.go:77] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(aclJSON, aclBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/acl.go:123] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(aclJSON, aclBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/acl.go:157] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(aclJSON, aclBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:177] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(fullpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:193] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(fullpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:200] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(fullpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:214] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(fullpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:217] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxovm.DebugTx(tx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/kernel/kernel.go:223] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(fullpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/driver.go:131] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > client, _ := client.NewEnvClient()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/driver.go:162] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > process.Stop(time.Duration(snc.mgr.cfg.StopTimeout) * time.Second)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:34] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > rootpath, _ = filepath.Abs(rootpath)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:76] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > framework.register()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:156] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ := proto.Marshal(vc.desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:165] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ := proto.Marshal(snc.desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:189] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > client.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:238] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > gscf.activate(name, desc.GetVersion())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/tdpos/rollback.go:50] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tp.context.UtxoBatch.Delete([]byte(keyCandidateVote))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:389] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Rename(binpath, binpath+".old")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:445] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ := proto.Marshal(oldsnc.desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:446] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > vsnc.gscf.versionTable.Put(makeNativeCodeName(oldsnc.desc), descbuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/framework.go:488] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > gscf.db.Delete(getNativeCodeKey(name))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/runtime.go:152-155] - G204: Subprocess launching should be audited (Confidence: HIGH, Severity: LOW)
  > exec.Command(h.binpath,
        "--sock", h.sockpath,
        "--chain-sock", h.chainSockPath,
    )


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/runtime.go:177] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > h.cmd.Process.Signal(syscall.SIGTERM)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/native/runtime.go:187] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > h.cmd.Process.Kill()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/proposal/proposal.go:420] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > prp.context.UtxoBatch.Put([]byte(thawUtxoKey), []byte{})


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/proposal/proposal.go:421] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > prp.context.UtxoBatch.Put([]byte(utxoKey), uItemBinary)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/proposal/proposal.go:427] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descThaw, _ := contract.Parse(string(tx.Desc))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/proposal/proposal.go:505] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > prp.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/proposal/proposal.go:506] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > prp.context.UtxoBatch.Put([]byte(utxoKey), uItemBinary)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm_manager.go:76] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > vmconfig, _ := v.getVMConfig(v.config.Driver)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm_manager.go:157] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ = proto.Marshal(&desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm_manager.go:159] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > store.Put("contract", contractCodeDescKey(contractName), descbuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm_manager.go:160] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > store.Put("contract", contractCodeKey(contractName), code)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/memory/creator.go:62] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > requestBuf, _ := proto.Marshal(request)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/consensus.go:398] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pc.context.UtxoBatch.Delete([]byte(key))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/xvm/code_manager.go:118] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ := json.Marshal(desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/xvm/code_manager.go:121] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(basedir)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contract/wasm/vm/xvm/code_manager.go:165] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(filepath.Join(c.basedir, name))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/consensus/consensus.go:337] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pc.context.UtxoBatch.Put([]byte(key), txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/code/handler.go:27] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > out, _ := json.Marshal(body)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/common/log/log.go:24] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.MkdirAll(lc.Filepath, os.ModePerm)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/driver/native/driver.go:28] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > syscall.Dup2(int(f.Fd()), 2)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/driver/native/driver.go:29] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > f.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/driver/native/driver.go:50] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > flagset.Parse(os.Args[1:])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/driver/native/driver.go:100] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > nativeCodeService.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/call/c1/c1.go:20] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > cntstr, _ := ctx.GetObject([]byte("cnt"))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/call/c1/c1.go:22] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > cnt, _ = strconv.Atoi(string(cntstr))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/call/c2/c2.go:19] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > cntstr, _ := ctx.GetObject([]byte("cnt"))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/call/c2/c2.go:21] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > cnt, _ = strconv.Atoi(string(cntstr))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/counter/counter.go:32] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > cnt, _ = strconv.Atoi(string(value))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/eleccert/eleccert.go:108] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tsInt, _ := strconv.ParseInt(ts, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/eleccert/eleccert.go:110] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > userJSON, _ := json.Marshal(userStruc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/eleccert/eleccert.go:140] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > userFileJSON, _ := json.Marshal(userFile)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:62] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(value, vals)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/common/common.go:58] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > f, _ := ioutil.ReadFile(file)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:69] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > e.ctx.PutObject([]byte(key), valsJSON)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:75] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > valsJSON, _ := json.Marshal(e.approvalOf[key])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:76] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > e.ctx.PutObject([]byte(key), valsJSON)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:199] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > num, _ := strconv.ParseInt(s, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:209] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > supplyJSON, _ := json.Marshal(vals)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:210] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.PutObject([]byte("totalsupply"), supplyJSON)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:258] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tokenID, _ := strconv.ParseInt(tokenIDStr, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:292] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tokenID, _ := strconv.ParseInt(tokenIDStr, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:324] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > tokenID, _ := strconv.ParseInt(tokenIDStr, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:48] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > rawid, _ := hex.DecodeString(id)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:53] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > out, _ := json.MarshalIndent(tx, "", "  ")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:54] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Stderr.Write(out)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:57] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > rawid, _ := hex.DecodeString(id)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:61] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > out, _ := json.MarshalIndent(block, "", "  ")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:62] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Stderr.Write(out)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:96] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > bodyStr, _ := json.Marshal(body)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/math/math.go:110] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > value, _ := nci.GetObject(key)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/exec/runner.go:20] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.SetOutput(&resp)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/exec/runner.go:29-32] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.SetOutput(&code.Response{
                Status:  code.StatusError,
                Message: string(buf[:n]),
            })


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/exec/runner.go:42] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.SetOutput(&resp)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/exec/runner.go:48] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.SetOutput(&resp)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/exec/runner.go:52] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.SetOutput(&resp)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/pipeline.go:152] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ok, _ := pm.xc.Utxovm.HasTx(tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/pipeline.go:155] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pm.xc.Utxovm.RollbackContract([]byte(""), tx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/sync.go:52] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > bcsBuf, _ := proto.Marshal(bcs)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/xchain/xchain.go:113] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > flags.Parse(os.Args[1:])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:273] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xc.Utxovm.RegisterVM3(x3kernel.GetName(), x3kernel)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:295] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > inUnconfirm, _ := xc.Utxovm.HasTx(tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:307] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msgInfo, _ := proto.Marshal(batchTxMsg)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:308] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msg, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion1, xc.bcname, header.GetLogid(), xuper_p2p.XuperMessage_BATCHPOSTTX, msgInfo, xuper_p2p.XuperMessage_SUCCESS)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:372] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ib, _ := xc.Ledger.GetPendingBlock(preblkhash)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:413] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xc.con.CheckMinerMatch(in.Header, block.Block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:480] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xc.con.ProcessConfirmBlock(in.Block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:620] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xc.con.ProcessConfirmBlock(b)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:630] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msgInfo, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:631] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msg, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion1, xc.bcname, "", xuper_p2p.XuperMessage_SENDBLOCK, msgInfo, xuper_p2p.XuperMessage_NONE)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore.go:656] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xc.Utxovm.Walk(ledgerLastID)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchaincore_net.go:19] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msg, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bid.GetBcname(), "", xuper_p2p.XuperMessage_GET_BLOCK, msgbuf, xuper_p2p.XuperMessage_NONE)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg.go:163] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xm.rootKernel.RemoveBlockChainData(name)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:111] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > _, needRepost, _ := xm.ProcessTx(txStatus)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:116] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xm.P2pv2.SendMessage(context.Background(), msg, opts...)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:199] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xm.P2pv2.SendMessage(context.Background(), msg, opts...)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:255] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > xm.P2pv2.SendMessage(context.Background(), msg, opts...)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/contractsdk/go/example/erc721/erc721.go:68] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > valsJSON, _ := json.Marshal(e.balanceOf[key])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:281] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:282-283] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCK_RES, resBuf, xuper_p2p.XuperMessage_CHECK_SUM_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:291] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:292-293] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCK_RES, resBuf, xuper_p2p.XuperMessage_UNMARSHAL_MSG_BODY_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:300] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:301-302] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCK_RES, resBuf, xuper_p2p.XuperMessage_BLOCKCHAIN_NOTEXIST)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:310] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:311-312] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCK_RES, resBuf, xuper_p2p.XuperMessage_GET_BLOCK_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:316] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(block)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:329-330] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_CHECK_SUM_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:337-338] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_UNMARSHAL_MSG_BODY_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:344-345] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_BLOCKCHAIN_NOTEXIST)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:351-352] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_GET_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_GET_BLOCKCHAIN_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:355] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(bcStatusRes)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:368-369] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_CONFIRM_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_CHECK_SUM_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:377-378] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_CONFIRM_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_UNMARSHAL_MSG_BODY_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:385-386] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_CONFIRM_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_BLOCKCHAIN_NOTEXIST)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:392-393] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, bcname, logid,
            xuper_p2p.XuperMessage_CONFIRM_BLOCKCHAINSTATUS_RES, nil, xuper_p2p.XuperMessage_CONFIRM_BLOCKCHAINSTATUS_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:396] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > resBuf, _ := proto.Marshal(tipStatus)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/xchain/xchain.go:95] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > f.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/xchain/xchain.go:94] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pprof.WriteHeapProfile(f)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/xchain/xchain.go:68] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pprof.StartCPUProfile(perfFile)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/wasm_query.go:69] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal([]byte(c.args), &args)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/wasm_deploy.go:125] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > buf, _ := proto.Marshal(desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:184] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hash.Write(counter)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:185] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hash.Write(z)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:186] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hash.Write(s1)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:200] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > mac.Write(msg)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:201] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > mac.Write(shared)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:277] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hash.Write(Km)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/ecies/libecies/ecies.go:356] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hash.Write(Km)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/hash/hash.go:12] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > h.Write(data)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/crypto/hash/hash.go:26] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > h.Write(data)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/global/common.go:128] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(bytesBuffer, binary.BigEndian, &seed)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/wasm_deploy.go:98] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > initArgs, _ := json.Marshal(x3args)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_access.go:164] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > r.ParseForm()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_access.go:194] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(body, &respBody)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_access.go:242] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > r.ParseForm()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_access.go:271] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(body, &respBody)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_save.go:205] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > r.ParseForm()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/hdwallet/key/key_save.go:255] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(body, &respBody)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/vote.go:60] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > desc, _ := json.Marshal(contractDesc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/file_storage_unix.go:43] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > f.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/core/xchainmg_net.go:264] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > _, needRepost, _ := xm.ProcessTx(v)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_status.go:80] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > out, _ := json.MarshalIndent(m, "", "  ")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:130] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > flock.release()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_status.go:71] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(buf.Bytes(), &iface)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:145] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > logw.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:209] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fs.logw.Write([]byte("=============== " + t.Format("Jan 2, 2006 (MST)") + " ===============\n"))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:215] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fs.logw.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:218] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > rename(filepath.Join(fs.path, "LOG"), filepath.Join(fs.path, "LOG.old"))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_status.go:69] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pbm.Marshal(&buf, status)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:244] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > n, _ := fs.logw.Write(fs.buf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_query.go:66] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal([]byte(c.args), &args)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_deploy.go:69] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > buf, _ := proto.Marshal(desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:613] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fs.logw.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:698] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > n, _ := fmt.Sscanf(name, "MANIFEST-%d%s", &fd.Num, &tail)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:716] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fmt.Sscanf(filepath.Base(name), "%d.ldb", &fdNum)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:730] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fmt.Sscanf(filepath.Base(name), "%d.ldb", &fdNum)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:747] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fmt.Sscanf(filepath.Base(name), "%d.ldb", &fdNum)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/kv/mstorage/multi_disk_storage.go:763] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fmt.Sscanf(filepath.Base(name), "%d.ldb", &fdNum)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_deploy.go:53] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > c.cmd.MarkFlagRequired("version")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/genesis.go:43] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > maxSizeMB, _ := strconv.Atoi(rc.MaxBlockSize)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:332] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put(append([]byte(pb.BlocksTablePrefix), block.Blockid...), blockBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:335] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put(append([]byte(pb.BlockHeightPrefix), sHeight...), block.Blockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:375] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put(append([]byte(pb.ConfirmedTablePrefix), tx.Txid...), pbTxBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:503] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > hasTx, _ := l.confirmedTable.Has(tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:630] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put(append([]byte(pb.ConfirmedTablePrefix), tx.Txid...), pbTxBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:633] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > oldPbTxBuf, _ := l.confirmedTable.Get(tx.Txid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:660] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put(append([]byte(pb.ConfirmedTablePrefix), tx.Txid...), pbTxBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:666] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Delete(append([]byte(pb.PendingBlocksTablePrefix), block.Blockid...))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:674] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batchWrite.Put([]byte(pb.MetaTablePrefix), metaBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/ledger/ledger.go:701] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > exist, _ := l.blocksTable.Has(blockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/call_handler.go:82] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ok, _ := cryptoClient.VerifyECDSA(publicKey, v.Sign, data)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/call_handler.go:107-108] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > res, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion2, "", logid,
        xuper_p2p.XuperMessage_GET_AUTHENTICATION_RES, nil, xuper_p2p.XuperMessage_GET_AUTHENTICATION_ERROR)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/node.go:128] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > muAddr, _ := multiaddr.NewMultiaddr(fmt.Sprintf("/ip4/0.0.0.0/tcp/%d", cfg.Port))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/node.go:169] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > no.kdht.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/node.go:170] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > no.host.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/server.go:99] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > peers, _ := filter.Filter()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/server.go:110] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > peers, _ := filter.Filter()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/stream.go:89] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > s.s.Reset()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/stream.go:94] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > s.node.strPool.DelStream(s)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/stream_pool.go:69] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > s.Reset()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/stream_pool.go:98] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > val.s.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/p2pv2/stream_pool.go:152] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > sp.DelStream(str)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_deploy.go:51] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > c.cmd.MarkFlagRequired("cname")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_activate.go:115] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > desc, _ := json.Marshal(contractDesc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/permission/acl/impl/manager.go:66] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(jsonBuf, acl)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/permission/acl/impl/manager.go:91] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > json.Unmarshal(jsonBuf, acl)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/js/vm.go:90] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(&f)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:52] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msgInfo, _ := proto.Marshal(in)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:53] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msg, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion1, in.GetBcname(), in.GetHeader().GetLogid(), xuper_p2p.XuperMessage_POSTTX, msgInfo, xuper_p2p.XuperMessage_NONE)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:58] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > s.mg.P2pv2.SendMessage(context.Background(), msg, opts...)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:71] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > oneOut, needRepost, _ := s.mg.ProcessTx(v)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:88] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > msg, _ := xuper_p2p.NewXuperMessage(xuper_p2p.XuperMsgVersion1, "", in.GetHeader().GetLogid(), xuper_p2p.XuperMessage_BATCHPOSTTX, txsData, xuper_p2p.XuperMessage_NONE)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:93] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > s.mg.P2pv2.SendMessage(context.Background(), msg, opts...)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/server/server.go:392] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > descbuf, _ := proto.Marshal(desc)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_activate.go:59] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > c.cmd.MarkFlagRequired("version")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/test/util/model.go:43] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > rset, wset, _ := x.Cache.GetRWSets()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/test/util/test_writer.go:28] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > t.buf.Write(p[:idx])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/test/util/test_writer.go:31] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > t.buf.Write(p[idx+1:])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/async.go:116] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > uv.flushTxList(txList)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/async.go:142] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > uv.RollBackUnconfirmedTx()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/tx_contract_generator.go:59] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > instance.SetContext(txCtx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/tx_generation.go:67] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxoTx.Txid, _ = txhash.MakeTransactionID(utxoTx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:495] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(append([]byte(pb.MetaTablePrefix), []byte(LatestBlockKey)...), newBlockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:512] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(append([]byte(pb.MetaTablePrefix), []byte(UTXOTotalKey)...), uv.utxoTotal.Bytes())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:530] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxoTx.Txid, _ = txhash.MakeTransactionID(utxoTx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:567] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxoTx.Txid, _ = txhash.MakeTransactionID(utxoTx)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:602] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > refTxid, _ := hex.DecodeString(keyTuple[len(keyTuple)-2])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:611] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > offset, _ := strconv.Atoi(keyTuple[len(keyTuple)-1])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:665] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > refTxid, _ := hex.DecodeString(string(keyTuple[len(keyTuple)-2]))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:672] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > offset, _ := strconv.Atoi(string(keyTuple[len(keyTuple)-1]))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:750] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.Release()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:764] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.Release()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:936] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put([]byte(utxoKey), uItemBinary)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:953] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete([]byte(utxoKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:981] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete([]byte(utxoKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1002] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put([]byte(utxoKey), uItemBinary)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1041] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put([]byte(utxoKey), uBinary)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1052] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete([]byte(utxoKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1134] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(append([]byte(pb.UnconfirmedTablePrefix), tx.Txid...), pbTxBuf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1235] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ok, _ := pm.IdentifyAK(ak, tx.AuthRequireSigns[i], digestHash)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1428] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.Release()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1433] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ctx.Release()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1478] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > uv.undoUnconfirmedTx(childTx, txMap, txGraph, batch, undoDone)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1487] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(append([]byte(pb.UnconfirmedTablePrefix), tx.Txid...))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1535] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(append([]byte(pb.UnconfirmedTablePrefix), []byte(txid)...))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo.go:1724] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(append([]byte(pb.UnconfirmedTablePrefix), []byte(txid)...))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:34] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.RemoveAll(workspace)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:87] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > pk, _ := cryptoClient.GetEcdsaPrivateKeyFromJSON([]byte(f.Users[miner].PrivateKey))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:155] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ecdsdPk, _ := ecdsa.GenerateKey(elliptic.P256(), rand.Reader)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:167] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > block, _ := ledger.FormatBlock(txlist, []byte("miner-1"), ecdsdPk, 123456789, 0, 0, preHash, f.U.GetTotal())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/native_activate.go:53] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > c.cmd.MarkFlagRequired("vote-height-offset")


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:215] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > bobBalance, _ := utxoVM.GetBalance(f.BobAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:216] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > aliceBalance, _ := utxoVM.GetBalance(f.AliceAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:228] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxoVM.Play(nextBlockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:229] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > bobBalance, _ = utxoVM.GetBalance(f.BobAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:230] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > aliceBalance, _ = utxoVM.GetBalance(f.AliceAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:240] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > utxoVM.Play(nextBlockid)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:241] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > bobBalance, _ = utxoVM.GetBalance(f.BobAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:242] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > aliceBalance, _ = utxoVM.GetBalance(f.AliceAddress)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/dbutils.go:68] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(rawKey, buf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/env.go:45] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > env.modelCache.fill(verData)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:48] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(delKey)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:49] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(putKey, []byte(valueVersion))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:54] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(putKey, []byte(valueVersion))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:94] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(delKey)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:104] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(putKey, verData.PureData.Value)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:106] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Delete(delKey)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel.go:111] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > batch.Put(putKey, []byte(previousVersion))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel_cache.go:118] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > val, _ := xc.inputsCache.Get(rawKey)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel_cache.go:129] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > valBuf, _ := proto.Marshal(val)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel_cache.go:153] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > _, _ = xc.Get(bucket, key)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xmodel/xmodel_cache.go:242] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > valBuf, _ := proto.Marshal(vd)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/comm_trans.go:253] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > fee, _ := strconv.ParseInt(c.Fee, 10, 64)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/compile/compile.go:41] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > targetFile.Close()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/compile/compile.go:43] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Remove(target)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/cli.go:121] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > viper.Unmarshal(&c.RootOptions)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/cmd/cli/cli.go:108] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > viper.ReadInConfig()


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/example/xvm-run/main.go:79] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Write(buf, binary.LittleEndian, uint64(addr))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/example/xvm-run/main.go:81] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Write(buf, binary.LittleEndian, uint32(addr))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/emscripten/resolver.go:40] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(&mem[mutableGlobalsBase])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/emscripten/resolver.go:120] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Stderr.Write(buf)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/emscripten/resolver.go:152] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Offsetof(new(mutableGlobals).DynamicTop)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:26] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, &ptr)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:27] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, &len)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:28] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, &cap)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:33] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(v.Addr().Pointer())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:34] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(&r.mem[ptr])


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:47] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, &ptr)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:48] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, &len)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:62] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Read(r.buf, binary.LittleEndian, ref.Interface())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:94] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Write(e.buf, binary.LittleEndian, vv)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/codec.go:96] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > binary.Write(e.buf, binary.LittleEndian, v.Interface())


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/runtime.go:54] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > os.Stderr.Write(codec.Bytes(uint32(p), uint32(n)))


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/xvm/runtime/go/js/ref.go:41] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(&r)


[/home/gavin/work/go/src/github.com/xuperchain/xuperunion/utxo/utxo_fake.go:205] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > block, _ := ledger.FormatRootBlock([]*pb.Transaction{tx})
```


<!-- 

## 下版本
+ 应该区分用户提交的业务形态的交易（并在数据库中存一份）和提交的 raw tx。
++ 这么做也有利于展示交易被回滚的情况，以告知用户。不然用户发了一笔交易，发生回滚就突然不见了。
+ utxo 现在是单纯的 10 min lock_until，而 unconfirmed tx 现在又不更新 utxo，应该设置长一点防止 出块超过 10min（现在这种情况在主网上还是很可能出现的）。这样可能导致间隔 10min 的两笔交易用了同样的 utxo，都能submit成功，但最终矿工只打包一个，用户莫名丢失了一笔交易但我们却没能提示。
++ 这块要好好再设计，不能等入块了才解锁，不然如果一直不上链就永远锁住了。应该做成 submit 给 bytomd 就锁住，入块了就设置is_spend，如果一直没入块后面 expired 了才解锁utxo 。做好 utxo 和 balance 的删除/更新。
++ 最好还是加上判断确认数（5～6次）才能进行花费 。
+ 应该区分 未确认交易 导致的 available balance 和 total balance
+ asset 价格 redis  查不到 应该重新拿，而不是查 mysql。asset 价格不放数据库。
+ sql IN 语句是否存在性能问题，是否用 数字型 而非字符串 加快查询
+ api 不应该和 bytomd 交互，应该做一个 channel/callback/mq 给 updater 或者 load balancer，统一和 bytomd 进行交互（即 所有和bytomd 的交互应该是一个统一的出入口）
++ bytomd 目前是单节点 没有 load balancing，应该做上以免 submit tx 造成的 ddos 或别的原因造成的不可用
+++ bytomd load balancing 要注意节点状态不一致的处理，从哪个节点同步数据。拿块可以通过判断最高高度，问题主要是 ws 去哪个节点拿 tx -->

<!-- 
## mining pool

+ MinDiff
+ DiffType
+ GetTargetHex
+ leading zero
+ retarget
+ processShare
    + s.hashrateExpiration, 
    + s.minuteHashrateExpiration
+ shareTimeRing

## Tensority  
### research
+ mulMatrix
    * __blas__
        - openblas
            + parameters tweaking
        - single type blas?
    * simd
        - __golang simd__
            + https://github.com/bjwbell/gensimd
            + https://github.com/mengzhuo/intrinsic
            + https://github.com/yesuu/simd/blob/master/mul_amd64.s
            + https://github.com/rkusa/gm
            + https://github.com/Everlag/goSIMD
            + https://www.google.com/search?q=go+simd&oq=go+simd&aqs=chrome..69i57j0l5.2290j0j7&sourceid=chrome&ie=UTF-8
            + https://www.google.com/search?q=yeppp&oq=yeppp&aqs=chrome..69i57j69i61j0l4.289j0j4&sourceid=chrome&ie=UTF-8
            + https://yushuangqi.com/blog/2016/go-ru-he-shi-yong--simd-zhi-ling.html
                * https://github.com/golang/go/blob/master/src/cmd/internal/obj/x86/asm6.go
            + https://www.cryptologie.net/article/406/simd-instructions-in-go/
            + https://golanglibs.com/top?q=simd
            + https://godoc.org/github.com/slimsag/rand/simd
            + https://github.com/sbinet/vector
            + https://github.com/reiver/go-float64x4
            + https://github.com/pennello/go_swar
            + should able to use int, save time for float64 conversion
        - cpp simd
        - gpu simd
    * opencv
    * cuda
        - https://archive.fosdem.org/2014/schedule/event/hpc_devroom_go/attachments/slides/486/export/events/attachments/hpc_devroom_go/slides/486/FOSDEM14_HPC_devroom_14_GoCUDA.pdf
    * openmp
        - give up
            + weird `go build bytomd`
                * no need to provide `CGO_LDFLAGS="-g -O2 -fopenmp"`
            + goood things
                * go test&build need to provide `CGO_LDFLAGS="-g -O2 -fopenmp"`
    * eigen
    * opencl
+ mat_init
    * simd?
    * gpu simd?
+ 有没有 生成 doc 的文件
+ mining/tensority
+ 搞清楚 BigEdian LittleEdian 的区别

### DONE
+ Time & space opt for dataIdentity[] init in mulMatrix()
+ clean up code & update tensority test
+ confirm all use SHA-3-256 
+ no need for parallelize extSeed, as SHA-3-256 is fast enough
    * .
        ```
        // 67 ms
        cache := calcSeedCache(seed.Bytes())

        // 1.030978394s
        data := mulMatrix(hash.Bytes(), cache)

        // 191.217µs
        hashMatrix(data)

        dataIdentity time:  57.954µs
        result time:  501.596µs
        ui32 time:  3.719813ms
        f64 time:  71.828938ms
        loop tmp time:  845.702µs 
        loop sha3 time:  6.524µs
        1 loop ma time:  240.240435ms
        4 loops ma time:  918.162585ms
        wg 4 loops ma time:  821.528586ms
        ```
+ go vs cpp, single_thread vs multi_thread
    * 矩阵点乘因为 库的问题，cpp 的 openblas 不如  go 的gonum/mat 快 改成多线程并发以后也是  cpp 不如 go 好   cpp多线程反而比cpp单线程更慢了
    * 我的笔记本上
        - go 单线程 gonum\mat: 920ms
        - go 4线程 gonum\mat: 820ms
        - cpp 单线程 openblas: 2.4s
        - cpp 4线程 openblas: 5.3s (理论上cpp 4线程能优化到700ms，我也不知道我为什么写出来这么渣...)
+ cpp -O3
    * ` g++ byte_order.c sha3.c  test_BytomPoW.cpp -I /opt/OpenBLAS/include/ -L/opt/OpenBLAS/lib -lopenblas -lpthread -std=c++11 -pthread -mavx2 -O2`
        - mulMatrix: 2.53 -> 1.65
        - total: 2.98898s -> 2.05s
+ Kui's first cpp slower
    * 12.9 s
+ extend seed in cpp
+ cpp multi-thread slower
    * 2.4s vs 5.3s
+ Kui's second cpp
    * 160ms!
+ 现在做的东西也不知道有没有价值，还是在优化代码,主要是 区块验证/挖矿这块
还没有做 P2P, 也没有接触虚拟机
    * 北京那边发挥很不稳定
    * 2.8s 被优化到 12.9s
    * then 160ms!
        - GPU?
        - 多线程
        - SIMD
+ cpu flag
    * `cat /proc/cpuinfo`
+ toIdentityMatrix
    * 0.189s -> 0.172s
    * 17ms faster
+ SIMD
    * Beijing
        - mul 230ms
        - total 450ms
        - single-thread 365ms
    * combine
        - sThread 167ms
        - sThread total 289ms
        - sThread opt-init16 total 256ms
        - mThread 0.333546s
        - mThread total 0.429112s
+ cgo
    * mul 178ms
    * total 280ms
+ shared lib
    * dl
        - https://www.google.com/search?q=golang+c+shared+lib&oq=golang++c+shared+lib&aqs=chrome..69i57j69i60j0.4787j0j7&sourceid=chrome&ie=UTF-8
        - https://github.com/rainycape/dl
    * plugin
        - https://golang.org/pkg/plugin/
        - https://medium.com/learning-the-go-programming-language/writing-modular-go-programs-with-plugins-ec46381ee1a9
    * https://github.com/golang/go/issues/16805
    * https://www.ardanlabs.com/blog/2013/08/using-c-dynamic-libraries-in-go-programs.html
+ monero
    * Monero verification time
        - https://bitcointalk.org/index.php?topic=583449.0
        - https://www.reddit.com/r/Monero/
        - https://monero.stackexchange.com/
        - https://forum.getmonero.org/
        - https://mattermost.getmonero.org/login
        - https://telegram.me/bitmonero
+ bytes
    * It is analogous to the facilities of the strings package.
    * `func (b *Buffer) Bytes() []byte`
        - Bytes returns a slice of length b.Len() holding the unread portion of the buffer. The slice is valid for use only until the next buffer modification (that is, only until the next call to a method like Read, Write, Reset, or Truncate). The slice aliases the buffer content at least until the next buffer modification, so immediate changes to the slice will affect the result of future reads.
    * [为什么电脑数据一个字节是8位？](https://www.guokr.com/question/542532/)
+ tensor
    * [X]win64 ver
        - define flag
        - no need fPIC
    * [X]win32 ver
+ openmp
    * 4-core on 4-core
        - faster
    * 4-core on 1-core
        - no change
    * 1-core on 1-core
        - no change
    * 1-core on 4-core
        - faster
+ coinbase data
    * Getblocktemplate allow you to define coinbase. You can check btcpool code. In stratum.cc, we define coinbase. See initfromGbt function. gbt stands for getblocktemplate.
    * 看下btc的交易结构及coinbase交易. 没有pre tx所以 就可以利用这个字段来写自定义信息. 比特币是在coinbase交易的输入的脚本里写的.
 -->

<!-- 
# Why I don't like Bytom
If you look into the Bytom mining code, you will find it hard to understand. In fact, it's designed to collaborate with bitmain's hardware. How can a blockchain product be promising if it doesn't have its own right to choose the algo?
 --> 

## Bytom 架构

### 商业模型
> https://gguoss.github.io/2017/06/28/Bytom-s-data-structure/
> 
> __资产账户__
> ![bytom_asset](/img/bytom/bytom_asset.png)
> 
> + assetid 是全局唯一的资产识别id
> + alias 是资产的别名，可便于记忆，如(gold, silver) 
> + vmversion 是为了软分叉时，做到动态过度。
> + program 是指发布该资产时需要执行的程序
> + initialblockhash 是指该资产是在哪个块高度被登记
> + signer 管理公私钥对，以便用该资产的私钥签名，只有拥有该资产私钥的人才能发布该资产
> + definition 对该资产的解释说明等。

### 软件架构

#### 技术特点
* 区块处理
    - btc
* tx
    - butxo 支持多资产
        + "扩展 utxo，扩展字段不上链，相当于拆表"?"分两类，一类 上链的，一类钱包的"?
    - txin txout 分多 action、做映射
* merkle tree
    - trie (pat)
    - 2 叉，不平衡
    - 保存交易状态
* p2p
    - tendermint reactor模型
    - eth discover
    - eth block sync
* 合约
    - chain

#### BVM
+ 智能合约
    * 合约交易
        - Bytom 默认不支持含 btm 的交易对，只支持 非btm交易对
        - btm 只支持标准(P2SH)交易
        - 但有个 解决方案：将合约交易转为 P2SH 交易



<!-- 
## blockcenter

最近在负责　blockcenter　session　登录交互这一部分，记录一下开发时的所习所想。

主要基础知识还是来自：

+ 图解密码学
+ [Crypto 101](https://github.com/crypto101/book)
+ [Practical-Cryptography-for-Developers-Book](https://github.com/nakov/Practical-Cryptography-for-Developers-Book)
+ Applied Cryptography: Protocols, Algorithms, and Source Code in C 应用密码学
+ [An Intensive Introduction to Cryptography](https://intensecrypto.org/public/index.html)
+ [HTTP 身份验证](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Authentication)
 -->

### Cross-Origin Resource Sharing (CORS)

XMLHttpRequest和Fetch API遵循同源策略。 这意味着使用这些API的Web应用程序只能从加载应用程序的同一个域请求HTTP资源，除非响应报文包含了正确CORS响应头。

跨域资源共享（ CORS ）机制 __允许__ Web 应用服务器进行跨域访问控制，从而使跨域数据传输 __得以安全__ 进行。现代浏览器支持在 API 容器中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨域 HTTP 请求所带来的风险。

#### 什么情况下需要 CORS

+ XMLHttpRequest 或 Fetch 发起的跨域 HTTP 请求
+ Web 字体 (CSS 中通过 @font-face 使用跨域字体资源), 因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。
+ WebGL 贴图
+ 使用 drawImage 将 Images/video 画面绘制到 canvas
+ 样式表（使用 CSSOM）


<!-- 

## DevCon
### mine
    + payment channel
    + UBC
        * btcpayserver
    + dao
        + smart signature
        + DEX
        + games

### other
+ bytom email system
+ love wall
+ 在线生成 bycoin手机钱包打赏二维码
+ equity debugger
+ CVE wanner for bytom
+ bytomd compilation optimizer
+ terram


-->