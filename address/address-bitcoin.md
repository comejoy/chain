# bitcoin地址说明
目前btc、usdt的地址类型一致。usdt地址目前为久地址类型P2PKH，BTC地址大部分为P2SH地址
类型但依然存在大量P2PKH活跃。Bech32目前系统不支持，但有少量活跃Bech32地址。

# bitcoin地址校验规则
* 长度为25-34位字符（不包含1位前缀）
* 检查是否以1、3开头，适配测试环境以m、n开头
* P2PKH、P2SH大小写敏感，Bech32为大小写不敏感。目前均采用P2PKH、P2SH地址

# 地址支持类型
|地址规则|类型说明|地址前缀|地址长度（去除前缀）|例如|
|--|------|--|--|--|
|P2PKH|普通公钥地址|1|25-34|1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2|
|P2SH|脚本哈希地址|3|34|3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy|
|Bech32|隔离见证地址|bc1|39|bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq|

# bitcoin编码
### base58编码

### 编码集
``123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz``