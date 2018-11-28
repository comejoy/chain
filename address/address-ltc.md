# litecoin地址说明
litecoin沿用bitcoin地址生成规则，将版本前缀由bitcoin的'0x00'替换为'0x30'最终编码后为L
测试环境为'0x6F'，编码后为Q。

# litecoin地址校验规则
* 长度为25-34位字符（不包含1位前缀）
* 检查是否以L开头，适配测试环境以Q开头
* 大小写敏感
* 转账支持P2PKH、P2SH地址

# 地址支持类型
|地址规则|类型说明|地址前缀|地址长度（去除前缀）|例如|
|--|------|--|--|--|
|P2PKH|普通公钥地址|L|25-34|LKKSCYdyWP7fJDMZ1KUDbpj3yPmQ22MQrv|

# litecoin编码
### base58编码

### 编码集
``123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz``

# reference
https://bitcoin.stackovernet.com/cn/q/14575