# bitcoinCash地址说明
bch地址沿用bitcoin地址编码规则，P2PKH、P2SH
后续bch推出新的CashAddr地址，
目前新老地址均支持交易

# bitcoinCash地址校验规则
* 老地址长度为25-34位字符（不包含1位前缀）
* 检查是否以1、3开头，适配测试环境以2、4开头
* P2PKH、P2SH大小写敏感，Bech32为大小写不敏感。老地址目前均采用P2PKH、P2SH地址
* bch新地址类型CashAddr地址使用隔离见证地址，前缀为"bitcoincash:"(测试环境使用"bchtest:"前缀),新地址大小写不敏感，但不支持大小写混合

# 地址支持类型
|地址规则|类型说明|地址前缀|地址长度（去除前缀）|例如|
|--|------|--|--|--|
|P2PKH|普通公钥地址|1|25-34|1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2|
|P2SH|脚本哈希地址|3|34|3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy|
|CashAddr|比特现金地址|bc1|39|bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a|

# bitcoinCash编码
### 老地址使用base58编码
### 新地址使用使用base32编码

### 编码集
base58字符集：``123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz``
base32字符集：``qpzry9x8gf2tvdw0s3jn54khce6mua7l``

### reference
https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki