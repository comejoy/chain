# 安装ethereum节点流程

## 下载安装ethereum可执行文件
[ethereum执行档下载链接](https://geth.ethereum.org/downloads/)

### 下载适合服务器版本的压缩包 （以centos为例）   
```` 安装ethereum节点可执行文件
[centos]$ wget 'https://gethstore.blob.core.windows.net/builds//geth-linux-amd64-1.8.22-7fa3509e.tar.gz'
[centos]$ gunzip geth-linux-amd64-1.8.22-7fa3509e.tar.gz
[centos]$ tar -xvf geth-linux-amd64-1.8.22-7fa3509e.tar
[centos]$ cd geth-linux-amd64-1.8.22-7fa3509e
[centos]$ sudo cp geth /usr/local/bin/
````

### 检查是否安装成功
````angular2html
[centos]$ geth version
Geth
Version: 1.8.22-stable
Git Commit: 7fa3509e2eaf1a4ebc12344590e5699406690f15
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.11.5
Operating System: linux
GOPATH=
GOROOT=/home/travis/.gimme/versions/go1.11.5.linux.amd64

[centos]$ rm -fr $HOME/geth-linux-amd64-1.8.22-7fa3509e
````

## 创建启动脚本
````angular2html
[centos]$ cd ~/sbin
[centos]$ cat eth_start.sh
#!/sbin/bash

nohup geth --maxpeers 100 --syncmode fast --rpc --rpcaddr 0.0.0.0 --rpcapi db,eth,net,web3,miner,personal  >> ~/logs/geth.log 2>&1 &
````

## 启动服务
````angular2html
[centos]$ sh eth_start.sh 
OK - ethereum start ok!
````

### 查看日志启动是否正常-方式一
````angular2html
[centos]$ cd ~/logs
[centos]$ tail -f geth.log
WARN [02-11|11:09:55.284] Found deprecated node list file /home/ec2-user/.ethereum/static-nodes.json, please use the TOML config file instead.
INFO [02-11|11:09:55.289] Starting peer-to-peer node               instance=Geth/v1.8.22-stable-7fa3509e/linux-amd64/go1.11.5
INFO [02-11|11:09:55.289] Allocated cache and file handles         database=/home/ec2-user/.ethereum/geth/chaindata cache=512 handles=2048
INFO [02-11|11:09:55.344] Initialised chain configuration          config="{ChainID: 1 Homestead: 1150000 DAO: 1920000 DAOSupport: true EIP150: 2463000 EIP155: 26750
00 EIP158: 2675000 Byzantium: 4370000 Constantinople: 7280000  ConstantinopleFix: 7280000 Engine: ethash}"
INFO [02-11|11:09:55.344] Disk storage enabled for ethash caches   dir=/home/ec2-user/.ethereum/geth/ethash count=3
INFO [02-11|11:09:55.344] Disk storage enabled for ethash DAGs     dir=/home/ec2-user/.ethash               count=2
INFO [02-11|11:09:55.344] Initialising Ethereum protocol           versions="[63 62]" network=1
INFO [02-11|11:09:55.378] Loaded most recent local header          number=0 hash=d4e567…cb8fa3 td=17179869184 age=49y9mo4w
INFO [02-11|11:09:55.378] Loaded most recent local full block      number=0 hash=d4e567…cb8fa3 td=17179869184 age=49y9mo4w
INFO [02-11|11:09:55.378] Loaded most recent local fast block      number=0 hash=d4e567…cb8fa3 td=17179869184 age=49y9mo4w
INFO [02-11|11:09:55.378] Loaded local transaction journal         transactions=0 dropped=0
INFO [02-11|11:09:55.378] Regenerated local transaction journal    transactions=0 accounts=0
INFO [02-11|11:09:55.399] New local node record                    seq=3 id=5f0ba2b890b3281c ip=127.0.0.1 udp=30303 tcp=30303
INFO [02-11|11:09:55.399] Started P2P networking                   self=enode://8ede8806a355854dbfa548006ff2e36793216f57c075fd04af1320e3b5472e9f4629adf268eb24b5e49ef
aebc00e5d911402cbf3caa31ef02de7b02eec873d11@127.0.0.1:30303
INFO [02-11|11:09:55.401] IPC endpoint opened                      url=/home/ec2-user/.ethereum/geth.ipc
INFO [02-11|11:09:55.401] HTTP endpoint opened                     url=http://0.0.0.0:8545               cors= vhosts=localhost
INFO [02-11|11:10:10.535] New local node record                    seq=4 id=5f0ba2b890b3281c ip=18.182.34.177 udp=30303 tcp=30303
INFO [02-11|11:13:14.172] Etherbase automatically configured       address=0xEebd32E2b3dbDe06b34558F37A865a95B09408a3
INFO [02-11|11:14:05.400] Block synchronisation started
INFO [02-11|11:14:08.811] Imported new block headers               count=192 elapsed=676.953ms number=192 hash=723899…123390 age=3y7mo1d
INFO [02-11|11:14:08.814] Imported new block receipts              count=2   elapsed=142.116µs number=2   hash=b495a1…4698c9 age=3y7mo1d  size=8.00B
````

