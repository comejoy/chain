# bitcoinCash地址说明
bch地址沿用bitcoin地址编码支持P2PKH、与CashAddr
后续bch推出新的CashAddr地址，
目前新老地址均支持交易，可以相互转换，转换为1、3开头地址

# bitcoinCash地址校验规则
* 老地址长度为25-34位字符（不包含1位前缀）
* 老地址检查是否以1、3开头，适配测试环境以m、n开头
* 新地址检查是否以'bitcoincash:'开头，适配测试环境'bchtest'
* 旧地址大小写敏感，新地址大小写不敏感
* bch新地址类型CashAddr地址使用隔离见证地址，前缀为"bitcoincash:"(测试环境使用"bchtest:"前缀),新地址大小写不敏感，但不支持大小写混合
* 转账支持P2PKH、P2SH地址

# 地址支持类型
|地址规则|类型说明|地址前缀|地址长度（去除前缀）|例如|
|--|------|--|--|--|
|P2PKH|普通公钥地址|1|25-34|1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2|
|CashAddr|比特现金新地址|bc1|39|bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a|

# bitcoinCash编码
### 老地址使用base58编码
### 新地址使用使用base32编码

### 编码集
base58字符集：``123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz``
base32字符集：``qpzry9x8gf2tvdw0s3jn54khce6mua7l``

### reference
https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki
https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/cashaddr.md