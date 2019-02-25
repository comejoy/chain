# 安装usdt节点流程

## 下载安装omnicore可执行文件
[omnicore执行档下载链接](https://github.com/OmniLayer/omnicore/releases)

### 下载适合服务器版本的压缩包 （以centos为例）   
```` 安装usdt节点可执行文件
[centos]$ wget 'https://github.com/OmniLayer/omnicore/releases/download/v0.4.0/omnicore-0.4.0-x86_64-linux-gnu.tar.gz'
[centos]$ gunzip omnicore-0.4.0-x86_64-linux-gnu.tar.gz
[centos]$ tar -xvf omnicore-0.4.0-x86_64-linux-gnu.tar
[centos]$ cd omnicore-0.4.0/bin
[centos]$ sudo cp * /usr/local/bin/
````

### 检查是否安装成功
````angular2html
[centos]$ omnicored --version
Omni Core Daemon version v0.4.0
Copyright (C) 2009-2017 The Bitcoin Core and Omni Core developers

Please contribute if you find Omni Core useful. Visit <http://omnilayer.org>
for further information about the software.
The source code is available from <https://github.com/OmniLayer/omnicore>.

This is experimental software.
Distributed under the MIT software license, see the accompanying file COPYING
or <http://www.opensource.org/licenses/mit-license.php>.

This product includes software developed by the OpenSSL Project for use in the
OpenSSL Toolkit <https://www.openssl.org/> and cryptographic software written
by Eric Young and UPnP software written by Thomas Bernard.

[centos]$ rm -fr $HOME/omnicore-0.4.0
````


## 创建节点启动目录及日志目录
````创建节点启动目录及日志目录
[centos]$ mkdir -p $HOME/sbin
[centos]$ mkdir -p $HOME/dom-omnicore/conf
[centos]$ mkdir -p $HOME/dom-omnicore/chaindata
[centos]$ mkdir -p $HOME/logs
````

## 创建启动配置文件（以生产为例）
[omnicore配置文件参考](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/configuration.md)   
[bitcoin配置文件参考](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)
```` 典型的配置文件 usdtmainnet.conf
server=1
testnet=0
rpcuser=dom_omnicore
rpcpassword=omnicore_dom
rpcallowip=127.0.0.1
rpcallowip=xx.xx.xx.xx
rpcallowip=xx.xx.xx.xx
port=18335
rpcport=8335
txindex=1
logtimestamps=1
````

### option说明
|Name                |Type      |Default   |Description                                                 |
|--------------------|----------|----------|------------------------------------------------------------|
|`server`            |boolean   |`0`       |tells Bitcoin-Qt to accept JSON-RPC commands                |
|`testnet`           |boolean   |`0`       |Run on the test network instead of the real bitcoin network |
|`rpcuser`           |String    |`""`      |On client-side, you add the normal user/password pair to send commands|
|`rpcpassword`       |boolean   |`""`      |On client-side, you add the normal user/password pair to send commands|
|`rpcallowip`        |boolean   |`localhost`|By default, only RPC connections from localhost are allowed.  Specify as many rpcallowip= settings as you like to allow connections from other hosts,  either as a single IPv4/IPv6 or with a subnet specification|
|`port`              |Number    |`8332`    |Listen for connections on <port> (default: 8333 or testnet: 18333)|
|`rpcport`           |Number    |`8332`    |Listen for JSON-RPC connections on <port> (default: 8332 or testnet:18332)|
|`txindex`           |boolean   |`0`       |Rebuild chain state and block index from the blk*.dat files on disk|
|`logtimestamps`     |boolean   |`1`       |Prepend debug output with timestamp (default: 1)            |

## 创建启动脚本
````angular2html
[centos]$ cd ~/sbin
[centos]$ cat usdt_start.sh
#!/bin/bash
PROJECTNAME=omnicored

nohup omnicored -conf="${HOME}/dom-omnicore/conf/usdtmainnet.conf" -datadir="${HOME}/dom-omnicore/chaindata/" >${HOME}/logs/omnicore.log 2>&1 &

ps -ef|grep ${PROJECTNAME}|grep -v grep>/dev/null
stat=$?
if [ $stat -eq 0 ];then
    echo "OK - ${PROJECTNAME} start ok!"
else
    echo "ERROR - ${PROJECTNAME} start error, please check log in ${HOME}/logs/omnicore.log!"
fi
````

## 启动服务
````angular2html
[centos]$ sh usdt_start.sh 
OK - omnicored start ok!
````

### 查看日志启动是否正常-方式一
````angular2html
[centos]$ cd ~/logs
[centos]$ tail -f omnicore.log
2019-02-25 04:05:39 Initializing Omni Core v0.4.0 [main]
2019-02-25 04:05:39 Loading trades database: OK
2019-02-25 04:05:39 Loading send-to-owners database: OK
2019-02-25 04:05:39 Loading tx meta-info database: OK
2019-02-25 04:05:39 Loading smart property database: OK
2019-02-25 04:05:39 Loading master transactions database: OK
2019-02-25 04:05:39 Loading fee cache database: OK
2019-02-25 04:05:39 Loading fee history database: OK
2019-02-25 04:05:39 Loading persistent state: NONE (no usable previous state found)
2019-02-25 04:05:39 Omni Core initialization completed
````

### 查看日志启动是否正常-方式二
````angular2html
[centos]$ cd $HOME/dom-omnicore/chaindata
[centos]$ tail -f debug.log
2019-02-25 04:05:40 UpdateTip: new best=000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f height=0 version=0x00000001 log2_work=32.000022 tx=1 date='2009-01-03 18:15:05' progress=0.000000 cache=0.0MiB(0tx)
2019-02-25 04:05:40 mapBlockIndex.size() = 1
2019-02-25 04:05:40 nBestHeight = 0
2019-02-25 04:05:40 setKeyPool.size() = 100
2019-02-25 04:05:40 mapWallet.size() = 0
2019-02-25 04:05:40 mapAddressBook.size() = 1
2019-02-25 04:05:40 init message: Loading addresses...
2019-02-25 04:05:40 torcontrol thread start
2019-02-25 04:05:40 ERROR: Read: Failed to open file /home/centos/dom-omnicore/chaindata/peers.dat
2019-02-25 04:05:40 Invalid or missing peers.dat; recreating
2019-02-25 04:05:40 init message: Loading banlist...
2019-02-25 04:05:40 ERROR: Read: Failed to open file /home/centos/dom-omnicore/chaindata/banlist.dat
2019-02-25 04:05:40 Invalid or missing banlist.dat; recreating
2019-02-25 04:05:40 init message: Starting network threads...
2019-02-25 04:05:40 dnsseed thread start
2019-02-25 04:05:40 net thread start
2019-02-25 04:05:40 Loading addresses from DNS seeds (could take a while)
2019-02-25 04:05:40 init message: Done loading
2019-02-25 04:05:40 addcon thread start
2019-02-25 04:05:40 msghand thread start
2019-02-25 04:05:40 opencon thread start
2019-02-25 04:05:43 receive version message: /Satoshi:0.14.99/: version 70015, blocks=564542, us=18.179.14.249:53846, peer=1
2019-02-25 04:05:44 Pre-allocating up to position 0x100000 in rev00000.dat
2019-02-25 04:05:44 UpdateTip: new best=00000000839a8e6886ab5951d76f411475428afc90947ee320161bbf18eb6048 height=1 version=0x00000001 log2_work=33.000022 tx=2 date='2009-01-09 02:54:25' progress=0.000000 cache=0.0MiB(1tx)
2019-02-25 04:05:44 UpdateTip: new best=000000006a625f06636b8bb6ac7b960a8d03705d1ace08b1a19da3fdcc99ddbd height=2 version=0x00000001 log2_work=33.584985 tx=3 date='2009-01-09 02:55:44' progress=0.000000 cache=0.0MiB(2tx)
2019-02-25 04:05:44 UpdateTip: new best=0000000082b5015589a3fdf2d4baff403e6f0be035a5d9742c1cae6295464449 height=3 version=0x00000001 log2_work=34.000022 tx=4 date='2009-01-09 03:02:53' progress=0.000000 cache=0.0MiB(3tx)
2019-02-25 04:05:44 UpdateTip: new best=000000004ebadb55ee9096c9a2f8880e09da59c0d68b1c228da88e48844a1485 height=4 version=0x00000001 log2_work=34.32195 tx=5 date='2009-01-09 03:16:28' progress=0.000000 cache=0.0MiB(4tx)
2019-02-25 04:05:44 UpdateTip: new best=000000009b7262315dbf071787ad3656097b892abffd1f95a1a022f896f533fc height=5 version=0x00000001 log2_work=34.584985 tx=6 date='2009-01-09 03:23:48' progress=0.000000 cache=0.0MiB(5tx)
````

## 参考文件
[github 官方地址](https://github.com/OmniLayer/omnicore)   
[github rpc调用接口](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/rpc-api.md)     
[omnicore配置文件文档](https://github.com/OmniLayer/omnicore/blob/master/src/omnicore/doc/configuration.md)   
[bitcoin配置文件文档](https://github.com/bitcoin/bitcoin/blob/master/share/examples/bitcoin.conf)     
[bitcoin org参考文档](https://bitcoincore.org/)