## 命令行模式(syncing同步完成后)
[JavaScript Console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)   
[JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API)  
[Management API](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)
````angular2html
[centos]$ geth attach
Welcome to the Geth JavaScript console!

instance: Geth/v1.8.22-stable-7fa3509e/linux-amd64/go1.11.5
coinbase: 0xeebd32e2b3dbde06b34558f37a865a95b09408a3
at block: 7264641 (Mon, 25 Feb 2019 13:13:50 CST)
 datadir: /home/ec2-user/.ethereum
 modules: admin:1.0 debug:1.0 eth:1.0 ethash:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
 
> eth.syncing
false
> eth.blockNumber
7264663
> eth.getBalance ("0x7679962e959913563560e1bc3a31b0f3608f252b")
623240985000000000
> admin.nodeInfo
{
  enode: "enode://8ede8806a355854dbfa548006ff2e36793216f57c075fd04af1320e3b5472e9f4629adf268eb24b5e49efaebc00e5d911402cbf3caa31ef02de7b02eec873d11@18.182.34.177:30303",
  enr: "0xf896b8404eea611c9a5a4208e4deeb0aa8ad5be83795c578fd52a2af43c49c638f5d1c11553dbcd210e51d1592c3b0b374227a886fa515c4664fd7f4a256f998c09e75290483636170c6c5836574683f8269648276348269708412b622b189736563703235366b31a1038ede8806a355854dbfa548006ff2e36793216f57c075fd04af1320e3b5472e9f8374637082765f8375647082765f",
  id: "5f0ba2b890b3281c0ec7d2798327858488afbc76e5a16067be52973383ac3ac2",
  ip: "18.182.34.177",
  listenAddr: "[::]:30303",
  name: "Geth/v1.8.22-stable-7fa3509e/linux-amd64/go1.11.5",
  ports: {
    discovery: 30303,
    listener: 30303
  },
  protocols: {
    eth: {
      config: {
        byzantiumBlock: 4370000,
        chainId: 1,
        constantinopleBlock: 7280000,
        daoForkBlock: 1920000,
        daoForkSupport: true,
        eip150Block: 2463000,
        eip150Hash: "0x2086799aeebeae135c246c65021c82b4e15a2c451340993aacfd2751886514f0",
        eip155Block: 2675000,
        eip158Block: 2675000,
        ethash: {},
        homesteadBlock: 1150000,
        petersburgBlock: 7280000
      },
      difficulty: 9.249568946441871e+21,
      genesis: "0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3",
      head: "0x4cb70d998ee2c9a44f1252c72c03cb8149afef755007cbc7f118b764e3463363",
      network: 1
    }
  }
}
> admin.peers
[{
    caps: ["eth/62", "eth/63"],
    enode: "enode://b2e1e8cec9af6e5dcfb5bc191bbeda4f31a2217deaffff77c8106423dde04ca4d13716345392d818bb320e68a5f3ad16b4fdcd298341b2b0b4b0284185458c81@18.136.200.192:30303",
    id: "05eec498e3493f27826f16e8925fee6962826d1423b6ccc17a3362a47b64a1d9",
    name: "Geth/v1.8.19-stable-dae82f09/linux-amd64/go1.11.2",
    network: {
      inbound: false,
      localAddress: "10.100.2.198:48298",
      remoteAddress: "18.136.200.192:30303",
      static: false,
      trusted: false
    },
    protocols: {
      eth: {
        difficulty: 9.249574998567443e+21,
        head: "0x4c995f9187e700bd43194e8515d196cf5a15b13f0fd3a830a6e7b5202898186d",
        version: 63
      }
    }
},
...]
````

## eth数据文件默认路径
````angular2html
[centos]$ cd ~/.ethereum
[centos]$ ls -ltr
total 20
-rw-rw-r-- 1 ec2-user ec2-user 13614 Jul 25  2018 static-nodes.json
drwx------ 2 ec2-user ec2-user    91 Jan 25 19:32 keystore
srw------- 1 ec2-user ec2-user     0 Feb 11 11:09 geth.ipc
drwx------ 5 ec2-user ec2-user   101 Feb 25 13:09 geth
-rw------- 1 ec2-user ec2-user   572 Feb 25 13:17 history
````

## 参考文件
[github 官方地址](https://github.com/ethereum/go-ethereum)   
[CommandLine Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)    
[JavaScript Console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)    
[JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API)  
[Management API](https://github.com/ethereum/go-ethereum/wiki/Management-APIs)   


