# 安装biticoin节点流程

## 下载安装biticoin可执行文件
[omnicore执行档下载链接](https://github.com/OmniLayer/omnicore/releases)

### 下载适合服务器版本的压缩包 （以centos为例）   
```` 安装btc节点可执行文件
[centos]$ wget 'https://bitcoincore.org/bin/bitcoin-core-0.17.1/bitcoin-0.17.1-x86_64-linux-gnu.tar.gz'
[centos]$ gunzip bitcoin-0.17.1-x86_64-linux-gnu.tar.gz
[centos]$ tar -xvf bitcoin-0.17.1-x86_64-linux-gnu.tar
[centos]$ cd bitcoin-0.17.1/bin
[centos]$ sudo cp * /usr/local/bin/
````

### 检查是否安装成功
````angular2html
[centos]$ bitcoind -version
Bitcoin Core Daemon version v0.17.1
Copyright (C) 2009-2018 The Bitcoin Core developers

Please contribute if you find Bitcoin Core useful. Visit
<https://bitcoincore.org> for further information about the software.
The source code is available from <https://github.com/bitcoin/bitcoin>.

This is experimental software.
Distributed under the MIT software license, see the accompanying file COPYING
or <https://opensource.org/licenses/MIT>

This product includes software developed by the OpenSSL Project for use in the
OpenSSL Toolkit <https://www.openssl.org> and cryptographic software written by
Eric Young and UPnP software written by Thomas Bernard.

[centos]$ rm -fr $HOME/bitcoin-0.17.1
````


## 创建节点启动目录及日志目录
````创建节点启动目录及日志目录
[centos]$ mkdir -p $HOME/sbin
[centos]$ mkdir -p $HOME/dom-bitcoin/conf
[centos]$ mkdir -p $HOME/dom-bitcoin/chaindata
[centos]$ mkdir -p $HOME/logs
````

## 创建启动配置文件（以生产为例）
[omnicore配置文件参考](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/configuration.md)   
[bitcoin配置文件参考](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
```` 典型的配置文件 bitcoin-mainnet.conf
deprecatedrpc=accounts
txindex=1
rpcpassword=dom_bitcoin
rpcuser=bitcoin_dom
testnet=0
listen=1
server=1
rpcallowip=127.0.0.1
rpcallowip=xx.xx.xx.xx
rpcallowip=xx.xx.xx.xx
rpcallowip=xx.xx.xx.xx
port=18332
rpcport=8332
prune=0
````

### option说明
|Name                |Type      |Default   |Description                                                 |
|--------------------|----------|----------|------------------------------------------------------------|
|`server`            |boolean   |`0`       |tells Bitcoin-Qt to accept JSON-RPC commands                |
|`testnet`           |boolean   |`0`       |Run on the test network instead of the real bitcoin network |
|`rpcuser`           |String    |`""`      |On client-side, you add the normal user/password pair to send commands|
|`rpcpassword`       |boolean   |`""`      |On client-side, you add the normal user/password pair to send commands|
|`rpcallowip`        |boolean   |`localhost`|By default, only RPC connections from localhost are allowed.  Specify as many rpcallowip= settings as you like to allow connections from other hosts,  either as a single IPv4/IPv6 or with a subnet specification|
|`port`              |Number    |`8333`    |Listen for connections on <port> (default: 8333 or testnet: 18333)|
|`rpcport`           |Number    |`8332`    |Listen for JSON-RPC connections on <port> (default: 8332 or testnet:18332)|
|`txindex`           |boolean   |`0`       |Rebuild chain state and block index from the blk*.dat files on disk|
|`logtimestamps`     |boolean   |`1`       |Prepend debug output with timestamp (default: 1)            |
|`pure`              |boolean   |`0`       |Reduce storage requirements by enabling pruning (deleting) of old blocks. This allows the pruneblockchain RPC to be called to delete specific blocks, and enables automatic pruning of old blocks if a target size in MiB is provided. This mode is incompatible with -txindex and -rescan. Warning: Reverting this setting requires re-downloading the entire blockchain. (default:0 = disable pruning blocks, 1 = allow manual pruning via RPC,>=550 = automatically prune block files to stay under the specified target size in MiB)|
|`deprecatedrpc`     |String    |`""`      |When bitcoin is configured with the -deprecatedrpc=accounts setting, specifying a label/account/dummy argument will return both outgoing and incoming transactions. Without the -deprecatedrpc=accounts setting, it will only return incoming transactions (because it used to be possible to create transactions spending from specific accounts, but this is no longer possible with labels).When -deprecatedrpc=accounts is set, it's possible to pass the empty string "" to list transactions that don't have any label. Without -deprecatedrpc=accounts, passing the empty string is an error because returning only non-labeled transactions is not generally useful behavior and can cause confusion.|

## 创建启动脚本
````angular2html
[centos]$ cd ~/sbin
[centos]$ cat btc_start.sh
#!/bin/bash
PROJECTNAME=bitcoind

nohup bitcoind -conf="${HOME}/dom-bitcoin/conf/bitcoin-mainnet.conf" -port=18333 -rpcport=8333 -datadir="${HOME}/dom-bitcoin/chaindata/" > ~/logs/bitcoin.log 2>&1 &

ps -ef|grep ${PROJECTNAME}|grep -v grep>/dev/null
stat=$?
if [ $stat -eq 0 ];then
    echo "OK - ${PROJECTNAME} start ok!"
else
    echo "ERROR - ${PROJECTNAME} start error, please check log in ${HOME}/logs/bitcoin.log!"
fi
````

## 启动服务
````angular2html
[centos]$ sh btc_start.sh 
OK - bitcoind start ok!
````

### 查看日志启动是否正常-方式一
````angular2html
[centos]$ cd ~/logs
[centos]$ tail -f bitcoin.log
2019-02-19T09:23:58Z init message: Loading wallet...
2019-02-19T09:23:58Z txindex thread start
2019-02-19T09:23:58Z Syncing txindex with block chain from height 563629
2019-02-19T09:23:58Z [default wallet] nFileVersion = 170100
2019-02-19T09:23:58Z [default wallet] Keys: 4004 plaintext, 0 encrypted, 13789 w/ metadata, 4004 total. Unknown wallet records: 1
2019-02-19T09:23:58Z [default wallet] Wallet completed loading in             125ms
2019-02-19T09:23:58Z [default wallet] keypool added 1 keys (0 internal), size=2000 (1000 internal)
2019-02-19T09:23:58Z [default wallet] setKeyPool.size() = 2000
2019-02-19T09:23:58Z [default wallet] mapWallet.size() = 1011
2019-02-19T09:23:58Z [default wallet] mapAddressBook.size() = 9790
2019-02-19T09:23:58Z mapBlockIndex.size() = 563740
2019-02-19T09:23:58Z nBestHeight = 563738
2019-02-19T09:23:58Z Leaving InitialBlockDownload (latching to false)
2019-02-19T09:23:58Z Bound to [::]:18333
2019-02-19T09:23:58Z Bound to 0.0.0.0:18333
2019-02-19T09:23:58Z init message: Loading P2P addresses...
2019-02-19T09:23:58Z torcontrol thread start
2019-02-19T09:23:58Z Loaded 41092 addresses from peers.dat  122ms
2019-02-19T09:23:58Z init message: Loading banlist...
2019-02-19T09:23:58Z init message: Starting network threads...
2019-02-19T09:23:58Z init message: Done loading
2019-02-19T09:23:58Z net thread start
2019-02-19T09:23:58Z dnsseed thread start
2019-02-19T09:23:58Z msghand thread start
2019-02-19T09:23:58Z opencon thread start
2019-02-19T09:23:58Z addcon thread start
2019-02-19T09:23:59Z New outbound peer connected: version: 70015, blocks=563738, peer=0
2019-02-19T09:24:00Z Imported mempool transactions from disk: 6433 succeeded, 0 failed, 0 expired, 0 already there
2019-02-19T09:24:00Z txindex is enabled at height 563738
2019-02-19T09:24:00Z txindex thread exit
2019-02-19T09:24:07Z New outbound peer connected: version: 70015, blocks=563738, peer=1
2019-02-19T09:24:09Z P2P peers available. Skipped DNS seeding.
2019-02-19T09:24:09Z dnsseed thread exit
2019-02-19T09:24:13Z New outbound peer connected: version: 70015, blocks=563738, peer=2
2019-02-19T09:24:13Z New outbound peer connected: version: 70015, blocks=563738, peer=3
2019-02-19T09:24:14Z New outbound peer connected: version: 70015, blocks=563738, peer=4
2019-02-19T09:24:15Z New outbound peer connected: version: 70015, blocks=563738, peer=5
2019-02-19T09:24:15Z New outbound peer connected: version: 70015, blocks=561968, peer=6
2019-02-19T09:24:17Z New outbound peer connected: version: 70015, blocks=563738, peer=7
````

### 查看日志启动是否正常-方式二
````angular2html
[centos]$ cd $HOME/dom-omnicore/chaindata
[centos]$ tail -f debug.log
...
2019-02-11T14:07:27Z UpdateTip: new best=000000000000000000093150fa8de93219f77f5ed2c3a95e55dc1b7ac08d19cd height=527753 version=0x20000000 log2_work=89.042898 tx=322754771 date='2018-06-16T16:27:01Z' progress=0.867375 cache=115.1MiB(469389txo)
2019-02-11T14:07:27Z UpdateTip: new best=0000000000000000002343479bd1362ee254c44ac94134839170693daf15698e height=527754 version=0x20000000 log2_work=89.042946 tx=322757158 date='2018-06-16T16:29:49Z' progress=0.867381 cache=115.5MiB(473093txo)
2019-02-11T14:07:27Z UpdateTip: new best=0000000000000000000fa8908b496b866e31e19e019020d07e8eaf6ac72b112d height=527755 version=0x20000000 log2_work=89.042994 tx=322758624 date='2018-06-16T16:39:17Z' progress=0.867385 cache=115.7MiB(474164txo)
2019-02-11T14:07:27Z UpdateTip: new best=00000000000000000023bed00bee55efe23ec3081be390ab7848b87daa671be2 height=527756 version=0x20000000 log2_work=89.043042 tx=322759748 date='2018-06-16T16:47:13Z' progress=0.867388 cache=115.9MiB(475662txo)
2019-02-11T14:07:27Z UpdateTip: new best=0000000000000000002a2782e96e55b493f2e210825c7eae7891aafba06cd3db height=527757 version=0x20000000 log2_work=89.04309 tx=322760498 date='2018-06-16T16:52:15Z' progress=0.867390 cache=116.1MiB(477339txo)
2019-02-11T14:07:27Z UpdateTip: new best=00000000000000000037793c50bb13aa160703e31b0979a07fa61693614d1ffc height=527758 version=0x20000000 log2_work=89.043138 tx=322760800 date='2018-06-16T16:54:22Z' progress=0.867391 cache=116.1MiB(477989txo)
````

## 参考文件
[github 官方地址](https://github.com/OmniLayer/omnicore)   
[github rpc调用接口](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/rpc-api.md)     
[omnicore配置文件文档](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/configuration.md)   
[bitcoin配置文件文档](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)     
[bitcoin org参考文档](https://bitcoincore.org/)
[bitcoin 执行档下载地址](https://bitcoincore.org/)


